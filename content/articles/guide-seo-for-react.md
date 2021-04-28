---
title: SEO guide for react
---

## Introduction

<br />

### Important aspects of optimization
* Keyword research
* Server side rendering vs static rendering
* Tags (open graph tags)
* Canonical/Meta tags
* robots.txt
* sitemap.xml

<br />

### Curl as googlebot

See how google downloads your website ([link](https://gist.github.com/chrisle/2252209)).
Replace `$@` with your website.

```shell
curl --user-agent "Googlebot/2.1 (+http://www.google.com/bot.html)" -v $@
```

<br />
## Installment of next

See react-next snippet

<br />

## Tags

### Title tags

```html
<title key="...">...</title>
```

* Relatively short (60 characters or so)
* Features core concept
* compelling
* maybe include a keyword there. keyword research.
* think through the name of the title. IMPORTANT part in crawling

Examples of keywords that could be used:
* History
* Team
* Development
* Software
* Android

<br />

### Meta description tag

```html
<meta name="description" content="..." key="..." />
```

* Second half of search engine result page.
  First is the title. (e.g see any result from google search)
* Description for social media sites
* What makes a good description?
    * about 160 characters
    * contains keywords
    * express value/specification
        * e.g. prices, description of products
    * prompt/call to action. encourages user to use product/service.

Examples:
* We provides prices that are low...
* Let us guide you through the custom software design
  and development process. Send us a message...
* Use our free online estimate calculator to instantly check the cost of your
  custom software...
* Cutting edge technology...



<br />
