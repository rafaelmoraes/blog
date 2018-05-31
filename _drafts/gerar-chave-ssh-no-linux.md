---
title: Gerar chave SSH no Linux
keywords: gerar chave ssh linux generate ssh key
---

Aqui você encontrará um tutorial para gerar o seu par de chaves [SSH](https://pt.wikipedia.org/wiki/Secure_Shell) e assim conseguir acessar remotamente os dispositivos que também utilizem o SSH sem a necessidade de informar uma senha explicitamente.

Entre suas funcionalidades o [OpenSSH](https://pt.wikipedia.org/wiki/OpenSSH) nos permite acessar remotamente outros computares de modo criptografado desde que ambos possuam suporte ao protocolo SSH, como a conexão é protegida somente os envolvidos tem acesso ao conteúdo trafegado, a autenticação pode ser feitas de duas maneiras uma delas é informando explicitamente a senha do usuário da maquina remota e a outra utilizando a autenticação via chaves SSH, para gerar essas chaves basta seguir os passos a abaixo.

## Instalar o OpenSSH

Provavelmente você já tem o OpenSSH instalado, mas caso não o tenha, execute o comando abaixo para instalá-lo, pois o utilizaremos para gerar o par de chaves.

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

A próxima opção será se você deseja configurar uma senha para a sua chave, recomendo que você não informe nenhuma senha e aperte *enter*, caso contrário você precisará informar a senha todas as vezes que a chave for utilizada.

{% highlight sh %}
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
{% endhighlight %}

As chaves foram criadas no diretório $HOME/.ssh, uma das chaves é a privada *id_rsa* e jamais deve ser exposta ou fornecida para terceiros, a outra é a chave pública *id_rsa.pub* é ela que deve ser enviada para os locais que desejamos acessar sem ter que informar explicitamente uma senha, o modo de enviá-la varia conforme o local, por exemplo no [Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) você precisa enviar o conteúdo da chave pública pelo painel administrativo, já em servidores Linux tradicionais você geralmente precisa incluir o conteúdo da *id_rsa.pub* no arquivo ~/.ssh/authorized_keys.

Como você já deve ter observado essas chaves são arquivos de texto e podem ser visualizado com um simples ```cat ~/.ssh/id_rsa.pub```, nestes arquivos contém um conjunto de caracteres que não fazem sentido para os humanos(com exceção do seu e-mail), jamais os altere, visto que isso invalidaria o par de chaves e você não poderá mais se conectar com os dispositivos até então acessíveis, para corrigir seria necessário gerar um novo par de chaves e atualizar a sua chave pública em todos os locais que você havia configurado a sua chave pública.

Utilizar o SSH em conjunto com a autenticação via chaves, realmente facilitam o nosso dia a dia principalmente se você precisa acessar muitos locais via SSH, espero que este tutorial tenha sido útil, até mais.

### Fontes

* [https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
