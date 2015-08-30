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
is not a new concept for you. Moreover with the growth of Docker and LXC emerged questions such
as: What is LXC? What is the difference between virtual machines and linux containers?

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
