# SendEmail

[![Build Status](https://travis-ci.org/dwyl/sendemail.svg?branch=tidyup)](https://travis-ci.org/dwyl/sendemail)
[![codecov.io](https://codecov.io/github/dwyl/sendemail/coverage.svg?branch=master)](https://codecov.io/github/dwyl/sendemail?branch=master)
[![Code Climate](https://codeclimate.com/github/dwyl/sendemail/badges/gpa.svg)](https://codeclimate.com/github/dwyl/sendemail)
[![Dependency Status](https://david-dm.org/dwyl/sendemail.svg)](https://david-dm.org/dwyl/sendemail)
[![devDependency Status](https://david-dm.org/dwyl/sendemail/dev-status.svg)](https://david-dm.org/dwyl/sendemail#info=devDependencies)

[![Node.js Version](https://img.shields.io/node/v/sendemail.svg?style=flat "Node.js 0.12 and 4.x latest both supported")](http://nodejs.org/download/)
[![npm](https://img.shields.io/npm/v/sendemail.svg)](https://www.npmjs.com/package/sendemail)
[![Join the chat at https://gitter.im/dwyl/chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/dwyl/chat/?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

*Send* ***welcome, verification, password reset, update and reminder emails***
from *any* node.js app.


## Why?

While we would *prefer* for *email* to be *phased out*
and replaced by something *better*, the fact remains
that *most* people still use email as their *primary*
means of (*digital*) communication.

![dilbert-email](http://jointeffortmarketing.com/wp-content/uploads/2013/10/dilbert-email.png)

Given that email is not going away, we need to make
sending email to people as ***simple and reliable*** as possible.

## What?

*Reliably* ***Send*** *beautiful* **email** with ***dependable delivery***.  
When email that *has* to get through as quickly as possible
so everyone can get on with their lives.

### Decisions We Made (*to Get Started as Fast as Possible*)

In *crafting* this module (*for our own use*) we have made a few
of technical decisions (*for pragmatic reasons*):

1. use [***environment variables***](https://github.com/dwyl/learn-environment-variables)
for storing sensitive information (*API Keys*)
and projects-specific configuration (*Template Directory*)
2. use ***[Amazon Web Services Simple Email Service](https://aws.amazon.com/ses/getting-started/) ("AWS SES")***
for *reliably* sending email messages because it has good documentation,
excellent "*deliverability*" and ***no minimum*** spend (10 cents per 1000 emails sent)!
(_there's also a **generous** "[**Free Tier**](https://aws.amazon.com/ses/pricing/)" of **65k emails per month** if you're new to AWS_)
3. use ***Handlebars*** for email template rendering. Handlebars is very
easy to use and allows us to send ***beautiful*** **HTML** emails without
the complexity or learning curve of many other view libraries.

> **Note**: if you prefer to use a different Email Service provider or template/view
library for your project,  
[***please let us know***](https://github.com/dwyl/sendemail/issues)!
We are happy to support alternatives to make this project more
*useful* to other  
people with *specific needs*.

## *How*?

### Checklist (*everything you need to get started in 5 minutes*)
+ [ ] install the `sendemail` module from NPM
+ [ ] Ensure that you have an AWS Account and have downloaded your credentials.
+ [ ] set the required [*environment variable*](https://github.com/dwyl/learn-environment-variables) (*see below*)
+ [ ] If you don't already have a /**templates** directory in your
project create one!
+ [ ] create a pair of email templates in your /**templates** directory
one called `hello.txt` the other `hello.html`
+ [ ] borrow the code for `hello.txt` and `hello.html` from the **/examples/templates** directory of this project!
+ [ ] create a file called `welcome.js` and paste some sample
code in it (see: [/examples/templates/**send-welcome-email.js**]() )

### 1. Install `sendemail` from NPM

```sh
npm install sendemail --save
```

### 2. Set your *Environment Variables*

`sendemail` requires you set ***four*** environment variable to
*securely* store your AWS API Keys.

> We recommend you use [`env2`](https://github.com/dwyl/env2) to load your
Environment Variables from a file so you can easily keep track of which
variables you are using in each environment.

Create a file in the root of your project called `.env` and paste the following:
```sh
export TEMPLATE_DIRECTORY=./examples/templates
export SENDER_EMAIL_ADDRESS=your.aws.verified.email.address@gmail.com
export AWS_REGION=eu-west-1
export AWS_ACCESS_KEY_ID=YOURKEY
export AWS_SECRET_ACCESS_KEY=YOURSUPERSECRET
```

> If you are ***new*** to ***environment variables***, we have a
> ***quick introduction***: https://github.com/dwyl/learn-environment-variables


### 3. Create your *Template(s)*

Create a ***pair*** of templates *simple* `.html` (*pretty design*) *and* `.txt` (*plaintext*) templates to *get started*.

Here's what our ***pair*** of templates look like side-by-side:

![welcome-email-templates-side-by-side](https://cloud.githubusercontent.com/assets/194400/10602078/23d7555c-770e-11e5-983e-4999923a61b2.png)

[ Click the image to expand/zoom ]

>***Question***: Should we create *plaintext* templates (*in addition to html*?)?  
***Quick*** **Answer**: ***Yes***.  
> For ***Expanded Answer***, see: ***Plain Text Templates?*** section in **Notes** (*below*).

If you are stuck, have a look at **/examples/templates/**


### 4. *Send* an Email!

Create a file called `email.js` and paste the following:

```js
var email   = require('sendemail'); // no api key
email.set_template_directory('./relative/path/to/template/directory');

var person = {
  name : "Jenny",
  email: "your.name+test" + Math.random() + "@gmail.com",
  subject:"Welcome to DWYL :)"
}

email('welcome', person, function(error, result){
  console.log(' - - - - - - - - - - - - - - - - - - - - -> email sent: ');
  console.log(result);
  console.log(' - - - - - - - - - - - - - - - - - - - - - - - - - - - -')
})
```

#### Result:

![example dwyl welcome email](https://cloud.githubusercontent.com/assets/194400/10716076/1d89ff82-7b24-11e5-9e24-d5343735b76b.png)

For *full code of working example* see: [/examples/templates/**send-welcome-email.js**](https://github.com/dwyl/sendemail/blob/master/examples/send-welcome-email.js)  
Note: you still need to set your *environment variables*
for the email to be sent.

<br />
___

## *Notes*?

### Which Email Service Provider?

We are *currently* using ***AWS SES*** for ***dwyl***.

If you want to use an alternative mail sender,
e.g: [sendgrid](http://sendgrid.com/)
or [mailgun](https://www.mailgun.com/pricing)  
please ***tell us***: https://github.com/dwyl/sendemail/issues
(*we are* ***always*** *happy to help*)

### Which View/Template Libaray?

For *simplicity* we are using
[***Handlebars***](http://handlebarsjs.com/),
handlebars is ***tried and tested*** and while it does not attempt
to do anything *fancy* ("*VirtualDOM*"), it does allow
you to do sophisticated templates with includes and iterators
and supports compilation so its ***fast***
(*fast enough ... how many millions of emails are you sending per day...?*)  
Yes... [*mustache*](https://github.com/janl/mustache.js/) is "*faster*"
than handlebars ... but in our *experience* having *conditionals*
(*i.e. "logic"*) is ***very useful*** for reducing the *number* of required templates while not (*significantly*) increasing complexity.
we don't think `if` statements in views are a "*crime*" ... ***do you***...?

> If anyone feels *strongly* about switching to an *alternative*
template engine, please raise an issue:
https://github.com/dwyl/sendemail/issues  
(*please give clear reasons, i.e.* ***not*** *"react-ify-licious-heah because its* ***so cool*** ... ")

### *Plain Text*  Templates?

In our *experience*, *while* most *modern* email clients
(Gmail, Apple/iOS Mail, Yahoo! Mail, Outlook)  
have `HTML` email ***enabled by default***,
*often* the people who *prefer* ***text-only***
(e.g: people with Blackberry phones,
***visual/physical impairment*** or *company* email
email systems - *with aggressive filtering*)
are "***higher value***" customers. Also,
possibly *more importantly* (*depending on who is using your product/service*) you can have *technical privacy-concious* people
that ***only*** read `.txt` email to avoid sending
and tracking data ... but, if you're building a tool for
non-technical people, focus on the fact that `.txt` email
is more ***accessible*** and prevents your messages getting
blocked by spam filters.


## Useful Links:

### Background Reading

+ Designing for the inbox:
https://www.campaignmonitor.com/dev-resources/guides/design/  
(*plenty of detail & highly informative*)
+ Who Cares About Plain Text [email]?
https://blog.aweber.com/email-deliverability/who-cares-about-plain-text.htm
+ Can Email Be Responsive? http://alistapart.com/article/can-email-be-responsive
+ Coding your emails (*What’s so hard about HTML emails?*):
https://www.campaignmonitor.com/dev-resources/guides/coding/
+ Best Practices for Plain Text Emails and Why They’re Important:
https://litmus.com/blog/best-practices-for-plain-text-emails-a-look-at-why-theyre-important

### Technical/Implementation Detail

+ Detail: https://aws.amazon.com/ses/details/
+ Getting Started: https://aws.amazon.com/ses/getting-started/
+ Video Tutorial: https://www.youtube.com/watch?v=0NT8KRXRFG8
+ Basic Tutorial: http://timstermatic.github.io/blog/2013/08/14/sending-emails-with-node-dot-js-and-amazon-ses/
+ Testing: http://docs.aws.amazon.com/ses/latest/DeveloperGuide/mailbox-simulator.html

### Stats/Trends

+ Email Client Stats: https://emailclientmarketshare.com/
+ Email Adoption: https://en.wikipedia.org/wiki/HTML_email#Adoption


## Want *Examples*? [![Join the chat at https://gitter.im/dwyl/chat](https://badges.gitter.im/Ask%20For%20More.svg)](https://gitter.im/dwyl/chat/?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

There are *many* situations where you want to send people email.

### *Welcome* Email!

A simple "hello & welcome to our community" email
you send to people when they register to learn more about
your product/service.
see: [/examples/templates/**send-welcome-email.js**](https://github.com/dwyl/sendemail/blob/master/examples/send-welcome-email.js)



### *Verify* Email Address

As part of registering new people for your Hapi app you
will need to *verify* their email addresses to ensure that people
are not signing up with *fake* emails (or *worse* using someone else's email!)

### *Set a New Password*

People forget passwords, we need to help them
set a new password as quickly & securely as possible.

### *Reminder* Email?

Remind people they signed up but have not *used* the product/service?

### *Notification* Email?

Sam liked your post/photo/update ...
social validation that your life has meaning!
