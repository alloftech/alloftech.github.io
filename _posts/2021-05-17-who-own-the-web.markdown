---
layout: post
title: Who Owns The Web?
date: 2021-05-17
cover_image: 2021-05-17-feature.jpg
author: Simon Dosda
categories: culture web
excerpt_separator: <!--more-->
last_modified_at: 2021-07-13
---

> Photo by [Bill Oxford](https://unsplash.com/@bill_oxford).

I know the question might sound a bit silly. Nobody owns the web; anyone connected to the network can be a host and deliver content; that's the beauty of the internet!

But if you do so, you will indeed be able to access your server, but only by its IP address, something like _176.12.57.1_. Good luck with finding anyone to visit your page!

<!--more-->

The web wouldn't be the same without domain names associated with these IP addresses, but in the free world that is internet, who own them? Who decides what domains are available and have the right to sell them?

## A bit of history

Before answering these questions, we need to understand how the internet started.

The internet is far from young. It first started in 1969 under the name of ARPANET (Advanced Research Projects Agency Network). At its beginning, it was used to connect some of the major US universities and allow them to share files.

But as you can guess, back then, it was far from being what we call the internet today. Computers were linked by IP addresses, and the network could only be used by experts as there was no way to navigate it swiftly.

![arpanet](/assets/images/2021-05-17-arpanet.jpg)
_ARPANET, the ancestor of internet_

In 1989, [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee), a researcher at CERN, wrote the first proposal for the World Wide Web. The WWW, what we simply call the web now, was a proposal to merge different evolving technologies and standards in order to turn the internet into a powerful and easy-to-use global information system.

This proposal was based on using the HyperText Transfer Protocol (HTTP) to navigate between web resources identified by Uniform Resource Locators (URLs) instead of IP addresses.

## How URLs work

Let's see what composes a URL before going further to ensure we all use the same vocabulary. Let's decompose the URL of my blog (why not?) that is hosted on [GitHub](https://simondosda.github.io).

![url lingo](/assets/images/2021-05-17-lingo.png)

You can see it as a reverse hierarchy. To register your domain name, you first have to choose a domain extension, then check if the domain name you want is available for this extension. You can then use it as it is but also create as many subdomains as you want for this domain.

The whole URL links to an IP address, in this case [185.199.108.153](http://185.199.108.153).

### How does your browser know to which IP address it should go when you type an URL?

The Domain Name System (DNS) takes care of linking URL to IP addresses. You can think of it as a directory with all domains and their corresponding IP addresses.

This information is stored on root servers that are located all over the world. These servers belong to different organizations. Some belong to the Internet Corporation for Assigned Names and Numbers (ICANN), the non-profit organization responsible for managing domain namespace, but others belong to universities, internet companies like Verisign or even the US Army.

## Who owns top-level domains?

We can now go back to our main question.

If you have created a website already, you know that in order to have a domain name, you need to buy one from a registrar, a company that manages the reservation of Internet domain names.

### Who do registrars purchase domains from, and who decides what the available top-level names are?

Until the end of the nineties, the function of administering top-level domains and IP addresses was performed by a single man, [Jon Postel](https://en.wikipedia.org/wiki/Jon_Postel), a Computer Science researcher who had been involved in the creation of ARPANET. Yes, it was a different time!

In 1998, the non-profit organization ICANN was formed with the mission to privatize the management of Internet names and addresses in a manner that allows for the development of competition and facilitates global participation in Internet management.

The Internet Assigned Numbers Authority (IANA) organization, which belongs to ICANN, is the one that decides nowadays what the available top-level domains are, but it does not sell them directly. Instead, it creates settlements with domain name registries to manage them. You can find the list [on their website](https://www.iana.org/domains/root/db).

Registrars then deal with these organizations to register domain names. For example, when a registrar registers a _.com_ domain name for an end-user, it must pay an annual fee to VeriSign, the registry operator for _.com_, as well as an annual administration fee to ICANN.

![registrar](/assets/images/2021-05-17-registrar.png)

## Final thoughts

In the end, it is your internet provider that is in charge of directing you to the required IP address when you enter an URL. To access the registry of domain names, they rely on root servers, which are under the responsibility of the ICANN.

They could rely on something else, but this is the consensus made historically and will probably never change. It is interesting to see how this system, which holds tremendous responsibilities, was put into place so smoothly and with so little criticism.

Much of the ICANN's criticisms come from the acceptance or rejection of new top-level names.

For instance, it refused the purchase of _.islam_ and _.halal_ top-level domain names by a Turkish company after the Organisation of Islamic Cooperation objected that they should be administered by an organization representing all Muslims.

On the other hand, it allowed the purchase of the _.sucks_ domain name by the Vox Populi Registry, which uses this domain to charge exorbitant amounts of money to companies so that they can protect their names.

Eventually, the fact that I didn't know about the existence of the ICANN or how all of this worked before writing this article just amazed me. I hope it was insightful to you too!
