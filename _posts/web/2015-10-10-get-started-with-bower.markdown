---
layout: post
title: "Get Started With Bower"
modified:
categories: web
excerpt: "Essentials to get started with Bower"
tags: ['bower','dependencies','frontend','package']
image:
  thumb: bower-logo.png
date: 2015-10-10T15:04:43+05:30
---

Get Started with Bower

We use lot of thirdy party libraries while building a project. It makes sense not to reinvent the wheel and integrate industry accepted standard solutions. Given the active community behind them, they are less prone to bugs.

We use gems in ruby projects, modules in node projects and frontend libraries (jquery, bootstrap, angular, etc) in client side projects.

Imagine you have a big frontend project where you use lot of such libraries. Some of these libraries are not standalone and they require one to be loaded before loading itself to function as expected. For instance, jquery needs to be loaded if you want to use bootstrap javascript plugins. And sometimes such libraries are tightly dependent on versions. eg bootstrap x.x might not work with jquery y.y. It becomes a nightmare to update libraries when you want to update a particular library which is tightly dependent on other libraries.

##Bower to rescue

Bower makes it a cakewalk to manage your frontend dependencies. You can relate it to bundler if you have worked with ruby frameworks. You can list all your gems in one file and `bundle install` will install all such gems required in your project. You do not need to check-in source code of such libraries to your version control. Your colleagues can use to same gemfile to setup the project with exact defined dependencies.

Bower works in a similar fashion. You can search for libraries, install them locally and register new libraries for others to use using Bower.

##Installing Bower

From the docs ...

Bower is a command line utility. Install it with npm.
{% highlight bash %}
$ npm install -g bower
{% endhighlight %}
Bower requires node, npm and git.

##Lets go through some basic Bower commands

{% highlight bash %}
$ bower search jquery
{% endhighlight %}

will list all bower registries based on your keyword 'jquery'.

{% highlight bash %}
$ bower install jquery-placeholder
{% endhighlight %}

will create a folder called bower_components in your project and install jquery-placeholder library inside it. You will also notice jquery installed in this folder as jquery-placeholder depends on it.

Its ok to git ignore this folder bower_components. This practice is subjective and it has its pros and cons. Check this stackoverflow [question](http://stackoverflow.com/questions/22327758/should-bower-components-be-gitignored){:target="_blank"} to understand more about it.

You will still need to reference (<script>) the file in your project to use the library.

{% highlight bash %}
$ bower init
{% endhighlight %}

will interactively create a file called bower.json. It will collect all the information about your project and its dependencies.

{% highlight js %}
// bower.json
{
  name: 'sideproject',
  version: '0.0.0',
  authors: [
    'Nirav <niyando@gmail.com>'
  ],
  description: 'my weekend project to build bla bla',
  license: 'MIT',
  ignore: [
    '**/.*',
    'node_modules',
    'bower_components',
    'test',
    'tests'
  ],
  dependencies: {
    'jquery-placeholder': '~2.1.3'
  }
}
{% endhighlight %}

Lets just focus on dependencies for now. Other fields are only used when you wish to register your library on bower for distribution. Dependecies object contains information about currently installed libraries and their versions.

Now you can install new dependencies with flag --save to update the bower file. Bower is smart enough to download the version that works with your existing dependencies. Incase of a conflict or multiple options, it will prompt you with available options.

{% highlight bash %}
$ bower uninstall package_name
{% endhighlight %}

will uninstall package and delete required files from bower_components. Use flag --save to update the bower file.

{% highlight bash %}
$ bower list
{% endhighlight %}

will show your dependencies. Bower also does a check to see if a newer version of each of the packages is available.

{% highlight bash %}
$ bower update package_name
{% endhighlight %}

as it says, will update the package. It will take care of prompting the conflicts if any.

##Creating Bower Package

Lets say that you did not find any bower package that can generates a carousel that slides cats pictures vertically and you ended up writing your own solution. You believe in giving back to the community and decide to distribute your library using bower.

Here is how you should do it.

First thing ..you cannot distribute your package without hosting it at some git endpoint (github, bitbucker, your own gitlab). You will need to point your bower package to a remote repository where it can consume contents of your package.

We will use Github for our reference. Push your library on Github

Now you need to configure your project. Its as simple as adding a bower.json file. Minimally, you need following

{% highlight js %}
{
    "name": "vertical-cat-slider",
    "version": "0.0.1"
}
{% endhighlight %}

Names of bower packages are unique and it will show conflict when you try to register your package using an existing package name.

Next is version. Git tags are your package versions. While consuming your package, consumers will be able to download/update the source as per tags available. It helps you in managing your releases.

So before registering your package its important to tag your repo with a version number and push it using --tags flag

{% highlight bash %}
$ git tag 0.0.1
$ git push --tags
{% endhighlight %}


Its also a good idea to ignore some files in your source as it wont be really useful to your end user and they would be endup downloading the entire project. For eg, test suites, development dependencies etc. To ignore such things, you can use ignore key in bower.json

{% highlight js %}
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ]
{% endhighlight %}

Push your updated bower.json to remote and you are ready to register your package.

{% highlight bash %}
$ bower register vertical-cat-slider git://github.com/flying_cat.git
{% endhighlight %}


to confirm it got registered, you can check
{% highlight bash %}
$ bower info vertical-cat-slider
{% endhighlight %}

##Upating your package

After a few days, you realized that you missed a rare case where your slider breaks. You know the fix and you have already pushed it to your git remote. Now you want to update your package. Here is how you should do.

{% highlight bash %}
$ bower version [<newversion> | major | minor | patch]
{% endhighlight %}


from the docs,
>The newversion argument should be a valid semver string, or a valid second argument to semver.inc (one of “build”, “patch”, “minor”, or “major”). In the second case, the existing version will be incremented by 1 in the specified field. If run in a git repo, it will also create a version commit and tag, and fail if the repo is not clean.

You can read more about semver string [here](http://developer.telerik.com/featured/mystical-magical-semver-ranges-used-npm-bower/){:target="_blank"}

For our purpose, we will do
{% highlight bash %}
$ bower version patch -m "Fixes rare case when lazy cat image shows up"
{% endhighlight %}

This should increment your version to 0.0.2 in your bower file. Push the bower file to remote with flag --tags and you are done. The new version is available for your users to update.

This should be enough to get you started. There is more to bower and you can learn all about it on the [official docs](http://bower.io/docs/api/){:target="_blank"}.
Until next time, cya!
