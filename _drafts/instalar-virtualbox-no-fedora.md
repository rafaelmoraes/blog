---
title: Instalar o VirtualBox no Fedora 27 e 28
keywords: instalar virtualbox fedora 27 28
---

A virtualização facilita o nosso dia a dia como desenvolvedor de software, tanto no ambiente de desenvolvimento quanto no ambiente de produção, entre os muitos programas de virtualização existentes talvez o mais popular seja o [VirtualBox](https://www.virtualbox.org/) e com toda a certeza ele é muito simples de usar, sendo assim este artigo mostra como instalá-lo no Fedora 27 e 28.

## Adicionar o repositório

Para instalar precisamos adicionar o repositório oficial do VirtualBox e atualizar a lista de repositórios.

{% highlight sh %}
sudo wget https://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo -O /etc/yum.repos.d/virtualbox.repo && \
sudo dnf -y upgrade
{% endhighlight %}

## Instalar as dependências

Agora vamos instalar as dependências necessárias.

{% highlight sh %}
sudo dnf install -y binutils gcc make patch libgomp glibc-headers \
glibc-devel kernel-headers kernel-devel dkms
{% endhighlight %}

Antes de prosseguir execute o comando abaixo e verifique se o seu sistema está usando a versão do mais recente do [kernel](https://pt.wikipedia.org/wiki/N%C3%BAcleo_(sistema_operacional)), caso não esteja reinicie o seu sistema e só então avance para o próximo passo.

{% highlight sh %}
rpm -qa kernel | sort -V | tail -n 1 | sed s/kernel-// && uname -r
{% endhighlight %}

## Instalar o VirtualBox

Com o kernel mais recente em uso e todas as dependências instaladas podemos finalmente instalar o VirtualBox, no momento da escrita deste artigo a versão mais recente é a 5.2. a qual será instalada.

{% highlight sh %}
sudo dnf install -y VirtualBox-5.2
{% endhighlight %}

Em seguida execute as configurações necessárias para o funcionamento do VirtualBox.

{% highlight sh %}
sudo /usr/lib/virtualbox/vboxdrv.sh setup
{% endhighlight %}

Agora precisamos adicionar o seu usuário ao grupo de usuários da VirtualBox.

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

Agora você pode utilizar o sistema operacional que desejar sem a necessidade de instalá-lo ou configurar o [dual boot](https://pt.wikipedia.org/wiki/Multi_boot) diretamente em seu computador, no meu caso eu utilizo o VirtualBox para ter uma maquina virtual com o Windows e algumas distribuições Linux onde configuro um ambiente o mais próximo possível do ambiente de produção.

## Fontes

* [https://www.virtualbox.org/wiki/Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads)
* [https://www.virtualbox.org/manual/UserManual.html](https://www.virtualbox.org/manual/UserManual.html)
