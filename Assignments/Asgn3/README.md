# Assignment 3: Docker 
## Introduction
Docker is a way to wrap a solution with all it's dependencies in a container as a lightweight alternative to virtual machines and solving problems like dependency hell and deployment optimization. In order to ramp up and be able to package solutions the following will be needed:

## Complete by 9/29
### Dependencies
+ Make an account in [Docker Hub](https://hub.docker.com/)
+ We assume you have read the [Bash shell tutorial](http://www.ee.surrey.ac.uk/Teaching/Unix/) from assignment 0 or that you are familiar with the basic Unix commands.
+ Install Docker:
    - If you are working on Linux, Mac, or Windows Professional
    	+ Download the appropriate installer from [here](https://www.docker.com/products/docker#/windows) and follow the instructions.
    	+ Install Docker Compose from [here](https://docs.docker.com/compose/install/)
    - If you have a Windows Home installation, make sure to download Docker from [here](https://www.docker.com/products/docker-toolbox) and make sure to choose the option to install VirtualBox as Docker needs virtualization in non-Linux systems. Everytime that you need to execute Docker commands in the command line make sure to run *docker-machine start* to boot up the virtual machine in which Docker runs in your system and then run the appropriate [environment configuration command](https://getcarina.com/docs/tutorials/load-docker-environment-on-windows/). To make sure that it's setup correctly run *docker run hello-world*. In case it doesn't display 

    	> Hello from Docker!

    	There's a [troubleshooting](https://docs.docker.com/toolbox/faqs/troubleshoot/) section in the Docker documentation to configure the virtual machine.

+ Optional: Java, Python, and PiP are required if you want to follow the long example on how to use Docker.

### Readings
+ Work through the [Docker Documentation: Docker Overview](https://docs.docker.com/engine/understanding-docker/) to understand the terminology and the purpose of Docker. Also, work through the steps in [Get Started With Docker](https://docs.docker.com/engine/getstarted/step_two/) starting from 'Understanding images and containers' to 'Tag, push, & pull your image' to understand the basics of Docker. If you are running on Windows make sure you are running the MINGW console (either the one install by Git or the one installed through Docker) as some of the commands are not available in powershell (such as touch).
	From this you'll learn:
	- How Docker works
	- Docker's purpose
	- Containers and images
	- How to push and pull images from Docker hub
	- How to build images from dockerfiles
+ Read through [Docker Compose](https://docs.docker.com/compose/overview/) to understand how to run multiple containers. Read through Overview, installation, and 'getting started'.
+ Read [this article](https://gigaom.com/2014/06/10/why-companies-like-google-spotify-and-red-hat-are-embracing-dockers-open-source-containers/) to understand how the industry is using Docker to scale systems.
+ **OPTIONAL:** Read the [JHipster documentation](https://jhipster.github.io/docker-compose/) on how to package JHipster solution into a container. Read this only **after** the lecture on JHipster to better understand what is being done and what will be needed. You only have to read up to 'Building and running a Docker image of your application'. This helps you get an automated DockerFile. While in the project this will need to get done by hand, having an autogenerated file might help you get through.
+ **OPTIONAL:** [This](https://prakhar.me/docker-curriculum/#webapps) is a very thorough explanation on how to deploy a whole web app to Amazon using Docker and also explains best-practice usages of docker in real life development. Read through to understand usage how to use multiple containers using different techniques, port-forwarding, and how to search for existing images and services in Docker Hub and base your products off these. There's no need to follow the tutorial as many of the topics covered will also get covered by other groups, but reading it definitely explains how powerful containment technology is. Section 2 deploys a simple container to have a webapp running using flask in AWS. Section 3 explains how multi-container apps work in real life. 

## In-class assignment
### Pre-assignment
+ Quiz on the reading materials.
+ If you are using Docker-Toolset make sure you run `docker-machine start` and set the environment variables accordingly.
+ To verify you have downloaded everything before class, download and run our docker image by running `docker run -i --name verif hoyosjs/installation_verification`. Basically you are fetching an image from DockerHub and runnining a container of it under the name `verif` in interactive mode. 
+ Fill [this form](https://docs.google.com/a/vanderbilt.edu/forms/d/e/1FAIpQLScZ7tOaBJjbu95gM8l1jv4OmnYgagh8jVmrwxa1dukQd19xRA/viewform?c=0&w=1) with the output of the command.
+ Stop the running image with `docker stop verif`.
+ Clean the images and containers by running `docker rm verif` and `docker rmi hoyosjs/installation_verification`

### Pack JHipster's team solution into a container
+ Create a file named `Dockerfile` with no extension.
+ Make sure you can get the `openjdk:8` image by running `docker search openjdk`
+ Create a Dockerfile on the folder containing your _JHipster_ solution.
+ As explained in the [reading material](https://docs.docker.com/engine/getstarted/step_four/) edit the `Dockerfile` to base your new image on `openjdk:8`
+ Add a maintainer to the `Dockerfile` (i.e. yourself)
+ Change the working directory with [`WORKDIR`](https://docs.docker.com/engine/reference/run/#workdir) to `/app/jhipster-project`.
+ Add the `war` file generated by building your _JHipster_ project in [production mode](https://index.docker.io/v1/search?q=alpine&n=25) to the working dir. Your file is in `build/libs/` 
+ Add a line `EXPOSE 8080` to expose port 8080 by default.
+ As the last line of your Dockerfile use `CMD` to run the war file generated by jhipster. To run this you'll have to call `java` with the `jar` flag. You should also add `--spring.profiles.active=dev,swagger` at the end of the command 
+ Build your image with `docker build -t <your_docker_hub_user>/jhipster-project .`
+ Run `docker run -P -d --name web-service jhipster-project` to run your app. If you pass the flag `-i` it will run in interactive mode (binds the STDIN), if you pass `-d` it runs in detached (daemon) mode. 
+ Kill the running container with the `stop` command in docker with your containers name (hint: `web-service`).
+ You can try accessing it from your browser. First run `docker ps` and get the port it's running on. If you are running Docker native go to `localhost:<port_number>` otherwise run `docker-machine env` and copy the tcp address for your machine ignoring the port number, and on your browser go to `<tcp_address>:<Port_number>`. 
+ Push your image to the hub using `docker login` and `docker push`.

### Getting the grading dependency configured
+ Create a file called `vunet` and write your vunetid. Double check the spelling.
+ Create a new docker file.
+ Base it off `hoyosjs/`
+ Change the `WORKDIR` to `/app/data/`
+ Add the vunet file to the container by adding the `ADD ./vunet .` to the Dockerfile
+ Build the image and call it `grading_helper`.

### Docker compose with grading dependency 
+ Create a file `docker-compose.yml`
+ Add a service called jhipster that depends on your image and forward ports as necessary.
+ Add a service called grading that depends on the grading image you built. 
	Each service gets its own top level ‘object’ written as ‘service1’ or ‘service2’
	Beneath each you need to specify the image name, the ports each uses, or the dockerfile used to build it.

	 Example:
	 ```yaml
	jhipster:
	  build: .
	  dockerfile: Dockerfile
	  ports:
	    - "8080:8080"
	  volumes:
	    - .:/usr/src/app

	grading_helper:
	  image: hello-world 
	  ports:
	    - "8080:8080"
	  volumes:
	    - .:/usr/src/app
	 ```

+ Run `docker-compose up` and wait.
+ If you performed all steps correctly you should get a token which you should [send here](https://goo.gl/forms/Rq0kb2PowbwHEMr23).

### Final step
+ Please add the Dockerfiles for both the images you generated to your Github repo under `Assignment/Asgn3` and push the changes to your `dev` branch

### Acknowledgements
+ The first reading is directly from the Docker documentations as a general overview of how to work with and how Docker works.
+ Integration with other tools is referred directly from documentations as they are from regularly updated packages.
+ [Prakhar Srivastav](http://prakhar.me/) is a grad student in Columbia University's CS department with one of the most complete crash courses on Docker listing many alternatives in his course. His GitHub repo for the course has active discussion on the issues of the course and is actively updated making it a reliable source as well as appearing in several Quora answers on the topic. 
+ Thanks to Harrison Stall for Beta testing these readings!

### Team members
+ [Campbell, Grey](mailto:grey.e.campbell@vanderbilt.edu)
+ [Chung, Hoon Tae](mailto:hoon.tae.chung@vanderbilt.edu)
+ [Degroot, Max](mailto:alexander.m.degroot@vanderbilt.edu)
+ [Goggans, Connor A](mailto:connor.goggans@vanderbilt.edu)
+ [Hoyos Ayala, Juan Sebastian](mailto:juan.s.hoyos@vanderbilt.edu)
+ [Hua, Hongtao](mailto:hongtao.hua@vanderbilt.edu)
+ [Samost, Jonah](mailto:jonah.samost@vanderbilt.edu)
+ [Mbusa, Paul](mailto:paul.y.mbusa@vanderbilt.edu)
