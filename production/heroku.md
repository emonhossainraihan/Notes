A "dyno hour" is simply one hour of a dyno running. 

A stack is an operating system image that is curated and maintained by Heroku


If you have just 1 app running just 1 dyno, that dyno could be available 24/7 forever, since even a 31 day month consists of 31 x 24 = 744 hours, which is less than the 1000 free dyno hours you have at your disposal.

However, if your one dyno on your one app is a "web dyno" (i.e. a web server), then note that free web dynos sleep after 30 minutes of inactivity (in which case the next request to the web dyno "wakes it up").

Note that a dyno is basically just a virtual machine. It is your "server". One dyno can certainly support many many users, depending on the complexity / performance requirements of your app. You can "scale" your app both "vertically" (meaning increased computing capabilities per dyno) and "horizontally" (meaning running multiple dyno instances that load balance your app's traffic).

Heroku use AWS infrastructure, and using AWS (or GCP/Azure) directly is cheaper and more flexible, but it involves a bigger investment to learn and maintain.

Heroku = PaaS

AWS = IaaS

https://www.quora.com/What-is-a-Heroku-Dyno/answer/Yoni-Rabinovitch
