---
title: Using Redis as primary database to serve 50k Indian SME’s on Connect app
status: Published
date: '2019-12-15'
excerpt: >-
  When the Internet is boasting stories about AI and using Big data to solve
  complex decision problems in the world. Indian SME’s space is struggling with
  a very different problem “Bring offline Small…
categories:
  - category: Redis
  - category: Product
  - category: Technology
  - category: Entrepreneurship
meta:
  canonicalLink: >-
    how-we-used-redis-to-serve-50k-indian-smes-in-less-than-6-months-315858db27d0
  description: >-
    When the Internet is boasting stories about AI and using Big data to solve
    complex decision problems in the world. Indian SME’s space is struggling
    with a very different problem “Bring offline Small…
  title: Using Redis as primary database to serve 50k Indian SME’s on Connect app
---
When the Internet is boasting stories about AI and using Big data to solve complex decision problems in the world. Indian SME’s space is struggling with a very different problem “Bring offline Small businesses Online”.

The problem does not seem difficult but here is when it becomes challenging.

-   Majority of them missed the Browser age leapfrogged to Mobile.
-   Only have access to basic 2G and 3G data.
-   Internet for them is Whatsapp & Facebook. Less adaptive for new applications.

All these scenarios lead to a serious problem “Their business network needs to grow and it needs to happen online.”

---

### The solution

![Connect first version (launched within 30 days)](./asset-1.png)

We at [ZipLoan](https://medium.com/@ziploan) buckled up to solve it and launched [Connect](https://play.google.com/store/apps/details?id=com.biz.connect) as a platform to help them come online and be as slick as their existing experience on Android.

The solution included bringing them community chats and “Profile” as a window to showcase their products/portfolio.

With very less investment in marketing and letting it grow organically, we gained what we were not expecting. The platform had reached almost 8k downloads every month. Does not sound much but for an organic showcase and early POC, this is a good number to prove the point.

With millions of messages floating in the platform we needed an architecture that could provide the window for scale and make sure every message gets delivered in less than <1ms and can work brilliantly even on 2G networks.

The early POC was on firebase’s real-time database and it didn’t work for our use case much as we would not able to plug in our spam control to filter out noise for our users. Plus the rate as which are growing it was fire on the pockets.

After hustling in with more than 10 POC of real-time protocols like XMPP, AMQP, MQTT etc we settled for [NATS](https://nats.io/) and so far it has proved to be a blessing. Now, all we needed was a high I/O database that can scale and be always available.

#### Redis as Primary Database

So many folks on the internet have written stories about why not to trust memory based databases yet after working with Redis for about 6 years I had my gut pumping to try it out as the primary database for our ‘SME communities’.

The solution required us to achieve persistency on the memory based database and it just cannot go down.

We engineered our way around and followed Redis basics to replicate data across multiple nodes and some nodes will be constantly backed up on Amazon at various regions. Hence if at any point our database nodes went down, we will have them up instantly.

![](./asset-2.jpeg)

The above architecture was religiously followed and our servers are quite healthy and at the peak time of server usage, we have a rate of <1ms per query! Pretty amazing isn’t?

Now, with Redis sentinel (a server to encourage a slave to master if the master is down) all we required was a Proxy to redirect the request to the online node if any of the masters went down. What better then HAProxy!

`gist:hiteshjoshi/822723acf7b003d597c4538908a8a43a`


In the last six months, there have been absolutely zero downtime and NO lags on database query even after multi millions of records sitting upon Redis.

Questions about scaling Redis? Shoot away!
