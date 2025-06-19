# SEO/Architecture Discussion

## Architecture Agnostic SEO Concerns:

Below I hope to organize my thoughts on what is important for SEO performance regardless of the final system architecture. You may already familiar with these concepts, this is as much for my own reference as is to convey the information to others. I will relate these concerns to our architecture options later in this document.

#### Here are some references that I found helpful, most of which are short reads if you're interested in diving deeper:

[Semrush: SEO for Developers, 10 Best Practices to Know](https://www.semrush.com/blog/seo-for-developers/)

This provides the biggest concerns in a nice list. I will cover most of these concerns in a similar format.

[Search Engine Land](https://searchengineland.com/guide/ultimate-site-migration-seo-checklist)

Some info on impact levels, a great step by step checklist. I'll be referencing this in the future for sure.

[Seosly: How To Migrate Without Losing Traffic & SEO](https://seosly.com/blog/how-to-migrate-without-losing-traffic-seo/)

Another great checklist that emphasizes crawling both sites yourself and details how to organize site structure to preserve the sitemap and link equity. It starts with the most concise argument for redirects as a method of sustaining link equity.

--

### Link Equity

Link equity is preserved on a couple different levels. One, for the high level pages where link equity is most important, the endpoint itself (mgc.com/example) must refer to the same domain structure and the request response should be OK (200) rather than a redirect. Two, the links referred to on that page (the next level of the site tree) should still point internally and have the same endpoint structure. This is where redirects over links to subdomains are of the utmost importance.

### Redirects

The following is one of the scenarios search engines dislike the most: A link or button exists on a page and points to another internal page, but in a new version of the same page, that link now points externally. It's likely they assume this breaks trust with the user, since on the client side there's an endpoint that no longer exists and a new external path in it's place.

In this scenario, pointing to a subdomain (even with the same endpoint), is considered external. This can create a large hit to SEO performance, especially when the change is first made. However, when the original endpoint still exists, but simply redirects (through a 301 http response) to a page on a subdomain, the SEO gods are appeased and link equity can be 100% preserved.

I'm a little bit cheeky here because I'm unclear as to whether this is just the proper way to make changes in the eyes of the search engine, or if simply tricking the crawlers into thinking the endpoint never changed at all (this is actually likely since it would crawl the "external" page as a seperate process, it just sees a 301, knows it's not an error, and moves on). Regardless of the why, it is well documented that this method works and is the best way to make changes to site structure (see Seosly article).

### The Sitemap

A sitemap is the structure of an entire site, typically in XML format (sometimes a better visual representation is created for human users). A sitemap is provided to search engine crawlers as a sitemap.xml file located at the root the server.

The search engine crawlers will create their own version of a sitemap if it either does not exist, or does not encapsulate all of the pages/endpoints the crawler finds on the site. This is often the case with wordpress as typically only adds files to the sitemap (rendering all the pages server side as wordpress does means crawlers can find everything fine on their own).

There are benefits to providing the sitemap to the crawlers in advance also, especially when making structural changes. This allows the crawlers to verify the site's structure quicker and with less iteration. This also allows for showing subdomain structures properly so links are less likely to be viewed as external. You can also demonstrate any quirks like /home and the root being the same.

When migrating severs in any amount, generating a comprehensive sitemap and mimicking it's exact structure is imperative for SEO. Verification is done by generating a sitemap of the new server using the same tool and making sure they match up.

### Robots File

Along with the sitemap file, a robots.txt file is also typically stored at the root. This file tells search engines what they should and shouldn't crawl. This is handy because you can exempt any pages where SEO is not a concern (for MGC, this is almost everything except for landing pages, product pages, and the blog).

This helps SEO rankings since pages with reactive elements tend to look strange to crawlers and often show as errors, hurting the "site health" score. This also means the crawlers can be more efficient, meaning crawling pages like new articles happens faster without needing to check the entire site for other changes.

### Server Location and IP Address

Through my research I've come to believe that changing servers and IP addresses has little impact on SEO, here's a concise article with some info specific to IP addresses and some notes directly from google.

[WooRank: How will changing IP address impact SEO](https://www.woorank.com/en/blog/how-will-changing-ip-address-impact-seo)

Google makes a large portion of it's cloud infrastructure income due to the Kubernettes platform, which allows instances of websites or applications to be spun up and spun down across the globe to dynamically meet traffic demand. This is the reason that if I go to Amazon today and again tomorrow, I'll likely be accessing an entirely different server and therefor IP address than I did the say before. All search engines care about is the endpoint, site, and page structure being exactly the same across these instances.

There are a couple of notes specific to migrating to new servers. Since most of the time (especially in the case of WP engine) sites are sharing fractions of servers (and therefor have the same IP address), things like "noisy neighbors" or sites on the same server being marked as spam can have an impact on SEO for all domains pointing to that server. This can be remedied by requesting a new IP most of the time, however it's also possible to get entire machines for relatively cheap nowadays. This eliminates any issues that could arise from shared servers.

Another issue with changing servers is if the main datacenter is in an entirely different region, this is only an issue for sites that are distributing 2-3 instances manually, rather than a true distributed system (meaning: not MGC, we will likely still be in some east coast datacenter).

### Structured Data (Snippets)

Structured data allows for snippets in search engine displays that show information like product information. This can help performance of product pages.

### Canonical Tags

This is a way of strictly identifying links as internal to ensure that search engines can assess site structure efficiently.

### Semantic HTML

Tags like `<main> <article> <header> <p>`, rather than a bunch of divs can help search engines read page structures better and can make pages like blog posts look more valuable. Wordpress themes do a notoriously poor job of this since they rely on the id of the element rather than the tag itself. Relying on the id is effective for creating reactive ui's, but generally unnecessary for static pages where SEO is important.

### Secure Certificates

HTTPS is known to help significantly with SEO and it's worth enforcing even on completely static pages. Wordpress does this most of the time, when building custom web apps this is also common since HTTPS is required for cookies, and cookies are required for authorization and event tracking.

### Mobile Friendly

Having mobile friendly versions of landing pages tends to boost SEO, this is taken care of to some degree in most wordpress, and vanilla css themes, although some tweaking is often still required.

---

## Architecture Options

Below are options for the future architecture of MGC, arranged in order of the least change to the most.

All but one of these architectures have a blue line (separating the two servers), the information in the previous section dictates the way we cross this line is probably the most important factor for future SEO performance. Not which server existed first, or which side certain functionality lies on, but how the line is crossed. We only see one or two examples of crossing this line in the diagrams below, but we will have a handful of these paths.

**The blue line shall only be crossed by a 301 redirect from an EXISTING endpoint. NOT a link to the "external" source.**

### All static pages and initial purchases on WordPress:

In order to make purchases possible on our wordpress server, we will create the necessary data structures in our go database to handle verifying purchases from outside the system itself. This can be done by simply receiving requests from wordpress at certain endpoints and expecting all the information about the purchase, student, time, etc in a certain json format. Webhooks shouldn't be necessary since we can get all the info we need client side on wordpress.

This does open up discussions about running email delivery services in tandem, it's unclear to me without some further discussion whether this will be more or less complicated than relying on api requests to kick off the email from the go server.

All purchase information will be sent to Stripe and this will be the source of absolute truth for purchases. Setting up products in Stripe and which server has access to those products will be important.

We will also need to discus any period we wish to run the old system and new system in tandem This is more complex since we would need an additional copy of our static server to point to the old system while it is still in use, likely using yet another subdomain (maybe legacy.medgascerts or something).

I believe the implementation below would be sufficient to make this possible:

![Figma 1](https://github.com/user-attachments/assets/493437ac-0686-467e-8c1a-988e6e961132)

### Moving Google Analytics events to Go:

One way we can simplify the architecture while keeping SEO optimized pages on wordpress is to sent events to google analytics from our go server (or client). This allows for google analytics to still be the source of truth for how these pages perform, all the way through to a user making a purchase, without needing those purchases to actually happen on wordpress.

This is possible in multiple ways, my favorite is server side using google tag manager. Here's an article that details how to set this up.
[Simo Ahava: Server-side Tagging In Google Tag Manager](https://www.simoahava.com/analytics/server-side-tagging-google-tag-manager/)

This not only simplifies how commerce is handled, but also opens some doors to how we can handle/analyze our commerce data. One example: We can keep all product, discount, contact, etc. info away from Stripe (if we so choose). That means they only process credit cards for us at amounts we send them for every transaction. We can then run any reports we need to on our database, export directly to quickbooks, etc.

![Figma 2](https://github.com/user-attachments/assets/f58ccc72-fa74-45ed-b18c-c92bd3ddb0f4)

### Moving web and product analytics to PostHog:

I will be using Posthog on our site for some of it's unrelated functionality regardless of our architecture. Posthog is an open source suite of data products. Their web analytics are very similar to google analytics, but provide more custom events, which can be triggered server side. This allows for the client to send the data to a trusted source first (the server) meaning more events get tracked even when add blockers or certain browser protections are present.

Posthog also has "product analytics", which allow for sales funnel metrics to be viewed alongside actual sales, and there are a handful of ways these two can influence each other on the platform.

Other notable functionality includes:

- Session replay: This shows an actual replay of user interactions on certain pages, helpful for designing good interfaces, but also for analyzing sales funnels, handling support, etc.
- Surveys: No-code, embeddable surveys that have great UX, allows survey data to be viewed alongside other tools like error tracking.
- Data pipelines: These allow for piping data to many different integrations, **we could actually send analytics back to google analytics if that UI is prefered**, trigger email campaigns in sendgrid or mailchimp based off of any server event (user action, new blog post, etc), or link directly with hubspot to handle support or customer resources.

![Figma 3](https://github.com/user-attachments/assets/3a94b3a6-88c5-4432-a915-38dd6a07c2fd)

### Moving static pages to HTML files on Go server:

THE BLUE LINE IS GONE

There are a handful of reasons hosting everything on a single server is a plus, the largest of which is not being concerned with handling "external" links correctly to preserve SEO performance. Some others are design consistency, general simplicity, hosting costs, data reliability, etc. I am biased towards this approach. In light of that bias, I will dedicate most of this section to some of the downsides I see, and options for managing them.

Wordpress is easier to edit. We know the interface, it's a decent one for making quick updates. The best option we have for mimicking this on a go server is rendering pages from text in markdown documents like this one. This makes editing text and writing new blog posts easy and efficient, but relies on pre-made page designs.

We must create an exact copy of the sitemap. Mimicking the sitemap as close as we can becomes even more important when swapping everything over. This requires generating the maps multiple times and verifying the accuracy of the rebuild. The complexity can be mitigated to some degree by limiting what get's crawled in a robots.txt file.

Content management is more complex. Things like images and videos should be added to the assets directory or object storage and then referenced by their relative path or url, this is not as intuitive as dragging and dropping in wordpress.

Legacy data may not transfer. I've made successful migrations for a lot of MGC's existing database. Posthog allows importing past analytics data. Other new elements are similar, but I do not feel comfortable guaranteeing that 100% of our legacy data will transfer to new systems.

That being said, here is what I consider the cleanest architechture, as far as SEO is concerned.

![Figma 4](https://github.com/user-attachments/assets/68868f5e-8c32-46a1-ba44-e24079b29dd4)
