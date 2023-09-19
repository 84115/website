---
layout: post
title: "Xscreensaver Wallpaper"
date: 2020-02-12 18:16
comments: false
categories: linux x-server
---

###Install Xwinwrap

This article assumes you already have Xscreensaver installed.

Note for a full list of backgrounds run `ls /usr/lib/xscreensaver/`.

1. `git clone https://github.com/ujjwal96/xwinwrap.git`
2. `cd xwinwrap`
3. `make`
4. `sudo make install`
5. `make clean`

{% codeblock %}
#!/bin/bash
xwinwrap -b -fs -sp -fs -nf -ov  -- /usr/lib/xscreensaver/cubicgrid -root -window-id WID &
{% endcodeblock %}

[Source](https://github.com/ujjwal96/xwinwrap.git)

