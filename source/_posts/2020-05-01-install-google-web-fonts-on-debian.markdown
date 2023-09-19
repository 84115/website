---
layout: post
title: "Install Google Webfont On Debian"
date: 2020-01-26 12:00
comments: false
categories: linux fonts
---

### Download desired fonts
We'll use Raleway in this example `https://fonts.google.com/specimen/Raleway` and download "Raleway.zip".

### Install Google Fonts on Debian
```bash
cd /usr/share/fonts
mkdir googlefonts
cd googlefonts
unzip -d . PATH_TO_DOWNLOADS/Raleway.zip
chmod -R --reference=/usr/share/fonts/opentype /usr/share/fonts/googlefonts
```

### Register fonts
```bash
sudo fc-cache -fv
```

### Check if font installed
Note: The Name is camelcased if the fonts name has spaces.

```bash
fc-match Raleway
```
