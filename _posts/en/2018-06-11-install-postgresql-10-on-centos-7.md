---
date: 2018-06-11 12:08:00
title: Install PostgreSQL 10 on CentOS 7
keywords: install configure postgresql 10 centos 7
read_in: 6
translations: /instalar-postgresql-10-no-centos-7
---

At this article we'll see how to install [PostgreSQL 10](https://www.postgresql.org/) on [CentOS 7](https://www.centos.org/), if you're hurry or not have interest about installation details go to the end of this article and use the script that contains all necessary commands, so let's beginning.

> The goal this tutorial is to install and to configure a instance of PostgreSQL to be used on **development environment**, many configurations necessary to production environment are ignored.

## Installation

Before install we'll need to do CentOS ignore the PostgreSQL of your repositories, for do it execute the text below in CentOS terminal.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql\*' \
  -i /etc/yum.repos.d/CentOS-Base.repo
{% endhighlight %}

Taken due precautions we can advance with the installation, notice that we're using official PostgreSQL packages.

{% highlight sh %}
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm && \
sudo yum install -y postgresql10 postgresql10-server
{% endhighlight %}

Now is necessary to run the setup to create the necessary files.

{% highlight sh %}
sudo /usr/pgsql-10/bin/postgresql-10-setup initdb
{% endhighlight %}

## Access configurations

Firstly we'll allow access for remote clients.

{% highlight sh %}
sudo sed -e "/listen_addresses = '\*'/d" \
  -e "/#listen_addresses = 'localhost'/a listen_addresses = '\*'" \
  -i /var/lib/pgsql/10/data/postgresql.conf
{% endhighlight %}

And now we'll allow the remote access for all user, databases and IP addresses.

{% highlight sh %}
sudo sed 's/host.*all.*all.*127.0.0.1.*ident/host all all all md5/' \
  -i /var/lib/pgsql/10/data/pg_hba.conf
{% endhighlight %}

## Initialization

After the installation step, we'll to configure auto-start on CentOS boot and start PostgreSQL.

{% highlight sh %}
sudo systemctl enable postgresql-10.service && \
sudo systemctl start postgresql-10.service
{% endhighlight %}

If happen some problem, you can to check what went wrong running the command below.

{% highlight sh%}
sudo systemctl status postgresql-10.service
{% endhighlight %}

## Create database and user

With PostgreSQL running, it's time to finally to create an user and a database, on this case the user-name and the password will to be the value return to *$USER* variable, that is the user of the CentOS that ran the command, but you is free to use the values that you wish.

{% highlight sh %}
sudo -u postgres createdb $USER
sudo -u postgres psql -U postgres -d $USER -c "CREATE ROLE $USER WITH SUPERUSER LOGIN PASSWORD '$USER';"
{% endhighlight %}

To confirm if all went right, run the command below and you'll should access the PostgreSQL [CLI](https://en.wikipedia.org/wiki/Command-line_interface) without to need inform any parameter.

{% highlight sh %}
psql
{% endhighlight %}

Case you can't access, I suggest to you uninstall completely the PostgreSQL with command below and rehash the tutorial from the begin.

{% highlight sh %}
sudo yum remove -y pgdg-centos10 postgresql10 postgresql10-server && \
sudo rm -rf /var/lib/pgsql/10
{% endhighlight %}

## Only for the hurried

If you're hurry or not worried at installation details, the script below is a compiled with all commands presents in this article, run it and PostgreSQL will be installed.

> If you followed the step-by-step **not** need to run this script.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql\*' \
  -i /etc/yum.repos.d/CentOS-Base.repo && \
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm && \
sudo yum install -y postgresql10 postgresql10-server && \
sudo /usr/pgsql-10/bin/postgresql-10-setup initdb && \
sudo sed -e "/listen_addresses = '\*'/d" \
  -e "/#listen_addresses = 'localhost'/a listen_addresses = '\*'" \
  -i /var/lib/pgsql/10/data/postgresql.conf && \
sudo sed 's/host.*all.*all.*127.0.0.1.*ident/host all all all md5/' \
  -i /var/lib/pgsql/10/data/pg_hba.conf && \
sudo systemctl enable postgresql-10.service && \
sudo systemctl start postgresql-10.service && \
sudo -u postgres createdb $USER && \
sudo -u postgres psql -U postgres -d $USER -c "CREATE ROLE $USER WITH SUPERUSER LOGIN PASSWORD '$USER';"

{% endhighlight %}

## Wrapping up

We reached at end of this tutorial, now you have installed and configured on your CentOS a PostgreSQL to be used in development environments, see you next time. =]

## Resources

* [https://wiki.postgresql.org/wiki/YUM_Installation](https://wiki.postgresql.org/wiki/YUM_Installation)
* [https://www.postgresql.org/docs/current/static/sql-createrole.html](https://www.postgresql.org/docs/current/static/sql-createrole.html)
* [https://www.postgresql.org/docs/current/static/app-createuser.html](https://www.postgresql.org/docs/current/static/app-createuser.html)
