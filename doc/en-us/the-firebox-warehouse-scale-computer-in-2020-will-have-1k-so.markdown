## [The FireBox Warehouse Scale Computer in 2020 Will Have 1K Sockets, 100K Cores, 100PB NV RAM, and a 4Pb/s Network](/blog/2014/9/17/the-firebox-warehouse-scale-computer-in-2020-will-have-1k-so.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 17, 2014 at 8:56AM</span></div>

<div class="body">

That's the eye popping prediction from Krste Asanović, [University of California, Berkeley](https://aspire.eecs.berkeley.edu/), in a presentation he gave at [FAST '14](https://www.usenix.org/conference/fast14) titled: [FireBox: A Hardware Building Block for 2020 Warehouse-Scale Computers](https://www.youtube.com/watch?v=dffsqiHyylQ) ([pdf](https://www.usenix.org/sites/default/files/conference/protected-files/fast14_asanovic.pdf)).

FireFox looks system like this:

![](https://farm6.staticflickr.com/5563/15065833919_d6f96bf82f.jpg)

## Trends in Warehouse Scale Computers (WSC)s:

*   **Small scale services are the way to build large code bases**. The rationale is large systems are built from small systems and building a large system from scratch almost always fails.
*   **Tail tolerance is the new fault tolerance**. WSCs must be fault tolerant, but predictable performance has become important as well. So tail tolerance is now a requirement. Results must be returned in a reliable and predictable amount of time. For programming techniques to achieve tail tolerance take a look at [Google: Taming The Long Latency Tail - When More Machines Equals Worse Results](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html). In hardware: a faster network, reduce per message overhead, partition resources (bandwidth, cores, caches, memories).
*   **Moving towards a solid state dacenter.** New conventional wisdom on memory hierarchy: fast DRAM buffers are moving closer to the processor, bulk memory will become NVRAM, disk is cold storage. 3D flash will replace 2D flash or there will be a new flash successor. Flash is pretty slow so they are assuming some new NVRAM technology will be available. 
*   **Data encrypted all the time**. Data is never stored or transmitted in the clear.
*   **Moore's law is slowing to between 3 and 5+ years**. Smaller from here on out costs more, so smaller transistors will be more expensive. By 2020 Moore's law is over. For memory scaling may still occur through 3D flash or new NVM, but for logic the game is over, CMOS is it. All innovation has to happen above the transistor level.
*   **Custom hardware design is the new reality**. If transistor scaling has stopped all the system improvements have to come from above the processor. It will require more specialized hardware to do more. For WSCs white boxes are at an end. To support SOA, tail tolerance/fault tolerance/detection/recovery/prediction of faults will require custom hardware, not off the shelf chips. Custom high radix switches, custom racks and cooling, System on a Chip (SoC) integrating processors & NIC. There's a large upswing in the number of hardware engineer jobs being advertised at datacenter companies. The upside is slower progress in transistor growth will allow more time for tool chains to catch up and allow for really customization. [Chisel](https://chisel.eecs.berkeley.edu/), for example, is an open-source hardware construction language developed at UC Berkeley.
*   **Move to an open Instruction Set Architecture (ISA)**. There's not reason to be tied to AMD or Intel. It's just an interface. [RISC-V](http://riscv.org/) is one example. 1st generation WSC used open-source software. 2nd generation WSC is using open-source hardware (OpenCompute) and OpenFlow API for networking. 3rd generation could be open-source chip design. They plan to release their FireBox WSC chip generator. 
*   **Message passing simplifies programming of large servers**. It's a simple and consistent programming abstraction. Talking between any two cored will take ~100ns, 2X DRAM latency.

##  FireBox Overview:<span style="font-size: 12px;"> </span>

*   **1000 sockets in a server. 100K cores in a server**. These are individual System on Chips that have high bandwidth memory attached. Each socket has a logic chip plus associated high bandwidth DRAM right next to the CPU. 
*   **100PB  NonVolatile Memory spread over up to 1000 memory modules**.
*   **Goal is to make the network invisible to software**. It's all connected with integrated silicon photonics. Provides much greater bandwidth density than does electronics. All components are connected with terabit/second optical fibers with a huge switch in the middle that connects everything together.  
*   The box is so big to reduce management costs, that is manage fewer boxes. 
*   **Goal is to support huge in-memory (NVM) databases directly**. Current systems are not designed for this. Data has to be sharded across nodes and send messages to keep everything in consistent. 
*   **Rethinks how software talks to the network**. No need to go to the device driver to get something done and modern processors are terrible at talking to IO. Bring the network right into the core and make it very low overhead to send and receive messages. 
*   **Custom SoC hardware implements the above features along with encryption**. The only time data is being operated on in the clear is inside the SoC. 
*   And it's all built with an open-source hardware generator. 
*   Cores will be homogonous in order to simplify resource management. Specialization is provided by a suite of vector unit accelerators (not GPUs) around each core. Programs can operate directly on data in place without having to ship it somewhere else.

Obviously this is a complicated talk so viewing the source video is highly recommended. There are many more details in the talk.

</div>