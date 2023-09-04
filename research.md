---
layout: page
title: Research
permalink: /research/
---
<span style="color:grey">(NOTE: I just finished my PhD and started a new position, so my research will develop and change over the next few months. I will update this page as I go along. For now, the text below describes the main focus during my PhD)</span>



<br>
<br>

My research focuses on interactions amongst genes, and how these interactions relate to cell type and state. An immediate problem here is that neither genetic interactions, nor genes, nor cell type/state, are properly defined.

<br>

The three fundamental questions we think about are:
- What is the role of higher order interactions amongst genes?
- What are cell types and states?
- How does causality manifest itself in complex systems? 

<br>

We try to address all three by describing a cluster of cells as a network of interactions amongst the genes they express. Inspired by the role of interactions in statistical physics <a href="https://arxiv.org/abs/2006.06010" target="_blank">we define interactions</a> in a model-free way that can be generalised to higher orders. We can then associate a <a href="https://en.wikipedia.org/wiki/Hypergraph" target="_blank">hypergraph</a> of interactions to each cluster of cells. This approach offers a number of advantages: 

<br>

- It allows us to ask new questions about genetic interactions: How common are interactions amongst more than two genes? What biological processes do higher order interactions correspond to? 

- Traditionally, cell types are defined with respect to mean expression of their genes. In our framework, mean expression can be seen as a kind of zeroth-order interaction. Our approach is thus a natural generalisation, characterising a cluster of cells by <em>all</em> orders of the internal interactions.  

- Finally, humans like causality. We prefer explanations that identify a cause for each observed phenomenon, but this is often hard in complex systems. To see how our definition of interaction relates to causal structure, we simulate causal graphs, and see how causal structure manifests itself in the hypergraph of interactions. 