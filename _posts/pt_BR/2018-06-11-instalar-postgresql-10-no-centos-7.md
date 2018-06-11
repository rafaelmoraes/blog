---
date: 2018-06-11 18:15:00
title: Instalar PostgreSQL 10 no CentOS 7
keywords: instalar postgresql 10 centos 7
read_in: 7
# translations: /install-postgresql-10-on-centos-7
---

Neste artigo veremos como instalar o [PostgreSQL 10](https://www.postgresql.org/) no [CentOS 7](https://www.centos.org/), se você está com pressa ou não tem interesse nos detalhes da instalação vá até o final do artigo e use o script que contém todos os comandos necessários, então vamos lá.

> O objetivo deste tutorial é instalar e configurar uma instância do PostgreSQL destinada ao **ambiente de desenvolvimento**, várias medidas necessárias para um ambiente de produção são ignoradas.

## Instalação

Antes de instalar precisamos fazer o CentOS ignorar o PostgreSQL presente em seus repositórios, para isso copie e cole o texto abaixo no terminal do CentOS.

{% highlight sh %}
sudo sed -e '/exclude=postgresql\*/d' \
  -e '/^\[base\]$\|^\[updates\]$/a exclude=postgresql\*' \
  -i /etc/yum.repos.d/CentOS-Base.repo
{% endhighlight %}

Tomado as devidas precauções podemos prosseguir com a instalação, note que estamos utilizando os pacotes oficiais do PostgreSQL.

{% highlight sh %}
sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm && \
sudo yum install -y postgresql10 postgresql10-server
{% endhighlight %}

Agora é necessário executar o setup para criar os arquivos necessários.

{% highlight sh %}
sudo /usr/pgsql-10/bin/postgresql-10-setup initdb
{% endhighlight %}

## Configurações de acesso

Primeiramente vamos permitir o acesso de clientes remotos.

{% highlight sh %}
sudo sed -e "/listen_addresses = '\*'/d" \
  -e "/#listen_addresses = 'localhost'/a listen_addresses = '\*'" \
  -i /var/lib/pgsql/10/data/postgresql.conf
{% endhighlight %}

E agora vamos liberar o acesso remoto para todos os usuários, banco de dados e endereços IP.

{% highlight sh %}
sudo sed 's/host.*all.*all.*127.0.0.1.*ident/host all all all md5/' \
  -i /var/lib/pgsql/10/data/pg_hba.conf
{% endhighlight %}

## Inicialização

Após a etapa de instalação, vamos configurar o início automático junto com a inicialização do CentOS e iniciar o PostgreSQL.

{% highlight sh %}
sudo systemctl enable postgresql-10.service && \
sudo systemctl start postgresql-10.service
{% endhighlight %}

Caso ocorra algum problema, você pode verificar o que deu errado executando o comando abaixo.

{% highlight sh%}
sudo systemctl status postgresql-10.service
{% endhighlight %}

## Criar banco de dados e usuário

Com o PostgreSQL funcionando, chegou a hora de finalmente criar um usuário e um banco de dados, neste caso o nome de usuário e a senha serão o valor retornado pela variável *$USER*, ou seja o usuário do CentOS que executou o comando, mas você é livre para utilizar os valores que desejar.

{% highlight sh %}
sudo -u postgres createdb $USER
sudo -u postgres psql -U postgres -d $USER -c "CREATE ROLE $USER WITH SUPERUSER LOGIN PASSWORD '$USER';"
{% endhighlight %}

Para verificar se tudo deu certo, execute o comando abaixo e você deverá acessar o [CLI](https://pt.wikipedia.org/wiki/Interface_de_linha_de_comandos) do PostgreSQL sem precisar informar nenhum parâmetro.

{% highlight sh %}
psql
{% endhighlight %}

Caso não consiga acessar, sugiro que você desinstale complemente o PostgreSQL com o comando abaixo e refaça o tutorial desde o início.

{% highlight sh %}
sudo yum remove -y pgdg-centos10 postgresql10 postgresql10-server && \
sudo rm -rf /var/lib/pgsql/10
{% endhighlight %}

## Somente para os apressados

Se você está com pressa ou não tem interesse nos detalhes da instalação, o script abaixo é um compilado de todos os comandos presentes neste artigo, execute-o e o PostgreSQL será instalado.

> Se você seguiu o passo a passo **não** precisa executar este script.

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

## Concluindo

Chegamos ao fim deste tutorial, agora você tem instalado e configurado no seu CentOS um PostgreSQL para ser utilizado em ambientes de desenvolvimento, até à próxima. =]

## Fontes
* [https://wiki.postgresql.org/wiki/YUM_Installation](https://wiki.postgresql.org/wiki/YUM_Installation)
* [https://www.postgresql.org/docs/current/static/sql-createrole.html](https://www.postgresql.org/docs/current/static/sql-createrole.html)
* [https://www.postgresql.org/docs/current/static/app-createuser.html](https://www.postgresql.org/docs/current/static/app-createuser.html)
