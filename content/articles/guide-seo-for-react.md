---
title: Guide - SEO for react
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

* Having at least one h1 per page, helps seo.

<br />

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


### Open graph tag

```html
<meta name="og:type" content="website" />
<meta name="og:image" content="https://i.imgur.com/image-name.png" />
<meta name="og:image:type" content="image/png" />
<meta name="og:image:width" content="1200" />
<meta name="og:image:height" content="630" />
<meta name="og:image:alt" content="company logo" />
<meta name="og:image:title" content="Bringing west coast tech..." key="og:title" />
<meta name="og:url" content="arc.com/about" key="og:url"/>
```

* Created by facebook for sharing websites
* host an image (e.g. imgur)
* open graph protocol ([open graph protocol link](https://ogp.me/))
* width and height (1200 & 630 is the standard size people have found works best)
* title is the title that is going to be displayed
* key ensures that the property is unique on the page
* URL The canonical URL of your object that will be used as its permanent ID in the graph


<br />

### Canonical tags

```html
<link rel="canonical" key="canonical" href="https://arc.com/about" />
```

* default version of your page (main version)
* Tells search engines which url is the correct version of the page
* https://google.com vs http://google.com vs https://www.google.com


<br />


## Robots.txt & sitemap.xml

* Explicit instrucions to the search engine.
* Gives points to seo
* They need to be at the root url

### Robots.txt

* Gives rules on how site can be crawled
* Links to the sitemap

<br />

### Sitemap.xml

* Informs search engines of the site structure
* Provides some meta information about individual pages

<br />

### Scripts used to build sitemap.xml and robots.txt
*by @embiem*

* Install `fs-extra`. `npm install --save fs-extra`
* Insert this script in scrips `package.json
    * `postbuild: node ./scripts/sitemap.js`

formatDate.js
```javascript
module.exports = function formatDate(date) {
  var d = new Date(date),
    month = "" + (d.getMonth() + 1),
    day = "" + d.getDate(),
    year = d.getFullYear();

  if (month.length < 2) month = "0" + month;
  if (day.length < 2) day = "0" + day;

  return [year, month, day].join("-");
};

```

<br />

getPathsObject.js
```javascript
const fs = require("fs");

module.exports = () => {
  const fileObj = {};

  const walkSync = dir => {
    // Get all files of the current directory & iterate over them
    const files = fs.readdirSync(dir);
    files.forEach(file => {
      // Construct whole file-path & retrieve file's stats
      const filePath = `${dir}${file}`;
      const fileStat = fs.statSync(filePath);

      if (fileStat.isDirectory()) {
        // Recurse one folder deeper
        walkSync(`${filePath}/`);
      } else {
        // Construct this file's pathname excluding the "pages" folder & its extension
        const cleanFileName = filePath
          .substr(0, filePath.lastIndexOf("."))
          .replace("pages/", "");

        // Add this file to `fileObj`
        fileObj[`/${cleanFileName}`] = {
          page: `/${cleanFileName}`,
          lastModified: fileStat.mtime
        };
      }
    });
  };

  // Start recursion to fill `fileObj`
  walkSync("pages/");

  return fileObj;
};
```

<br />

sitemap.js
```javascript
const fs = require("fs-extra");
const getPathsObject = require("./getPathsObject");
const formatDate = require("./formatDate");

// ROBOTS.txt (change baseurl for yours)
const robotsTxt = `User-agent: *
Sitemap: https://arc.com/sitemap_local.xml
Disallow:`;

fs.writeFileSync("public/robots.txt", robotsTxt);
console.log("robots.txt saved!");

// SITEMAP.XML
const pathsObj = getPathsObject();
const today = formatDate(new Date());

// Modified script (@embiem was adding _documetn and _app so they are being filtered)
const sitemapXml = `<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  ${Object.keys(pathsObj)
    .filter(path => path !== "/_document" && path !== "/_app")

    .map(
      path => `<url>
    ${
      path === "/index"
        ? `<loc>https://arc.com</loc>`
        : `<loc>https://arc.com${path}</loc>`
    }
    <lastmod>${
      pathsObj[path].lastModified
        ? formatDate(new Date(pathsObj[path].lastModified))
        : today
    }</lastmod>
  </url>`
    )}
</urlset>`;

fs.writeFileSync("public/sitemap_local.xml", sitemapXml);
console.log("sitemap_local.xml saved!");
```

<br />

## Cross browser support

* Use analytics
* Active while in development (suggestions during typing)

<br />

### List of browsers

* Chrome
* Firefox
* Safari
* Microsoft edge

<br />

### Safari

Caveats:
* <img /> safari doesnt render svg text tags inside of the img tag.
    * we copy the svg file and replace the image tag.
*  Make appropriate adjustments of styles
    * check for displays (e.g. `display: none`)


<br />

## Deployment


* publish the project 

<br />

### Heroku

see heroku snippets

<br />


### now

* same company as next.js
* free SSL
* Automatic CDN


How to use?

1. Make an account
2. install now globally `npm install -g now`
3. `now init nextjs`
4. `cd nextjs`
5. `now`

<br />

## Google Analytics

[Google analytics](https://analytics.google.com/analytics/web/provision/#/provision)

<br />

### Features

* Goals: see what the users did before reaching something you are interested in.
  * Conversion: track important actions (see how much your website is providing to the users)
* Filtering: avoid adding data from testing (you as a user).
  * go to views and remove yourself as a user
  * go to admin button
  * add view (test view). set your current timezone
  * add view (master view).
  * select a filter.
  * add a filter (remove home traffic).
  * exclude your ip.
* Events: Collect data and organize it
  * Can have a category, action, label value
    * Label and value are optional

<br />

### Installation

Installment

1. create or login with google account
2. select what you want to measure
3. select url and website name
4. select industry and timezone accordingly
5. copy tracking id

React package

1. Install `react-ga` package

```html
npm install --save react-ga
```

2. use the following function `ReactGA.initialize('tracking_id')`
3. Watch for url changes and send info to react-ga
    * watch out for re render of components. so it sends once per visit.

* You can send events to analytics by using the event function

<br />

