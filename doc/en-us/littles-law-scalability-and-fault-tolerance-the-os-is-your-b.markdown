## [Little’s Law, Scalability and Fault Tolerance: The OS is your bottleneck. What you can do?](/blog/2014/2/5/littles-law-scalability-and-fault-tolerance-the-os-is-your-b.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 5, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7389/12310559274_fcd3c37870_o.png)<span style="font-style: italic;">This is a guest</span> [repost](http://blog.paralleluniverse.co/2014/02/04/littles-law/) <span style="font-style: italic;">by Ron Pressler, the founder and CEO of</span> [Parallel Universe](http://paralleluniverse.co/)<span style="font-style: italic;">, a Y Combinator company building advanced middleware for real-time applications. </span>

> _Little’s Law helps us determine the maximum request rate a server can handle. When we apply it, we find that the dominating factor limiting a server’s capacity is not the hardware but the <span class="caps"><span class="caps">OS.</span></span> Should we buy more hardware if software is the problem? If not, how can we remove that software limitation in a way that does not make the code much harder to write and understand?_

Many modern web applications are composed of multiple (often many) <span class="caps"><span class="caps">HTTP</span></span> services (this is often called a micro-service architecture). This architecture has many advantages in terms of code reuse and maintainability, scalability and fault tolerance. In this post I’d like to examine one particular bottleneck in the approach, which hinders scalability as well as fault tolerance, and various ways to deal with it (I am using the term “scalability” very loosely in this post to refer to software’s ability to extract the most performance out of the available resources). We will begin with a trivial example, analyze its problems, and explore solutions offered by various languages, frameworks and libraries.

## Our Little Service

Let’s suppose we have an <span class="caps"><span class="caps">HTTP</span></span> service accessed directly by the client (say, web browser or mobile app), which calls various other <span class="caps"><span class="caps">HTTP</span></span> services to complete its task. This is how such code might look in Java:

<div class="highlight">

    import ...;

    @Path("myservice")
    public class MyRestResource {
        private static final Client httpClient = ClientBuilder.newClient();

        @GET
        @Produces(MediaType.TEXT_HTML)
        public String getIt() {
        	int failures = 0;

        	// call foo (synchronous)
        	String fooResponse = null;
        	try {
        	    fooResponse = httpClient.target("http://s1.acme.com/foo").request().get().readEntity(String.class);
        	} catch(ProcessingException e) {
        	    failures++;
        	}

        	// call bar (synchronous)
        	String barResponse = null;
        	try {
        	    barResponse = httpClient.target("http://s1.acme.com/bar").request().get().readEntity(String.class);
        	} catch(ProcessingException e) {
        	    failures++;
        	}

          monitorOperation(failures);
          return combineResponses(fooResponse, barResponse);
        }
    }

</div>

