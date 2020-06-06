---
layout: post
title: "Octopress: Custom 404 page"
date: 2013-06-21 21:38
comments: true
categories: Octopress
---


Open up `config.ru` from the root directory and update the sinatra `not_found` route to the following:
{% codeblock config.ru lang:ruby %}
not_found do
  send_sinatra_file('404')
end
{% endcodeblock %}
This will redirect to `http://yourdomain.com/404/` where the page is found, but we still need to create a page.

To do this use `rake new_page[404]`. This will create a new file named `index.markdown` in your `/source/404` directory:
{% codeblock 404/index /404/ %}
---
layout: page
title: "404 Error"
comments: false
sharing: false
footer: false
---
Whoops, the page you’re looking for cannot be found.

Maybe you can find what you are looking for in the [archives](/blog/archives/).
{% endcodeblock %}
View [my 404 page](/404/) here.