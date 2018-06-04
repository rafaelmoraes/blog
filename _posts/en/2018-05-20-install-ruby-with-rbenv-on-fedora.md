---
date: "2018-05-28 21:30:00"
title: "Install Ruby with rbenv on Fedora 27 e 28"
keywords: "how to install ruby rbenv fedora 27 28"
read_in: 8
translations: /instalar-ruby-com-rbenv-no-fedora
---

This article is a tutorial for to install the [Ruby](https://www.ruby-lang.org/en/) on [Fedora](https://getfedora.org) 27 and 28 by the [rbenv](https://github.com/rbenv/rbenv) together with some of its plugins.

Install the Ruby from official Fedora packages, isn't a very good solution in my opinion, for usually these packages aren't updated on the same velocity of new Ruby versions are release, another issue is that on much cases is necessary works simutally with distincts Ruby version, as solution to the cited issues  we can use the rbenv to install and manage the Ruby versions.

## Install the requirements

Firstly is necessary to install the requirements.

{% highlight bash %}
sudo dnf install -y git gcc-c++ bzip2 openssl-devel libyaml-devel \
  libffi-devel readline-devel zlib-devel gdbm-devel ncurses-devel
{% endhighlight %}

## Install the rbenv

> Every commands __not__ should be executed as __super user__.

We'll use the [Git](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git), to clone the rbenv to default install directory.

{% highlight bash %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
{% endhighlight %}

Optionally you can run the below command to improve the rbenv performance, but don't sweat if something fail, all continues working normally, just go to next step.

{% highlight bash %}
cd ~/.rbenv && src/configure && make -C src
{% endhighlight %}

## Install rbenv plugins

The rbenv has diverse plugins that facilities its use, among them I chose 2 that are indispensable in my opinion, which can be installed follow the guide below, you can finding more plugins [here](https://github.com/rbenv/rbenv/wiki/Plugins).

### Install the ruby-build

The [ruby-build](https://github.com/rbenv/rbenv-default-gems) automatize the installation of Ruby, is possible use it separately or together with the rbenv transparently.

{% highlight bash %}
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
{% endhighlight %}

### Install the rbenv-default-gems

The [rbenv-default-gems](https://github.com/rbenv/rbenv-default-gems) install automatically the gems defined at file _~/.rbenv/plugins/rbenv-default-gems_ always that a new Ruby version is installed.

{% highlight bash %}
git clone https://github.com/rbenv/rbenv-default-gems.git ~/.rbenv/plugins/rbenv-default-gems
{% endhighlight %}

### Configure the rbenv on Shell

With the rbenv and plugins installed, now is necessary to include it on _$PATH_ and then integrate it with the [shell](https://en.wikipedia.org/wiki/Shell_(computing)), case you not use the [bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) as shell, you should to change the _~/.bashrc_ to file that is read by your shell on session start, as for example at case of the [zsh](https://en.wikipedia.org/wiki/Z_shell) would be necessary to change _~/.bashrc_ to _~/.zshrc_.

{% highlight bash %}
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
{% endhighlight %}

Now restart the session with shell, for do it close and then open your terminal, or run the command below, remembering that the command only works for the bash.

{% highlight bash %}
source ~/.bashrc
{% endhighlight %}

## Install the Ruby

We'll installing the version 2.5.1, because at moment that this article being writing, this is the more recent stable version, this step can be take some minutes, so just waiting paciently the download and installation.

{% highlight bash %}
rbenv install 2.5.1
{% endhighlight %}

The Ruby was installed, now is needed to use the rbenv to define which version of Ruby we want to use, we can to define a global version and/or to define especifical versions for the directories that we wish.

For define the version 2.5.1 as global, run.

{% highlight bash %}
rbenv global 2.5.1
{% endhighlight %}

For to use the version 2.5.1 in a specific directory independently of global version, inside the chosen directory, run.

{% highlight bash %}
rbenv local 2.5.1
{% endhighlight %}

The command above will create the hidden file _.ruby-version_ which containing the Ruby version that should be utilized in this directory and in its subdirectories,when the rbenv find this file its will ignore the global version  and will automatically to change version of the Ruby to version defined in the file.

In both cases, firstly is needed to install the Ruby version wished, because the commands _global_ and _local_ just changes the version in use, they don't installation.

For to know which Ruby version is in use, run.

{% highlight bash %}
ruby --version
{% endhighlight %}

With this we come to the end of this tutorial, now your Fedora supports multiple versions of Ruby simultaneously.

### Resources

* [https://github.com/rbenv/rbenv/wiki](https://github.com/rbenv/rbenv/wiki)
* [https://github.com/rbenv/ruby-build/wiki](https://github.com/rbenv/ruby-build/wiki)
