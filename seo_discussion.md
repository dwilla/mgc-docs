# SEO/Architecture Discussion

## Architecture Agnostic SEO Concerns:
Below I hope to organize my thoughts on what is important for SEO performance regardless of the final system architecture. It's likely you are already familar with these concepts, this is as much for my own reference as it is to convey them to others. I will relate these concerns to our architecture options later in this document.

#### Here are some references that I found helpful, most of which are relatively short reads if you're interested in diving deeper:

[Semrush: SEO for Developers, 10 Best Practices to Know](https://www.semrush.com/blog/seo-for-developers/)

This provides the biggest concerns in a nice list. I will cover most of these concerns in a similar format.

[Search Engine Land](https://searchengineland.com/guide/ultimate-site-migration-seo-checklist)

Some info on impact levels, mostly a great step by step checklist. I'll be referncing this in the future for sure.

[Seosly: How To Migrate Without Losing Traffic & SEO](https://seosly.com/blog/how-to-migrate-without-losing-traffic-seo/)

Another great checklist that emphasizes crawling both sites yourself and details how to organize site structure to preserve the sitemap and link equity. It starts with the most concise argument for redirects as a method of sustaining link equity.

--
### Link Equity
Link equity is preserved on a couple different levels. One, the endpoint itself (mgc.com/example) must refer to the same domain structure, and the request response should be OK (200) rather than a redirect. Two, the links refered to on that page (the next level of the site tree) should still point internally and have the same endpoint structure. This is where redirects over links to subdomains become extremely important.

### Redirects
The following is one of the scenarios search engines dislike the most: A link or button exists on a page and points to another internal page, but in a new version of the same page, that link now points externally. It's likely they assume this breaks trust with the user, since on the client side there's an enpoint that no longer exists and a new external path in it's place.
In this scenario, pointing to a subdomain (even with the same endpoint) is considered external. This can create a large hit to SEO performance, especially when the change is first made. However, when the original endpoint still exists, but just redirects (through a 301 http response) to a page on a subdomain, the SEO gods are appeased and link equity can be 100% preserved.
I'm a little bit cheeky here because I'm unclear as to whether this is just the proper way to make changes in the eyes of the search engine, or if it is simply tricking the crawlers into thinking the endpoint never changed at all (this is actually pretty likely since it would crawl the "external" page as a seperate process, it just sees a 301, knows it's not an error, and moves on). Regardless of the why, it is well documented that this method works and is the best way to make changes to site structure (see Seosly article).

### The Sitemap
A sitemap is the structure of an entire site, typically in XML format (sometimes a better visual representation is created for human users). A sitemap is provided to search engine crawlers as a sitemap.xml file located at the root the server.
The search engine crawlers will create their own version of a sitemap if it either does not exist, or does not encapsulate all of the pages/endoints the crawler finds on the site. This is often the case with wordpress as typically only adds files to the sitemap (rendering all the pages server side as wordpress does means crawlers can finc everything fine on their own).
There are benefits to providing the sitemap to the crawlers in advance also, especially when making structural changes. This allows the crawlers to verify the site's structure quicker and with less iteration. This also allows for showing subdomain structures properly so links are less likely to be viewed as external. You can also demonstrate any quirks like /home and the root being the same.
When migrating severs in any amount, generating a comprehensive sitemap and mimicking it's exact structure is imperitive for SEO. Verification can be done by generating a sitemap of the new server using the same tool and making sure they match up.

### Robots File
Along with the sitemap file, a robots.txt file is also typically stored at the root. This file tells search engines what they should and shouldn't crawl. This is handy because you can exempt any pages where SEO is not a concern (in the case of MGC, this is almost everything except for landing pages, product pages, and the blog).
This helps SEO rankings since pages with reactive elements tend to look strange to crawlers and often show as errors, hurting the "site health" score. This also means the crawlers can be more efficient, meaning crawling things like new articles happens faster without needing to check the entire site for other changes.

### Server Location and IP Address
add notes and some more references here \\ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖ ➖

### Structured Data (Snippets)
Structured data allows for snippets in search engine displays that show things like product information. This can help performance of product pages.

### Canonical Tags
This is a way of strictly identifying links as internal to ensure that search engines can assess site structure quickly. 

### Semantic HTML
Tags like <main>, <article>, <header>, <p>, rather than just a bunch of divs can help search engines read page structures better and can make pages like blog posts look more valuable.

### Secure Certificates
HTTPS is known to help significantly with SEO and it's worth enforcing even on completely static pages.

### Mobile Friendly
Having mobile friendly versions of landing pages tends to boost SEO, this is taken care of to some degree in most wordpress, and vanilla css themes, although some teaking is often still required.


---
## Architecture Options
Below are various options for the future architecture of MGC, arranged in order of the least change to the most.

### All static pages and initial purchases on WordPress:


### Moving Google Analytics events to Go:


### Moving web and product analytics to PostHog:


### Moving static pages to HTML files on Go server:


