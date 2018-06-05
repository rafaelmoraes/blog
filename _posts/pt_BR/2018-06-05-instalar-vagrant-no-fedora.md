---
date: 2018-06-05 15:20:00
translations: /install-vagrant-on-fedora
read_in: 8
title: Instalar o Vagrant no Fedora 27 e 28
keyword: instalar vagrant fedora
description: Tutorial para instalar o Vagrant com o VirtualBox no Linux Fedora, facilitando a criação e gerenciamento de maquinas virtuais.
---

O [Vagrant](https://www.vagrantup.com) é uma ferramenta de linha de comando([CLI](https://pt.wikipedia.org/wiki/Interface_de_linha_de_comandos)), que abstrai a complexidade dos principais virtualizadores do mercado e nos permite criar maquinas virtuais de uma maneira extremamente simples, para utilizá-lo basta criar o arquivo de [manifesto](https://pt.stackoverflow.com/questions/137147/qual-%c3%a9-o-significado-de-um-arquivo-manifest-em-programa%c3%a7%c3%a3o) e à partir deste arquivo uma [máquina virtual](https://pt.wikipedia.org/wiki/M%C3%A1quina_virtual) será provida com tudo que precisamos configurado, sendo assim vamos ver como instalá-lo no [Fedora](https://getfedora.org/pt_BR/).

## Requisitos

Para que o Vagrant funcione é preciso ter instalado algum software de virtualização, você pode descobrir quais são suportados [aqui](https://www.vagrantup.com/docs/providers/), outro requisito é que o seu computador precisa ser capaz de acessar outros via [SSH](https://pt.wikipedia.org/wiki/Secure_Shell).

Neste tutorial será utilizado o [VirtualBox](https://www.virtualbox.org/) para virtualização, caso não tenha algum ou alguns dos requisitos e não sabe como instalá-los você pode seguir este tutorial para [instalar o VirtualBox](/instalar-virtualbox-no-fedora) e este para [instalar e configurar o SSH](/gerar-chave-ssh-no-linux).

## Instalar o Vagrant

Com o VirtualBox e OpenSSH instalados podemos prosseguir com o download e instalação, para isso copie e execute o texto abaixo em seu terminal.

{% highlight sh %}
wget https://releases.hashicorp.com/vagrant/2.1.1/vagrant_2.1.1_x86_64.rpm && \
sudo dnf install -y ./vagrant*.rpm && \
rm vagrant_*.rpm*
{% endhighlight %}

O Vagrant por padrão não utiliza imagens convencionais de instalação, aquela que você acessa o site da sua distro Linux preferida, faz o download e instala na sua maquina ou servidor, ao invés dessas imagens o Vagrant utiliza as [boxes](https://www.vagrantup.com/docs/boxes.html), que basicamente são imagens derivadas das distribuição oficiais, onde tudo que não é útil ou essencial para o Vagrant é removido, qualquer um pode criar as suas próprias boxes, você pode encontrar algumas boxes compatíveis com o VirtualBox [aqui](https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=created&provider=virtualbox&q=).

Para prover uma maquina virtual o Vagrant utiliza o arquivo *Vagrantfile* onde ficam todas os detalhes sobre o provisionamento, por exemplo: arquivos que serão compartilhados, quantidade de memória RAM, o endereço IP, as portas que serão compartilhadas, os programas que serão instalados automaticamente, entre [outros](https://www.vagrantup.com/docs/vagrantfile/).

## Criar o Vagrantfile

Vamos criar o *Vagrantfile* no diretório que desejamos compartilhar com a [VM](https://pt.wikipedia.org/wiki/M%C3%A1quina_virtual), neste exemplo iremos provisionar uma VM com o [Fedora 28 cloud base](https://alt.fedoraproject.org/cloud/), mas você pode utilizar a box que desejar, apenas tenha certeza que a box escolhida é compatível com o VirtualBox, caso contrário não irá funcionar com esse tutorial.

{% highlight sh %}
vagrant init fedora/28-cloud-base
{% endhighlight %}

O *Vagrantfile* utiliza a sintaxe do [Ruby](https://www.ruby-lang.org/pt/)(mas você não precisa saber programar em Ruby), você pode alterar as configurações conforme a sua necessidade, neste artigo utilizaremos a configuração padrão para não fugirmos do objetivo principal do tutorial.

## Iniciar a VM

Após criar o *Vagrantfile* vamos solicitar ao Vagrant que inicie a VM, o comando abaixo funciona da seguinte forma: ele procura pelo *Vagrantfile* partindo do diretório atual até a raiz do seu sistema de arquivos, depois de encontrar e ler o *Vagrantfile* será verificado se a box configurada existe localmente, caso não exista será realizado o seu download e somente então a VM será iniciada com todas a configurações definidas no *Vagrantfile*.

Todo este processo pode demorar alguns minutos dependendo do seu hardware e rede, principalmente quando é necessário realizar o download da box.

{% highlight sh %}
vagrant up
{% endhighlight %}

## Acessar a VM

O acesso a VM é feito via SSH, facilitando às nossas vidas o Vagrant disponibiliza o comando abaixo para nós conectarmos a VM, assim não precisamos nos preocupar com usuários, endereços IP ou senhas.

{% highlight sh %}
vagrant ssh
{% endhighlight %}

Se tudo deu certo, agora você está logado na VM e pode fazer tudo o que você faria em uma distribuição comum, instalar programas, realizar downloads, criar arquivos e tudo estará te aguardando na próxima vez que você ligar e acessar a VM.

Por padrão o diretório compartilhado é montado na VM em */vagrant*, mas você pode alterá-lo no *Vagrantfile* se quiser, para acessá-lo, execute.

{% highlight sh %}
cd /vagrant
{% endhighlight %}

Executando ```ls``` no diretório */vagrant* você deve ver o arquivo *Vagrantfile* na lista de arquivos retornada, todos os arquivos que forem criados, deletados ou editados neste diretório terão seu estado alterado tanto na sua maquina quanto na VM, não importando em qual maquina a operação tenha sido realizada.

## Desligar a VM

Para desligar uma VM com o Vagrant execute o comando abaixo no diretório onde está o *Vagrantfile* ou em algum dos subdiretórios.

{% highlight sh %}
vagrant halt
{% endhighlight %}

## Deletar a VM

Caso você não precise mais da VM e deseja deletá-la, basta executar.

{% highlight sh %}
vagrant destroy
{% endhighlight %}

## Finalizando

Abrindo o VirtualBox você poderá ver todas as VMs criadas pelo Vagrant, pois como dito anteriormente o Vagrant não é um virtualizador, para evitar problemas futuros recomendo que você não altere as configurações da VM pelo [GUI](https://pt.wikipedia.org/wiki/Interface_gr%C3%A1fica_do_utilizador) do VirtualBox, sempre utilize o *Vagrantfile* para isso.

Com isso chegamos ao fim deste tutorial e agora você deve ser capaz de facilmente criar maquinas virtuais com o Vagrant.

### Fontes

* [https://www.vagrantup.com/docs/index.html](https://www.vagrantup.com/docs/index.html)
* [https://app.vagrantup.com/fedora/boxes/28-cloud-base](https://app.vagrantup.com/fedora/boxes/28-cloud-base)
