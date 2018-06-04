---
date: 2018-06-04 19:25:00
read_in: 5
translations: /instalar-virtualbox-no-fedora
title: Install the VirtualBox on Fedora 27 and 28
keywords: install virtualbox configure fedora 27 28
description: Tutorial to install and configure the VirtualBox with the extension pack on Fedora 27 and 28.
---

The [virtualization](https://en.wikipedia.org/wiki/Virtualization) makes easy our day a day as software developer, both at development environment as the production environment, among many programs of virtualization existing maybe the most popular is the [VirtualBox](https://www.virtualbox.org/) and all certain it is much simple of to use, therefore this article show how to install it on [Fedora](https://getfedora.org) 27 and 28.

## Add the repository

Before installing we'll to add the official repository of the VirtualBox and upgrade the operational system.

{% highlight sh %}
sudo wget https://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo -O /etc/yum.repos.d/virtualbox.repo && \
sudo dnf -y upgrade
{% endhighlight %}

## Install the requirements

Now let's go to install the requirements.

{% highlight sh %}
sudo dnf install -y binutils gcc make patch libgomp glibc-headers \
glibc-devel kernel-headers kernel-devel dkms
{% endhighlight %}

Before of go forward execute the command below and verify if your operational system already using the most recent [Kernel](https://en.wikipedia.org/wiki/Linux_kernel) version.

{% highlight sh %}
rpm -qa kernel | sort -V | tail -n 1 | sed s/kernel-// && uname -r
{% endhighlight %}

## Install the VirtualBox

With the Kernel more recent in use and all requirements installed we can finally install the VirtualBox, at the moment of write of this article the most recent version is the 5.2, so we'll to install it.

{% highlight sh %}
sudo dnf install -y VirtualBox-5.2
{% endhighlight %}

Next require to VirtualBox to run the setup.

{% highlight sh %}
sudo /usr/lib/virtualbox/vboxdrv.sh setup
{% endhighlight %}

Now include your user to VirtualBox user group.

{% highlight sh %}
sudo usermod -a -G vboxusers $USER
{% endhighlight %}

## Extension pack

Optionally you can to install the extension pack of VirtualBox to improve the integration between the virtual machines and the host machine, for this you should to do download of the extension pack compatible with the version of your VirtualBox.

{% highlight sh %}
wget https://download.virtualbox.org/virtualbox/5.2.12/Oracle_VM_VirtualBox_Extension_Pack-5.2.12.vbox-extpack
{% endhighlight %}

For to install execute the command below, agree the terms of use and then the installation will be done.

{% highlight sh %}
sudo VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack*.vbox-extpack
{% endhighlight %}

## Wrapping up

Now you can to use the operation system that you wish without to install it or use the [dual boot](https://en.wikipedia.org/wiki/Multi-booting) directly in your computer, on my case I use the VirtualBox for have a virtual machine with Windows and some others with Linux distributions where I configure the development environment as similar as possible with the production environment.

## Resources

* [https://www.virtualbox.org/wiki/Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads)
* [https://www.virtualbox.org/manual/UserManual.html](https://www.virtualbox.org/manual/UserManual.html)
