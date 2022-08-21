---
layout: post
title: "Redirect Pihole Admin Page"
date: 2022-08-21 09:00:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, pihole]
---

Really simple instructions today but it's always bothered me that the admin page for configuring the pihole DNS service requires you to use `http://<IP/HOSTNAME>/admin` to reach the configuration page. 

I went digging through the lighttpd config file as well as executing some google-fu and found that you create a file `/etc/lighttpd/external.conf` which will add additional config to the process that shouldn't be over written by the pihole update process: 

```
### Add support to redirect the default admin page from /admin to the root /
url.redirect = ("^/$" => "/admin" )
```

This line should allow you to visit the site directly `http://<IP/HOSTNAME>` with no `/admin` on the URL and the admin page should load.
