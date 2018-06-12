## Instalar as dependências

Primeiro precisamos configurar o CentOS para ignorar o PostgreSQL existente nos seus repositórios nativos.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql*' \
  -i /etc/yum.repos.d/CentOS-Base.repo
{% endhighlight %}

Agora instalaremos as dependências necessárias.

{% highlight sh %}
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
sudo yum install -y postgresql10-devel
{% endhighlight %}

## Configurar o bundler

Após instalar as depedências, precisamos informar ao [bundler](https://bundler.io) o caminho para o *pg_config* caso contrário a instalação da gem [pg](https://rubygems.org/gems/pg) irá falhar.

{% highlight sh %}
bundle config build.pg --with-pg-config=/usr/pgsql-10/bin/pg_config
{% endhighlight %}

Executando o commando acima o bundle irá automáticamente passar os parâmetros necessários para a instalação da gem pg utilizar o PostgreSQL 10.

## Fontes

* [https://wiki.postgresql.org/wiki/YUM_Installation](https://wiki.postgresql.org/wiki/YUM_Installation)
* [https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7](https://rafaelmoraes.pro/instalar-postgresql-10-no-centos-7)
 [https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem](https://stackoverflow.com/questions/6040583/cant-find-the-libpq-fe-h-header-when-trying-to-install-pg-gem)
