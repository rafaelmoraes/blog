---
date: 2018-06-04 18:00:00
read_in: 6
translations: /generate-ssh-key-on-linux
title: Gerar chave SSH no Linux
keywords: gerar criar chaves chave ssh linux fedora
description: Guia para gerar as chaves SSH no Linux, para acessar outros dispositivos sem utilizar explicitamente uma senha.
---

Neste artigo utilizaremos o [OpenSSH](https://pt.wikipedia.org/wiki/OpenSSH) para gerar o par de chaves, caso você não o conheça, o OpenSSH é um aglomerado de ferramentas que em conjunto são uma implementação do protocolo [SSH](https://pt.wikipedia.org/wiki/Secure_Shell), entre essas ferramentas existe uma que permite que dispositivos se conectem remotamente de modo criptografado desde que os envolvidos tenham suporte ao SSH, como é uma conexão segura um mecanismo de autenticação é necessário, o qual geralmente pode ser feito de duas maneiras, uma delas é informando explicitamente a senha do usuário do dispositivo remoto, ou utilizando as chaves SSH em vez de informar explicitamente uma senha, após essa breve introdução vamos por a mão na massa.

## Instalar o OpenSSH

Provavelmente você já tem o OpenSSH instalado, mas caso não o tenha, execute o comando abaixo para instalá-lo no [Fedora](https://getfedora.org/pt_BR/) e seus [spins](https://spins.fedoraproject.org/), se você não utiliza o Fedora você precisará substituir o comando abaixo pelo correspondente na sua distribuição.

{% highlight bash %}
sudo dnf install -y openssh
{% endhighlight %}

## Gerar as chaves

Vamos usar o utilitário [ssh-keygen](https://en.wikipedia.org/wiki/Ssh-keygen) do OpenSSH para gerar o par de chaves, então execute o comando abaixo e não se esqueça de substituir *seu@email.com* pelo seu e-mail ou por qualquer outra coisa que você deseje utilizar como identificador desta chave.

{% highlight sh %}
ssh-keygen -t rsa -b 4096 -C "seu@email.com"
{% endhighlight %}

Quando lhe for apresentado a opção abaixo, recomendo que aperte *enter* e assim as chaves serão salvas no diretório padrão *$HOME/.ssh*.

{% highlight sh %}
Enter file in which the key is ($HOME/.ssh/id_rsa):
{% endhighlight %}

A próxima opção será para você configurar uma senha para a sua chave, recomendo que você não informe nenhuma e aperte *enter*, caso contrário você precisará informar a senha todas as vezes que a chave for utilizada, o seu seja todas as vezes que você for conectar com outro dispositivo.

{% highlight sh %}
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
{% endhighlight %}

As chaves foram criadas no diretório *$HOME/.ssh*, uma das chaves é a privada *id_rsa* e jamais deve ser exposta ou fornecida para terceiros (exceto se você saiba o que está fazendo), a outra é a chave pública *id_rsa.pub* é ela que deve ser enviada para os serviços e dispositivos que desejamos acessar sem ter que informar explicitamente uma senha, o modo de enviá-la pode variar, por exemplo no [Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) você precisa enviar o conteúdo da chave pública pelo painel administrativo, já em servidores Linux tradicionais você geralmente precisa incluir o conteúdo da *id_rsa.pub* no arquivo *~/.ssh/authorized_keys*.

Como você já deve ter observado essas chaves são arquivos de texto e podem ser visualizados simplesmente com:

{% highlight sh %}
# pública
cat ~/.ssh/id_rsa.pub

# privada
cat ~/.ssh/id_rsa
{% endhighlight %}

Estes arquivos contém um conjunto de caracteres que não fazem sentido para os humanos(com exceção do seu e-mail), jamais os altere, visto que isso invalidaria o par de chaves e você não poderá mais se conectar com os dispositivos até então acessíveis, para corrigir seria necessário gerar um novo par de chaves e atualizar a sua chave pública em todos os locais que você havia configurado a sua chave pública.

Utilizar o SSH em conjunto com a autenticação via chaves, realmente facilitam o nosso dia a dia principalmente se você precisa acessar muitos locais via SSH, espero que este tutorial tenha sido útil, até mais.

### Fontes

* [https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
