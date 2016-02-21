## [Second Life Architecture - The Grid](/blog/2008/12/20/second-life-architecture-the-grid.html)

    

    

**Update:**[Presentation: Second Life’s Architecture](http://www.infoq.com/news/2008/12/Second-Life-Ian-Wilkes). _Ian Wilkes, VP of Systems Engineering, describes the architecture used by the popular game named Second Life. Ian presents how the architecture was at its debut and how it evolved over years as users and features have been added._  

[Second Life](http://secondlife.com/whatis/) is a 3-D virtual world created by its Residents. Virtual Worlds are expected to be more and more popular on the internet so their architecture might be of interest. Especially important is the appearance of open virtual worlds or metaverses.  
_What happens when video games meet Web 2.0? What happens is the [metaverse](http://www.metaverseroadmap.org/overview/)_.

# Information Sources

*   [Second Life runs MySQL](http://conferences.oreillynet.com/presentations/mysql06/wilkes_ian.pdf)

*   [Interview with Ian Wilkes](http://dev.mysql.com/tech-resources/interviews/ian-wilkes-linden-lab.html)

*   [TechTrends: Inside Linden Lab](http://www.springerlink.com/content/a087424077663u0p/)

*   [Town Hall with Cory Linden](http://blog.secondlife.com/2006/12/20/town-hall-with-cory-introductory-transcript/)

*   InformationWeek articles ([1](http://www.informationweek.com/news/software/hosted/showArticle.jhtml?articleID=197800179), [2](http://www.informationweek.com/news/software/open_source/showArticle.jhtml?articleID=198500108)) and [blog](http://www.informationweek.com/blog/main/archives/2007/05/linden_lab_plan.html<br></a>)

*   [Second Life Wiki: Server Architecture](http://wiki.secondlife.com/wiki/Server_architecture)

*   [Wikipedia: Second Life Server](http://en.wikipedia.org/wiki/Second_Life#Server)

*   [Second Life Blog](http://blog.secondlife.com/2007/08/24/more-open-source-our-web-services-libraries/)
*   [Second Life: A Guide to Your Virtual World](http://www.amazon.com/gp/product/0321501667?ie=UTF8&tag=innoblog-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=0321501667)![](http://www.assoc-amazon.com/e/ir?t=innoblog-20&l=as2&o=1&a=0321501667)

# Platform

*   MySQL

*   Apache

*   Squid

*   Python

*   C++

*   Mono

*   Debian

# What's Inside?

### The Stats

*   ~1M active users

*   ~95M user hours per quarter

*   ~70K peak concurrent users (40% annual growth)

*   ~12Gbit/sec aggregate bandwidth (in 2007)

### Staff (in 2006)

*   70 FTE + 20 part time

<cite>"about 22 are programmers working on SL itself. At any one time probably 1/3 of the team is on infrastructure, 1/3 is on new features and 1/3 is on various maintenance tasks (bug fixes, general stability and speed improvements) or improvements to existing features. But it varies a lot."</cite>

### Software

**Client/Viewer**

*   [Open Source client](http://wiki.secondlife.com/wiki/Source_downloads)

*   Render the Virtual World

*   Handles user interaction

*   Handles locations of objects

*   Gets velocities and does simple physics to keep track of what is moving where

*   No collision detection

**Simulator (Sim)**  
Each geographic area (256x256 meter region) in Second Life runs on a single instantiation of server software, called a simulator or "sim." And each sim runs on a separate core of a server.  
The Simulator is the primary SL C++ server process which runs on most servers. As the viewer moves through the world it is handled off from one simulator to another.

*   Runs [Havok 4](http://en.wikipedia.org/wiki/Havok_(software)) physics engine

*   Runs at 45 frames/sec. If it can't keep up, it will attempt time dialation without reducing frame rate.

*   Handles storing object state, land parcel state, and terrain height-map state

*   Keeps track of where everything is and does collision detection

*   Sends locations of stuff to viewer

*   Transmits image data in a prioritized queue

*   Sends updates to viewers only when needed (only when collision occurs or other changes in direction, velocity etc.)

*   Runs [Linden Scripting Language](http://en.wikipedia.org/wiki/Linden_Scripting_Language) (LSL) scripts

*   Scripting has been recently upgraded to the much faster [Mono scripting engine](http://wiki.secondlife.com/wiki/Mono)

*   Handles chat and instant messages
    *   *   One big clustered filesystem ~100TB

        *   Stores asset data such as textures.
        *   [Eventlet](http://wiki.secondlife.com/wiki/Eventlet) is a networking library written in Python. It achieves high scalability by using non-blocking io while at the same time retaining high programmer usability by using coroutines to make the non-blocking io operations appear blocking at the source code level.

        *   [Mulib](http://wiki.secondlife.com/wiki/Mulib) is a REST web service framework built on top of eventlet
        *   2000+ Servers in 2007

        *   ~6000 Servers in early 2008

        *   Plans to upgrade to ~10000 (?)

        *   4 sims per machine, for both class 4 and class 5

        *   Used all-AMD for years, but are moving from the Opteron 270 to the Intel Xeon 5148

        *   The upgrade to "class 5" servers doubled the RAM per machine from 2GB to 4GB and moved to a faster SATA disk

        *   Class 1 - 4 are on 100Mb with 1Gb uplinks to the core. Class 5 is on pure 1Gb

    **Asset Server**

    **MySQL database**

    **Backbone**

    ### Hardware

    _Do you have more details?_

    