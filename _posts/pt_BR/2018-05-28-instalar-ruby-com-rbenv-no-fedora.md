---
date: "2018-05-28 21:30:00"
title: "Instalar Ruby com rbenv no Fedora 27 e 28"
keywords: "tutorial instalar ruby rbenv fedora 27 28"
read_in: 8
translations: /install-ruby-with-rbenv-on-fedora
---

Este artigo é um tutorial para instalar o [Ruby](https://www.ruby-lang.org/pt/) no [Fedora](https://getfedora.org/pt_BR/) 27 e 28 através do [rbenv](https://github.com/rbenv/rbenv) em conjunto com alguns de seus plugins.

Instalar o Ruby dos pacotes fornecidos oficialmente pelo Fedora não é uma solução muito boa na minha opinião, visto que geralmente esses pacotes não são atualizados na mesma velocidade de lançamento de novas versões do Ruby, outra questão é que em muitos casos é necessário trabalhar simultaneamente com versões distintas do Ruby, como solução podemos utilizar o rbenv para instalar e gerenciar as versões do Ruby.

## Instalar as dependências

Primeiramente é necessário instalar as dependências.

{% highlight bash %}
sudo dnf install -y git gcc-c++ bzip2 openssl-devel libyaml-devel \
  libffi-devel readline-devel zlib-devel gdbm-devel ncurses-devel
{% endhighlight %}

## Instalar o rbenv

> Todos os comandos __não__ devem ser executados como __super usuário__.

Utilizaremos o [Git](https://git-scm.com/book/pt-br/v2/Come%C3%A7ando-Uma-Breve-Hist%C3%B3ria-do-Git) para clonar o rbenv para o diretório padrão de instalação.

{% highlight bash %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
{% endhighlight %}

Opcionalmente você pode executar o comando abaixo para melhorar a performance do rbenv, não se preocupe se algo falhar, tudo continuará funcionando normalmente, apenas avance para o próximo passo.

{% highlight bash %}
cd ~/.rbenv && src/configure && make -C src
{% endhighlight %}

## Instalar plugins no rbenv

O rbenv possui diversos plugins que facilitam o seu uso, dentre eles selecionei 2 que são indispensáveis na minha opinião, os quais podem ser instalados seguindo o guia abaixo, é possível encontrar mais plugins [aqui](https://github.com/rbenv/rbenv/wiki/Plugins).

### Instalar o ruby-build

O [ruby-build](https://github.com/rbenv/rbenv-default-gems) automatiza a instalação do Ruby, é possível utilizá-lo separadamente ou em conjunto com o rbenv de modo transparente.

{% highlight bash %}
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
{% endhighlight %}

### Instalar o rbenv-default-gems

O [rbenv-default-gems](https://github.com/rbenv/rbenv-default-gems) instala automaticamente as gems definidas no arquivo _~/.rbenv/plugins/rbenv-default-gems_ sempre que uma nova versão do Ruby for instalada.

{% highlight bash %}
git clone https://github.com/rbenv/rbenv-default-gems.git ~/.rbenv/plugins/rbenv-default-gems
{% endhighlight %}

### Configurar o rbenv no Shell

Com o rbenv e plugins instalados, agora é necessário incluí-lo no _$PATH_ e em seguida integrá-lo com o [shell](https://pt.wikipedia.org/wiki/Shell_(computa%C3%A7%C3%A3o)), caso você não utilize o [bash](https://pt.wikipedia.org/wiki/Bash) como shell, você deve alterar o _~/.bashrc_ para o arquivo que é lido pelo seu shell no início da sessão, como por exemplo no caso do [zsh](https://en.wikipedia.org/wiki/Z_shell) seria preciso substituir _~/.bashrc_ por _~/.zshrc_.

{% highlight bash %}
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
{% endhighlight %}

Agora reinicie a sessão com shell, para isso feche e em seguida abra o seu terminal, ou execute o comando abaixo, lembrando que o comando funciona apenas para o bash.

{% highlight bash %}
source ~/.bashrc
{% endhighlight %}

## Instalar o Ruby

Iremos instalar a versão 2.5.1, pois no momento da escrita deste artigo é a versão estável mais recente, esta etapa poderá demorar alguns minutos, então aguarde pacientemente o download e instalação.

{% highlight bash %}
rbenv install 2.5.1
{% endhighlight %}

O Ruby foi instalado, agora é preciso utilizar o rbenv para definir qual versão do Ruby queremos utilizar, podemos definir uma versão global e/ou definir versões específicas para os diretórios que desejarmos.

Para definir a versão 2.5.1 como global, execute.

{% highlight bash %}
rbenv global 2.5.1
{% endhighlight %}

Para utilizar a versão 2.5.1 em um diretório específico independente da versão definida globalmente, dentro do diretório desejado execute.

{% highlight bash %}
rbenv local 2.5.1
{% endhighlight %}

O comando acima irá criar o arquivo oculto _.ruby-version_ contendo a versão do Ruby que deve ser utilizada neste diretório e em seus subdiretórios, ao identificar este arquivo o rbenv irá ignorar a versão definida como global e automaticamente irá alterar a versão do Ruby para a versão definida no arquivo.

Em ambos os casos, primeiramente é preciso instalar a versão do Ruby desejada, pois os comandos _global_ e _local_ apenas alteram a versão em uso, eles não realizam a instalação.

Para saber qual versão do Ruby está em uso, execute.

{% highlight bash %}
ruby --version
{% endhighlight %}

Com isso chegamos ao fim deste tutorial, agora seu Fedora é capaz de suportar várias versões do Ruby simultaneamente.

### Fontes

* [https://github.com/rbenv/rbenv/wiki](https://github.com/rbenv/rbenv/wiki)
* [https://github.com/rbenv/ruby-build/wiki](https://github.com/rbenv/ruby-build/wiki)
