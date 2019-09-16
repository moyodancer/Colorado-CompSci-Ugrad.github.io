---
title: Using github.io websites @ CU
keywords: faq
sidebar: mydoc_sidebar
permalink: web_publishing_using_github_io.html
folder: web_publishing
---

## Introduction

Many research groups, student groups and projects need a website to
disseminate their work. It's common to use content manage tools
such as [wordpress](http://wordpress.com) for this purpose, but
those systems require maintenance and a server.

##Note:
This solution of redirecting a `colorado.edu` domain name to github pages is not avaialble to private websites, student websites or websites not affilitated with an official function of the University of Colorado. OIT has final discretion over approval of forwarding such pages.

## An alternative: GitHub static pages

An alternative is to use [github pages](https://pages.github.com/).
You can follow the directions there to create your web pages. Github
provides free "static web pages" hosting, meaning you don't need to
provision your down server.

Faculty and student group website can be hosted using github pages
and then use a custom subdomain in the `colorado.edu` domain space.
Examples include:
* [http://plv.colorado.edu](http://plv.colorado.edu)

Alternatively, many research labs purchase their own domain. An example is
[http://www.cairo-lab.com/](http://www.cairo-lab.com/) --
in that case, you should skip this writeup and go to the GitHub pages
documentation directly.

## Steps to redirecting a `colorado.edu` domain name to github pages

Follow these steps:
* Insure that your web page relates to university functions and broadly follows
university branding standards.

* Choose a domain name that is not in use. You can use the `dig` or `nslookup`
tool for this pupose.

* Once your site is visible at
e.g. `http://YOUR-GITHUB-USERNAME.github.io`, send mail to
[help@colorado.edu](mailto:help@colorado.edu) asking create a CNAME
record that points your subdomain to your default pages domain. For
example, if you plan to use the subdomain `xyzzy.colorado.com`, ask
them to configure a CNAME record to point `xyzzy.colorado.com` to
`YOUR-GITHUB-USERNAME.github.io`. You should include [the link to the
github pages directions on how to do
this](https://help.github.com/en/articles/setting-up-a-custom-subdomain)
and communicate your role in the university and the purpose of the website.

## Links

* GitHub Pages info -- [https://pages.github.com/](https://pages.github.com/)
* Setting up a custom domain name -- [https://help.github.com/en/articles/setting-up-a-custom-subdomain](https://help.github.com/en/articles/setting-up-a-custom-subdomain])
* Longer tutorial at Medium -- [https://medium.com/@yadavnikhil012/publishing-github-page-website-on-a-custom-domain-with-https-enforcement-c034e1e53415](https://medium.com/@yadavnikhil012/publishing-github-page-website-on-a-custom-domain-with-https-enforcement-c034e1e53415)
