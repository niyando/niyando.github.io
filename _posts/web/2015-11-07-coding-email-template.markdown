---
layout: post
title: "Coding Email Template"
modified:
categories: web
excerpt: "Best practices on designing and testing email templates."
tags: ["email","template","rails","css","design"]
image:
  thumb: coding-emails.html
date: 2015-11-07T16:15:46+05:30
comments: true
---


Email is an important medium of communication with your customer. Right from `signing up` to `placing the first order` to `sending recommended products`, all communications happen through email. So it is imperative to get it right.

Coding an email template is a complex thing. Here are the key problems that I as a developer face writing email templates.

###Its 2015 and I still use <pre>&lt;</pre>tables<pre>&gt;</pre> to build my email templates.

 Email templates are not standard web pages that can be viewed on browsers. They are served by and under email clients. All email clients have a rendering engine that is responsible for displaying emails. There are no standard rules around such rendering engines. To some, doing x is bad and to some doing y is bad.

 To give an example, Gmail's rendering engine supports `<div>` tag in your template but there are good chances that other clients such as outlook, lotus, etc would not. There is a long list of css attributes that are not supported across mail clients. You cannot easily ignore the possibilities of your customer viewing your emails on such clients. Here is a quick [report]( https://emailclientmarketshare.com/){:target="_blank"} from litmus labs that shows marketshare of major email clients.

So yes, you need to write html the way people did in 90's.

<strong>Use in-line CSS:</strong> Styles aren't always supported.  
<strong>Use table layouts:</strong> Div layouts are css dependent and many of the email clients can't cope.  
<strong>Don't use rowspan:</strong> This causes weird spacing issues.  
<strong>Don't use background images:</strong> Support for these is limited.  
<strong>Style image tags with "display:block":</strong> Else weird spacing issues.  
<strong>If using multiple tables nest them in one parent table:</strong> This stops more weird spacing issues.  
<strong>Don't use Javascript:</strong> Again not supported very well.  
<strong>Make sure your email is legible with no images:</strong> The user may not load them.  

Writing code to support IE6 suddenly seems alright.

<iframe src="//giphy.com/embed/HteV6g0QTNxp6" width="480" height="267" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/HteV6g0QTNxp6">via GIPHY</a></p>

###Responsive Emails. No Bootstrap.

Your customers check your emails on their desktops, phones, tablets and these days (pls god kill me) on their wrist watches. Imagine someone pinching on their watch to zoom the email content to read it.

Bootstrap was designed keeping web browsers in mind. If you really want to use it in your email template, you will have to restructure everything in `<table>`. Even after doing this, most of the bootstrap goodness is still not possible to achieve in webmail clients (gmail,yahoomail,hotmail etc).

###External style sheets

Can you use external style sheets?  
Yes

Should you use external style sheets?  
No

<iframe src="//giphy.com/embed/IwmztXQO7BUzu" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/cmon-come-on-IwmztXQO7BUzu">via GIPHY</a></p>

In most cases, customer's email client will block the stylesheet being loaded from the remote server. Spam filters don't like this behavior of calling the home server for additional resources. Your email won't appear as you designed. Some clients even strip the style in the head. So, you're put further behind 90's to write inline css. Imagine writing same style for similar elements. This is a rather tedious and hard to maintain approach.

<hr>

###Few tips and tricks to make the whole process easy.



###Get started with [Ink](http://foundation.zurb.com/emails.html){:target="_blank"} (now Foundation) framework.

 Seriously. You asked for responsiveness to your content and they do it right across all the mail clients. Consider it as a bootstrap alternative for emails. They provide a similar grid concept that makes it very flexible to code your html to support device specific design. It also encourages developers to write standard html code that is supported across all email clients. Explore more goodies under the [documentation](http://zurb.com/ink/docs.php){:target="_blank"}.

 You could also use my boilerplate [gist](https://gist.github.com/niyando/f24e3920c8815d799609){:target="_blank"} to get started quickly.

###Use Premailer

If your backend is based on Ruby on Rails, [premailer-rails](https://github.com/fphilipe/premailer-rails){:target="_blank"} is an excellent drop in solution to style your emails. It offloads all the hard work of writing inline styles. Before you send an email, it reads all the style of external and internal stylesheet and adds them to matching html elements (inline) in your email body. This allows you to keep HTML and CSS in separate files, just as you're used to from web development, thus keeping your sanity.

Doesn't that sound like a huge relief?

<iframe src="//giphy.com/embed/uc5KhUtOiS0zm" width="480" height="357" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/pokemon-pikachu-sigh-uc5KhUtOiS0zm">via GIPHY</a></p>

###Bonus Trick

Testing emails is also not easy. If its vanilla html, you can just test it on your browser. But once you integrate it with rails, it becomes erb. The only way to test the output would be to send mail using some service (mandrill, sailthru etc) which you also use on production and then check the actual copy under email client. Of course, this is the best way to test but it doesn't seem very sensible when you are still developing the template. You would end up sending 10 emails to test 10 changes.

Use [mailcatcher](https://github.com/sj26/mailcatcher){:target="_blank"} gem. It acts as a simple smtp server that catches all messages sent to it, stores them and displays on a web interface. It proves to be very handy during development phase.

##Conclusion

Coding Emails: Love it or hate it. You cannot ignore it!  
I hope these tips prove helpful to you.

Keep spamming :P !