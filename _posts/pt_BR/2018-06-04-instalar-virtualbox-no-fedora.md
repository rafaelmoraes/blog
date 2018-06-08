---
date: 2018-06-04 19:25:00
read_in: 5
translations: /install-virtualbox-on-fedora
title: Instalar o VirtualBox no Fedora 27 e 28
keywords: instalar virtualbox fedora 27 28
description: Tutorial para instalar e configurar o VirtualBox com o extension pack.
---

A [virtualização](https://pt.wikipedia.org/wiki/Virtualiza%C3%A7%C3%A3o) facilita o nosso dia a dia como desenvolvedor de software, tanto no ambiente de desenvolvimento quanto no ambiente de produção, entre os muitos programas de virtualização existentes talvez o mais popular seja o [VirtualBox](https://www.virtualbox.org/) e com toda a certeza ele é muito simples de usar, sendo assim este artigo mostra como instalá-lo no [Fedora](https://getfedora.org/pt_BR/) 27 e 28.

## Adicionar o repositório

Antes de instalar precisamos adicionar o repositório oficial do VirtualBox e atualizar o sistema operacional.

{% highlight sh %}
sudo wget https://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo -O /etc/yum.repos.d/virtualbox.repo && \
sudo dnf -y upgrade
{% endhighlight %}

## Instalar as dependências

Agora vamos instalar as dependências.

{% highlight sh %}
sudo dnf install -y binutils gcc make patch libgomp glibc-headers \
glibc-devel kernel-headers kernel-devel dkms
{% endhighlight %}

Antes de prosseguir execute o comando abaixo e verifique se o seu sistema está usando a versão mais recente do [Kernel](https://pt.wikipedia.org/wiki/Linux_(n%C3%BAcleo)), caso não esteja reinicie o sistema operacional, só avance para o próximo passo após ter certeza de que está utilizando a versão mais recente do Kernel.

{% highlight sh %}
rpm -qa kernel | sort -V | tail -n 1 | sed s/kernel-// && uname -r
{% endhighlight %}

## Instalar o VirtualBox

Com o Kernel mais recente em uso e todas as dependências instaladas podemos finalmente instalar o VirtualBox, no momento da escrita deste artigo a versão mais recente é a 5.2. a qual será instalada.

{% highlight sh %}
sudo dnf install -y VirtualBox-5.2
{% endhighlight %}

Em seguida solicite ao VirtualBox para realizar as configurações necessárias.

{% highlight sh %}
sudo /usr/lib/virtualbox/vboxdrv.sh setup
{% endhighlight %}

Agora inclua o seu usuário ao grupo de usuários do VirtualBox.

{% highlight sh %}
sudo usermod -a -G vboxusers $USER
{% endhighlight %}

## Pacote de extensão

Opcionalmente você pode instalar o pacote de extensão do VirtualBox para melhorar a integração das maquinas virtuais com a maquina hospedeira, para isso você deve realizar o download do pacote de extensão compatível com a versão do VirtualBox instalado.

{% highlight sh %}
wget https://download.virtualbox.org/virtualbox/5.2.12/Oracle_VM_VirtualBox_Extension_Pack-5.2.12.vbox-extpack
{% endhighlight %}

Para instalar execute o comando abaixo, aceite os termos de uso e então a instalação será realizada.

{% highlight sh %}
sudo VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack*.vbox-extpack
{% endhighlight %}

## Finalizando

Agora você pode utilizar o sistema operacional que desejar sem a necessidade de instalá-lo ou configurar o [dual boot](https://pt.wikipedia.org/wiki/Multi_boot) diretamente em seu computador, no meu caso eu utilizo o VirtualBox para ter uma maquina virtual com o Windows e algumas distribuições Linux onde configuro um ambiente de desenvolvimento o mais próximo possível do ambiente de produção.

## Fontes

* [https://www.virtualbox.org/wiki/Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads)
* [https://www.virtualbox.org/manual/UserManual.html](https://www.virtualbox.org/manual/UserManual.html)
* [https://www.virtualbox.org/manual/ch08.html#vboxmanage-extpack](https://www.virtualbox.org/manual/ch08.html#vboxmanage-extpack)