To define a <span class="caps"><span class="caps">REST</span></span> service, our example uses [<span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span>](https://jersey.java.net/documentation/latest/user-guide.html#jaxrs-resources), though a plain Servlet or any other framework could have been used. To invoke other services, we use a [<span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span> client](https://jersey.java.net/documentation/latest/user-guide.html#d0e3762), though other libraries could have been used (<span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span> client is powerful and general, and supports integrating other libraries to perform the actual <span class="caps"><span class="caps">HTTP</span></span> request; e.g. Jersey’s <span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span> client integrates with Netty, Jetty or Grizzly clients).

To keep the example simple, instead of calling many “micro services”, we call just two, _foo_ and _bar_, but we keep in mind that a real application might call many more. We also want to monitor failed service calls, so we count and report them.

Now let’s find the bottlenecks in our approach.

## Little’s Law

[Little’s Law](http://en.wikipedia.org/wiki/Little%27s_law) is a mathematical theorem useful in determining the capacity of a system such as ours that receives and processes external requests. For a _stable_ system, it ties the average request arrival rate, _λ_, the average time each request is processed by the system, _W_, and the number of concurrent requests pending in the system, _L_, in a neat little formula:

> _L = λW_

What’s remarkable about this result is that it does not depend on the precise distribution of the requests, the order in which requests are processed or any other variable that might have conceivably affected the result.

Here’s an example: if 1000 requests, on average, arrive each second, and each takes <span class="caps"><span class="caps">0.5</span></span> seonds to process, on average, then our system is required to handle 1000*<span class="caps"><span class="caps">0.5</span></span> = 500 requests concurrently.

Normally, however, the system’s capacity, _L_, is a given, and the request processing time, _W_, is a feature of the software and depends on its complexity and internal latencies. If we know _L_ and _W_ we can figure out the rate of requests we can support:

> _λ = L/W_

To handle more requests, we need to increase _L_, our capacity, or decrease _W_, our processing time, or latency.

What happens if requests arrive at a greater rate than _λ_? The system will no longer be stable. Requests will start queuing up. At first, they will experience much increased latency, but quickly system resources will be exhausted and the server will become unavailable.

## What Dominates the Capacity (_L_)

_L_ is a feature of the environment (hardware, <span class="caps"><span class="caps">OS</span></span>, etc.) and its limiting factors. It is the minimum of all limits constraining the number of concurrent requests. What are those limits?

Well, first, we have the number of concurrent <span class="caps"><span class="caps">TCP</span></span> connections the server can support. Normally, a server can support several tens-of-thousands of concurrent <span class="caps"><span class="caps">TCP</span></span> connections, and some shops have had success maintaining [over 2 million](http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/) open connections.

Second, we have bandwidth. Unless we are streaming <span class="caps"><span class="caps">HD</span></span> video, the requests and responses travelling back and forth over the <span class="caps"><span class="caps">LAN</span></span> are no more than a few kilobytes in length. If the total “chit-chat” volume of a single request is under <span class="caps"><span class="caps">1MB</span></span> (usually, it is well under), given today’s high-bandwidth LANs, our network could support anywhere between <span class="caps"><span class="caps">100K</span></span> to over a million concurrent requests (remember, we are talking about concurrent requests, not requests per second; services that require large message volumes usually take longer to process, so we’re those requests take longer than a second or even several seconds, and the network can handle that).

Third, there’s <span class="caps"><span class="caps">RAM.</span></span> The number of concurrent requests that can fit in <span class="caps"><span class="caps">RAM</span></span> depends on how much memory each request consumes, but assuming we can keep this to well below <span class="caps"><span class="caps">1MB</span></span>, and given the low cost of <span class="caps"><span class="caps">RAM</span></span>, this number is probably well over 1 million, and certainly over several hundreds-of-thousands.

Fourth is the <span class="caps"><span class="caps">CPU.</span></span> Just how many concurrent requests the <span class="caps"><span class="caps">CPU</span></span> can support depends on the application logic, but given that most processing is done by the microservices and that most of the time our requests just wait for the microservices to responsd and don’t waste <span class="caps"><span class="caps">CPU</span></span>, this number is anywhere between several hundreds of thousands and several millions. Indeed, productions systems employing similar architectures rarely report <span class="caps"><span class="caps">CPU</span></span> as their bottleneck in practice.

So far, we have reason to believe we can keep _L_ somewhere between <span class="caps"><span class="caps">100K</span></span> and 1 million. Sounds great, huh? But there is one more limiting factor: the <span class="caps"><span class="caps">OS.</span></span> In our example, we employ the simple and familiar thread-per-request model. A request runs on a single <span class="caps"><span class="caps">OS</span></span> thread to completion, and when it’s done, the web server is free to use that thread to serve other requests. So the number of concurrent request we can handle is also limited by the number of threads the <span class="caps"><span class="caps">OS</span></span> can handle.

How do those threads behave? Well, they do some processing and then they block, waiting for a microservice to respond. Then they might do some more processing and block again. So the threads are not very busy (that’s why the <span class="caps"><span class="caps">CPU</span></span> isn’t saturated), but they’re not just sitting there idle, either: the <span class="caps"><span class="caps">OS</span></span> is required to schedule each of them anywhere between 2 and a few dozen times to complete the request.

So, how many such threads could the <span class="caps"><span class="caps">OS</span></span> handle concurrently? That depends on the <span class="caps"><span class="caps">OS</span></span>, but it is usually somewhere between <span class="caps"><span class="caps">2K</span></span> and <span class="caps"><span class="caps">15K.</span></span> Beyond that, thread scheduling will add significant latency to the requests, and once latency grows, _W_ increases and _λ_ drops again. Allowing the software to spawn thread willy-nilly may bring our application to its knees, so we usually set a hard limit on the number of threads we let the application spawn. This number is somewhere between 500 and <span class="caps"><span class="caps">15K</span></span>, but rarely more than that.

Because _L_ is the _minimum_ of all these limits, the <span class="caps"><span class="caps">OS</span></span> scheduler suddenly dropped our capacity, _L_, from the high 100Ks-low millions, to well under 20,000!

In conclusion, if we use the thread-per-request model on good-enough hardware, _L_ is _completely dominated_ by the number of threads the <span class="caps"><span class="caps">OS</span></span> can support without adding latency. We could, of course, buy more servers, but those cost money and incur many other hidden costs. We might be particularly reluctant to buy extra servers when we realize that software is the problem, and those servers we already have are under-utilized.

Before we look at other models that might work around this problem (and the issues they introduce), lets turn to examine _W_, the processing latency.

## Latency, in Sickness and in Health

Suppose our two micro-services, _foo_ and _bar_, each take 500ms on average to return a response (including network latency). Since we call them sequentially (for the time being) our web service’s request processing time, or processing latency, is 1 second. That’s our _W_. Now, suppose we’ve allowed the web server to spawn up to 2000 threads (that’s now our _L_). According to Little’s law, we can handle up to

> _λ = L/W_ = 2000/1 = 2000

requests per second before we become unstable and crash. We figure that even taking into account traffic spikes that number is good enough.

Problem is, this calculation is only valid when both _foo_ and _bar_ are healthy. What happens if one of them experiences trouble which increases its latency to 10 seconds? From _W = <span class="caps"><span class="caps">0.5</span></span> + <span class="caps"><span class="caps">0.5</span></span> = 1_ we’ve now gone to _W = <span class="caps"><span class="caps">0.5</span></span> + 10 = <span class="caps"><span class="caps">10.5</span></span>_ (let’s call it a round 10). What happened to _λ_? From 2000 requests per second it now dropped to 200, which we deem unacceptable.

So, to make our service fault-tolerant, we set timeouts for the service calls:

<div class="highlight">

    @Path("myservice")
    public class MyRestResource {
        private static final Client httpClient;
        static {
            ClientConfig configuration = new ClientConfig();
            configuration.property(ClientProperties.CONNECT_TIMEOUT, TimeUnit.SECONDS.toMillis(2));
            configuration.property(ClientProperties.READ_TIMEOUT, TimeUnit.SECONDS.toMillis(2));
            httpClient = ClientBuilder.newClient(configuration);
        }
        // ...
    }

</div>

We’ve assigned our <span class="caps"><span class="caps">HTTP</span></span> client a timeout parameter of 2 seconds to give it some leeway. This means that our maximum latency, even in the presence of failure, is 4 seconds, which yields a maximum request rate _λ_ of 2000/4 = 500 per second.

In fact, we can do better. If _foo_ goes bad and consistently times-out, there’s no need to try reaching it again and again, waiting for twho whole seconds each time. We can install a “circuit breaker” that trips if a service fails and prevents subsequent requests from attempting to call it. Ocassionally, a side process can sample _foo_ to see if it has recovered, and if so, close the circuit again. This can bring our latency back to under 1 second, and our request handling capacity back to 2000 requests per second even in the event of a failure, at the cost of added complexity. This kind of circuit breaker mechanism is exactly what Netflix’s open source [Hystrix](https://github.com/Netflix/Hystrix) library provides. Its circuit breakers help prevent _W_ from rising when something goes wrong. Also, instead of giving the server a single cap on the number of threads it can spawn, Hystrix makes it easy to allocate various capped thread-pools for different kinds of operations as a form of bulkheading failure (so that one operation that goes awry won’t exhaust all threads).

Now, assuming all our services are healthy or protected with circuit breakers, can we reduce _W_ further? As a matter of fact we can, and quite easily, in fact. In our example, ee call _bar_ only after _foo_ returns, so their latencies are compounded. This might be <span class="caps"><span class="caps">OK</span></span> for our little example, but if we call 20 services instead of 2, this could be a serious issue.

We notice that we don’t need the result of _foo_ to call _bar_, so we can issue the two (or twenty) calls at the same time, let both them do their business in parallel, and absorb their latencies into one another. Here’s how we can do it in Java:

<div class="highlight">

    @GET
    @Produces(MediaType.TEXT_HTML)
    public String getIt() throws InterruptedException{
        int failures = 0;

        // submit requests asynchronously

        Future<String> fooFuture = httpClient.target("http://s1.acme.com/foo").request().async().get().readEntity(String.class);
        Future<String> barFuture = httpClient.target("http://s1.acme.com/bar").request().async().get().readEntity(String.class);

        // collect responses (synchronously)

        String fooResponse = null;
        try {
            fooResponse = fooFuture.get();
        } catch(ProcessingException e) {
            failures++;
        }

        String barResponse = null;
        try {
            barResponse = barFuture.get();
        } catch(ProcessingException e) {
            failures++;
        }

        monitorOperation(failures);
        return combineResponses(fooResponse, barResponse);
    }

</div>

Futures let us dispatch both requests at once, and then wait for their results. If all goes well, we’ve just reduced _W_, our processing latency, from 1 second to 500ms. The same applies for 20 or more service calls.

The result is that our server can now handle up to 4000 requests per second, even if we call quite a few micro services.

Is that the best we can do? Unfortunately yes. Short of somehow greatly optimizing both _foo_ and _bar_, this is pretty much it, even though our hardware is still severely under-utilized. 4000 requests per second is the best we can do, and it might be much worse than that if we allow latency greater than 500ms for any of the micro services. What if this is not enough?

## Functional Callbacking

So we’ve taken down _W_ as much as we could, but _L_ is still constrained by the number of threads the <span class="caps"><span class="caps">OS</span></span> can efficiently handle. The only thing left for us to do now is somehow increase _L_, and to do that we have no option other than abandon the thread-per-request model.

[Node.js](http://nodejs.org/) is a server side JavaScript framework used (primarily) for web applications. JavaScript is single-threaded, so Node has no choice when it comes to not using the <span class="caps"><span class="caps">OS</span></span> thread scheduler. Let’s see how Node.js handles our problem:

<div class="highlight">

    var http = require('http');

    http.createServer(myService).listen(8080);

    function myService(request, response) {
        var completed = 0;
        var failures = 0;
        var fooResponse;
        var barResponse;

        function checkCompletion() {
            if (completed == 2) {
                monitorOperation(failures);
                response.write(combineResponses(fooResponse, barResponse));
                response.end();
            }
        }

        http.get({host: 'http://s1.acme.com', port: 80, path: '/foo'}, function(resp){
          resp.on('data', function(chunk){
          	fooResponse = chunk;
          	completed++;
          	checkCompletion();
          });
        }).on("error", function(e){
          completed++;
          failures++;
          checkCompletion();
        });

        http.get({host: 'http://s1.acme.com', port: 80, path: '/bar'}, function(resp){
          resp.on('data', function(chunk){
          	fooResponse = chunk;
          	completed++;
          	checkCompletion();
          });
        }).on("error", function(e){
          completed++;
          failures++;
          checkCompletion();
        });
    }

</div>

Node request handlers do not have a thread for themselves and they don’t block. Instead, they run for a while, and when they need to wait for, say, a service call, they give the framework a callback to execute when the call completes. By doing that, Node has turned JavaScript’s lack of threading into an advantage. Because it can’t use a thread-per-request model, Node uses asynchronous callbacks which do not suffer from the <span class="caps"><span class="caps">OS</span></span> thread limitation.

But while this approach completely eschews the <span class="caps"><span class="caps">OS</span></span> thread limit problem, it introduces several others. First, any accidental blocking of a handler function, or even if it happens to be running a lengthy computation, effectively blocks the entire Node.js instance; no other requests can be processed. This is often mitigated by running several Node instances on one machine and load-balancing them, which also helps take advantage of all <span class="caps"><span class="caps">CPU</span></span> cores, but it wastes <span class="caps"><span class="caps">RAM</span></span> and makes parallelizing certain computational tasks difficult (the latter might or might not matter, depending on what you want to accomplish).

More importantly, however, it forces an asynchronous, callback-based programming style. This style is harder to write and harder to reason about, as your code is not executed in the order it is written. With complex, dependent operations, this style is the nightmare colloquially known as callback hell (Node.js is trying to make this simpler by adopting a more comprehensively-functional style with something called promises; we’ll look at a similar approach right away).

One of the things working in Node’s favor, however, is that it’s single threaded. Why is that good? Because it keeps our example code simpler. While the calls to _foo_ and _bar_ are submitted asynchronously and their callbacks can be executed at any time and any order (once they complete), we can still safely increment the `failures` and `completed` variables, because whenever the callbacks run, they will all run on the same thread – never concurrently. So while we have to wrap our heads around callbacks, thankfully we don’t need to factor concurrency races into the equation as well.

But the <span class="caps"><span class="caps">JVM</span></span> does support multiple threads, and if you have them, why not use them? Let’s see how [Play](http://www.playframework.com/), a <span class="caps"><span class="caps">JVM</span></span> web framework, handles the problem. Again, our goal is to avoid hogging an <span class="caps"><span class="caps">OS</span></span> thread for the duration of the request:

<div class="highlight">

    public static Promise<Result> getIt() {    
        Promise<String> fooPromise = WS.url("http://s1.acme.com/foo").get().map(
            result -> result.toString();
          );

        Promise<String> barPromise = WS.url("http://s1.acme.com/foo").get().map(
            result -> result.toString();
          );

        // Not actually waiting
        Promise<List<ResultType>> results = Promise.waitAll(fooPromise, barPromise);

        return async(results.map((List<String> rs) -> {
        	String fooResponse = rs.get(0);
        	String barResponse = rs.get(1);
        	// monitoring ????
        	return ok(combineResponses(fooResponse, barResponse));
          }
        });
    }

</div>

Play is written in Scala, which supports (among other styles), and sometimes encourages, a functional programming style, and this is reflected in Play’s Java <span class="caps"><span class="caps">API</span></span> as well. I’ve written the code sample above in Java 8 because prior versions of Java were verbose to the point of exhaustion when written in the functional style, while the Scala <span class="caps"><span class="caps">API</span></span> requires understanding of Scala for-comprehensions.

In practice, the functional style is callback-based, but it usually contains rigorous mechanisms for combining callbacks, which, once you learn them, can make your code much more methodic.

So, the functions in lines 3 and 7 will be executed when their calls (to _foo_ and _bar_ respectively) complete, and the one in line 14 will after when both complete, because the two promises (essentially futures) were combined with `Promise.waitAll`.

But what about our failure count? We cannot keep it in a local variable or even in a class field because the two callbacks can be called on any thread, even concurrently, and if they were to modify any shared state, race conditions are bound to happen.

To solve this, we need those promises to return not just a string, but a string and some other additional monitoring values (yes, I know failures specifically are reported differently, but this holds true for any other monitoring value we may want to collect), combined in a new class we need to define.

In addition, because the callbacks are not executed in the same thread as the original request handler (or the same thread as one another) we cannot use any library functions that rely on `ThreadLocal` state. Threads are gone.

If you don’t like Play’s <span class="caps"><span class="caps">API</span></span> (which certainly feels very foreign to Java), the Netflix [RxJava](https://github.com/netflix/rxjava) project offers a functional <span class="caps"><span class="caps">API</span></span> that operates along the same principles, not tied to a single web framework, and more familiar to Java programmers (and a lot nicer, <span class="caps"><span class="caps">IMO</span></span>).

So while the functional style is elegant and helps compose callbacks nicely and in an idiomatic manner, using it entails a pretty serious learning curve, and it might bring about some baffling problems and race-conditions if you happen to interact with code that does not play well with the functional bits. In short, once you go functional you might find that you need to go functional all the way (at least within the service), or risk some serious head scratchers, especially if you’re not programming in a language that restricts shared mutable state, like Clojure or Erlang (or Haskell, if you’re so inclined).

… aaaand that’s it. We’re no longer using threads to guide the flow of our code and delineate requests, so _L_ is no longer dominated by the <span class="caps"><span class="caps">OS</span></span>’s scheduling capacity. It is solely determined by our hardware capacity and whatever (hopefully small) overhead the frameworks/libraries we use incur. We only need to buy more hardware if our hardware is saturated.

All is well and good except for one thing: we’ve lost the thread-per-request model. Nobody likes nesting callbacks, and while some may argue that a functional style is the way to go no matter what you do, it is unfamiliar, arguably more difficult to reason about in some circumstances, and as yet unproven in the industry. Also, threads are nice. They give us a clear program flow along with a stack for intermediate state. Must we completely throw this wonderful abstraction out the window? Are scalability (and fault tolerance), and simple, easy to follow code mutually exclusive? Luckily, they are not.

## Lightweight Threads

If you remember how we started, it was by realizing that the server’s capacity, or _L_ in Little’s formula, is dominated by the <span class="caps"><span class="caps">OS</span></span>’s thread scheduling capability. Becasue we had to cap our threads at some relatively small number, they became a precious resource. The circuit-breakers and the functional programming style were required _because threads are expensive_. But what if they didn’t have to be?

Some languages, most notably Erlang and Go, provide lightweight threads (processes in Erlang; goroutines in Go). The open-source [Quasar](https://github.com/puniverse/quasar) library provides them on the <span class="caps"><span class="caps">JVM</span></span> (where they’re called fibers). These lightweigh threads are not scheduled by the <span class="caps"><span class="caps">OS</span></span> but by the language or library runtime. The runtime can often do a far better job than the <span class="caps"><span class="caps">OS</span></span> at scheduling those lightweight threads because it knows more about their purpose. In particular it knows that they run in very short bursts and block very often; this is not generally true of heavyweight threads. The runtime scheduler usually employs what is known as M:N scheduling, where M lightweight threads are mapped onto N <span class="caps"><span class="caps">OS</span></span> threads, with M » N.

Just like anything else, lightweight threads aren’t magic, and they have their own tradeoffs. Their main problem is that all blocking function calls you issue on a lightweight thread must be integrated with the scheduler. If a library function is not aware of the scheduler it might block the entire <span class="caps"><span class="caps">OS</span></span> thread (Quasar monitors all running fibers and issues a warning when this happens; also, it easily handles non-frequent blocking of <span class="caps"><span class="caps">OS</span></span> threads). In many circumstances, though, this is a very acceptable price to pay for keeping your code simple while gaining scalability and fault-tolerance. This is also not too hard to enforce, as blocking calls usually perform one of a small set of operations – network <span class="caps"><span class="caps">IO</span></span>, file <span class="caps"><span class="caps">IO</span></span>, or database calls – so making sure to only use libraries that are aware of your lightweight threads is usually easy (in fact, Quasar makes it very easy to turn any asynchronous, callback-based, <span class="caps"><span class="caps">API</span></span> into a fiber-blocking <span class="caps"><span class="caps">API</span></span> with a [simple mechanism](http://docs.paralleluniverse.co/quasar/#transforming-any-asynchronous-callback-to-a-fiber-blocking-operation)). And if Google’s [proposed user-level threads](https://www.youtube.com/watch?v=KXuZi9aeGTw) make it into the Linux kernel (which will then allow user code to schedule <span class="caps"><span class="caps">OS</span></span> threads), Quasar will integrate with those and further reduce, or even completely remove, any possible conflict when integrating with non-fiber-aware blocking libraries.

The new open-source [Comsat](https://github.com/puniverse/comsat) library is a set of standard Java <span class="caps"><span class="caps">API</span></span> implementations (like <span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span> and <span class="caps"><span class="caps">JDBC</span></span>) that integrate with Quasar fibers. So let’s see how we can re-write our original example to be scalable and fault-tolerant, this time using lightweight threads via Comsat:

<div class="highlight">

    import ...;
    import co.paralleluniverse.fibers.ws.rs.client.AsyncClientBuilder;
    import co.paralleluniverse.fibers.SuspendExecution;

    @Path("myservice")
    public class MyRestResource {
        private static final Client httpClient = AsyncClientBuilder.newClient();

        @GET
        @Produces(MediaType.TEXT_HTML)
        public String getIt() throws InterruptedException, SuspendExecution {
            int failures = 0;

            // submit requests asynchronously

            Future<String> fooFuture = httpClient.target("http://s1.acme.com/foo").request().async().get().readEntity(String.class);
            Future<String> barFuture = httpClient.target("http://s1.acme.com/bar").request().async().get().readEntity(String.class);

            // collect responses (synchronously)

            String fooResponse = null;
            try {
                fooResponse = fooFuture.get();
            } catch(ProcessingException e) {
                failures++;
            }

            String barResponse = null;
            try {
                barResponse = barFuture.get();
            } catch(ProcessingException e) {
                failures++;
            }

            monitorOperation(failures);
            return combineResponses(fooResponse, barResponse);
        }
    }

</div>

You’ll notice that this is exactly the original example (after adding futures to absorb the two services’ latencies into each other) with some very minor changes! The first is adding `throws SuspendExecution` to our service method to designate it as a fiber-blocking method (alternatively you can annotate it with the `@Suspendable` annotation). The second is that we use the `AsyncClientBuilder` provided by Comsat, which provides the same <span class="caps"><span class="caps">JAX</span></span>-<span class="caps"><span class="caps">RS</span></span> client <span class="caps"><span class="caps">API</span></span>, only with an implementation that is fiber-aware.

What about circuit-breakers? They’re not as critical now. We could add timeouts if we want to quickly respond back with a failure if one of the services takes too long, but other than that, we don’t mind the increased latency. Sure _W_ might grow but _L_ is now only contrained by the hardware. Fibers are cheap, and we can handle hundreds-of-thousands of them, or even millions, at once (we might still want to use a library like Hystrix to prevent an unbounded number of fibers from piling up, but even without it our server can recover gracefully from a short-term failure).

So we _can_ have our cake and eat it, too! When you combine the awesome performance, stability, unparalleled tooling and monitoring of the <span class="caps"><span class="caps">JVM</span></span> with the power and simplicity of lightweight threads, you can make the absolute most of your hardware while keeping your code simple, easy to understand, and familiar. You don’t even need to learn new APIs.

Both Clojure and Scala provide fiber-like functionality with Scala Async and Clojure’s wonderful core.async. But those are limited for use by their respective languages (i.e. they cannot integrate with other <span class="caps"><span class="caps">JVM</span></span> languages), and even there, because they are based on macros, they are restricted to a single syntactical form: you can only explicitely block in the outermost “fiber” function – you can’t call another function that blocks.

## But What If I Like Functional Programming/<span class="caps"><span class="caps">CSP</span></span>/Actors/Rx and Do Want to Learn New APIs?

That’s great! We like all of these concepts, but believe they should be used where they make sense as a computational model – when they make programming easier – not as a convoluted way to work around <span class="caps"><span class="caps">OS</span></span> limitations. That’s why Quasar has Go-like channels complete with “reactive extensions” (or Rx) for good measure, as well as a full, Erlang-like actor system, all of which are built on top of the solid fiber foundation. They are great for making your business-logic, not just your service endpoints, scalable and fault tolerant. Quasar’s Clojure <span class="caps"><span class="caps">API</span></span>, [Pulsar](https://github.com/puniverse/pulsar) is even compatible with core.async.

And while Comsat provides fiber-aware implementation of standard Java APIs, it also offers an optional <span class="caps"><span class="caps">API</span></span> called Web Actors. Web Actors let you write web applications using the [actor model](http://en.wikipedia.org/wiki/Actor_model), popularized by Erlang. Web Actors give you excellent scalability and fault-tolerance, and are particularly fun to use in interactive web application, those that use WebSockets, or other server-push technologies (such as comet or <span class="caps"><span class="caps">SSE</span></span>). Web Actors were discussed in our [previous blog post](http://blog.paralleluniverse.co/2014/01/28/web-actors-1/).

## Conclusion

Little’s law determines the load (request rate) a server can withstand given its (concurrent request) capacity and processing latency. We learned that when using the simple thread-per-request, the <span class="caps"><span class="caps">OS</span></span> severly limits the server capacity. To maintain scalability and fault tolerance you must work around this limitation by either forgoing the simple thread-per-request model and adopting a functional programming style, or by using a language or a library that provides lightweight threads for your platform. If you’re developing for the <span class="caps"><span class="caps">JVM</span></span>, [Quasar](https://github.com/puniverse/quasar) gives you lightweight threads (fibers), and [Comsat](https://github.com/puniverse/comsat) gives you fiber-aware implementations to standard Java APIs.

## Related Articles

*   [Discuss on Hacker News](https://news.ycombinator.com/item?id=7179144)
*   [The Performance Of Distributed Data-Structures Running On A "Cache-Coherent" In-Memory Data Grid](http://highscalability.com/blog/2012/8/20/the-performance-of-distributed-data-structures-running-on-a.html)
*   [The Secret To 10 Million Concurrent Connections -The Kernel Is The Problem, Not The Solution](http://highscalability.com/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)
*   [Is It Time To Get Rid Of The Linux OS Model In The Cloud?](http://highscalability.com/blog/2012/1/19/is-it-time-to-get-rid-of-the-linux-os-model-in-the-cloud.html)
*   [Writing Interactive Web Applications with Web Actors](http://blog.paralleluniverse.co/2014/01/28/web-actors-1/)

</div>