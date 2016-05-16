# optimizationbenchmarking/evaluator-gui

[![Image Size](https://img.shields.io/imagelayers/image-size/optimizationbenchmarking/evaluator-gui/latest.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Image Layers](https://img.shields.io/imagelayers/layers/optimizationbenchmarking/evaluator-gui/latest.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Docker Pulls](https://img.shields.io/docker/pulls/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)
[![Docker Stars](https://img.shields.io/docker/stars/optimizationbenchmarking/evaluator-gui.svg)](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/)

The current version of this image is 0.8.5.

## 1. Components

This environment allows you to run the evaluator gui as a dockerized application on your machine. It includes all the necessary software components.

## 2. Usage

### 2.1. Basics

You can run this image by using:

    docker run --rm -t -i -p 80:8080/tcp optimizationbenchmarking/evaluator-gui:<VERSION>
  
You should replace `<VERSION>` with the version of the GUI you want to run. Simply leave it away to run the [`latest`](https://hub.docker.com/r/optimizationbenchmarking/evaluator-gui/tags/) version of the container.
  
This will keep the container open and you can receive log data. If you do not care about the logged results, use

    docker run -t -d -p 80:8080/tcp optimizationbenchmarking/evaluator-gui:<VERSION>
    
When executing this command, docker will print some kind of key, a long sequence of numbers of letters:

    docker run -t -d -p 80:8080/tcp optimizationbenchmarking/evaluator-gui
    7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921

 You should copy this key (`7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921`) somewhere. Once you want to shut down the container with `docker stop` and the key, e.g.,
 
    docker stop 7a18dc0f674523795b700ba5c1a7671ed41e27d5fdfe431023f0cc20aed43921
    
In the section below we discuss how you configure the port at which the GUI should be accessible and where the data should be stored.
    
### 2.2. Configuration


There are two main configuration issues you can handle: ports and data.

#### 2.2.1. Port

The port through which you access the application is specified by the [`-p` parameter](http://docs.docker.com/engine/reference/run/#expose-incoming-ports) provided to docker. The general form is `-p PORT:8080/tco`, where `PORT` be the port under which you want to access the gui from the outside world. Usually (as in the examples under 2.1), we would pick port `80` or `8080` and, hence, specify  `-p 80:8080/tcp` or `-p 8080:8080/tcp`. If you specify `80` as port, you can directly browse to the GUI via [localhost](http://localhost). For port `8080`, you need to specify [localhost:8080](http://localhost:8080).


#### 2.2.2. Data

The gui uses the data part `/data` inside the docker container. This volume can be "mounted" to a real outside directory by using the [`-v` parameter](https://docs.docker.com/engine/userguide/containers/dockervolumes#mount-a-host-directory-as-a-data-volume) provided to docker. Write `-v /ABSOLUTE/PATH/:/data` to specify `/ABSOLUTE/PATH/` as absolute path to the data folder to be used by the container. Use `-v $(pwd)/data/:/data` to map the current working directory as `/data/` folder into the container.