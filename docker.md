# Containers

## Introduction

It is common to build new analysis methods utilizing multiple programs, libraries, and modules, e.g., SAMtools or R with Bioconductor. Often you need very specific versions of the operating system and underlying programs, such as Ubuntu version 14.04, Bioconductor version 3.2, R version 3.2.2, and SAMtools 1.3, to correctly achieve desired results.

This delicate balance of dependencies is often called the “Software Dependency Hell”. It adversely impacts reproducibility of analyses and makes it very challenging to share your programs, pipelines, and analysis methods with collaborators and users who do not have access to identical systems, i.e., the same versions of all underlying software and operating system.

These issues have made it challenging for users to integrate new applications and analysis methods in the Cyverse Discovery Environment (DE), as the underlying execution platform could only support a limited number of versions of the same software, e.g., if your program expects to find the BWA program version 0.7.12 in /usr/local/bin/bwa, and another program expects 0.7.13, it was impossible for these to coexist without modifications to your code. With advances in container technology, these issues are now very easy to resolve, and support highly customized execution environments for every analysis. CyVerse is making use of Docker, one of the popular containerization platforms.

Additionally, public repositories of docker containers, such as [dockerhub.com](dockerhub.com) and [quay.io](quay.io), make it simple to share containers and use containers built by others.

## Containers vs. virtual machines (VMs)

The Docker website itself provides a concise comparison of containers and virtual machines [here](https://www.docker.com/what-docker#/VM).  The major commercial driver for developing and adopting the use of containers is web hosting.  Before containers, the best way to host a website in the cloud was to package everything into a virtual machine.  That way, if the host needed maintenance or a hardware update, the VM could be moved to a different server, web traffic could be sent to the new server, and there would ideally be no downtime.  Unfortunately, VMs are typically quite large in size, have a performance penalty because of the virtualization layer, and take several minutes (usually) to boot or reboot the VM image.

Containers provide an equally convenient way of packaging software along with all of its dependencies.  The way they are built up means they can do so with a much smaller footprint on the filesystem, with a much faster boot time (usually a few seconds), and because of the way containers create a unified filesystem, they sometimes end up having better performance than using the host operating system directly.

## Getting started with Docker

For future reference, Docker provides downloadable tools for most operating systems.  This means that you can build and develop apps and workflows on your own computer, run test jobs locally on your computer, and once everything is working, use that same docker app on any system that has the Docker Engine installed.


### Running an existing container

Before we make our own app, let's try running an existing app.  

```
docker run hello-world
```

Docker has lots of commands, the "run" command accepts an image name and tries to start up a container based on that image.  In this case, we said that the image was called "hello-world", and we did not give a specific version. If successful, your output will look like this:

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c04b14da8d14: Already exists
Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

If we look closely at the output, we see that Docker looked on the computer where it was running first to see if it had a downloaded copy of that container.  When it didn't find a container named hello-world, it went to search a hosting service (by default dockerhub) to see if there was a matching image.  Notice that since we didn't give a version, it just looked for the "latest" version, and since we didn't specify the author of the container, it looked in its "library" of official containers.  After it downloaded the container, it ran the container, and the container printed out some information for us.

This is a very simple toy example, but let's look at what went into creating this Docker image.  This image was actually generated from a GitHub repository, which is publically visible here: [https://github.com/docker-library/hello-world/tree/master/hello-world](https://github.com/docker-library/hello-world/tree/master/hello-world)

In the Github repository, there are three files:

* hello - this is a precompiled C program that is responsible for outputting the message we saw as standard output
* greeting.txt - a text file that contains the message "Hello from Docker!"
* Dockerfile - this file contains the recipe for creating the docker image we just ran

If we look inside the Dockerfile, here are the contents:

```
FROM scratch
COPY hello /
CMD ["/hello"]
```

#### [FROM](https://docs.docker.com/engine/reference/builder/#/from)

FROM is a Docker command that sets the "base image" that the rest of the recipe will build from.  "scratch" is a very minimal base image that is perfect for this simple app.  Practically speaking, apps will generally be built using something like "ubuntu" or "debian" as a fully featured base image, or "alpine" or "busybox" for a resulting image that has a small file size.

#### [COPY](https://docs.docker.com/engine/reference/builder/#/copy)

When Docker builds an image, it sets its current directory to wherever you run the docker command, which by default (and convention) is generally the same directory as the Dockerfile.  The COPY command takes a set of files and packages them up into a destination within the image.  In this case, the command is copying the "hello" command.  Notice that "greeting.txt" is never copied into the image.  Maybe it was used during compile time, but as far as we are concerned, it's just an informational file that lives only in the GitHub repository.

#### [CMD](https://docs.docker.com/engine/reference/builder/#/cmd)

The CMD is the default command that runs when we run a Docker container.  We could override the default command, as we'll show later, but this example is pretty much a one trick pony.

### Building your own container

