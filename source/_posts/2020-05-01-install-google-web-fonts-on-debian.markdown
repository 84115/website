---
layout: post
title: "Install Google Webfont On Debian"
date: 2020-01-26 12:00
comments: false
categories: linux fonts
---

## Download desired fonts
- `https://fonts.google.com/specimen/Raleway`

## Install Google Fonts on Debian
- `cd /usr/share/fonts`
- `sudo mkdir googlefonts`
- `cd googlefonts`
- `sudo unzip -d . ~/Downloads/Raleway.zip`
- `sudo chmod -R --reference=/usr/share/fonts/opentype /usr/share/fonts/googlefonts`

## Register fonts
- `sudo fc-cache -fv`

## Check if font installed
- `fc-match Raleway` (Name is camelcased if name has spaces)
