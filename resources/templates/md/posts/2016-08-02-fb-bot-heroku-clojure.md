{:title "A Facebook messenger bot in Clojure on Heroku -Part I"
 :layout :post
 :page-index 0
 :tags ["FB Messenger bot Clojure+HerokuBot" "FB Chatbot"]
 }


Here's a howto guide on creating and running a bot on the Facebook messenger platform using Clojure on the Heroku PaaS platform. Part inspired from this [post by Abhay](https://abhaykashyap.com/blog/post/tutorial-how-build-facebook-messenger-bot-using-django-ngrok).

The messenger platform requires the use of a HTTPS URL, and the URL needs to be available and accessible to test against. I found that using Heroku solves multiple goals, no need to expose the URL via ngrok+localhost, nor is getting a SSL certicate required since Heroku dynos are available on an HTTPS URI.

## Steps:
### Create a Heroku free tier account 

* Heroku has a free dyno tier of that allows running an application for 550 hours. 
* We need to get a Heroku account and download the Heroku command line toolbelt. 
[![ Heroku ](/img/heroku_bot/heroku.png "Heroku Free dynos")](https://devcenter.heroku.com/articles/free-dyno-hours)

___


### Create a Facebook App and page

I found the terminology a bit confusing at first. 

* The FB App(lication) is what hosts the chatbot within the Facebook platform. The primary task of the App is to provide a chat-like user interface.
* A Webhook (as named by Facebook) is the chat service hosted on an external web service. When a user types a message, the Facebook App "forwards" the message to the web service URI (via an HTTPS POST message with a particular format). The web service can respond to a message by POSTing back to a Facebook  URI.
* The chat web service is a REST-style web service that reads and writes JSON encoded data. 
* Additionally, for the FB App to be "discovered", it needs to be associated with a Facebook page. It appears that a user or a page on Facebook is the primary 'actor' with which other human actors interact. When a user navigates to a page on Facebook and clicks on "Message", Facebook forwards the messages to the App (which has been associated with that page).

___

### Create a Page and verify token

* There are 2 kinds of tokens required:
* A *verify token* is what FB uses to verify that the webhook is valid and authenticated
* A *page token* is used to authenticate the connection between the App and the Page.
* Note that the page tokens are not displayed on Facebook, if you haven't had it stored, you might need to regenerate a new token

[![ Page Token](/img/heroku_bot/pagetoken.png "Page Token")](https://developers.facebook.com/docs/messenger-platform/quickstart)

___


### Clone the repo and deploy it

* The repo is available at [Github](https://github.com/shark8me/heroku-cbot)
* Run `lein test` to verify that tests pass
* Deploy the App from instructions at the link below.

[![ Heroku ](/img/heroku_bot/heroku_clojure.png "Heroku Clojure docs")](https://devcenter.heroku.com/articles/getting-started-with-clojure#deploy-the-app)

___


### Config vars

* The page token and verify token are included in the HTTP responses and need to be accessed by Clojure Application.
* The [Environ library](https://github.com/weavejester/environ) is used to load these tokens. Typically, Environ loads this from a _.lein-env_ file.
* Since Heroku bundles the jar as an uberjar, the tokens need to be packaged into this uberjar to be accessible on the Heroku dyno.
* A cleaner way to set the token's values is to use Heroku's config, which can be set from the command line.

[![ Heroku ](/img/heroku_bot/heroku_config_var.png "Heroku Config var docs")](https://devcenter.heroku.com/articles/getting-started-with-clojure#define-config-vars)


___


Testing the App is described in [Part II](/posts-output/2016-08-06-fb-bot-testing/)
