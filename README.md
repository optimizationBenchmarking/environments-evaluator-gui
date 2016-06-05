# optimizationbenchmarking/evaluator-gui

[![Image Layers and Size](https://imagelayers.io/badge/optimizationbenchmarking/evaluator-gui:latest.svg)](https://imagelayers.io/?images=optimizationbenchmarking%2Fevaluator-gui:latest)
[![Docker Pulls](https://img.shields.io/docker/pulls/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Docker Stars](https://img.shields.io/docker/stars/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)

The current version of this image is 0.8.6.

## 1. Installing Docker

Docker can be installed following the guidelines below:

* for [Linux](https://docs.docker.com/linux/step_one/), you can run  `curl -fsSL https://get.docker.com/ | sh` on your command line and everything is done automatically (if you have `curl` installed, which is normally the case),
* for [Windows](https://docs.docker.com/windows/step_one/)
* for [Mac OS](https://docs.docker.com/mac/step_one/)

## 2. Usage

You can run this image by using `docker run` in a normal terminal (Linux), the *Docker Quickstart Terminal* (Mac OS), or the *Docker Toolbox Terminal* (Windows). The [Evaluator GUI](https://github.com/optimizationBenchmarking/evaluator-gui/) will then be started inside the container. This GUI is a web-based application, meaning that you can access it via your web browser. With your browser, you can then upload and download experimental data, install example data sets from the web, start evaluation processes, and download the generated reports as PDF, LaTeX sources, or HTML. In the following, we first discuss a  

### 2.1. Basics

If you have installed Docker, you can run this image by using the following command in a normal terminal (Linux), the *Docker Quickstart Terminal* (Mac OS), or the *Docker Toolbox Terminal* (Windows):

    docker run -t -i -p 9999:8080/tcp optimizationbenchmarking/evaluator-gui
  
This runs the [`latest`](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/tags/) version of the evaluator GUI. You can access it with your browser at address

- [`http://localhost:9999`](http://localhost:9999) under Linux or
- `http://<dockerIP>:9999` under Windows and Mac OS, where `dockerIP` is the IP address of your Docker container. This address is displayed when you run the container. You can also obtain it with the command `docker-machine ip default`.

When starting the image, you can also specify `optimizationbenchmarking/evaluator-gui:<VERSION>` where `<VERSION>` should be replaced with the version of the GUI you want to run.
  
The above command will keep the container's console open and you can receive log data and close the program with `Ctrl-C`. If you do not care about the logged results and want to run the container in the background, use

    docker run -t -d -p 9999:8080/tcp optimizationbenchmarking/evaluator-gui
    
When executing this command, Docker will print some kind of key, a long sequence of numbers of letters, which *could* look like the following:

    docker run -t -d -p 9999:8080/tcp optimizationbenchmarking/evaluator-gui
    7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921

You should copy this key (in this example `7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921`, but it could also be something else) somewhere. Once you want to shut down the container with `docker stop` and the key, e.g.,
 
    docker stop 7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921
    
Of course, the key in your situation may look different.

In the section below we discuss how you configure the port at which the GUI should be accessible and where the data should be stored.
    
### 2.2. Detailed Configuration

There are two main configuration issues you can handle: ports and data volume.

#### 2.2.1. Port

The port through which you access the application is specified by the [`-p` parameter](http://docs.docker.com/engine/reference/run/#expose-incoming-ports) provided to Docker. The general form is `-p PORT:8080/tcp`, where `PORT` should be replaced with the number of the port under which you want to access the GUI from the outside world. Often, we would pick port `80` or `8080` and, hence, specify  `-p 80:8080/tcp` or `-p 8080:8080/tcp`. If you specify `80` as port, you can directly browse to the GUI via [localhost](http://localhost) on Linux (replace `localhost` with the IP address of the Docker container on Windows or Mac OS). For port `8080`, you need to specify [localhost:8080](http://localhost:8080) (under Linux).

These ports are recommended if you run the system as a server to be accessed from the network. However, they may be unavailable under certain conditions:

- You already run a web server at port 80.
- You already run an application server at port 8080.
- You use Docker under Windows or Mac OS.

In this case, it may be easier to stick with some obscure port, like the `9999` in our examples above.

#### 2.2.2. Data

The GUI uses the data part `/data` inside the Docker container. This volume can be "mounted" to a real outside directory by using the [`-v` parameter](https://docs.docker.com/engine/userguide/containers/dockervolumes#mount-a-host-directory-as-a-data-volume) provided to docker. Write `-v /ABSOLUTE/PATH/:/data` to specify `/ABSOLUTE/PATH/` as absolute path to the data folder to be used by the container. Use `-v $(pwd)/data/:/data` to map the current working directory as `/data/` folder into the container.

## 3. Components

This environment allows you to run the evaluator GUI as a dockerized application on your machine. It includes all the necessary software components which it mainly imports from the image [evaluator-runtime](https://hub.docker.com/r/optimizationbenchmarking/evaluator-runtime/), including:

- [`evaluatorGui.jar`](https://github.com/optimizationBenchmarking/evaluator-gui/) [version 0.8.6](https://github.com/optimizationBenchmarking/evaluator-gui/releases/download/0.8.6/evaluatorGui.jar)
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