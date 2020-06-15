---
layout: page
title: Research
permalink: /research/
---

<!-- COMING SOON: Something about my research. -->


<p>In September 2018 I came to Edinburgh to join the <a href="https://www.ed.ac.uk/mrc-human-genetics-unit/research/ponting-group">group of Chris Ponting</a> at the MRC Institute of Genetics & Molecular Medicine. My research is co-supervised by Luigi Del Debbio and Ava Khamseh at the <a href="https://higgs.ph.ed.ac.uk">Higgs Centre for Theoretical Physics</a>. We try to make sense of genetic interactions, and hope to contribute to the understanding of <a href="https://en.wikipedia.org/wiki/Gene_regulatory_network">gene regulatory networks</a> and the difference between <a href="https://www.humancellatlas.org">cell types</a>. We do this by learning interactions from large gene expression (scRNA-seq) data sets, and investigating the role of higher order couplings. What all of this means is explained below.</p> <br>


<h2>Biology </h2>
<p>
	We have come <a href="https://www.ebi.ac.uk/gwas/">very far</a> in explaining the differences between us in terms of variation in our DNA. There is another, perhaps more mysterious source of heterogeneity though: the variation between individual cells. Each cell in your body contains the same DNA (modulo <a href="https://en.wikipedia.org/wiki/Ploidy">ploidy</a>, <a href="https://en.wikipedia.org/wiki/Mosaic_(genetics)">mosaicism</a>, cancer, and other "higher order" genetics), yet teeth, hair, skin and brain look very different, even at single-cell resolution. <br>
	Our current best explanation for this is that the genome as a whole is a complex regulatory system that changes its behaviour in response to internal and external signals, and can result in very different cells by changing this regulation. While the behaviour is very complex, the flow of regulatory information seems to pass through only three classes of molecules: DNAs, RNAs, and proteins. The zeroth order dynamics are linear - information flows from DNA to RNA trough transcription, and from RNA to proteins through translation. However, in practice, it's better represented as the diagram below:
	<section class="center">
		<img src="/assets/central_dogma.png" width="300">
	</section>
	I.e. the three classes of molecules are constantly interacting and communicating. I dashed the arrow from DNA directly to proteins, since while there are examples of this (<a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3740156/">nucleosome positioning</a>, <a href="https://en.wikipedia.org/wiki/DNA_polymerase#Function">conformational changes in DNA polymerase</a>, and perhaps even <a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5238726/">DNA mediated protein splicing</a>), they are limited to proteins that are direcly linked to DNA itself, or not completely accepted (if you know of any other examples, let me know!).
</p><br>

<p> Ideally, if we want to understand cell identity, we would measure all molecules in a cell over time, and try to find patterns associated with things we're interested in, like cancer, aging, stemness etc. These measurements are hard. While we can read the nucleotide sequence of DNA very well, it's regulatory degree of freedom is it's three-dimensional configuration, which is <a href="https://en.wikipedia.org/wiki/Chromosome_conformation_capture">hard</a> to measure. For proteins, it's even <a href="https://en.wikibooks.org/wiki/Structural_Biochemistry/Proteins/X-ray_Crystallography">harder</a>. Luckily, for RNA molecules, the relevant degrees of freedom seem to be their sequence, and concentration. That is nice because those are <a href="https://en.wikipedia.org/wiki/RNA-Seq">"easy"</a> to measure.
We can map a measured RNA molecule back to its gene of origin, and define a gene's activity by how many of it's corresponding RNA molecules we find. 
There's a company called <a href="https://www.10xgenomics.com">10X Genomics</a> that has done exactly this for over a million cells from the brain of a mouse. <br>

The fundamental units of information that travel through this network are genes. The word gene is actually not very rigorously defined, so we might as well take the previous sentence as a definition of the word gene, and thus cheat our way out of all the caveats that usually come with any definition in biology. Our hypothesis is that there is a meaningful description of the system as a whole, the cell, in terms of interactions between these genes. We still need to think about what we mean by interaction. </p> <br>

<h2>Interactions</h2>
<p>There are regions in the genome called enhancers and promotors. They can be associated to a genes far away (separated by >1M base pairs) on the genome. Somehow, proteins binding to these enhancers and promotors can alter which genes are transcribed, and at which rate. Nobody actually knows how this works though. One theory I particularly like is the <a href="https://www.sciencedirect.com/science/article/pii/S009286741730185X">phase separation model</a>, but is has not been conclusively shown to be an important mechanism. Regardless of the exact mechanism of regulation, effects like this induce a notion of genetic interaction, since genes can thus influence other genes. The most basic examples of this are pairwise activation and inhibition. Without a clear map of the gene regulatory network, we can not a priori exclude the possibility of more complex, higher order interactions. In fact, since a gene can have many enhancers, there can be a combinatorially high number of higher order interactions. In elaborate experiments with yeast, <a href="https://www.mendeley.com/catalogue/0bb93e22-4a57-3dc4-b52c-9d2970cb71ab/?utm_source=desktop&utm_medium=1.19.4&utm_campaign=open_catalog&userDocumentId=%7B7377d51b-b43f-47bd-b4b6-026d03e3c5f1%7D">higher order interactions have been shown to be very common</a>.
</p><br>



<h2>Physics</h2>
This is where ideas from physics come in. In statistical physics and quantum field theory, we describe systems of many interacting parts through the effective interactions between their degrees of freedom. Effective here means that we do not care about the precise mechanism, but rather about what interactions would be necessary to explain the behaviour we see, if the variables we look at were the fundamental ones. That means you only find meaningful interactions if there is indeed a sense in which you've focused on the right variables, and can indeed "integrate the others out". As an example of how this can go wrong, consider <a href="https://en.wikipedia.org/wiki/Berkson%27s_paradox">Berkson's paradox</a>. 

Once you have selected a set of variables as your fundamental ones, there is still the technical problem of inferring these effective interactions. That is a problem we currently solve with machine learning. In a <a href="https://arxiv.org/abs/1810.11503">2018 paper</a> by my supervisors it was shown that a class of neural networks called restricted Boltzmann machines are very good at estimating these effective interactions. One thing you lose in this transition to effective interactions in directionality. Classic pictures of gene regulation have a causal direction. Gene A activates gene B, but the converse does not need to be true. 

In the physics models we draw inspiration from, interactions appear symmetrically. The system as a whole is described by an energy function $$E(\mathbf{s})$$, where $$\mathbf{s}$$ is the vector of degrees of freedom. One of the simplest models is the <a href="https://en.wikipedia.org/wiki/Ising_model" target="_blank">Ising model</a>, defined by the following energy function:

$$E(\mathbf{s}) = \sum_{i=1}^N H_i s_i + \sum_{i, j=1}^N J_{ij} s_i s_j$$

The parameter $$J_{ij}$$ is then naturally thought of as an interaction between variables $$s_i$$ and $$s_j$$ since $$\frac{\partial E(\mathbf{s})}{\partial s_i} = J_{ij} s_j$$, and vice versa. We can generalise this to higher order interactins by simply writing

$$\begin{align}
	E(\mathbf{s}) &= \sum_{i=1}^N H_i s_i + \sum_{i, j=1}^N J_{ij} s_i s_j + \sum_{i, j, k=1}^N K_{ijk} s_i s_j s_k +  \sum_{i, j, k, l=1}^N L_{ijkl} s_i s_j s_k s_l + ... \\ 
	&= \sum_n \sum_{i_1, ..., i_n=1}^N J^{(n)}_{i_1, ..., i_n} s_{i_1}...s_{i_n} 
\end{align} $$

Where now $$J^{(n)}$$ are an $$n$$-th order interactions. These interactions are symmetric in their indices, and therefore don't have a causal direction, so they are harder to interpret. Why be interested in interactions that you can not even interpret? Well, in <a href="https://journals.aps.org/pr/abstract/10.1103/PhysRev.106.620">one of my favourite papers of all time</a>, E.T. Jaynes showed that these $$n$$-th order interactions are exactly the interactions in the maximum entropy model after you've observed $$n$$ moments of your data. They are therefore the interactions that introduce the least amount of bias. 

I'm currently comparing different ways of estimating such symmetric interactions between genes, using the data from mouse brains mentioned above. I'm trying to make sure that I'm using the right set of genes, and the right set of cells, but that the results are robust to this choice. That already turned out to be <a href="https://en.wikipedia.org/wiki/Hofstadter%27s_law">a lot of work</a>. Hopefully, we can soon confidently quantify and characterise the higher order genetic interactions in mouse brains, and how they differ between cell types. 



