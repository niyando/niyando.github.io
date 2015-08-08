---
layout: post
title: "All About AJAX"
modified:
categories: web
excerpt: "Understanding AJAX and its evolution. "
tags: ['web','ajax','asynchonous','jsonp','cors','javascript']
image:
  feature:
  thumb: ajax.png
comments: true
date: 2015-08-08T12:12:29+05:30
---

>Ajax (also AJAX; /ˈeɪdʒæks/; short for asynchronous JavaScript and XML) is a group of interrelated Web development techniques used on the client-side to create asynchronous Web applications.

##Excuse me?

Open up twitter on your browser, compose a 140 chars message and tweet it. Did you notice how it tweets your message without reloading the whole page? Did you ever notice same happening when you comment on your friend's facebook post? Welcome to the asynchronous web.

Technically, AJAX makes it possible to initiate a http request (get/post/put etc) as a background process without halting/blocking the current execution of js code. Its the backbone of single page applications.

##Synchronous vs asynchronous execution

When you execute something synchronously, you wait for it to finish before moving on to another task. When you execute something asynchronously, you can move on to another task before it finishes.

For a real life example, lets say you call up your friend to check if he has your book.

<b>you:</b> hey dude! did i by any chance forgot my book at your place?<br>
<b>friend:</b> I am not sure. Could you please hold the phone while I check it.<br>
(after couple of minutes)<br>
<b>friend:</b> Hey, sorry to keep you on hold but I couldn't find the book here.May be you should check with John Doe. I saw it last there.<br>
<b>you:</b> ok thnx. will check with him.<br>
{: .notice}

