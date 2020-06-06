---
layout: post
title: "Octopress: My Thoughts After Switching From Wordpress"
date: 2013-07-05 18:13
comments: true
categories: Octopress
---



Octopress is a framework built on top of Jekyll. It's a static site generator which means that it lacks advance feature which would normally require scripting and a back-end like Wordpress. If you want to know whether you should switch to Octopress or would like to know some tweaks, tips or adjustments then continue through this post.



Why To Migrate From Wordpress
-----------------------------

* Wordpress is a great blogging and CMS platform, but it just had too many features for  my personal blog.
* So far I have found Octopress easier to maintain than Wordpress. There is no need to maintain wp-super-cache which serves up static pages in the same ways as Octopress along with not needing to manage a local and external LAMP stack.
* WordPress blogs are a common target for hackers and spammers. To avoid this you need to keep your version of Wordpress constantly up to date.
* Octopress allow you to create pages using Markdown instead of or along with HTML. Markdown has a cleaner, simpler and more readable syntax.
* Octopress uses Jekyll to build pages. All pages of the pages are static and no server-side processing involved. This means that any requested page can deliver immediately by the server to the user.
* THEME ETC, READY FROM THE GET GO



Features You Might Miss
-----------------------

* You can’t edit online from anywhere. With Wordpress a mobile app was available so you did not need to be at your computer/laptop to publish new posts.
* Yay Markdown. Shame there is no solid way to manipulate images except by hand. I have currently worked around this by using Picasa to host my images which will cache my images and resize my images.
* Octopress does not feature drafts, previews or publishing. When you deploy your site, you deploy everything. And the publication date is the date it was started, not when it's finished.
* Although it's nice not having to use the web interface it's also a feature which I also miss being able to post from anywhere, on almost any device.
* **SLOW PREVIEW LARGE POST**
* Octopress has less plugins and themes available than Wordpress, If you want more features you will need to wait for someone to write it or write it yourself.
* By default Octopress does not have a comment system. Although there is the option to use an external service for page comments such as Disqus. But using a service like this means that the comments are not hosted on your service and you lack the flexibility of the Wordpress commenting system.



Conclusion
----------

Octopress is a great platform targeted at programmers who want to start blogging without having the hassle of customizing static page generators and caching on Wordpress. Personally I think that Octopress is ideal for my personal blog or a blog maintained by several developers which is where version control becomes essential. Although it is not practical for other user like a normal blogger who just wants to write content, a copy-editor or a business looking for more advance features such as LOREMIPSUM.


  
Tips, Tweaks & Adjustments
==========================

...



Faster Page Generation Using Isolation
--------------------------------------

If you have a lot post in `source/_posts` then it could take a long time to compile your posts every time you update you blog.  

If you are only working on 1 post at a time then it would make sense to only compile that page, to do this use `rake isolate[post_name]`.  
This will isolate the post your are working on then automatically move all other posts to `source/_stash`.  

When you are ready to publish your site, use `rake integrate`, which will move all posts from `source/_stash` and move them back to `source/_posts`.



Custom 404 Page
---------------

[This is taken from a tutorial I have written in the past.](/blog/2013/06/21/octopress-custom-404-page/)  

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



Related Posts
-------------

There is a related post plugin already included in Jekyll, to use it  open up the `_layouts/posts.html` template and edit it  
{% codeblock lang:html %}
<a>CODE...</a>
{% endcodeblock %}
To enable this function open up `_config.yml` and set `lsi: true`  
The lsi option will use a statistical analysis to calculate which pages are most relevant.  
Now you need to install GSL using Ruby Gems.
{% codeblock lang:bash %}
$ gem install gsl
{% endcodeblock %}
After GSL is installed you can regenerated your blog.



Category List / Cloud Tree
--------------------------

Download the files from [Github](https://github.com/tokkonopapa/octopress-tagcloud).  
1. Save `tag_cloud.rb` to `plugins/tag_cloud.rb`  
2. Save `category_list.html` to `source/_includes/custom/asides/category_list.html`  
3. Append to default_asides in `_config.yml`:
   {% codeblock lang:yml %}default_asides: [custom/asides/category_list.html]{% endcodeblock %}
  
This plugin was created by [tokkonopapa](https://github.com/tokkonopapa).



Escaping Special Character In Markdown
--------------------------------------
You can escape special characters in Markdown used in formatting using a `\`.
For a list of characters you can escape take a look at this [article](http://daringfireball.net/projects/markdown/syntax#backslash) from [Daring Fireball](http://daringfireball.net).


  
Popular Posts
-------------

1. Open up your Gemfile and add this: `gem 'octopress-popular-posts'`  
2. Install it using Bundler: `bundle install`  
3. Run the installation command through Bundler: `bundle exec octopress-popular-posts install`  
The popular posts asides will now generate whenever you run: `rake generate`  
4. Open up `config.yml` and add this line:
```
popular_posts_count: 3
```  
Append to default_asides in `_config.yml`: {% codeblock lang:yml %}default_asides: [custom/asides/popular_posts.html]{% endcodeblock %}




Offloading Images to Picasa
---------------------------

At first I hosted all of my images using Octopress, then later switch to Picasa to host my images becuase of the following.

* Images will be delivered from CDN (free)
* Images will cache & resize on the fly.
* You can create custom image sizes.
* Optimize image by selecting image quality.


  
Conclusion
----------

[Jekyll](http://jekyllrb.com/docs/plugins/#available_plugins)
[Octopress](http://github.com/imathis/octopress/wiki/3rd-party-plugins)