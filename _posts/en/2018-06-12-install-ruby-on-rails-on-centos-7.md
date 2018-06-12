---
date: 2018-06-12 18:50:00
title: Install Ruby on Rails on CentOS 7
read_in: 7
keywords: install configure ruby rails centos 7
translations: /instalar-ruby-on-rails-no-centos-7
---

This article we'll see how to install and to configure the [Ruby on Rails](https://rubyonrails.org/) on [CentOS 7](https://www.centos.org), this tutorial is recommended only to development environments, if you're hurry at end of this article there is a script that make the installation and configuration automatically, aware of this let's begin.

## Install requirements

{% highlight sh %}
sudo yum install -y epel-release # Necessary to install nodejs
sudo yum install -y autoconf automake bison bzip2 curl gcc-c++ \
  git libffi-devel libtool libyaml-devel make nodejs openssl-devel \
  patch readline readline-devel sqlite-devel zlib zlib-devel
{% endhighlight %}

## Install Ruby

We'll to install [Ruby](https://www.ruby-lang.org/en/) through [rbenv](https://github.com/rbenv/rbenv) for makes easy the installation and futures updates.

{% highlight sh %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src && cd ~
git clone https://github.com/sstephenson/ruby-build.git \
  ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
{% endhighlight %}

Now we'll to use rbenv to install Ruby, at moment of the writing this article the stable version more recent is 2.5.1, the which one will be installed and configured as global version.
This step can take some minutes depending on your hardware and network.

{% highlight sh %}
rbenv install 2.5.1 && rbenv global 2.5.1
{% endhighlight %}

## (OPTIONAL) Not install documentation

If you is like me not uses the local documentation of the gems, run the command below and none documentation will installed with the gems.

{% highlight sh %}
echo "gem: --no-ri --no-doc" > ~/.gemrc
{% endhighlight %}

## Install Ruby on Rails

With requirements and Ruby installed and correctly configured, it's time to install the Rails, at this case will install the more recent stable version, so not is necessary to specify version.

{% highlight sh %}
gem install rails
{% endhighlight %}

## Create a new project

To know if everything went right, let's to create a new project and start the server.

{% highlight sh %}
rails new app-rails-teste && cd app-rails-teste
rails s
{% endhighlight %}

If all went right, by accessing the address ```http://localhost:3000``` on your browser, you'll see the welcome page of Rails.

## Something went wrong?

Case something went wrong, I suggest to you run the script below to do the complete uninstall and follow again the steps from begin.

{% highlight sh %}
sudo yum remove -y epel-release autoconf automake bison bzip2 \
  curl gcc-c++ git libffi-devel libtool libyaml-devel make \
  nodejs openssl-devel patch readline readline-devel \
  sqlite-devel zlib zlib-devel
rm -rf ~/.rbenv
sed -i '/rbenv/d' ~/.bashrc
source ~/.bashrc
rm ~/.gemrc
cd ~
rm -rf ~/app-rails-teste

{% endhighlight %}

## For the hurrieds

If you're hurry or not concern about the details of the installation, run the script below in the CentOS terminal and everything will installed and configured automatically.

{% highlight sh %}
sudo yum install -y epel-release # Necessary to install nodejs
sudo yum install -y autoconf automake bison bzip2 curl gcc-c++ \
  git libffi-devel libtool libyaml-devel make nodejs openssl-devel \
  patch readline readline-devel sqlite-devel zlib zlib-devel && \
  git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src && cd ~
git clone https://github.com/sstephenson/ruby-build.git \
  ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
rbenv install 2.5.1 && rbenv global 2.5.1
echo "gem: --no-ri --no-doc" > ~/.gemrc
gem install rails
rails new ~/app-rails-teste && cd ~/app-rails-teste
rails s
{% endhighlight %}

After run the script access ```http://localhost:3000``` in your browser to check if everything is working.

## Wrapping up

At this article you saw how to install and to configure the Rails on CentOS 7, remembering that this tutorial is recommended for development environments and with this we reached at the end of more one article.

See you next! =]

## Resources

* [https://github.com/rbenv/ruby-build/wiki](https://github.com/rbenv/ruby-build/wiki)
* [https://rafaelmoraes.pro/instalar-ruby-com-rbenv-no-fedora](https://rafaelmoraes.pro/instalar-ruby-com-rbenv-no-fedora)
* [https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora](https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora)
