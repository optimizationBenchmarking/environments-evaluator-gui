# optimizationbenchmarking/evaluator-gui

[![Image Layers and Size](https://imagelayers.io/badge/optimizationbenchmarking/evaluator-gui:latest.svg)](https://imagelayers.io/?images=optimizationbenchmarking%2Fevaluator-gui:latest)
[![Image Layers](https://img.shields.io/imagelayers/layers/optimizationbenchmarking/evaluator-gui/latest.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Docker Pulls](https://img.shields.io/docker/pulls/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Docker Stars](https://img.shields.io/docker/stars/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)

The current version of this image is 0.8.6.

## 1. Components

This environment allows you to run the evaluator gui as a dockerized application on your machine. It includes all the necessary software components which it mainly imports from the image [evaluator-runtime](https://hub.docker.com/r/optimizationbenchmarking/evaluator-runtime/), including:

- [`evaluatorGui.jar`](https://github.com/optimizationBenchmarking/evaluator-gui/) [version 0.8.5](https://github.com/optimizationBenchmarking/evaluator-gui/releases/download/0.8.5/evaluatorGui.jar)
- [`Java 8 OpenJDK`](http://openjdk.java.net/projects/jdk8/)
- [`TeX Live`](http://www.tug.org/texlive/)
- [`ghostscript`](http://ghostscript.com/)
- [`R`](https://www.r-project.org/)

For [`R`](https://www.r-project.org/) we automatically pre-install libraries which may be needed by our software, namely

- [`apcluster`](https://cran.r-project.org/web/packages/apcluster/index.html)
- [`cluster`](https://cran.r-project.org/web/packages/cluster/index.html)
- [`fpc`](https://cran.r-project.org/web/packages/fpc/index.html)
- [`mclust`](https://cran.r-project.org/web/packages/mclust/index.html)
- [`NbClust`](https://cran.r-project.org/web/packages/NbClust/NbClust.pdf)
- [`stats`](http://stat.ethz.ch/R-manual/R-patched/library/stats/html/stats-package.html)
- [`vegan`](https://cran.r-project.org/web/packages/vegan/index.html)


## 2. Usage

You can run this image by using `docker run`. The [Evaluator GUI](https://github.com/optimizationBenchmarking/evaluator-gui/) will then be started inside the container. This GUI is a web-based application, meaning that you can access it via your web browser. With your browser, you can then upload and download experimental data, install example data sets from the web, start evaluation processes, and download the generated reports as PDF, LaTeX sources, or HTML. In the following, we first discuss a  

### 2.1. Basics

#### 2.1.0. Install Docker

If you do *not* yet have [Docker](http://www.docker.com/) installed under your System, you can pick one of the options below. Our software has only been tested under Linux:

<ol>
<li><em>Linux</em>: on your Linux system, you can run
<pre>curl -fsSL https://get.docker.com/ | sh</pre>    
as recommended in the official <a href="https://docs.docker.com/linux/step_one/">Docker installation page</a>.</li>
<li><em>Windows</em>: <a href="https://docs.docker.com/windows/">https://docs.docker.com/windows/</a></li>
<li><em>Mac OS X</em>: <a href="https://docs.docker.com/mac/">https://docs.docker.com/mac/</a></li>
</ol>
This will install Docker. 

#### 2.1.1. Running the Image

After you have installed Docker, you can run this image by using:

    docker run -t -i -p 80:8080/tcp optimizationbenchmarking/evaluator-gui:<VERSION>
  
You should replace `<VERSION>` with the version of the GUI you want to run. Simply leave it away to run the [`latest`](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/tags/) version of the container.
  
The above command will keep the container's console open and you can receive log data and close the program with `Ctrl-C`. If you do not care about the logged results and want to run the container in the background, use

    docker run -t -d -p 80:8080/tcp optimizationbenchmarking/evaluator-gui:<VERSION>
    
When executing this command, Docker will print some kind of key, a long sequence of numbers of letters, which *could* look like the following:

    docker run -t -d -p 80:8080/tcp optimizationbenchmarking/evaluator-gui
    7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921

You should copy this key (in this example `7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921`, but it could also be something else) somewhere. Once you want to shut down the container with `docker stop` and the key, e.g.,
 
    docker stop 7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921
    
Of course, the key in your situation may look different.

In the section below we discuss how you configure the port at which the GUI should be accessible and where the data should be stored.
    
### 2.2. Configuration

There are two main configuration issues you can handle: ports and data volume.

#### 2.2.1. Port

The port through which you access the application is specified by the [`-p` parameter](http://docs.docker.com/engine/reference/run/#expose-incoming-ports) provided to Docker. The general form is `-p PORT:8080/tco`, where `PORT` should be replaced with the number of the port under which you want to access the GUI from the outside world. Usually (as in the examples under 2.1), we would pick port `80` or `8080` and, hence, specify  `-p 80:8080/tcp` or `-p 8080:8080/tcp`. If you specify `80` as port, you can directly browse to the GUI via [localhost](http://localhost). For port `8080`, you need to specify [localhost:8080](http://localhost:8080).


#### 2.2.2. Data

The GUI uses the data part `/data` inside the Docker container. This volume can be "mounted" to a real outside directory by using the [`-v` parameter](https://docs.docker.com/engine/userguide/containers/dockervolumes#mount-a-host-directory-as-a-data-volume) provided to docker. Write `-v /ABSOLUTE/PATH/:/data` to specify `/ABSOLUTE/PATH/` as absolute path to the data folder to be used by the container. Use `-v $(pwd)/data/:/data` to map the current working directory as `/data/` folder into the container.