## [Why My Soap Film is Better than Your Hadoop Cluster](/blog/2012/6/13/why-my-soap-film-is-better-than-your-hadoop-cluster.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, June 13, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7226/7352800910_1da3f91dea_o.png)

The ever amazing [<span>slime mold</span>](http://highscalability.com/blog/2012/4/9/why-my-slime-mold-is-better-than-your-hadoop-cluster.html) is not the only way to solve complex compute problems without performing calculations. There is another: [soap film](http://en.wikipedia.org/wiki/Soap_film). Unfortunately for soap film it isn’t nearly as photogenic as slime mold, all we get are boring looking pictures, but the underlying idea is still fascinating and ten times less spooky.  

As a quick introduction we’ll lean on Long Ouyang, who has really straightforward  explanation of how soap film works in [<span>Approaching P=NP: Can Soap Bubbles Solve The Steiner Tree Problem In Polynomial</span>](http://www.tjhsst.edu/~rlatimer/techlab06/Students/OuyangPaper06F.pdf).  

It’s computers, so playing the role of the motivating graph problem we have the [<span>Steiner tree problem</span>](http://en.wikipedia.org/wiki/Steiner_tree_problem), which Ouyang explains as:

> <span>Find the minimum spanning tree for a bunch of vertices, given that you can add additional points.</span>

<span>Soap helps solve this problem because:</span>

*   <span>Soap, in water, acts a surfactant, which decreases the surface tension in water.</span>
*   <span>This acts to minimize the surface energy of the liquid.</span>
*   <span>This should minimize surface area (graph weight), and solve the problem.</span>

<span>Dip two glass plates, for example, with pegs between them into soap water, the soap would form a Steiner Tree around the pegs.</span>  

<span>Here’s an example with 4 points from</span> [<span>Valery Ochkov</span>](http://communities.ptc.com/docs/DOC-2555)<span>:</span>  
![](https://lh3.googleusercontent.com/z19lz7HPy6zaFFq9bRjUMNHTobz_Qgle3t8Mh-Lvg2TA58BeZBPsO_i5Z3k5RzKyzyWI1ziyDAEKtuloZgN6ezFI2zurkiwn5nUDiB0dyErhfSFj-g)  

<span>Jim Bergquist has several good soap film examples on his blog. He talks about the Steiner Tree Problem in</span> [<span>Steiner Trees and Minimal Surfaces</span>](http://httprover2.blogspot.com/2010/07/steiner-trees-and-minimal-surfaces.html)<span>:</span>

> <span>There is a physical analog to the Steiner Tree Problem that allows one to find the solutions without doing a single calculation. The soap films of a wire frame model will form a minimal surface. If one uses the positions of a set of parallel wires in the frame to represent the points and eliminates the unwanted surfaces of the bubbles produced by dipping the frame in a soap solution, a set of minimal surfaces will be formed which is analogous to the Steiner tree. The analogy is even better if one looks just at a plane perpendicular to the parallel wires.</span>

<span>Even more on Steiner Trees in</span> [<span>Steiner trees and spanning trees in six-pin soap films</span>](http://arxiv.org/pdf/0806.1340v1.pdf)<span>.</span>  

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7087/7167577455_31c555d818_n.jpg?__SQUARESPACE_CACHEVERSION=1339196291066)</span></span>  

<span>We aren’t just limited to soap films,</span> [<span>three-dimensional microfluidic networks can also be used to solve computationally hard problems</span>](http://www.pnas.org/content/98/6/2961.long)<span>. Here’s an example for the</span> <span>design of a parallel algorithm that uses moving fluids in a three-dimensional microfluidic system to solve a nondeterministically polynomial complete problem (the maximal clique problem) in polynomial time.</span>  

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7224/7167583593_eab207a2ce_n.jpg?__SQUARESPACE_CACHEVERSION=1339196424506)</span></span>  

<span>Maybe one day we can use a REST API to hook our computers into a menagerie of slime mold, soap film, and fluidic networks. Now that’s a specialized cloud.</span>

## <span>Related Articles</span>

*   [<span>Bubble Science Kit</span>](http://www.amazon.com/Thames-Kosmos-Fundamentals-Bubble-Science/dp/B001ALPJG8/)
*   [<span>Soap Films Help to Solve Mathematical Problems</span>](http://www.sciencedaily.com/releases/2011/01/110125123235.htm)
*   [<span>Steiner trees and spanning trees in six-pin soap films</span>](http://arxiv.org/pdf/0806.1340v1.pdf)
*   [<span>Pentagon Soap Films</span>](http://httprover2.blogspot.com/2010/07/pentagon-soap-films.html)
*   [<span>"Minimal Surfaces" a Misnomer?</span>](http://httprover2.blogspot.com/2010/07/minimal-surfaces-misnomer.html)
*   [<span>Steiner Trees and Minimal Surfaces</span>](http://httprover2.blogspot.com/2010/07/steiner-trees-and-minimal-surfaces.html)
*   [<span>Design Challenge Results: “Evolution is Smarter than You Are”</span>](http://pandasthumb.org/archives/2006/08/design-challeng-1.html)
*   [<span>Soap Bubbles, Their Colours and the Forces which Mould Them</span>](http://books.google.com/books?id=EcgCKTPYqCIC&dq=soap%20bubbles%20boys&pg=PA90#v=onepage&q&f=false)
*   [<span>Newtonian systems, bounded in space, time, mass and energy can compute all functions</span>](http://www-compsci.swan.ac.uk/~csjvt/JVTPublications/MarbleRun(Sept05).pdf)
*   [<span>Using three-dimensional microfluidic networks for solving computationally hard problems</span>](http://www.pnas.org/content/98/6/2961.long) <span>(</span>[<span>ppt)</span>](http://bi.snu.ac.kr/Research/MEC/2001/2001-11-02(jylee).ppt)
*   [<span>On Solving NP-complete Problems with Unconventional Models of Computation</span>](http://algo2.iti.kit.edu/schultes/umc/umcasg1.pdf)
*   [<span>Focus: Soap Film Has Memory</span>](http://physics.aps.org/story/v27/st7)
*   [<span>Getting there in much less time with a little soap or slime</span>](http://www.cs4fn.org/optimization/gettingthere.php)
*   [<span>Approaching P=NP: Can Soap Bubbles Solve The Steiner Tree Problem In Polynomial Time?</span> ](http://www.tjhsst.edu/~rlatimer/techlab06/Students/OuyangPaper06F.pdf)<span>(</span>[<span>ppt</span>](http://www.docstoc.com/docs/39238650/Approaching-P=NP-Can-Soap-Bubbles-Solve-The-Steiner-Tree)<span>)</span>
*   <span>[The Use of Soap Film to Solve Torsion Problems?](http://home.iitk.ac.in/~ag/ME321/Torsion_SoapFilm.pdf)</span>

</div>