## [Google AppEngine - A First Look](/blog/2008/4/8/google-appengine-a-first-look.html)

    

    I haven't developed an AppEngine application yet, I'm just taking a look around their documentation and seeing what stands out for me. It's not the much speculated [super cluster VM](http://highscalability.com/rumors-signs-and-portents-concerning-freeish-google-cloud). AppEngine is solidly grounded in code and structure. It reminds me a little of the guy who ran a [website out of S3](http://blog.tomevslin.com/2008/03/amazon-s3-backs.html) with a splash of [Heroku](http://highscalability.com/heroku-simultaneously-develop-and-deploy-automatically-scalable-rails-applications-cloud) thrown in as a chaser.  

The idea is clearly to take advantage of our [massive multi-core future](http://www.eetimes.com/showArticle.jhtml;?articleID=206105179) by creating a shared nothing infrastructure based firmly on a core set of infinitely scalable database, storage and CPU services. Don't forget Google also has a few other services to leverage: email, login, blogs, video, search, ads, metrics, and apps. A shared nothing request is a simple beast. By its very nature shared nothing architectures must be composed of services which are themselves already scalable and Google is signing up to supply that scalable infrastructure. Google has been busy creating a platform of out-of-the-box scalable services to build on. Now they have their scripting engine to bind it all together.  

Everything that could have tied you to a machine is tossed. No disk access, no threads, no sockets, no root, no system calls, no nothing but service based access. Services are king because they are easily made scalable by load balancing and other tricks of the trade that are easily turned behind the scenes, without any application awareness or involvement.  

Using the CGI interface was not a mistake. CGI is the perfect metaphor for our brave new app container world: get a request, process the request, die, repeat. Using AppEngine you have no choice but to write an app that can be splayed across a pointy well sharpened CPU grid. CGI was devalued because a new process had to be started for every request. It was too slow, too resource intensive. Ironic that in the cloud that's exactly what you want because that's exactly how you cause yourself fewer problems and buy yourself more flexibility.  

The model is pure abstraction. The implementation is pure pragmatism. Your application exists in the cloud and is in no way tied to any single machine or cluster of machines. CPUs run parallel through your application like a swarm of busy bees while wizards safely hidden in a pocket of space-time can bend reality as much as they desire without the muggles taking notice. Yet the abstraction is implemented in a very specific dynamic language that they already have experience with and have confidence they can make work. It's a pretty smart approach. No surprise I guess.  

One might ask: is LAMP dead? Certainly not in the way Microsoft was hoping. AppEngine is so much easier to use than the AWS environment of EC2, S3, SQS, and SDB. Creating an app in AWS takes real expertise. That's why I made the comparison of AppEngine to [Heroku](http://highscalability.com/heroku-simultaneously-develop-and-deploy-automatically-scalable-rails-applications-cloud). Heroku is a load and go approach for RoR whereas AppEngine uses Python. You basically make a Python app using services and it scales. Simple. So simple you can't do much beyond making a web app. Nobody is going to make a super scalable transcoding service out of AppEngine. You simply can't load the needed software because you don't have your own servers. This is where Amazon wins big. But AppEngine does hit a sweet spot in the market: website builders who might have previously went with LAMP.  

What isn't scalable about AppEngine is the scalability of the complexity of the applications you can build. It's a simple request response system. I didn't notice a cron service, for example. Since you can't write your own services a cron service would give you an opportunity to get a little CPU time of your own to do work. To extend this notion a bit what I would like to see as an event driven state machine service that could drive web services. If email needs to be sent every hour, for example, who will invoke your service every hour so you can get the CPU to send the email? If you have a long running seven step asynchronous event driven algorithm to follow, how will you get the CPU to implement the steps? This may be Google's intent. Or somewhere in the development cycle we may get more features of this sort. But for now it's a serious weakness.  

Here's are a quick tour of a few interesting points. Please note I'm copying large chunks of their documentation in this post as that seems the quickest way to the finish line...  

*   Very nice project page at [Google App Engine](http://code.google.com/appengine). Already has a FAQ, articles, blog, forums, example applications, nice [tutorial](http://code.google.com/appengine/docs/gettingstarted/), and a nice touch is how to work with [Django](http://code.google.com/appengine/articles/django.html). Some hard chargers are already posting questions to the forum.  
    *   Python only. More languages will follow. As you are uploading clear text into the engine there's no hiding from mother Google.  
    *   You aren't getting root. Applications run in a sandbox, which is a secure environment that provides limited access to the underlying operating system. These limitations allow App Engine to distribute web requests for the application across multiple servers, and start and stop servers to meet traffic demands.  
    - An application can only access other computers on the Internet through the provided URL fetch and email services and APIs. Other computers can only connect to the application by making HTTP (or HTTPS) requests on the standard ports.  
    - An application cannot write to the file system. An app can read files, but only files uploaded with the application code. The app must use the App Engine datastore for all data that persists between requests.  
    - Application code only runs in response to a web request, and must return response data within a few seconds. A request handler cannot spawn a sub-process or execute code after the response has been sent.  
    *   The data access trend continues with the RDBMS being dissed infavor of a properties type interface.  
    - The datastore is not like a traditional relational database. Data objects, or "entities," have a kind and a set of properties. Queries can retrieve entities of a given kind filtered and sorted by the values of the properties. Property values can be of any of the supported property value types.  
    - The datastore uses optimistic locking for concurrency control. An update of a entity occurs in a transaction that is retried a fixed number of times if other processes are trying to update the same entity simultaneously.  
    - They have some notion of transaction: The datastore implements transactions across its distributed network using "entity groups." A transaction manipulates entities within a single group. Entities of the same group are stored together for efficient execution of transactions. Your application can assign entities to groups when the entities are created.  
    *   You've got mail: Applications can also send email messages using App Engine's mail service. The mail service also uses Google infrastructure to send email messages. If you've ever been marked a spammer because you send a little email, this is actually a nice feature.  
    *   It's mostly free for now: 500MB of storage, up to 5 million page views a month, and 10GB bandwidth per day. Additional resources will be available for $$.  
    *   Limits exist on various features. If a request takes too long it's killed. You can only get 1,000 results at a time. That sort of thing. Pretty reasonable.  
    *   Developers must download a Windows, Mac OS X or Linux SDK. Python 2.5 is required.  
    *   The SDK includes a web server application that simulates the App Engine environment. So this in the RoR and GWT type mold where you have a nice local development environment that emulates what happens in the deployment environment.  
    *   Google App Engine supports any framework written in pure Python that speaks CGI (and any WSGI-compliant framework using a CGI adaptor), including Django, CherryPy, Pylons, and web.py. You can bundle a framework of your choosing with your application code by copying its code into your application directory.  
    *   Google has their own framework called _webapp_. Nice MS style naming.  
    *   Here's a hello world application using webapp:  

    <pre>  
    import wsgiref.handlers  

    from google.appengine.ext import webapp  

    class MainPage(webapp.RequestHandler):  
      def get(self):  
        self.response.headers['Content-Type'] = 'text/plain'  
        self.response.out.write('Hello, webapp World!')  

    def main():  
      application = webapp.WSGIApplication(  
                                           [('/', MainPage)],  
                                           debug=True)  
      wsgiref.handlers.CGIHandler().run(application)  

    if __name__ == "__main__":  
      main()  
    </pre>

    This code defines one request handler, MainPage, mapped to the root URL (/). When webapp receives an HTTP GET request to the URL /, it instantiates the MainPage class and calls the instance's get method. Inside the method, information about the request is available using self.request. Typically, the method sets properties on self.response to prepare the response, then exits. webapp sends a response based on the final state of the MainPage instance.  

    The application itself is represented by a webapp.WSGIApplication instance. The parameter debug=true passed to its constructor tells webapp to print stack traces to the browser output if a handler encounters an error or raises an uncaught exception. You may wish to remove this option from the final version of your application.  

    *   Google is standardizing components on their infrastructure. Take the login interface. When your application is running on App Engine, users will be directed to the Google Accounts sign-in page, then redirected back to your application after successfully signing in or creating an account.  
    *   Forms looks normal. Lots of embedded html. [Take a look](http://code.google.com/appengine/docs/gettingstarted/handlingforms.html). Python like Perl has a nice bulk string handling syntax so this style isn't as fugly as it would be in C++ or Java.  
    *   Database access is built around Data Models: A model describes a kind of entity, including the types and configuration for its properties. Here's a taste:  

    <pre>  
    Example of creation:  
    from google.appengine.ext import db  
    from google.appengine.api import users  

    class Pet(db.Model):  
      name = db.StringProperty(required=True)  
      type = db.StringProperty(required=True, choices=set("cat", "dog", "bird"))  
      birthdate = db.DateProperty()  
      weight_in_pounds = db.IntegerProperty()  
      spayed_or_neutered = db.BooleanProperty()  
      owner = db.UserProperty()  

    pet = Pet(name="Fluffy",  
              type="cat",  
              owner=users.get_current_user())  
    pet.weight_in_pounds = 24  
    pet.put()  

    Example of get, modify, save:  
    if users.get_current_user():  
      user_pets = db.GqlQuery("SELECT * FROM Pet WHERE pet.owner = :1",  
                              users.get_current_user())  
      for pet in user_pets:  
        pet.spayed_or_neutered = True  

      db.put(user_pets)  
    </pre>

    Looks like your normal overly complex data access. Me, I appreciate the simplicity of a string based property interface.  

    *   You can use [Django's HTML Template](http://code.google.com/appengine/docs/gettingstarted/templates.html) system.  
    *   [Static files](http://code.google.com/appengine/docs/gettingstarted/staticfiles.html) are served using automated mapping mechanism. You don't get local disk store for your css, flash, and js files.  
    *   Applications are loaded using a command line tool: appcfg.py update helloworld/.  
    *   Applications are accessed like: http://application-id.appspot.com. You get your domain name.  
    *   There's a dashboard that has six graphs that give you a quick visual reference of your system usage: Requests per Second, Errors per Second, Bytes Received per Second, Bytes Sent per Second, Megacycles per Second (The amount of CPU megacyles your application uses every second), Milliseconds Used per Second, Number of Quota Denials per Second. I have no idea what a megacycle is either. I think it's bigger than a pint of beer.  
    *   Also I wonder if this is meant to compete with Facebook more than Amazon?  
    *   Developers with a lot of little projects will find AppEngine especially useful, which always leaves open a [Adoption Led Market](http://blogs.sun.com/webmink/entry/the_adoption_led_market) play.  

    ## Related Articles

    *   [HighScalability: Rumors of Signs and Portents Concerning Freeish Google Cloud](http://highscalability.com/rumors-signs-and-portents-concerning-freeish-google-cloud).  
    *   [Techcrunch: Google Jumps Head First Into Web Services With Google App Engine](http://www.techcrunch.com/2008/04/07/google-jumps-head-first-into-web-services-with-google-app-engine/).  
    *   [HighScalability: Heroku - Simultaneously Develop and Deploy Automatically Scalable Rails Applications in the Cloud](http://highscalability.com/heroku-simultaneously-develop-and-deploy-automatically-scalable-rails-applications-cloud).  
    *   [Video of the announcement](http://youtube.com/watch?v=3Ztr-HhWX1c) at Camp David, er [CampFireOne](http://googleappengine.blogspot.com/2008/04/introducing-google-app-engine-our-new.html).  
    *   [Techdirt: Google Finally Realizes It Needs To Be The Web Platform](http://techdirt.com/articles/20080407/225749782.shtml).  
    *   [TechCrunch Labs: Our Experience Building And Launching An App On Google App Engine](http://www.techcrunch.com/2008/04/08/techcrunch-labs-our-experience-building-and-launching-app-on-google-app-engine/)  
    *   [Zdnet: Let the PaaS wars begin](http://blogs.zdnet.com/SAAS/?p=486).  
    *   [ReadWriteWeb: Google: Cloud Control to Major Tom](http://www.readwriteweb.com/archives/google_cloud_control.php).  
    *   [ComputingAtScale: The Live Web](http://www.computingatscale.com/?p=61) has another heart.  
    *   [ComputingAtScale: Parallelism: The New New Thing!](http://www.computingatscale.com/?p=54).  
    *   [EETimes: CPU designers debate multi-core future](http://www.eetimes.com/showArticle.jhtml;?articleID=206105179).  
    *   [EETimes: Multicore puts screws to parallel-programming models](http://www.eetimes.com/news/latest/showArticle.jhtml?articleID=206504466).  
    *   [Slashdot: Panic in Multicore Land](http://developers.slashdot.org/developers/08/03/11/068231.shtml).  
    *   [Mashable: Google App Engine: An Early Look](http://mashable.com/2008/04/08/google-app-engine/).  
    *   [DeftLabs: Gazing Into The Clouds](http://deftlabs.com/2008/04/gazing-into-the-clouds/).  
    *   [Experimenting with Google App Engine](http://bret.appspot.com/entry/experimenting-google-app-engine). Bret Taylor writes a blog post about the blog he wrote for AppEngine that serves the blog post. Elegant touch.  
    *   [ReadWriteWeb: Google App Engine: History's Next Step or Monopolistic Boondoggle?](http://www.readwriteweb.com/archives/google_app_engine_history_or_monopoly.php).  
    *   [GigaOM: App Engine: Competition Is Good for Everyone](http://gigaom.com/2008/04/08/app-engine-competition-is-good-for-everyone/)  
    *   [The Blist: Batteries sold separately](http://www.b-list.org/weblog/2008/apr/08/batteries-sold-separately/) - "they’ve got the scalable bit and the hosting bit, but there’s a surprising lack of, well, “web” and “application” going on here."  
    *   [Foobar: Would you use Google App Engine?](http://www.geekzone.co.nz/foobar/4846) - " if you think you might like to compete with Google and/or become a really large web site or business yourself, then I don't think that Google App Engine should be your first choice"  
    *   [Silicon Valley Insider: Google's App Engine: Aiming At Facebook, Not Amazon](http://www.alleyinsider.com/2008/4/google_s_appengine_aiming_at_facebook_not_google) - "Google is not trying to provide pure utility here -- they are trying to provide utility tethered to their infrastructure."  
    *   [Niall Kennedy: Google App Engine for developers](http://www.niallkennedy.com/blog/2008/04/google-app-engine.html) - "Overall I am quite impressed with Google App Engine and its potential to remove operations management and systems administration from my task list. I am not confident in Google App Engine as a hosting solution for any real business..."    