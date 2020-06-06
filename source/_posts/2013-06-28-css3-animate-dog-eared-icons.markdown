---
layout: post
title: "CSS3: Animate Dog Eared Icons"
date: 2013-06-28 17:59
comments: true
categories:  [CSS3]
---

TLDR: [View the demo here](http://jsfiddle.net/84115/b3nkd/light/)

###The HTML
The html for this is  fairly simple. We are going to create a few divs and wrap them inside links.
{% codeblock lang:html %}
<a href="#"><div class="file orange">&lt;&#47;&gt;</div></a>
<a href="#"><div class="file blue">js</div></a>
<a href="#"><div class="file red">rb</div></a>
<a href="#"><div class="file yellow">py</div></a>
<a href="#"><div class="file purple">&lt;?&gt;</div></a>
{% endcodeblock %}

###Creating The Icon
As we are using a link for the HTML we need the CSS to do all the work.

First start off by creating the style for the `file` classes. 
This will create a rectangular box with text in the center.
{% codeblock lang:css %}
.file{
  background: #b0b0b0;
  border-radius: 5px;
  color: #fff;
  display: inline-block;
  font-family: Helvetica, Arial, sans-serif;
  font-size: 64px;
  font-weight: bold;
  height: 150px;
  margin: 10px;
  padding: 20px;
  line-height: 160px;
  text-align: center;
  width: 100px;
}
{% endcodeblock %}

###Creating The Dog Ear
Ok, nothing to fancy so far but we still need to create the dog ear for the top right of the icon. To do this we will be using borders to create a triangle which floats above the `div` using `absolute` and `relative` positioning.

One unusual thing about borders is that the browser draws them at an angle. One side of the border is colored for the colour of it's sides (top, right, bottom, left), and the rest are left transparent. You can set the border width to a higher value e.g. 32px.
{% codeblock lang:css %}
.border-effect{
  border-color:  red green blue orange;
  border-style: solid;
  border-width: 32px;
  width: 0;
  height: 0;
}
{% endcodeblock %}
And it will output a shape like this.
<span style="
  display:inline-block;
  border-color:  red green blue orange;
  border-style: solid;
  border-width: 32px;
  width: 0;
  height: 0;
"></span>

Using this technique the left and bottom borders can be set to `#888` & the top and right to `#fff`.

Here we have our shape now all we need to do is place it in the corner of our icon.
<span style="
  display:inline-block;
  border-color: #fff #fff #888 #888;
  border-style: solid;
  border-width: 32px;
  width: 0;
  height: 0;
"></span>
{% codeblock lang:css %}
.dog-ear{
  border-color: #888 #fff;
  border-style: solid;
  border-width: 0 32px 32px 0;
  content: "";
  right: 0;
  top: 0;
}
{% endcodeblock %}
If you want to learn more about border shapes take a look at this [post](http://jonrohan.me/guide/css/creating-triangles-in-css/) by [Jon Rohan](http://jonrohan.me).

<!-- more -->

###Moving The Dog Ear
First lets rename our `.dog-ear` to `.file:before`. This will make the element appear directly before the and element with the class `.file`.
Now we have our dog ear awkwardly sitting inside of our icon before our text; nobody wants that.
To fix this we will make `.file-before` use the `absolute` positioning property. The element positioned relative to its first positioned parent element.
{% codeblock lang:css %}
.file:before{
  position: absolute;
  right: 0;
  top: 0;
}
{% endcodeblock %}
Well now there in the corner of the page, you can fix this by setting `.file`'s position to relative.
{% codeblock lang:css %}
.file{
  position: relative;
}
{% endcodeblock %}

###Adding Some Colour
At the moment all of the elements look a bit drab; add some colour.
{% codeblock lang:css %}
.orange{background:#ff7461;}.orange:before{border-color:#c65b4b #fff;}
.red{background:#ff6161;}.red:before{border-color:#c64b4b #fff;}
.yellow{background:#f4cf77;}.yellow:before{border-color:#cdad60 #fff;}
.blue{background:#8cc8ec;}.blue:before{border-color:#75a8c3 #fff;}
.purple{background:#b094de;font-size: 50px;}.purple:before{border-color:#8a75ad #fff;}
{% endcodeblock %}

###Animating The Icons
Now we have our icons finished, but they don't do anything when you interact with them.
We can use CSS3 transitions and transform to animate the shape when the user hover and clicks it.

{% codeblock lang:css %}
/* This will reset the dog ear to make it look like it has unfolded. */
.file:hover:before{
  border-width: 0;
}

/* This zoom into the icon when you hover over it. */
.file:hover{
  transform: scale(1.05);
}

/* This zoom out the icon when you hover over it, then click. */
.file:hover:active{
  transform: scale(0.95);
}

{% endcodeblock %}

Now for the finishing touch: transitions.

{% codeblock lang:css %}
/* Don't forget to specify which properties you want to animate and avoid using "all" */

.file:before{
  transition: border 0.2s;
}

.file:hover{
  transition: transform 0.2s;
}
{% endcodeblock %}

Now you animations will smoothly transition between one state to another.

###The Final Product
View the demo of what we have just created.
{% jsfiddle b3nkd result,html,css %}