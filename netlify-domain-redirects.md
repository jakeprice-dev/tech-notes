---
id: 20210423105044
uuid: 40938b54-f4bf-40ed-82e0-d04dcca53648
title: Netlify Domain Redirects
date: 2021-04-23 10:50:44
modified: 
types: tech-note
categories: netlify
pinned: false
tags: [netlify, dns, domain, redirection]
private: false
draft: false
---

I have a Hugo site on Netlify.

I needed to redirect my old domain `jakeprice.tech` and `www.jakeprice.tech` to my new one `jacobprice.tech` using Netlify.

The domains are registered elsewhere, but their nameservers point to Netlify, and therefore all DNS records are on Netlify.

For my Hugo site in Netlify, I first added both `jakeprice.tech` and `www.jakeprice.tech` as domain aliases:

> `Site settings > Domain management > Domains > Add domain alias`

Then I initially created a `_redirects` file and placed it in the root of my Hugo site repository. That didn't work though, as you can see below.

```sh
curl -svo /dev/null https://jakeprice.tech  2>&1 | egrep '< (HTTP|location)'
< HTTP/2 200
```

I was seeing a `200` instead of a `301`. So my site was accessible at each domain, as though that domain was the main domain!

It turns out if you're using `_redirects` you need to put it in the `public/` directory of your Hugo site. It wasn't immediately obvious how I could do that in Netlify, as Netlify builds the `public/` directory - i.e. it's not committed to my Hugo site's repository (I'm sure there's a way to do it, I just haven't bothered to look, because I found a nice and simple solution).

An answer on Stack Overflow helped me find that out: [http status code 301 - How to make 301 redirect in Netlify work? - Stack Overflow](https://stackoverflow.com/questions/54207804/how-to-make-301-redirect-in-netlify-work)

Quite simply I could just add the redirects to my `netlify.toml` file in the root of my Hugo site's repository. Not that my redirect's were complicated but I used [this playground](https://play.netlify.com/redirects) to convert them and then committed the change to `netlify.toml`. 

Once Netlify picked up the commit and rebuilt the site the redirects started working as expected.

```sh
curl -svo /dev/null https://jakeprice.tech  2>&1 | egrep '< (HTTP|location)'
< HTTP/2 301
< location: https://jacobprice.tech/
```
