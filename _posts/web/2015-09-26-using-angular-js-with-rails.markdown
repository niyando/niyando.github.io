---
layout: post
title: "Using Angular Js With Rails"
modified:
categories: web
excerpt: "Building standalone angular app with rails backend."
tags: ['rails','angularjs','yeoman']
image:
  thumb: rails-angular.jpg
comments: true
date: 2015-09-26T16:23:45+05:30
---

Using Angular.js with Rails

>AngularJS (commonly referred to as "Angular") is an open-source web application framework maintained by Google and by a community of individual developers and corporations to address many of the challenges encountered in developing single-page applications.

##Why single page applications (SPA)?

We live in an era where user experience (UX) of your application really matters. It can become your unique selling point when marketing your application to your customers. Everyone loves less page reloads and more fluid experience.

If you've worked with Rails 4.0 or >, you should have come across Turbolinks. This awesome introduction by Rails team gives your rails app a SPA feel. When you navigate through different pages of your apps, instead of reloading the browser, it just changes the content of your page. It also updates the url of your page using HTML5 APIs to make sure that your app remains url friendly and SEO could be factored in. Using your app becomes snappier.

With introduction of Turbolinks 3 in rails 5, we will have partial replacement feature. Developers will have more control on the page elements. From the client side, we will be able to tell Turbolinks what content do we need to change/replace and what we don’t.

##So why should I use anything else to build SPA when I have turbolink in Rails?

Turbolinks as well as AnguluarJS can both be used to make a web application respond faster, in the sense that in response to a user interaction something happens on the web page without reloading and rerendering the whole page.

AngularJS helps you to build a rich client-side application, where you write a lot of JavaScript code that runs on the client machine. This code makes the site interactive to the user. It communicates with the server-side backend, i.e. with the Rails app, using a JSON API.

Turbolinks, on the other hand, helps to to make the site interactive without requiring you to code JavaScript. It allows you to stick to the Ruby/Rails conventions and code run on the server-side and still, "magically", use AJAX to replace, and therefore rerender, only the parts/partials of the page that have changed.

But, as your application grows, it makes more sense to have a client side js framework such as angular js. This framework would just consume raw json data using backend APIs and offload all client-side/view logic from the server. It helps to structure your js code in separate and reusable components (controllers, directives, factories, templates etc).

Also and most important, you could make the most of powerful angular's data bindings. It saves you from writing tedious DOM manipulations and you would just need to focus on data. The views will be updated as you alter the data.

##Integrating Angularjs with your backend (Rails)

While you can integrate an angular app inside your existing rails app repo, it makes more sense to separate rails and angular as standalone apps.

There are many advantages of separating frontend components out of your api service. As we do for ios/android apps, angular client can live on its own as a separate entity. It will be a static website that can be deployed on s3 or any static website host. It just needs to communicate with your api service. You could setup CORS (cross origin resource sharing) to make it possible.

<figure>
  <img width="600px" src="/images/standalone.png">
</figure>

##Some advantages of this workflow

* You could use [rails-api](https://github.com/rails-api/rails-api){:target="_blank"}, which is a subset of rails application. If you are just going to use rails to build apis, it doesn't make sense to have all functionality that a complete rails app provides. Its lightweight, faster and inclined more towards building API first architecture than a MVC architecture. While its a gem right now, it will be a core part of Rails 5 release.

* You could use yeoman angular-generator to generate an angular app and make the most of grunt & bower ecosystem to manage project build (concat,uglify,cdnify etc) and dependencies (angular modules).

* Deployments will become flexible. You won't need to depend on one to push the other.

* If you ever plan to change your backend stack (eg rails to play/revel), you would not need to worry about your client components. You would just need to maintain the API design.

* By splitting the development of client and backend you could distribute the work over two development teams and keep the application as a whole very extensible.

Personally, this approach seems 'less dependency and more productivity' to me.

##Scaffolding Angular App

Yeoman is one of the most popular scaffolding tool for modern apps. It helps in kickstarting a new project with industry accepted standards and conventions. Name a client framework and chances are high that there is already a yeoman generator available for it. It has excellent documentations and includes support for linting, minification, testing etc.

It provides bower to manage project dependencies and grunt/gulp to build, preview & test your projects.

[generator-angular](https://github.com/yeoman/generator-angular){:target="_blank"} is a popular generator to scaffold angular js applications. Get quickly started with it using this [example](http://yeoman.io/codelab/index.html){:target="_blank"} on the official yeoman site.

We will learn more about this approach and angularjs in upcoming posts.
Keep building!