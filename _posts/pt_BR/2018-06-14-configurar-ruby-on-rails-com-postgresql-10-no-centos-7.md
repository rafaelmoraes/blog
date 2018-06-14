---
date: 2018-06-14 14:15:00
title: Ruby on Rails com PostgreSQL 10 no CentOS 7
keywords: configurar usar instalar postgresql 10 gem pg ruby rails centos 7
translations: /configure-ruby-on-rails-with-postgresql-10-on-centos-7
read_in: 5
---

Utilizar o [PostgreSQL 10](https://www.postgresql.org/) com o [Ruby on Rails](https://rubyonrails.org/) no [CentOS 7](https://www.centos.org/), exige uma configuração um pouco diferente do habitual, neste artigo veremos como realizá-la.

Caso você ainda não tenha o PostgreSQL, você pode instalá-lo seguindo [este tutorial](https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7).

## Instalar as dependências

Primeiro configure o CentOS para ignorar o PostgreSQL existente nos seus repositórios.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql*' \
  -i /etc/yum.repos.d/CentOS-Base.repo
{% endhighlight %}

Agora instale as dependências necessárias.

{% highlight sh %}
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
sudo yum install -y postgresql10-devel
{% endhighlight %}

## Configurar o Bundler

Depois de instalar as dependências, adicione a gem [pg](https://rubygems.org/gems/pg) no Gemfile do seu projeto e configure no [Bundler](https://bundler.io) o caminho para o *pg_config*, caso contrário a instalação da gem não será realizada.

{% highlight sh %}
bundle config build.pg --with-pg-config=/usr/pgsql-10/bin/pg_config
{% endhighlight %}

E para finalmente instalar a gem, no diretório raiz do seu projeto execute.

{% highlight sh %}
bundle install
{% endhighlight %}

## Configurar database.yml

Antes de executar a sua aplicação, lembre-se de configurar o seu [database.yml](http://guides.rubyonrails.org/configuring.html#configuring-a-database) com os parâmetros necessários para utilizar o PostgreSQL.

## Finalizando

Neste breve artigo vimos como utilizar o PostgreSQL 10 com o Ruby on Rails no CentOS 7, o que não é um tarefa complicada, mas foge um pouco do habitual.

Até mais!

## Fontes

* [https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7](https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7)
* [https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem](https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem)
