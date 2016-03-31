## [Why My Soap Film is Better than Your Hadoop Cluster](/blog/2012/6/13/why-my-soap-film-is-better-than-your-hadoop-cluster.html)

    

    

![](http://farm8.staticflickr.com/7226/7352800910_1da3f91dea_o.png)

The ever amazing [    slime mold    ](http://highscalability.com/blog/2012/4/9/why-my-slime-mold-is-better-than-your-hadoop-cluster.html) is not the only way to solve complex compute problems without performing calculations. There is another: [soap film](http://en.wikipedia.org/wiki/Soap_film). Unfortunately for soap film it isn’t nearly as photogenic as slime mold, all we get are boring looking pictures, but the underlying idea is still fascinating and ten times less spooky.  

As a quick introduction we’ll lean on Long Ouyang, who has really straightforward  explanation of how soap film works in [    Approaching P=NP: Can Soap Bubbles Solve The Steiner Tree Problem In Polynomial    ](http://www.tjhsst.edu/~rlatimer/techlab06/Students/OuyangPaper06F.pdf).  

It’s computers, so playing the role of the motivating graph problem we have the [    Steiner tree problem    ](http://en.wikipedia.org/wiki/Steiner_tree_problem), which Ouyang explains as:

>     Find the minimum spanning tree for a bunch of vertices, given that you can add additional points.    

    Soap helps solve this problem because:    

*       Soap, in water, acts a surfactant, which decreases the surface tension in water.    
*       This acts to minimize the surface energy of the liquid.    
*       This should minimize surface area (graph weight), and solve the problem.    

    Dip two glass plates, for example, with pegs between them into soap water, the soap would form a Steiner Tree around the pegs.      

    Here’s an example with 4 points from     [    Valery Ochkov    ](http://communities.ptc.com/docs/DOC-2555)    :      
![](https://lh3.googleusercontent.com/z19lz7HPy6zaFFq9bRjUMNHTobz_Qgle3t8Mh-Lvg2TA58BeZBPsO_i5Z3k5RzKyzyWI1ziyDAEKtuloZgN6ezFI2zurkiwn5nUDiB0dyErhfSFj-g)  

    Jim Bergquist has several good soap film examples on his blog. He talks about the Steiner Tree Problem in     [    Steiner Trees and Minimal Surfaces    ](http://httprover2.blogspot.com/2010/07/steiner-trees-and-minimal-surfaces.html)    :    

>     There is a physical analog to the Steiner Tree Problem that allows one to find the solutions without doing a single calculation. The soap films of a wire frame model will form a minimal surface. If one uses the positions of a set of parallel wires in the frame to represent the points and eliminates the unwanted surfaces of the bubbles produced by dipping the frame in a soap solution, a set of minimal surfaces will be formed which is analogous to the Steiner tree. The analogy is even better if one looks just at a plane perpendicular to the parallel wires.    

    Even more on Steiner Trees in     [    Steiner trees and spanning trees in six-pin soap films    ](http://arxiv.org/pdf/0806.1340v1.pdf)    .      

        ![](http://farm8.staticflickr.com/7087/7167577455_31c555d818_n.jpg?__SQUARESPACE_CACHEVERSION=1339196291066)          

    We aren’t just limited to soap films,     [    three-dimensional microfluidic networks can also be used to solve computationally hard problems    ](http://www.pnas.org/content/98/6/2961.long)    . Here’s an example for the         design of a parallel algorithm that uses moving fluids in a three-dimensional microfluidic system to solve a nondeterministically polynomial complete problem (the maximal clique problem) in polynomial time.      

        ![](http://farm8.staticflickr.com/7224/7167583593_eab207a2ce_n.jpg?__SQUARESPACE_CACHEVERSION=1339196424506)          

    Maybe one day we can use a REST API to hook our computers into a menagerie of slime mold, soap film, and fluidic networks. Now that’s a specialized cloud.    

##     Related Articles    

*   [    Bubble Science Kit    ](http://www.amazon.com/Thames-Kosmos-Fundamentals-Bubble-Science/dp/B001ALPJG8/)
*   [    Soap Films Help to Solve Mathematical Problems    ](http://www.sciencedaily.com/releases/2011/01/110125123235.htm)
*   [    Steiner trees and spanning trees in six-pin soap films    ](http://arxiv.org/pdf/0806.1340v1.pdf)
*   [    Pentagon Soap Films    ](http://httprover2.blogspot.com/2010/07/pentagon-soap-films.html)
*   [    "Minimal Surfaces" a Misnomer?    ](http://httprover2.blogspot.com/2010/07/minimal-surfaces-misnomer.html)
*   [    Steiner Trees and Minimal Surfaces    ](http://httprover2.blogspot.com/2010/07/steiner-trees-and-minimal-surfaces.html)
*   [    Design Challenge Results: “Evolution is Smarter than You Are”    ](http://pandasthumb.org/archives/2006/08/design-challeng-1.html)
*   [    Soap Bubbles, Their Colours and the Forces which Mould Them    ](http://books.google.com/books?id=EcgCKTPYqCIC&dq=soap%20bubbles%20boys&pg=PA90#v=onepage&q&f=false)
*   [    Newtonian systems, bounded in space, time, mass and energy can compute all functions    ](http://www-compsci.swan.ac.uk/~csjvt/JVTPublications/MarbleRun(Sept05).pdf)
*   [    Using three-dimensional microfluidic networks for solving computationally hard problems    ](http://www.pnas.org/content/98/6/2961.long)     (    [    ppt)    ](http://bi.snu.ac.kr/Research/MEC/2001/2001-11-02(jylee).ppt)
*   [    On Solving NP-complete Problems with Unconventional Models of Computation    ](http://algo2.iti.kit.edu/schultes/umc/umcasg1.pdf)
*   [    Focus: Soap Film Has Memory    ](http://physics.aps.org/story/v27/st7)
*   [    Getting there in much less time with a little soap or slime    ](http://www.cs4fn.org/optimization/gettingthere.php)
*   [    Approaching P=NP: Can Soap Bubbles Solve The Steiner Tree Problem In Polynomial Time?     ](http://www.tjhsst.edu/~rlatimer/techlab06/Students/OuyangPaper06F.pdf)    (    [    ppt    ](http://www.docstoc.com/docs/39238650/Approaching-P=NP-Can-Soap-Bubbles-Solve-The-Steiner-Tree)    )    
*       [The Use of Soap Film to Solve Torsion Problems?](http://home.iitk.ac.in/~ag/ME321/Torsion_SoapFilm.pdf)    

    