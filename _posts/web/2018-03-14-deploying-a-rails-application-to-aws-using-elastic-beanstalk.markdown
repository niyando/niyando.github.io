---
layout: post
title: "Deploying a Rails Application to AWS Using Elastic Beanstalk"
modified:
categories: web
excerpt: "How to push a rails app to AWS using Elastic Beanstalk"
tags: ["rails", "ruby on rails", "deploy","AWS", "Elastic Beanstalk"]
image:
  thumb: rails-ebs.png
comments: true
date: 2018-03-14T19:27:04+05:30
---

Elastic Beanstalk is a tool created by AWS for deploying and scaling web applications in their eco-system. It's a great way to deploy your application and manage its resources the Heroku way on AWS. The eco system comprises of  services such as S3, ElastiCache, RDS, Route 53, SES etc which makes it easy and ideal to manage all resources of your application under one roof.

AWS also provides a [Free Tier](https://aws.amazon.com/free/){:target="_blank"} where it provides the platform and essential services (micro instances) for no price for 12 months. This makes it an attractive option for developers and large/small enterprises to host their projects on AWS.

This post outlines necessary steps to deploy your web application (Rails) to AWS using EBS (Elastic Beanstalk). Assuming an existing rails app using git for version control, let's get started.

1) Download the commandline tool for AWS Elastic Beanstalk

{% highlight bash %}
brew update
brew install aws-elasticbeanstalk
{% endhighlight %}

2) Create a [new account](https://portal.aws.amazon.com/billing/signup){:target="_blank"} on AWS.

3) Create a new IAM user to get the access_id and access_key. Check for IAM option under Services --> Security, Identity and Compliance. This pair will be used to give programmetic access to your AWS account. Make sure that you grant full access to all AWS resources to this user. Download the credentials.

4) Create a file called credentials under .aws folder in Users folder of your machine. Create a new .aws directory if it doesn't exist. For me the path is '/Users/Nirav/.aws'. Copy/paste the credentials in this file. The format should be as follows

{% highlight bash %}
[profile-name-1]
aws_access_key_id = loremipsom
aws_secret_access_key = loremipsomblabla
[profile-name-2]
aws_access_key_id = lolololol
aws_secret_access_key = blablabla
[default]
aws_access_key_id = defaultlolol
aws_secret_access_key = defaultkeykey
{% endhighlight %}

This is a great way to manage credentials for different AWS accounts. It's really helpful when you are working on multiple projects with multiple clients using different AWS accounts.

5) Go to the root of your application/project directory and do

{% highlight bash %}
eb init --profile=profilename
{% endhighlight %}

This tell Elastic Beanstalk to use a particular profile from the credentials file for this project.

6) Next you will need to go through the setup by answering few questions such as ...

- Name of your app
- Region where it should be hosted. (pick the one where your audience is)
- Confirm the programming language and its version.
- Set up SSH instance (will be required to ssh into your instance. Always create a new key-pair if you are working with different clients)

7) Time to create your environment.

{% highlight bash %}
eb create production (or staging or anything you want)
{% endhighlight %}

This will setup everything needed for your app (load balancer, EC2, etc) and will take some time to finish the magic.

8) Next step is rails specific to set your environment variable SECRET_KEY_BASE

{% highlight bash %}
eb setenv SECRET_KEY_BASE=$(rake secret)
{% endhighlight %}

That's about it!

To check the status of your application, do

{% highlight bash %}
eb status
{% endhighlight %}

To open your application in web browser, do

{% highlight bash %}
eb open
{% endhighlight %}

We still need to add data tier to the application (no good application is good without a database).

### Adding Database to your application

1) Go to your AWS console dashboard Services --> Compute --> Elastic Beanstalk
Select your application --> selecy configuration from the sidebar --> Add Data tier

2) Select the instance type (MySQL, Postgres, etc), add username-password for your DB and let the AWS create database instance for you. This will take sometime. This auto adds DB related ENV variables to your instance.

3) Next you need to add these ENV vars in your database.yml file

{% highlight yaml %}
production:
    <<: *default
    database: <%= ENV['RDS_DB_NAME'] %>
    username: <%= ENV['RDS_USERNAME'] %>
    password: <%= ENV['RDS_PASSWORD'] %>
    host: <%= ENV['RDS_HOSTNAME'] %>
    port: <%= ENV['RDS_PORT'] %>
{% endhighlight %}

Commit the changes and do

{% highlight bash %}
eb deploy production
{% endhighlight %}

And you're <b>LIVE</b>!

<div style="width:100%;height:0;padding-bottom:56%;position:relative;"><iframe src="https://giphy.com/embed/2WGYAGbzkemi5BMdvb" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/heavy-elon-musk-2WGYAGbzkemi5BMdvb">via GIPHY</a></p>

### Bonus Info:

- Check you application logs using 'eb logs'
- Dont .gitignore your .elasticbeanstalk folder. Commit it. Other folks in your team will also be able to use the config to deploy the app.
- Use micro instances for all services to get the free tier advantage. Scale the services when you need to.
- AWS doesn't charge anything for Elastic Beanstalk. They just charge you for the resources you're using.
- To ssh into root directory of your application, do 'eb ssh' and 'cd /var/app/current'.
- eb deploy auto runs your rails migrations.

Did it help you? Share!
Questions? Pls Comment

Cheers!



