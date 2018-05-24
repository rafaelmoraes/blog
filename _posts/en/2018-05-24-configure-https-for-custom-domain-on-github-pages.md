---
date: "2018-05-24 13:00:00"
title: "Configure HTTPS for custom domain on GitHub Pages"
keywords: "configure HTTPS GitHub Pages"
read_in: 5
translations: /configurar-https-para-dominio-personalizado-no-github-pages
---
In this article we'll see how to configure the [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protocol for site with custom domain hosted on the [GitHub Pages](https://pages.github.com/).

## The Limitation

Configure a custom domain for a site hosted on GitHub Pages is extremely simple, but utilize a custom domain with HTTPS not is a trivial task, for is impossible execute the traditional configurarion process, which basically consists to send and configure at web server the files provided by certificate authority (CA), therefore, is necessary a alternative solution.

## The Solution

How is impossible to execute the HTTPS configuration directly on GitHub Pages, we'll circumvent this limitation using the [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) server to 'notify' to clients that site supports HTTPS.

Firstly is necessary have sure that DNS service supports records of type [CAA](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization), the certificate authority also needs supports this resource, at article case is used [GoDaddy](https://godaddy.com/) DNS service and [Let's Encrypt](https://letsencrypt.org) certificate authority.

## Configure CAA record on DNS

With all requirements attended the next step is configure the DNS, so follow the below steps:

1 - Access the GoDaddy dashboard and go to the DNS manager as indicated at below image:

![Access GoDaddy DNS manager](https://imgur.com/mVCrul2.png)

2 - Now click in 'ADD' to configure a new record.

![Add new DNS record](https://imgur.com/YciEa2d.png)

3 - Select the option 'CAA'.

![select the CAA option](https://imgur.com/rIe7Frq.png)

4 - Verify if all fields are filled as the below image, with exception of field 'Name *', which in this case is filled with '@', because the sample uses the main domain and not a subdomain as for example: blog.rafaelmoraes.tech, after filled the fields correctly, click on the 'Save' button.

![Fill record fields fo CAA](https://imgur.com/QR1BjH2.png)

5 - Case all fields are filled correctly the new record should appear at the list likewise of image:

![CAA configured successfully](https://imgur.com/xRwAoNj.png)

Therewith we finished the DNS configuration.

## Configure GitHub Pages

With the DNS properly configured, now is necessary to configure the GitHub Pages to force use of the HTTPS, for this access the tab 'Settings' on site repository and activate the option 'Enforce HTTPS' as can be saw on the follow image:

![Configure GitHub Pages enforce HTTPS](https://imgur.com/Mn2G5Dc.png)

Done it, just wait few minutes(on my case was almost instant) that HTTPS will be active and therewith we finished, suggestions and reviews are welcome.

### Fontes

* [https://help.github.com/articles/securing-your-github-pages-site-with-https/](https://help.github.com/articles/securing-your-github-pages-site-with-https/)
* [https://br.godaddy.com/help/adicionar-um-registro-de-caa-27288](https://br.godaddy.com/help/adicionar-um-registro-de-caa-27288)
* [https://letsencrypt.org/docs/caa/](https://letsencrypt.org/docs/caa/)
