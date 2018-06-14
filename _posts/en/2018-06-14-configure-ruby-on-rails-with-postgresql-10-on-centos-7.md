---
date: 2018-06-14 14:15:00
title: Ruby on Rails with PostgreSQL 10 on CentOS 7
keywords: configure use install postgresql 10 gem pg ruby rails centos 7
translations: /configurar-ruby-on-rails-com-postgresql-10-no-centos-7
read_in: 5
---

Use [PostgreSQL 10](https://www.postgresql.org/) with [Ruby on Rails](https://rubyonrails.org/) on [CentOS 7](https://www.centos.org/), demand a configuration a bit different from usual, in this article we'll see how to make it.

If you haven't the PostgreSQL, you can to install it following [this tutorial](https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7).

## Install requirements

First of all, configure the CentOS to ignore the PostgreSQL existent at your repositories.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql*' \
  -i /etc/yum.repos.d/CentOS-Base.repo
{% endhighlight %}

Now install the requirements.

{% highlight sh %}
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
sudo yum install -y postgresql10-devel
{% endhighlight %}

## Configure the Bundler

After install the requirements, add the gem [pg](https://rubygems.org/gems/pg) in Gemfile of your project and configure on [bundler](https://bundler.io) the path to *pg_config*, otherwise the installation gem will fail.

{% highlight sh %}
bundle config build.pg --with-pg-config=/usr/pgsql-10/bin/pg_config
{% endhighlight %}

And finally to install the gem, at your project root path, run.

{% highlight sh %}
bundle install
{% endhighlight %}

## Configure database.yml

Rather of to run your app, remember to configure your [database.yml](http://guides.rubyonrails.org/configuring.html#configuring-a-database), with the needed parameters to use PostgreSQL.

## Wrapping up

In this soon article we saw how to use the PostgreSQL 10 with Ruby on Rails on CentOS 7, what isn't difficult task, but get away a bit from the usual.

See you!

## Resources

* [https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7](https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7)
* [https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem](https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem)
