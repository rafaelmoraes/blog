---
date: 2018-06-12 18:20:00
title: Instalar o Ruby on Rails no CentOS 7
read_in: 7
keywords: instalar configurar ruby rails centos 7
translations: /install-ruby-on-rails-on-centos-7
---

Neste artigo veremos como instalar e configurar o [Ruby on Rails](https://rubyonrails.org/) no [CentOS 7](https://www.centos.org), este tutorial é recomendado apenas para ambientes de desenvolvimento, se você está com pressa no final do artigo existe um script que realiza a instalação e configuração sem a necessidade de nenhuma intervenção, ciente disso vamos começar.

## Instalar as dependências

{% highlight sh %}
sudo yum install -y epel-release # Necessário para instalar o nodejs
sudo yum install -y autoconf automake bison bzip2 curl gcc-c++ \
  git libffi-devel libtool libyaml-devel make nodejs openssl-devel \
  patch readline readline-devel sqlite-devel zlib zlib-devel
{% endhighlight %}

## Instalar Ruby

Instalaremos o [Ruby](https://www.ruby-lang.org/en/) através do [rbenv](https://github.com/rbenv/rbenv) para facilitar a instalação e futuras atualizações.

{% highlight sh %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src && cd ~
git clone https://github.com/sstephenson/ruby-build.git \
  ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
{% endhighlight %}

Agora utilizaremos o rbenv para instalar o Ruby, no momento da escrita deste artigo a versão estável mais recente é a 2.5.1, a qual será instalada e configurada como versão global.
Esta etapa pode demorar alguns minutos dependendo do seu hardware e rede.

{% highlight sh %}
rbenv install 2.5.1 && rbenv global 2.5.1
{% endhighlight %}

## (OPCIONAL) Não instalar documentações

Se você assim como eu não utiliza a documentação local das gems, execute o comando abaixo e nenhuma documentação será instalada junto com as gems.

{% highlight sh %}
echo "gem: --no-ri --no-doc" > ~/.gemrc
{% endhighlight %}

## Instalar Ruby on Rails

Com as dependências e o Ruby instalados e devidamente configurados, chegou a hora de instalar o Rails, neste caso iremos instalar a versão estável mais recente, sendo assim não precisamos informar uma versão em específico.

{% highlight sh %}
gem install rails
{% endhighlight %}

## Criar um novo projeto

Para saber se tudo correu bem, vamos criar um novo projeto e iniciar o servidor.

{% highlight sh %}
rails new app-rails-teste && cd app-rails-teste
rails s
{% endhighlight %}

Se tudo correu como o esperado, você ao acessar o endereço ```http://localhost:3000``` no seu navegador verá a página de boas vindas do Rails.

## Algo deu errado?

Caso algo tenha dado errado, sugiro que você execute o script abaixo para realizar uma desinstalação completa e refaça o tutorial desde de o início.

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

## Para os apressados

Se você está com pressa ou não tem interesse nos detalhes da instalação, execute o script abaixo no terminal do CentOS e tudo será instalado e configurado automáticamente.

{% highlight sh %}
sudo yum install -y epel-release # Necessário para instalar o nodejs
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

Após a execução do script acesse ```http://localhost:3000``` no seu navegador para verificar se tudo está funcionando.

## Finalizando

Neste artigo você viu como instalar e configurar o Rails no CentOS 7, lembrando que este tutorial é recomendado apenas para ambientes de desenvolvimento e com isso chegamos ao fim de mais um artigo.

Até mais! =]

## Fontes
* [https://github.com/rbenv/ruby-build/wiki](https://github.com/rbenv/ruby-build/wiki)
* [https://rafaelmoraes.pro/instalar-ruby-com-rbenv-no-fedora](https://rafaelmoraes.pro/instalar-ruby-com-rbenv-no-fedora)
* [https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora](https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora)
