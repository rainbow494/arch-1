## [How Does the Use of Docker Effect Latency?](/blog/2015/12/16/how-does-the-use-of-docker-effect-latency.html)

    

    

![](https://c1.staticflickr.com/1/735/23495946660_7925a7a528_m.jpg)

A great question came up on the [mechanical-sympathy](https://groups.google.com/forum/#!forum/mechanical-sympathy) list that many others probably have as well: 

> I keep hearing about [Docker] as if it is the greatest thing since sliced bread, but I've heard anecdotal evidence that low latency apps take a hit. 

Who better to answer than [Gil Tene](https://twitter.com/giltene), Vice President of Technology and CTO, Co-Founder, of [Azul Systems](https://www.azul.com/)? Like Stephen Curry draining a deep transition three, Gil can always be counted on for his insight:

*   [The Black Magic Of Systematically Reducing Linux OS Jitter](http://highscalability.com/blog/2015/4/8/the-black-magic-of-systematically-reducing-linux-os-jitter.html)
*   [Your Load Generator Is Probably Lying To You - Take The Red Pill And Find Out Why](http://highscalability.com/blog/2015/10/5/your-load-generator-is-probably-lying-to-you-take-the-red-pi.html)

And here's [Gil's answer](https://groups.google.com/forum/#!msg/mechanical-sympathy/8QkiLhHC5-M/8_LlasLrBQAJ):

Putting aside questions of taste and style, and focusing on the effects on latency (the original question), the analysis from a pure mechanical point of view is pretty simple: Docker uses Linux containers as a means of execution, with no OS virtualization layer for CPU and memory, and with optional (even if default is on) virtualization layers for i/o. 

## CPU and Memory

From a latency point of view, Docker's (and any other Linux container's) CPU and memory latency characteristics are pretty much indistinguishable from Linux itself. But the same things that apply to latency behavior in Linux apply to Docker.

If you want clean & consistent low latency, you'll have to the same things you need to do on non-dockerized and non-containerized Linux for the same levels of consistency. E.g. if you needed to keep the system as a whole under control (no hungry neighbors), you'll have to do that at the host level for Docker as well.

If you needed to isolate sockets or cores and choose which processes end up where, expect to do the same for your docker containers and/or the threads within them.

If you were numactl'ing or doing any sort of directed numa-driven memory allocation, the same will apply.

And some of the stuff you'll need to do may seem counter-style to how some people want to deploy docker, but if you are really interested in consistent low latency, you'll probably need to break out the toolbox and use the various cgroups, tasksets and other cool stuff to assert control over how things are laid out. But if/when you do, you won't be able to tell the difference (in terms of CPU and memory latency behaviors) between a dockeriz'ed process and one that isn't.

## I/O

### Disk I/O

I/O behavior under various configurations is where most of the latency overhead questions (and answers) usually end up. I don't know enough about disk i/o behaviors and options in docker to talk about it much. I'm pretty sure the answer to anything throughput and latency sensitive for storage will be "bypass the virtualization and volumes stuff, and provide direct device access to disks and mount points".

### Networking

The networking situation is pretty clear: If you want one of those "land anywhere and NAT/bridge with some auto-generated networking stuff" deployments, you'll probably pay dearly for that behavior in terms of network latency and throughput (compared to bare metal dedicated NICs on normal linux). However, there are options for deploying docker containers (again, may be different from how some people would like to deploy things) that provide either low-overhead or essentially zero-latency-overhead network links for docker. Start with host networking and/or use dedicated IP addresses and NICs, and you'll do much better than the bridged defaults. But you can go to things like Solarflare's NICs (which tend to be common in bare metal low latency environments already), and even do kernel bypass, dedicated spinning-core network stack things that will have a latency behavior no different (on Docker) than if you did the same on bare metal Linux.

Docker (which is "userland as a unit") is not about packing lots of thing into a box. Neither is guest-OS-as-a-unit virtualization. Sure, they can both be used for that (and often are), but the biggest benefit they both give is the ability to ship around a consistent, well captured configuration. And the ability to develop, test, and deploy that exact same configuration. This later turns into being able to easily manage deployment and versioning (including roll backs), and being able to do cool things like elastic sizing, etc. There are configuration tools (puppet/chef/...) that can be used to achieve similar results on bare metal as well, of course (assuming they truly control everything in your image), but the ability to pack up your working stuff as a bunch of bits that can "just be turned on" is a very appealing.

I know people who use virtualization even with a single guest-per-host (e.g. an AWS r3.8xlarge instance type is probably that right now). And people who use docker the same way (single container per host). In both cases, it's about configuration control and how things get deployed, and not at all about packing things in a smaller footprint.

The low latency thing then becomes a "does it hurt?" question. And Docker hurts a lot less than hypervisor or KVM based virtualization does when it comes to low latency, and with the right choices for I/O (dedicated NICs, cores, and devices), it becomes truly invisible.

[On HackerNews](https://news.ycombinator.com/item?id=10747577)

    