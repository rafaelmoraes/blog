---
title: Instalar Vagrant no Fedora
keyword: instalar vagrant fedora
---

O [Vagrant](https://www.vagrantup.com) é uma ferramenta de linha de comando, que abstrair a complexidade dos principais virtualizadores do mercado e nos permite criar maquinas virtuais de uma maneira extremamente simples, para utilizá-lo basta criar o arquivo de manifest e a partir deste arquivo o uma [VM](https://pt.wikipedia.org/wiki/M%C3%A1quina_virtual) será provida com tudo que precisamos configurado, sendo assim vamos ver como instalá-lo.

## Requisitos

Para que o Vagrant funcione é preciso ter instalado algum software de virtualização, você pode descobrir quais são suportados [aqui](https://www.vagrantup.com/docs/providers/), outro requisito é que o seu computador precisa ser capaz de acessar outros via [SSH](https://pt.wikipedia.org/wiki/Secure_Shell).

Neste tutorial será utilizado o [VirtualBox](https://www.virtualbox.org/) para virtualização, caso não tenha instalado algum dos requisitos e não sabe como instalá-los você pode seguir este tutorial para [instalar o Virtualbox](/instalar-virtualbox-no-fedora) e este para [instalar e configurar o SSH](/gerar-chave-ssh-no-linux).

## Instalar o Vagrant

Com o VirtualBox e OpenSSH instalados podemos prosseguir o download e a instalação, para isso copie, cole e execute o texto abaixo em seu terminal.

{% highlight sh %}
wget https://releases.hashicorp.com/vagrant/2.1.1/vagrant_2.1.1_x86_64.rpm && \
sudo dnf install -y ./vagrant*.rpm && \
rm vagrant_*.rpm*
{% endhighlight %}

O Vagrant por padrão não utiliza imagens convencionais de instalação, aquela que você acessa o site da sua distro Linux preferida, faz o download e instala na sua maquina ou servidor, ao invés dessas imagens o Vagrant utiliza as suas [boxes](https://www.vagrantup.com/docs/boxes.html), que basicamente são imagens derivadas das distribuição oficiais, onde tudo que não é útil ou essencial para o vagrant é removido, qualquer um pode criar as suas próprias boxes, você pode encontrar as boxes compatíveis com o VirtualBox [aqui](https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=created&provider=virtualbox&q=).

Para prover uma maquina virtual o Vagrant utiliza o arquivo *Vagrantfile* onde ficam todas os detalhes sobre o provisionamento, por exemplo: arquivos que serão compartilhados, quantidade de memória RAM, o endereço IP, as portas que serão compartilhadas, os programas que serão instalados automaticamente, entre [outros](https://www.vagrantup.com/docs/vagrantfile/).

## Criar o Vagrantfile

Vamos criar o *Vagrantfile* no diretório que desejamos compartilhar com a VM, neste exemplo iremos provisionar uma VM com o Fedora 28, mas você pode utilizar a box que desejar ou precisar, apenas certifique que a box escolhida é compatível com o VirtualBox, caso contrário não irá funcionar.

{% highlight sh %}
vagrant init fedora/28-cloud-base
{% endhighlight %}

O *Vagrantfile* utiliza a sintaxe do [Ruby](https://www.ruby-lang.org/pt/)(mas você não precisa saber programar em Ruby) e você pode alterar as configurações conforme a sua necessidade, neste artigo utilizaremos a configuração padrão para não fugirmos do objetivo do tutorial.

## Iniciar a VM

Após criar o *Vagrantfile*, vamos solicitar ao Vagrant que inicie a VM, o comando abaixo funciona da seguinte forma: ele procura pelo *Vagrantfile* partindo do diretório atual até a raiz do seu sistema de arquivos, depois de encontrar e ler o *Vagrantfile* será verificado se a box configurada existe localmente, caso não exista será realizado o seu download e somente então a VM será iniciada, aplicando todas a configurações presentes no *Vagrantfile*.

Todo este processo pode demorar alguns minutos variando dependendo do seu hardware e rede, principalmente quando é necessário realizar o download da box.

{% highlight sh %}
vagrant up
{% endhighlight %}

## Acessar a VM

O acesso a VM é feito via SSH, facilitando as nossas vidas o Vagrant disponibiliza o comando abaixo para nós conectarmos com VM, assim não precisamos nos preocupar com usuários, endereços IP ou senhas.

{% highlight sh %}
vagrant ssh
{% endhighlight %}

Se tudo deu certo, agora você está logado na VM e pode fazer tudo o que você faria em uma distribuição comum, instalar programas, realizar downloads, criar aquivos e tudo estará te aguardando na próxima vez que você ligar e acessar a VM.

Por padrão o diretório compartilhado é montado em */vagrant*, mas você pode alterá-lo no *Vagrantfile* se quiser, para acessá-lo, execute.

{% highlight sh %}
cd /vagrant
{% endhighlight %}

Executando ```ls``` você deve ver o arquivo *Vagrantfile* na lista de arquivos retornada, todos os arquivos que forem criados, deletados ou editados neste diretório terão seu estado alterado tanto na sua maquina quanto na VM, não importando em qual maquina a operação tenha sido realizada.

Chegamos ao de mais um tutorial, agora você deve ser capaz de criar maquinas virtuais sem a necessidade de acessar diretamente o VirtualBox.

### Fontes

* [https://www.vagrantup.com/docs/index.html](https://www.vagrantup.com/docs/index.html)
* [https://app.vagrantup.com/fedora/boxes/28-cloud-base](https://app.vagrantup.com/fedora/boxes/28-cloud-base)
