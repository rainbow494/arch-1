## [When all the Program's a Graph - Prismatic's Plumbing Library](/blog/2013/2/14/when-all-the-programs-a-graph-prismatics-plumbing-library.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, February 14, 2013 at 11:35AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8516/8474378252_642e845366_m.jpg)

At some point as a programmer you might have the insight/fear that all programming is just doing stuff to other stuff.

Then you may observe after coding the same stuff over again that stuff in a program often takes the form of interacting patterns of flows.

Then you may think hey, a program isn't only useful for coding datastructures, but a program is a kind of datastructure and that with a meta level jump you could program a program in terms of flows over data and flow over other flows.

That's the kind of stuff Prismatic is making available in the Graph extension to their [plumbing](https://github.com/Prismatic/plumbing) package ([code examples](https://github.com/Prismatic/plumbing/blob/master/test/plumbing/graph_examples_test.clj)), which is described in an excellent post: [Graph: Abstractions for Structured Computation](http://blog.getprismatic.com/blog/2013/2/1/graph-abstractions-for-structured-computation).

You may remember Prismatic from previous profile we did on HighScalability: [Prismatic Architecture - Using Machine Learning On Social Networks To Figure Out What You Should Read On The Web](http://highscalability.com/blog/2012/7/30/prismatic-architecture-using-machine-learning-on-social-netw.html). We learned how Prismatic, an interest driven content suggestion service, builds programs in terms of graph stuff:

> As you might expect, they are doing things a little differently. They’ve chosen Clojure, a modern Lisp that compiles to Java bytecode, as their programming language of choice. The idea is to use functional programming to build fine-grained, flexible abstractions that are composed to express problem-specific logic. 
> 
> One example of functional power is their graph library, which they use all over the place.  For example, a graph of computations can be described to create the equivalent of a low-latency, pipelined set of map-reduce jobs for each user. Another example is the use of subgraphs to compactly describe service configuration in a modular way.
> 
> Another example is that their doc-analysis pipeline is a graph, where each elaboration of a document may depend on the previous (e.g., identifying topics for a document depends on having already extracted its text); the newsfeed generation process is a graph composed of many query and ranking steps; and each of the production services is itself a graph, where each resource (e.g. data store, memory index, HTTP handler, and so on) is a node that can depend on others.

Prismatic describes Graph thusly:

> Last week, we open-sourced Graph as part of our plumbing library. Graph is a very simple, declarative way to describe how data flows between functions in an FP program. It allows us to formalize the informal structure of good FP code, and enables higher-order abstractions over these structures that can help stamp out many persistent forms of complexity overhead.Concretely, a Graph represents the structured composition of any number of functions, using a Clojure map. Each entry in a Graph is a mapping from a node name (keyword) to a keyword function, which computes the value of its node from the values of other nodes and inputs from outside the Graph. The plumbing readme provides some simple examples of what Graph can do, and this test goes into more gory details on how to use Graph.

Dataflow style programming is not a new idea. But like state machines and other good notions, even if we recognize their value we are usually to lazy to take action in practice.

Hacking the same thing in the same old way is the minimally viable ethic. Perhaps seeing Prismatic's example as a successful deployment of these ideas in production will help us all jump out of the same old stuff.

## Related Articles

<div>

*   [Prismatic's "Graph" at Strange Loop](http://blog.getprismatic.com/blog/2012/10/1/prismatics-graph-at-strange-loop.html)
*   [Graph: Composable Production Systems in Clojure](http://www.infoq.com/presentations/Graph-Clojure-Prismatic)
*   [Dataflow Programming](http://en.wikipedia.org/wiki/Dataflow_programming)
*   [Introduction to Behavior Trees](http://www.altdevblogaday.com/2011/02/24/introduction-to-behavior-trees/)
*   [Linking Data and Actions on the Web](http://vimeo.com/50215125)

</div>

</div>