<b>you:</b> hey dude! did i by any chance forgot my book at your place?<br>
<b>friend:</b> I am not sure. Let me find it and I'll call u back?<br>
<b>you:</b> cool<br>
(now you're free to find the book somewhere else / do something else)<br>
(after couple of minutes)<br>
<b>friend:</b> I couldn't find the book here. May be you should check with John Doe. I saw it last there.<br>
<b>you:</b> Never mind. I found it in my garage. Thanks dude!<br>
{: .notice}

##Asynchronous Javascript

If you've done some scripting in javascript, you should have come across popular js function called setTimeout. Using this you can do something after a specified delay. Fire up your browser's dev console and copy paste following code.

{% highlight js %}
function consoleOne(){
  console.log(1);
}

setTimeout(consoleOne, 1000);

console.log('I aint blocked');

console.log(2);
{% endhighlight %}

If you noticed the order of logs, you will see that our setTimeout doesn't block the further execution of our code. It demands a function name that it invokes once its done waiting for the specified delay.

AJAX works on a similar principle. You can initiate a background http request providing a function (callback function) as an argument and this function will be invoked once the server responds with the data (XML/JSON/HTML/txt/etc). You can have access to resultant data inside the scope of this callback function.

Deep down javascript, it works under Event Loop. JS has a message queue that stores list of things to be processed and their respective callback functions to be invoked. When a message is encountered and processed, the associated callback function is called. You can learn more about [Event Loop Explained](http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/){:target="_blank"}

##AJAX before jQuery

{% highlight js %}
function loadSomeInfo(){
  var xmlhttp;
  if (window.XMLHttpRequest)
    {// code for IE7+, Firefox, Chrome, Opera, Safari
      xmlhttp=new XMLHttpRequest();
    }
  else
    {// code for IE6, IE5
      xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
  xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
      document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
  }
  xmlhttp.open("GET","some_info.txt",true);
  xmlhttp.send();
}
{% endhighlight %}

<figure>
  <img width="310px" src="/images/ajax1.jpg">
</figure>

I know. Not very pretty.

##AJAX post jQuery

{% highlight js %}
$.get('some_info.txt', function(response){
  // do something with response
});
{% endhighlight %}

<figure>
  <img src="/images/ajax2.gif">
</figure>

Apart from a great DOM manipulation api, jQuery brought in few nifty ajax apis (ajax, get, post, load, getJson). At its root, jQuery.ajax() rules it all.

{% highlight js %}
$.ajax({
  dataType: "json",
  url: url,
  data: data,
  success: success
});
{% endhighlight %}

Ajaxing became breeze and people started hammering it everywhere applicable. Learn more about deferred objects, promises, callbacks to master the art of writing asynchronous javascript.

##Same Origin Restriction

Go to www.stackoverflow.com, open up your browser console and run following command

{% highlight js %}
$.get('http://www.google.com/')
{% endhighlight %}

It doesn't work. Why?

>Cross-domain AJAX requests are forbidden by default because of their ability to perform advanced requests (POST, PUT, DELETE and other types of HTTP requests, along with specifying custom HTTP headers) that introduce many cross-site scripting security issues.

Domain A can only request some information hosted on Domain A's server.

##Why is Cross Domain AJAX a bad idea?

Lets say that you're logged into your facebook and checking your news feed. Now whenever you browse through fb in logged in state, browser sends related cookies to fb's server to verify your authenticity.

Now lets say that you get a random email with subject "Hi, Wanna have fun tonight? Click here". You click the link and it takes you to some fishy page monkeydonkey.com. It has nothing but some flashy content and images.

If Cross Domain AJAX were allowed, this monkeydonkey site could make a AJAX request to facebook.com to post a status message "I am an idiot" or post some random marketing content or send spam messages to your friends. Since you are already logged in some other tab, http request made my monkeydonkey will carry all required information in the headers to allow such actions. Same can also be applied to your net banking websites where consequences could be more serious.

Yes, you can avoid this at server level using CSRF tokens and other checks. But it sounds best to prevent it at the browser level.


##Solutions to achieve Cross Domain AJAX

Lets say that you have two applications running on domain.com and anotherdomain.com  respectively. You want both of these applications to communicate with each other to share common data. Given that you're owner of both the domains, you can do cross domain AJAX using following solutions

###1) JSONP

More than a solution, its a caveat and used as a hack by developers to achieve cross domain ajax. If you read through, [CORS wiki page](https://www.wikiwand.com/en/Cross-origin_resource_sharing){:target="_blank"} you will find that there are few resources such as stylesheets, scripts, images, fonts etc that are exempted from same origin policy. What it means that you can load all these resources from random thirdparty domains without any restrictions.

JSONP (JSON with padding) is a way to get around the same origin policy in browsers and access resources on another domain. JSONP does this by injecting a script tag into the dom, since the script tag is not restricted by the same origin policy.

So you can just do

{% highlight js %}
$.getJSON("http://www.anotherdomain.com/?callback=?", function(data){
  console.log(data);
});
{% endhighlight %}

Using getJSON with 'callback=?' will create a javascript tag and insert it into the dom:

{% highlight js %}
<script type='text/javascript' src='http://www.anotherdomain.com/?callback=callback1234'>
{% endhighlight %}

and on rails server

{% highlight ruby %}
class SomeController < ApplicationController
  def some_action
    data = {:name => "Apple iPhone 6", :price => "$900"}
    render :json => data, :callback => params[:callback]
  end
end
{% endhighlight %}

which yould generate a response as follows

{% highlight js %}
<script type='text/javascript' src='http://www.anotherdomain.com/?callback=callback1234'>
  callback1234({'name':'Apple iPhone 6', 'price':'$900'});
</script>
{% endhighlight %}

When this script evaluates it results in the the JSONP script tag being removed from the dom and our getJSON callback being called with the data.

JSONP is a pretty neat approach to expose your services client side across domains. However given the use of the script tag it does present a non trivial security vulnerability for the site using it.

###2) Custom Proxy

<figure>
  <img src="/images/proxy.png">
</figure>

Lets say that you are developing js plugins for my website. You need some data from accu weather's api. But you can't make a cross domain ajax to accu whether. I can provide you a endpoint where you can ask me to request accu whether on your behalf and give you the data.

So my website's endpoint will behave as a proxy for you.

You can setup your own proxy server to make such requests.

###3) CORS

>Cross-Origin Resource Sharing (CORS) is a W3C spec that allows cross-domain communication from the browser. By building on top of the XMLHttpRequest object, CORS allows developers to work with the same idioms as same-domain requests.

CORS support requires coordination between both the server and client. You need to allow origins and resources at server level before you could make such requests. Its not supported by old browsers. For the complete list you can refer [CanIUse](http://caniuse.com/#search=cors){:target="_blank"}

CORS is much more friendly to the client and easier to implement. Can be somewhat tricky to impelment on your server-side depending on what technology you're using. But, its a more modern approach than JSONP.

That is it folks. Hope you guys had good time reading this post. If you have any questions/concerns/correction, shoot in the comments. Would be happy to help.

Happy Ajaxing!