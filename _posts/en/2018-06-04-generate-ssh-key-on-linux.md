---
date: 2018-06-04 18:00:00
read_in: 5
translations: /gerar-chave-ssh-no-linux
title: Generate ssh key on Linux
keywords: generate create key keys ssh linux fedora
description: Guide to generate the ssh keys on Linux, for access another devices without explicitly to use a password.
---

In this article we'll to use the [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH), for generate a couple of keys, case you don't know, the OpenSSH is a group of tools that together are a implementation of the [SSH](https://en.wikipedia.org/wiki/Secure_Shell) protocol,among these tools there is one that allow remote devices encrypted  connections since that the involved it supports SSH, how is a safe connection  is necessary an authentication, which usually can be do in two ways, it one is typing the user password of the remote device, or using the SSH keys instead typing a password, after this shortly introduction let's put our hands dirty.

## Install the OpenSSH

Probably you already have the OpenSSH installed, but if you haven’t, execute the command below for to install it on [Fedora](https://getfedora.org/) or your [spins](https://spins.fedoraproject.org/), if you don't use Fedora you'll need to replace the command by correspondent command of your distribution.

{% highlight bash %}
sudo dnf install -y openssh
{% endhighlight %}

## Generate the SSH keys

We'll use the tool [ssh-keygen](https://en.wikipedia.org/wiki/Ssh-keygen) of OpenSSH for to generate the couple of keys, so execute the command below and remember of replace *your@email.com* by your email, or by something that you need to use as identification to this key.

{% highlight sh %}
ssh-keygen -t rsa -b 4096 -C "your@email.com"
{% endhighlight %}

When you to see the option below, I recommend to you to press *enter* and thus the keys will be to save at default directory *$HOME/.ssh*.

{% highlight sh %}
Enter file in which the key is ($HOME/.ssh/id_rsa):
{% endhighlight %}

The next option will be for you configure a password to your key, I recommend to you not typing nothing and press *enter*, otherwise you'll need to type the password every times the key is used.

{% highlight sh %}
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
{% endhighlight %}

The keys was created at directory *$HOME/.ssh*, one of the keys is the private *id_rsa* and never should be shown or given to thirds (except if you to know what are you doing), the another is the public key *id_rsa.pub* it's that should be sent to the services and devices that we've wish to access without explicitly to type a password, the way to send it can change, for example on [Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) you need to send the public key content by the settings dashboard and on the traditional Linux servers you usually need to include the content of *id_rsa.pub* in *~/.ssh/authorized_keys* file.

How you should already observed these keys are text files and you can visualize it with:

{% highlight sh %}
# public
cat ~/.ssh/id_rsa.pub

# private
cat ~/.ssh/id_rsa
{% endhighlight %}

These files contains a set of characters that no make a sense to humans(except your email), you should never change they, as it will invalidate the keys and you can't more connect with the devices accessible until at this moment, to fix it would be necessary generate a new couple of keys and update your public key at every place that you had already configured your public key.

Use SSH together with the authentication by keys, really makes easy our day by day mainly if you need to access many places by SSH, I hope that this tutorial had been useful, see you.

### Resources

* [https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
