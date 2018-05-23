---
date: "2018-05-23 17:00:00"
title: "Configurar HTTPS para domínio personalizado no Github Pages"
keywords: "configurar HTTPS Github Pages"
read_in: 5
---
Neste artigo veremos como configurar o protocolo [HTTPS](https://pt.wikipedia.org/wiki/Hyper_Text_Transfer_Protocol_Secure) para sites com domínio personalizado hospedados no [Github Pages](https://pages.github.com/).

## A limitação

Configurar um domínio personalizado para um site hospedado no [Github Pages](https://pages.github.com/) é extremamente simples, mas utilizar um domínio personalizado com HTTPS não é uma tarefa trivial, pois não é possível realizar o processo tradicional de configuração, o qual basicamente consiste em enviar e configurar no servidor web os arquivos fornecidos pela certificadora, sendo assim é necessário uma solução alternativa.

## A solução

Como não possível realizar a configuração do HTTPS diretamente no Github Pages, contornaremos esta limitação usando o servidor DNS para "informar" aos clientes que o site suporta HTTPS.

Primeiramente é necessário ter certeza que o serviço de [DNS](https://pt.wikipedia.org/wiki/Sistema_de_Nomes_de_Dom%C3%ADnio) utilizado suporta entradas do tipo [CAA](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization), a certificadora também precisa ter suporte a este recurso, no caso deste artigo é utilizado o serviço de DNS do [GoDaddy](https://br.godaddy.com/) e a certificadora [Let's Encrypt](https://letsencrypt.org).

## Configurar a entrada CAA no DNS

Com todos os requisitos atendidos a próxima etapa é configurar o DNS, para isto siga os passos abaixo:

1 - Acesse o dashboard do GoDaddy e vá para o gerenciador de DNS como indicado na imagem abaixo:

![Acessar gerenciador de DNS do GoDaddy](https://i.imgur.com/h3unc5b.png)

2 - Agora clique em "ADICIONAR" para configurar uma nova entrada.

![Adicionar nova entrada no DNS](https://imgur.com/GgqcGfB.png)

3 - Escolha a opção "CAA".

![Selecione a opção CAA](https://imgur.com/3ZIJxLE.png)

4 - Verifique se todos os campos estão preenchidos como os da imagem abaixo, com exceção do campo "Nome completo *", o qual neste caso está preenchido com "@", pois o exemplo utiliza o domínio principal e não um subdomínio como por exemplo: blog.rafaelmoraes.tech, depois de preencher os campos corretamente, clique no botão "Salvar".

![Informar dados da entrada CAA](https://imgur.com/PE9VSo8.png)

5 - Caso todos os campos tenham sido preenchidos corretamente a nova entrada deve aparecer na lista de modo similar ao da imagem:

![Entrada CAA configurada com sucesso](https://imgur.com/WHi4lnI.png)

Com isso finalizamos a configuração do DNS.

## Configurar Github Pages

Com o DNS devidamente configurado, agora é necessário configurar o Github Pages para forçar o uso do HTTPS, para isto acesse a aba "Settings" do repositório do site e ative a opção "Enforce HTTPS" como pode ser visto na imagem a seguir:

![Configurar Github Pages para forçar o HTTPS](https://imgur.com/Mn2G5Dc.png)

Feito isso, basta aguardar poucos minutos(no meu caso foi quase instantâneo) que o HTTPS estará ativo e com isso finalizamos, sugestões e críticas são bem-vindas.

### Fontes
[https://help.github.com/articles/securing-your-github-pages-site-with-https/](https://help.github.com/articles/securing-your-github-pages-site-with-https/)
[https://br.godaddy.com/help/adicionar-um-registro-de-caa-27288](https://br.godaddy.com/help/adicionar-um-registro-de-caa-27288)
[https://letsencrypt.org/docs/caa/](https://letsencrypt.org/docs/caa/)
