---
layout: post
title: "Build your own PaaS with Pallet, Ubuntu, and Java"
description: ""
category:
tags: []
---
{% include JB/setup %}

Lately I've been using Pallet, a Platform as a Service (PaaS) library written in Clojure. It's great to see the building blocks of cloud computing coming together in libraries, prime for using to create new innovative packaging and deployment capabilities.  You can think of Pallet as Chef Recipes or Puppet, but instead of configuration files or Ruby, you write Clojure.  One of the main reasons I like Pallet is that it is a rich library to build your own PaaS capability, and requires very little from the nodes you build.  In fact, the only dependency is being able to SSH into a node and execute commands.  No agents, no servers, no repositories out of the box.  Copying jar files, executing apt-get, etc are all possible and you have flexibility to build what you want.

While the Pallet site has comprehensive API documentation, there is only one quick start that gets you going with EC2.  While I enjoy EC2 as much as the next person, the use case I was working on called for a more local development environment.  As luck would have it, Pallet has a library called VMFest, which is an abstraction over Oracle's VirtualBox.  Later, we would also pickup their Docker support, but that's a story for another day.  The nice thing about VMFest is you can use VirtualBox as a cloud provider.  On a reasonable piece of hardware, this means you can spin up virtual machines in 5-7 minutes, and they're full VMs that you can control.  Last but not least, I wanted Ubuntu and Java on these VMs as a solid base example to work from.  The following is a walk through of how to get started with Pallet, use VMFest as a good starter compute service, and install your first package - Java.

Pallet-java-example, the tutorial repo, is in Github.

Prerequisites:

*    Oracle VirtualBox version 2.4 or later
*    Leiningen, e.g. brew install leinginen
*    Something to edit Clojure with, I recommend Light Table
*    Follow the VirtualBox setup in the README (refer to Pallet-VMFEST Readme for issues)

With these requirements in hand, let's look at how you create this environment.  i started with the Pallet lein plugin for creating an example project.

    lein new pallet example

With the environment in place, I then proceeded to modify the project.clj file to include the VMFest dependencies, and the Pallet Java crate.  In pallet, a crate is a collection of functions that are grouped together as a reusable unit, much like a Chef recipe.

{% gist 8086125 project.clj %}

You will notice that we use the Virtual Box web service.  There is also a local COM interface, which I presume is higher performing but is not portable across all environments. For this environment, a few more miliseconds to talk to the VirtualBox isn't a big deal, so we'll go portability for ease of setup on different environments.  One important note, you cannot have both vbox dependencies in your configuration, as they use classpath loading and clash with eachother.

The next step is to look at what the lein pallet plugin generated for us.  The good news is that this is almost everything we need.  Let's take a look at the edited file:

{% gist 8086125 pallet.clj %}

In pallet terms, a node is an instance of a software stack running on a compute service, i.e. a VM in our instance with all of the software installed.  You'll see that we have a node-spec, some server-specs, and a group-spec.  Pallet provides flexibility in defining profiles of what you want the node (e.g. machine level parameters), sever (most of your software), and the group (your cluster).  These are all then converged or lifted together (i.e. deployed).  These layers of configuration are applied in order of phases: bootstrap, install, configure.

Some important notes here

 *   In the base server, we define {:bootstrap (plan-fn (automated-admin-user))}, which is telling pallet that during the bootstrap phase of the node, execute the function for automated-admin user.  For the EC2 tutorial, you provide Pallet with your EC2 credentials to solve the chicken-in-egg problem of how do you login to create the first user.  Pallet-vmfest solves this with a sudo user and sudo password, stored in the .meta file in ./vmfest/models.  If this inforation is not present, the SSH fails and therefore pallet executions fail.
 *   In the group-spec, you see that we extend java/server-spec.  This is where the crate is defined and used. In the end, it's just more functions, and if you configure them into a group-spec, they will run.

Now we're just about ready to create our cluster, but first we need to get our Ubuntu base image, and then pull it all together.

{% gist 8086125 main.clj %}

There are a couple things going on here:

 *   add-image is from vmfest, and adds an image and a .meta file to your ~/.vmfest/models directory
 *   converge is the main function you'll use to bring up and down a cluster - converge to a server count, and converge to 0 to destroy it
 *  nodes is a function that prints out your current nodes, also from vmfest

This should help folks get started with Pallet, gives you an introduction to a PaaS running on your local server, and is a fun way to apply Clojure to a domain.  Big thanks go out to Hugo Duncan and Antoni Batechilli, they both are very helpful to all who join #pallet and get started.



To recap the links:

1.    [Pallet-java-example repo](https://github.com/AlBaker/pallet-java-example), i.e. the source for this article
2.    [Pallet official website](http://palletops.com/)
3.    [VMFest](https://github.com/tbatchelli/vmfest) and [Pallet-VMFest](https://github.com/pallet/pallet-vmfest)
4.    [Oracle VirtualBox](https://www.virtualbox.org/)
5.    [LightTable](http://www.lighttable.com)

Enjoy!

