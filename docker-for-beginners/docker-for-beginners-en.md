# Docker for Beginners

![alt tag](https://github.com/bugfactory/posts/blob/master/docker-for-beginners/imgs/docker.png)

Nowadays there is a lot of talk around docker in the geek world, indeed, 
this technology is increasing and every day new companies are embrancing it.
Docker became almost a required knowledge for developers and sysadmins.

Thinking about it, we want to create a sequence of posts to try to cover
the subject in the best way. In this first post we want to answer the basic
questions, showing the main concepts, how to install and administrate a docker environment.

I hope you enjoy the content of this post and if you have questions, sugestions or complains,
just leave a comment and we’ll try to answer as soon as possible.

## Basic Concepts

Before we start talking about Docker we have to keep in mind concepts such as: 
Linux Containers (LXC) and Virtual Machines (VMs). They always appear in papers related to docker.

The technology companies started adopting virtual machines a long time ago and I believe that
is not a new concept for you. Moreover with the growth of Docker and LXC some questions emerged.
What is LXC? What is the difference between virtual machines and linux containers?

LXC is a virtualization in operating system level, it is used to run multiple isolated Linux systems,
it makes use of kernel features such as cgroups and namespaces. These isolated environments are known
as containers. The kernel cgroups feature allows resource sharing such as: CPU, memory and network.
The namespace creates an isolation for containers, in other words it ensures that a container can’t
see or impact another.

When we build a virtual machine we have a complete system, with its own libraries, kernel and binary.
It means that we have a copy of a isolated physical machine. However LXC works in a different way,
because it uses the hosts’ resource, such as: libs, kernel and binaries. As a result to build a container
is faster and lighter than a virtual machine.

![alt tag](https://github.com/bugfactory/posts/blob/master/docker-for-beginners/imgs/docker-containers-vms.png)

The concept of cgroups and namespaces is much more complex than mentioned above. But our purpose is not
to go deeper in these concepts, but rather show docker tool, which uses both features. If you have
curiosity to know more about these them, a quick search on the internet will bring you great papers.

## A bit of history

The Docker development started by Solomon Hykes and others engineers at dotCloud. At the beginning it
was just an internal project to handle Linux Containers (LXC), then the developers realized the power
they had in hand and they decided to open the project.

So, what is docker? “Docker containers wrap up a piece of software in a complete filesystem that 
contains everything it needs to run: code, runtime, system tools, system libraries – anything you
can install on a server. This guarantees that it will always run the same, regardless of the 
environment it is running in.” (docker.com)

Although in the begging Docker used to make use of LXC to access the kernel’s virtualization features,
since version 0.9, the tool uses libcontainer library which was developed by Docker company using Go
language to do the same. Also Docker is still compatible with other interfaces (libvirt, LXC,
systemd-nspwan). As shown in the figure below.

![alt tag](https://github.com/bugfactory/posts/blob/master/docker-for-beginners/imgs/lxc-docker.png)

## Docker Container

Every Docker Container is built from an image, and its life cycle consists in the execution of a process,
thus while this process is running, the container also will run.

One container can run more than one process, but it isn’t a good practice. This concept is important to
understand how to “Dockerize” your application.

[“In almost all cases, you should only run a single process in a single container. Decoupling applications
into multiple containers makes it much easier to scale horizontally and reuse containers. If that service
depends on another service, make use of container linking.”](https://docs.docker.com/articles/dockerfile_best-practices/)

## Docker Image

They are read-only templates used to build containers. For instance, an Image would be the
operational system ubuntu with a web server installed. The Image is characterized as read-only
because from the moment you change it, it becomes another image. This concept will be more
easily to understand when we start using Docker in the command line.

## Docker Registry and Docker Hub

Imagine the following scenario: you work as a sysadmin in a company that is adopting Docker technology.
Inside this environment, developers want to manipulate and share many images, based in different
distributions and running different applications. They are creating new images and updating the ones
that already exist all the time. Also, these images must be private. How to manage such task?

That is the reason docker registry exists, it is an open source project which allows you to save,
share and manage your images in docker (in-house registry).

Docker Company provides a service of public registration, where users are able to save their images,
known as Docker Hub. Further we will show how to download, change and publish a image in docker hub,
but first you have to be registered into it.

## How to install

To install it is not complicated, and it is possible to find a variaety of articles teaching how 
to do it in the distribuition you like more. Just be aware about the ???????, such as:

 - 64-bit installation regardless of your distribution version
 - kernel must be 3.10 at minimum

The linux distribution used in this post was ubuntu 14.10. If you have an old kernel version I
advise to read at docker website.

To install docker we will need curl package.

```
$ sudo apt-get install curl
```

Now we can install Docker.

```
$ curl -sSL https://get.docker.com/ |sh
```

Look this message when we finished docker installation: *“If you would like to use Docker as a
non-root user, you should now consider adding your user to the "docker" group with something like”.*

```
$ usermod -aG docker <YOUR-USERNAME>
```

Remember that you will have to log out and back in for this to take effect!

Docker offers one test image that can be used to verify if the installation happened correctly.
The name of it could not be different, “hello-world”. When you execute the command below you
will be downloading the image and executing it in one container. 

```
$ docker run hello-world
```

## Starting with Docker

## Publishing your images

## References

 - Docker
 - Google
 - Friends
