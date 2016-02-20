## [The Machine: HP's New Memristor Based Datacenter Scale Computer - Still Changing Everything](/blog/2014/12/16/the-machine-hps-new-memristor-based-datacenter-scale-compute.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, December 16, 2014 at 8:52AM</span></div>

<div class="body">

<span id="docs-internal-guid-d4600c2d-4631-37b1-214c-ac232ff78b8c"></span>

> <span>The end of Moore’s law is the best thing that’s happened to computing in the last 50 years. Moore’s law has been a tyranny of comfort. You were assured your chips would see a constant improvement. Everyone knew what was coming and when it was coming. The entire semiconductor industry was held captive to delivering on Moore’s law. There was no new invention allowed in the entire process. Just plod along on the treadmill and do what was expected. We are finally breaking free of these shackles and entering what is the most exciting age of computing that we’ve seen since the late 1940s. Finally we are in a stage where people can invent and those new things will be tried out and worked on and find their way into the market. We’re finally going to do things differently and smarter.</span>
> 
> <span>--</span> [<span>Stanley Williams</span>](http://en.wikipedia.org/wiki/R._Stanley_Williams) <span>(paraphrased)</span>

![](https://farm9.staticflickr.com/8613/15986951346_1400d09b7f_m.jpg)

HP has been working on a radically new type of computer, enigmatically called _The Machine_ (not [<span>this machine</span>](http://personofinterest.wikia.com/wiki/The_Machine)). The Machine is perhaps the largest R&D project in the history of HP. It’s a complete rebuild of both hardware and software from the ground up. A massive effort. HP hopes to have a small version of their datacenter scale product up and running in two years.

<span>The story began when we first met HP’s</span> [<span>Stanley Williams</span>](http://en.wikipedia.org/wiki/R._Stanley_Williams) <span>about four years ago in</span> [<span>How Will Memristors Change Everything?</span>](http://highscalability.com/blog/2010/5/5/how-will-memristors-change-everything.html) <span>In the latest chapter of the memristor story, Mr. Williams gives another incredible talk:</span> [<span>The Machine: The HP Memristor Solution for Computing Big Data</span>](https://mediacosmos.rice.edu/app/plugin/embed.aspx?ID=vVUTzFCE006yZu3TF1sXKg&displayTitle=false&startTime=0&autoPlay=false&hideControls=false&showCaptions=false&width=420&height=236&destinationID=URka-_E5-0-e4girH4pDPQ&contentID=vq2oQtAqPkeAqm5F6uSnqA&pageIndex=1&pageSize=10)<span>, revealing more about how The Machine works.</span>

The goal of The Machine is to **collapse the memory/storage hierarchy**. Computation today is energy inefficient. **Eighty percent of the energy** and vast amounts of time are spent **moving bits** between hard disks, memory, processors, and multiple layers of cache. Customers end up spending more money on power bills than on the machines themselves. So the machine has no hard disks, DRAM, or flash. Data is held in power efficient memristors, an ion based nonvolatile memory, and data is moved over a photonic network, another very power efficient technology. When a bit of information leaves a core it leaves as a pulse of light.

<span>On graph processing benchmarks The Machine reportedly performs 2-3 orders of magnitude better based on energy efficiency and one order of magnitude better based on time. There are no details on these benchmarks, but that’s the gist of it.</span>

<span>**The Machine puts data first**</span><span>. The concept is to build a system around nonvolatile memory with processors sprinkled liberally throughout the memory. When you want to run a program you</span> <span>**send the program to a processor near the memory**</span><span>, do the computation locally, and send the results back. Computation uses a wide range of heterogeneous multicore processors. By only transmitting the bits required for the program and the results the savings is enormous when compared to moving terabytes or petabytes of data around.</span>

<span>The Machine is not targeted at standard HPC workloads. It’s not a LINPACK buster. The problem HP is trying to solve for their customers is where a customer wants to perform a query and figure out the answer by searching through a gigantic pile of data. Problems that need to store lots of data and analyze in realtime as new data comes in</span>

**Why is a very different architecture needed for building a computer?** Computer systems can’t not keep up with the flood of data that’s coming in. HP is hearing from their customers that they need the ability to handle ever greater amounts of data. **The amount of bits that are being collected is growing exponentially faster than the rate at which transistors are being manufactured**. It’s also the case that information collection is growing faster than the rate at which hard disks are being manufactured. HP estimates there are 250 trillion DVDs worth of data that people really want to do something with. Vast amount of data are being collected in the world are never even being looked at.

<span>So something new is needed. That’s at least the bet HP is making. While it’s easy to get excited about the technology HP is developing, it won’t be for you and me, at least until the end of the decade. These will not be commercial products for quite a while. HP intends to use them for their own enterprise products, internally consuming everything that’s made. The idea is we are still very early in the tech cycle, so high cost systems are built first, then as volumes grow and processes improve, the technology will be ready for commercial deployment. Eventually costs will come down enough that smaller form factors can be sold.</span>

What is interesting is HP is essentially building its own cloud infrastructure, but instead of leveraging commodity hardware and software, they are building their own best of breed custom hardware and software. A cloud typically makes available vast pools of memory, disk, and CPU, organized around instance types which are connected by fast networks. Recently there’s a move to treat these resource pools as independent of the underlying instances. So we are seeing high level scheduling software like [<span>Kubernetes and Mesos</span>](https://mesosphere.com/2014/07/10/mesosphere-announces-kubernetes-on-mesos/) becoming bigger forces in the industry. HP has to build all this software themselves, solving many of the same problems, along with the opportunities provided by specialized chips. You can imagine programmers programming very specialized applications to eke out every ounce of performance from The Machine, but what is more likely is HP will have to create a very sophisticated scheduling system to optimize how programs run on top of The Machine. **What's next in software is the evolution of a kind of Holographic Application Architecture, where function is fluid in both time and space, and identity arises at run-time from a two-dimensional structure. **Schedule optimization is the next frontier being explored on the cloud.

The talk is organized in two broad sections: hardware and software. Two-thirds of the project is software, but Mr. Williams is a hardware guy, so hardware makes up the majority of the talk.  The hardware section is based around the idea of **optimizing the various functions around the physics that is available**: **electrons compute; ions store; photons communicate**.

<span>Here’s is my gloss on Mr. Williams</span> [<span>talk</span>](https://mediacosmos.rice.edu/app/plugin/embed.aspx?ID=vVUTzFCE006yZu3TF1sXKg&displayTitle=false&startTime=0&autoPlay=false&hideControls=false&showCaptions=false&width=420&height=236&destinationID=URka-_E5-0-e4girH4pDPQ&contentID=vq2oQtAqPkeAqm5F6uSnqA&pageIndex=1&pageSize=10)<span>. As usual with such a complex subject much can be missed. Also, Mr. Williams tosses huge interesting ideas around like pancakes, so viewing the talk is highly recommended. But until then, let’s see The Machine HP thinks will be the future of computing….</span>

## <span>Why is a different architecture needed for building a computer?</span>

Compute architecture hasn’t changed in 60 years. The [<span>Von Neumann architecture</span>](http://en.wikipedia.org/wiki/Von_Neumann_architecture) was only meant to last a few years, but we are still using it. Data has to be moved around back and forth between memory, the processor, and the hard disk. **The process of moving data around is completely dominating computing today**. It’s what’s taking so much time and using so much power in datacenters.

Moore’s law is coming to an end real soon now. We have reached 14 nm (~50 atoms across) transistors. 7 nm is considered the physical limit. 10 nm will be ready in two years and 7 nm will be ready by 2020\. **We have seen roughly a factor 100 computing efficiency every decade for five decades**. That’s 10 orders of magnitude improvement in computing efficiency, which is the biggest improvement any human technology has ever seen.

How are we looking at breaking out of a Moore’s law paradigm? **Optimize the various functions around the physics that is available**. For 50 years the thing to which we attached computing was the electron. That won’t change. Electrons are ideal for carrying out a computation process. They are small, they are very light, you put a voltage and they move very rapidly, yet they have enough mass that they can be localized in a small region of space, and they interact very strongly so you can perform logic operations with electrons. Electrons are going to continue to be the computational part of the engine.

<span>**Instead of using electrons for storage we’ll be using ions**</span><span>. The reason is ions are classical mechanical particles, they are heavy, you put them someplace and they stay there, and you can come back in a century and where you put the ion is still where it is. Ions are truly a means for non-volatile storage of information. Ions can be put in small boxes and the boxes can be stacked up, making for high density low cost storage when compared to electrons.</span>

<span>**Data will be moved around using photons**</span><span>. The reason for that is photons have no mass so they move at the speed of light. Many different colors of light can be put in the same waveguide so you save tremendously on space and on energy usage. Much higher communications bandwidth and much lower power dissipation.  </span>

<span>The hardware around the machine is being optimized around each of these capabilities in order to do the computing, the storage, the communication. By building them appropriately there are favorable non-linear interactions.</span>

<span>**Electrons will continue to be the computing engine of the future**</span><span>, through through the proliferation of heterogeneous multicore chips. Multicore chips today exist because of the failure of Moore’s Law. The reason we have multicore chips is because the power dissipation of one large really fast chip would vaporize the chip. To fix the problem the chip is chopped up into smaller pieces. The problem has been multicores are harder to program, but we’re starting to figure that out and people are now thinking multicores is a good thing.</span>

Instead of all the cores being the same, the cores will be different. Instead of having 256 cores all the same on a chip, there will be different cores optimized for different types of functions.

**The challenge is Dark Silicon**. If all the cores on a chip were lit up at the same time the **chip would be vaporized**. You are never actually using all the cores on a chip. At 10nm you have to have half the cores unused at one time when compared to 14nm. If cores are function specific, it then makes sense for them to go dark when not it use. Specialized accelerators are cores that can run one type of computation super efficiently, but it can only do that one thing. For solving crystal structures, if you have a specialty core that can do a fourier transform in one clock cycle, that would be very helpful.

<span>You don’t need a big team of people to design the processors. There are companies that sell processor designs. You can work on just designing a couple of the specialized accelerators. These designs can be put out to bid. The balance of power will shift to designers and away from manufacturing, who will become low cost bidders.</span>

## **Memristors are Ion Boxes**

<span>**DRAM essentially takes an electron and puts it in a box**</span><span>. As the sizes of the boxes get smaller and smaller it’s much more difficult to keep the electrons inside, which means the DRAM is having to be refreshed much more frequently. It’s getting more difficult to reliable store electrons in DRAM or flash.</span>

<span>**The solution is to use ions, which is a charged atom, in a storage medium, instead of electrons**</span><span>. Atoms are better behaved than electrons because they are so much more massive. Atoms are classical mechanical particles, so even in 10nm box they are happy as a particle. Put an ion in a box, apply a voltage across the box, and the ion will move inside the box. Turn the voltage off and the ion will stay where it was, even for millions of years, even in a very small box. This is very stable non-volatile type of memory.</span>

**The state of ion can be** **read by applying a small voltage across the box and measuring the resistance**. The ion doesn’t move because the voltage isn’t high enough. Resistance of the box depends on where the ion is inside. When ions are uniformly distributed through the box you get a low resistance, which you can call a 1, if all the ions are pushed to one side of the box there’s a very high resistance, which you can call a 0.

**The ion boxes are memristors**. Compared to DRAM memristors are a **factor of 4 slower**, which can be moderated by parallel access. Memristors have much lower power requirements because no refresh is needed because the ions don’t leak out. The **bit density is much higher** because the boxes can be stacked close together. The chips are **much cheaper per bit**. They have been making chips for several years based on these ideas. In the talk the chip shown was 2.5 years old and used 50nm technology, the implication being the current iterations are much better, but they could not possibly comment.

<span>The idea is to collapse the memory/storage hierarchy and the process removing all the overhead and time of moving data through all the layers in the system. The problem with computation today is it is energy inefficient. 80% of the energy is spent and vast amounts of time are spent taking bits and moving them from hard disk, to memory, to processor, through multiple layers of cache, and then back down and out. Most of an OS is concerned with the mechanics of all this data handling.</span>

## <span>Using Photons to Communicate</span>

<span>Photons destroy distance. Nothing is faster than the speed of light.</span>

So why aren’t we already using photonics on chips like we are in telco networks? The cost of the interconnect would be a thousand times the cost of the computer itself. **The amount of data moving through a rack is larger than the amount of data moving through the North American telco networks**.

**What HP has been working on is reducing costs**. Light is a superior way of moving data around. It has high bandwidth (multiple wavelengths in a single wire), energy efficient (no energy loss),  low latency (no repeaters), and space efficient (saves on the pin count).

<span>What they have working now is the integration of photonics on silicon wafers using standard silicon processes.</span>

<span>This technology is used to create a communications fabric in the computer. The computer is a box with blades. A blade has memristor memory chips and multicore processors. The blades as they are inserted automatically reconfigures the photonic network so everything can talk to each other. The next thing to do is to take the boxes and stack them together to make a rack. Again, the network automatically configures itself. Next is to be able to assemble a datacenter’s worth of the racks and then have them all come up and be configured correctly.</span>

<span><span class="full-image-block ssNonEditable"><span>![](https://farm9.staticflickr.com/8568/15406252104_273a47ff2a.jpg?__SQUARESPACE_CACHEVERSION=1418657578906)</span></span>  
</span>

## <span>Software - The Two Thirds</span>

All the hardware so far is just ⅓ of the project. On top of The Machine must ride an operating system. An OS can take decades to develop. **HP is developing a stripped down version of Linux** that is very fast because all the logic dealing with the internal movement of data has been ripped out. They are also creating an **open source community to build a native version of an operating system**, along with all the applications that go on top, like analytics, visualization, exabyte-scale algorithms, and million-node management.

<span>Trying to cut the time frame from research, to development, to product as short as possible. Immense amount of work. It’s a very risky project. Huge scale. Huge ambition. Huge risk.</span>

## <span>A big warning to anyone trying to do something radically new.</span>

**Lesson learned**: they are having problems that could have never been predicted. And that’s after doing so much work to make sure their processes could be built with standard manufacturing processes.

What would happen if you tried to bring in something really different like carbon nanotubes or graphene? The challenges they’ve had with all standard materials have been huge. Bringing in something exotic will take decades before it can actually be built with a real manufacturing process. If you are going to get into this and do it big, you have to **do something as close as possible to what already exists**. You can’t stray very far from that.

## <span>Related Articles</span>

*   [<span>How Will Memristors Change Everything?</span>](http://highscalability.com/blog/2010/5/5/how-will-memristors-change-everything.html)

*   [<span>We're One Step Closer To Superfast Computers That Run On Light Instead Of Electricity</span>](http://www.fastcolabs.com/3039445/were-one-step-closer-to-superfast-computers-that-run-on-light-instead-of-electricity)

*   <span>Google is betting on</span> [<span>Quantum Computers</span>](https://plus.google.com/+QuantumAILab/posts)

*   [<span>Turing's Cathedral</span>](http://www.amazon.com/dp/0375422773) <span>- By George Dyson</span>

*   <span>[The New Style of IT](http://www.hipeac.net/system/files/HiPEAC14-faraboschi-pub.pdf)</span>

</div>