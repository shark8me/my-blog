{:title "A Facebook messenger bot in Clojure on Heroku-part II"
 :layout :post
 :page-index 0
 :tags ["FB Messenger bot Clojure+HerokuBot" "FB Chatbot"]
 }

Here's Part II of the guide to building a Facebook messenger bot using Clojure and Heroku.

In case you missed Part I, [here it is](/posts-output/2016-08-02-fb-bot-heroku-clojure/)

Hopefully you've been able to install and deploy the webhook described in Part I. In the following steps, we'll test the webhook independently and then integrate with the Facebook App and page.

### Test that web hook works for verify token and msg post

* The test needs to verify 2 requests. The first request is to check the verify token. This can be done by hitting the URL of the Heroku dyno with valid values for hub.verify_token parameter. 

`https://yourapp.herokuapp.com/?hub.mode=subscribe&hub.challenge=ssd&hub.verify_token=yourtoken`

* The second test should verify the response to a message from the chat. We can use [Wget](https://www.gnu.org/software/wget/) to send a POST request

`wget --post-file=msg_example https://yourapp.herokuapp.com/`

An example of the contents of the msg_example file :

`{"object":"page","entry":[{"id":"168497013587504","time":1472867455913, "messaging":[{"sender":{"id":"979241748869838"}, "recipient":{"id":"168497013587504"},"timestamp":1472866869009,"message":{"mid":"mid.1472866868779:6e9cadb743206a8c66","seq":10,"text":"hello"}}]}]}`

* Note that the value against the *text* key is the content typed by the user in the chat. 
* This should return a JSON response where the webhook echoes the `hello` back to the user.

___


### Adding the web hook 

* In the App's settings page, add a webhook that points to `https://yourapp.herokuapp.com/`
* Facebook will test the URL and return an error message if it could not get a valid response
* If the webhook is added successfully, we can finally test it live! 

[![Add Webhook](/img/heroku_bot/add_webhook.png "Add webhook)](https://developers.facebook.com/docs/messenger-platform/product-overview/setup#webhook_setup)
___


### Post a message and test if the App responds

* Open the Facebook page you created, and click on the "Message". 
* Type into the messenger window and you should get a response within a few seconds!

___

### Debugging

* The Heroku logs can be checked to see if is receiving the message requests. 
* Since Heroku's free dyno hours are limited, it is wise to shutdown the dyno by `heroku ps:scale web=0`. Since Facebook periodically pings the webhook, it will report an error and subsequently disable the webhook after a few hours.
* To fix this, go to your App page and in the Webhooks section, re-subscribe to the page `Select a page to subscribe your Webhook to the page events`


In the next part in this series, we'll take a look at [wit.ai](http://wit.ai), which will enable more intelligent conversations. 


