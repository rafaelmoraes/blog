---
date: 2018-06-05 15:22:00
translations: /instalar-vagrant-no-fedora
read_in: 8
title: Install Vagrant on Fedora 27 and 28
keyword: install use vagrant fedora 27 28
description: Tutorial for install the Vagrant with the VirtualBox on Linux Fedora, making easier the creation and management of virtual machines.
---

The [Vagrant](https://www.vagrantup.com) is a tool of command line([CLI](https://en.wikipedia.org/wiki/Command-line_interface)) that abstracts the complexity of main virtualizers available and allow us to create virtual machines in a way extremely simple, for use it just create the [manifest](https://en.wikipedia.org/wiki/Manifest_file) file and from this file a [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) will be provided with all what we need configured, therefore let's go see how to install it on [Fedora](https://getfedora.org).

## Requirements

For that Vagrant works is necessary have installed some virtualization software, you can know which are supported [here](https://www.vagrantup.com/docs/providers/),
another requirement is that your computer needs to be able to access another by [SSH](https://en.wikipedia.org/wiki/Secure_Shell).

This tutorial will be used the [VirtualBox](https://www.virtualbox.org/) for virtualization, case you don't have some requirements and not know how to install it you can follow this tutorial to [install VirtualBox](/install-virtualbox-on-fedora) and this one to [install and configure the SSH](generate-key-ssh-on-linux).

## Install Vagrant

With the VirtualBox and OpenSSH installed we can proceed with download and installation, for it you need to copy and run the text below on your terminal.

{% highlight sh %}
wget https://releases.hashicorp.com/vagrant/2.1.1/vagrant_2.1.1_x86_64.rpm && \
sudo dnf install -y ./vagrant*.rpm && \
rm vagrant_*.rpm*
{% endhighlight %}

The Vagrant by default no uses conventional installation images, those that you access the site of your Linux distribution preferred, make the download and install on you machine or server, instead those images the Vagrant uses the [boxes](https://www.vagrantup.com/docs/boxes.html), that basically are images derived from official distributions, where everything that not is helpful or essential for Vagrant is dropped, anyone can create own boxes, you can find some compatible boxes of VirtualBox [here](https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=created&provider=virtualbox&q=).

For provide a virtual machine the Vagrant uses the file *Vagrantfile* where stay all details about the provisioning, for example: shared files, amount of RAM memory, IP address, shared ports, software that are installed automatically, among [others](https://www.vagrantup.com/docs/vagrantfile/).

## Create Vagrantfile

Let's create the *Vagrantfile* in directory that we wish shared with the [VM](https://en.wikipedia.org/wiki/Virtual_machine), in this sample we'll provision a VM with [Fedora 28 cloud base](https://alt.fedoraproject.org/cloud/), but you can use the box that you wish, only be certain that the chosen box is supported by VirtualBox, otherwise it will not works with this tutorial.

{% highlight sh %}
vagrant init fedora/28-cloud-base
{% endhighlight %}

The *Vagrantfile* uses syntax of [Ruby](https://www.ruby-lang.org/)(but you don't need know programming in Ruby), you can change the configurations as necessary, at this article we'll use the default configuration, for we not to get away from the main goal.

## Start VM

After create the *Vagrantfile* we going to request to Vagrant that start the VM, the command below works as follows: it look up by the *Vagrantfile* beginning from current directory until the root of your file system, after to find and to read the *Vagrantfile* will be checked if the box defined in the file already there is locally, case not exists will be make your download and only then the VM will be started, with all configurations present in the *Vagrantfile*.

All this process can be delay depending of your hardware and network, mainly when is necessary to download of the box.

{% highlight sh %}
vagrant up
{% endhighlight %}

## Access VM

The access to VM is through SSH, making our lives easier the Vagrant provides the command below for we to connect to VM, then we don't need to concern with users, IP address or passwords.

{% highlight sh %}
vagrant ssh
{% endhighlight %}

If everything went right, now you're logged in VM and can do everything what do you'd in a common distribution, install programs, make downloads, create files and everything will be waiting for you at the next time that you boot and access the VM.

By default the directory shared is mounted in VM at */vagrant*, but you can change it in *Vagrantfile* if you want, to access it, run.

{% highlight sh %}
cd /vagrant
{% endhighlight %}

Running ```ls``` in directory */vagrant* you should see the file *Vagrantfile* at list of the files there is in this directory, every files created, deleted or updated in this directory will be are status changed on your machine and in VM simultaneously, not mattering on which machine the operation has been made.

## Shutdown VM

For shutdown a VM using Vagrant run the command below in directory where is the *Vagrantfile* or in your sub-directories.

{% highlight sh %}
vagrant halt
{% endhighlight %}

## Delete VM

Case you need to delete some VM, so run.

{% highlight sh %}
vagrant destroy
{% endhighlight %}

## Wrapping up

If you open the VirtualBox you can see all VMs created by Vagrant, as already said previously the Vagrant isn't a virtualizer, for avoid future troubles I recommend that you don't change the VM configurations through VirtualBox [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface), always use the *Vagrantfile* for this.

Then we arrived to end of this tutorial, now you should be able of easily to create virtual machines with Vagrant.

### Resources

* [https://www.vagrantup.com/docs/index.html](https://www.vagrantup.com/docs/index.html)
* [https://app.vagrantup.com/fedora/boxes/28-cloud-base](https://app.vagrantup.com/fedora/boxes/28-cloud-base)
