---
layout: post
title: "How should you plan your career in web development?"
modified:
categories: web
excerpt: "Brief guide to kick start your career in web development."
tags: ['web','development','career']
comments: true
image:
  feature: web-dev.jpg
date: 2015-07-18T11:52:05+05:30
---

**Note:** I have originally written this as an [answer](http://qr.ae/xAAvv) to a question on Quora. I am also posting it here to increase the visibility and reach wider audience.
{: .notice}

Three years back when I decided to plunge into web development industry, I had no definite guide or roadmap to go about it. There was a lot of information available on web technologies and tutorials, but I always felt it hard to figure out the logical flow to learn things. "Should I start with HTML or PHP?","Should I learn legacy PHP or some framework", "Do I really need to learn jQuery after Javascript?",.. etc questions popped up often and resisted my natural flow of learning.

What I always wanted was an optimal path to learning. Step by step approach to enter this industry. This post is intended for audience who wants to kick start their career in web development. I have broken down the learning curve into logical flow of tasks. You can treat this as a **todo** list. You learn by doing.

Vital thing is to get your hands dirty and code (I assume that you know the bare minimum of programming). So let's get started.


Take one step at a time, get it right and move ahead.

1) Learn to build some static web pages.
Jump over to [w3schools](http://www.w3schools.com/){:target="_blank"} and learn some basic HTML. Acquaint yourself with elements like head, body, div, p, span, form etc and get the overall idea of the markup structure. Once you're comfortable with building something like [Motherfucking](http://motherfuckingwebsite.com/){:target="_blank"} Website, jump to the next step.

2) Learn beautifying your HTML page using CSS.
Learn some basic CSS. Know the difference between id and class. Know how to select your HTML content & apply styling to it. Learn about different layouts. Try writing inline css; Try an internal stylesheet; Try writing an external stylesheet and include it in your HTML head. Learn why should you put it in head. When you can build something like this [template](https://css-tricks.com/examples/SuperSimpleTwoColumn/){:target="_blank"} , take a step further.

3) At this point, you should be pretty bored fiddling around your static web page. You've already learnt how to build a form in HTML but have no friggin idea what happens to it when you submit it.

#### Welcome to Server Side Scripting.

a) Just to kick start and understand things, start with legacy PHP. You can move to a disciplined MVC framework like Laravel or Ruby on Rails once you get a hang of how web works. Download and install XAMPP. This will setup everything (install php, mysql database and start a apache server locally on your system) for you to get going. Meet your new friend-forever, localhost aka 127.0.0.1

Learn basics of server side coding.
To start with, [learn how the web works](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/How_the_Web_works){:target="_blank"}.
Start serving your static HTML page. Once set, go dynamic.
Know how to send data through url query string/parameters and forms. Know the difference between a GET and a POST request. Know how to collect these data on your local server. Use these data to build response and render result. Start embedding some PHP in your HTML and make the response dynamic. Collect first_name and last_name in a form and show full name on submission. Incorporate some Template Engine (eg Mustache, Smarty) to separate view from your php logic.

b) Integrate MySQL with your application.
You will need to persist data at some point in your application.
Learn MySQL Database (tables, queries, relations, RDBMS concepts). Integrate it with your app locally and start storing/getting data.

c) HTTP is stateless. Each request is an independent entity. So it becomes important to attach some information about your user to each requests. John Doe should only see messages sent to him in his inbox. Learn about sessions and cookies. Know the difference between them. Design a login/signup flow and start registering users in your MySQL table.

4) At this stage, you have built a fully functional application that pretty much does some [CRUD](https://www.wikiwand.com/en/Create,_read,_update_and_delete){:target="_blank"}. Now what about the aesthetic appeal? Keep in mind, we live in web 2.0 where the world goes gaga over Seamless UI Apps.

#### Say hello to frontend scripting i.e Javascript

a) Learn the basics. Its just another programming language. Once you get the syntax right, dive more into learning advance things such as events, objects, listeners, callbacks, dom, cookies and browser.<br>
b) Learn jQuery. Its not a replacement for Javascript but a library built on top of it. It will help you achieve things done in javascript elegantly. Learn how to traverse dom, use selectors and most importantly, making asynchronous requests (ajax).<br>
c) Incorporate above concepts into your app. Start validating form fields client side. Try to animate something on hover. Try submitting your forms asynchronously using jquery ajax and show a success/error message.

5) Move over to industry standards. Pick and learn a server side MVC framework  thoroughly (Rails, Laravel, CakePHP, Django).

Start using Bootstrap for building your app's frontend. It takes care of most of the things (styling, layouts, validations, customisations, responsiveness etc) as per accepted industry standards.

6) Learn a version control system (svn/git). It tracks and provides control over changes to the source code. Something very important when you're working in a team. Create a repository on Github and push your work there. Invite your friend to collaborate with you on this cool project.

If you've nailed everything so far, you can proudly update your Linkedin position as a Web Developer.

But, this is not everything. It is just enough to put you on the right track. Web industry is wide and limitless. Everyday, there are new kids in the block that turn things upside down. The key is to keep yourself updated on best practises and tools used. As Jeffrey Way rightly said [This Damn Industry](http://code.tutsplus.com/articles/this-damn-industry--net-17054){:target="_blank"} demands constant learning.

>We're all in this together; We all feel behind the pack. But, then again, we stay the course because we love this damn industry.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">What I love about my job and this industry? I struggled all day…but conquered 90% of the problem. Tomorrow…we go again. <a href="https://twitter.com/hashtag/bringit?src=hash">#bringit</a></p>&mdash; David Walsh (@davidwalshblog) <a href="https://twitter.com/davidwalshblog/status/621882063403159553">July 17, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<br>
Subscribe yourself to some good blogs like [Tuts+](http://code.tutsplus.com/){:target="_blank"}. Make the most of online education presence such as [Codeacademy](http://www.codecademy.com/){:target="_blank"}, [Khan Academy](https://www.khanacademy.org/){:target="_blank"}, [Code School](https://www.codeschool.com/){:target="_blank"},  [Udemy](https://www.udemy.com/){:target="_blank"} etc. Follow industry veterans on Twitter and read their blogs. Attend tech conferences and build solid networks.

Yes, the journey is long.<br>But, you will enjoy every bit of it.

Bon Voyage.