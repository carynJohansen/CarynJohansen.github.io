---
title: "Disregulation of Gene Expression in Corn"
layout: splash
permalink: /maizereg/
header: 
    overlay_image: /assets/images/unnamed-chunk-10-1.png
author: "Caryn Johansen"
author_profile: true
---

## Short Summary

At U.C. Davis, my main interest was working to understand how plants responded to and adapted to their environments. I was particularly interested in crop varieties grown in extreme environments, and how crosses between crop varieties altered their crop yield.

To study this, I was using a moledule called ribonucleic acid, RNA, which is a metric of gene expression. In this study, I was measuring the gene expression of high performed corn varieties that had been crossed to corn's wild relative, [teosinte](https://teosinte.wisc.edu/questions.html).

## My Question

Are there genes whose gene expression change are significantly different from the gene expression of either of the parents?

As a starting point, before generating my own data, I used gene expression data from [Hufford et al.](http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1003477)

## Tools and Methods

Here is my general work flow:

1 __Project Set-Up__


For my sanity and for the benefit of my future self, I always observe a general project directory design:

    Project/
        ├── analysis/
        ├── data/
        │   ├── processed/
        │   └── raw/
        ├── logs/
        ├── README.md
        ├── useful_info/
        └── scripts/

Specific projects may deviate in little ways, but this is the general directory set up that I've found useful.

2 __Obtaining the data__

This is done all on the command line and in a linux environment. I write bash scripts and make copious use of submitting array jobs to UC Davis's high performance computing resource.

* find the project identification numbers for the files I want to download
* use samtools to download data files from [SRA](https://www.ncbi.nlm.nih.gov/sra)
* check to make sure file downloaded correctly
* Download reference corn genome from [maizeGDB](https://www.maizegdb.org/) and verify it downloaded correctly
* index the corn reference genome using kallisto

I used data from 151 samples from corn, teosinte, and corn-teosinte hybrids collected from leaf tissue.

3 __Generating gene expression counts__

Next, I quantified transcript abundance for the gene expression data.

For this step, I opted to use [kallisto](https://pachterlab.github.io/kallisto/), though a more rigorous comparison between kallisto and [salmon](https://combine-lab.github.io/salmon/) would be necessary to determine the correct tool for this experiment. 

This step was done using bash scripts. I bootstrapped the kallisto analysis 100 and took the mean of the bootstrapped values. The output of this step is data with the estimated abundance for each gene in the corn genome, across 151 samples.

4 __Linear Modeling of Gene Expression__

Now the fun part - building models to determine what genes are different. I compared the gene expressions from the corn-teosinte crosses to each of the parents, and identified genes that were significantly differentially expressed from the gene expression of either parent.

I performed this analysis in __R__ and used [__limma__](http://bioconductor.org/packages/release/bioc/html/limma.html) to build the linear models and produce the log fold change results.

## Analysis

I was excited to find that there were many genes that were expressed outside the expected expression patterns of their parents. About 40% of genes in the corn genome were disregulated in at least one out of 30 crosses.

There is a lot to parse out here, and this is a fairly complex story that is not nearly complete. Here are my high-level summaries of some observations of my data.

![full results](/assets/images/unnamed-chunk-12-1.png)

The y-axis is the log fold change between the hybrid offspring and the teosinte parent, and the x-axis is the log fold change between the hybrid offspring and the corn parent. __In other words__, a positive value indicates that the gene is expressed higher in the offspring than a parent and a negative value is expressed less in the offspring than a parent. The different colors are the different crosses (The first identifier is the corn parent, and the TILXX is the teosinte parent identifier.)

Below are visuals of what that means:

A gene that is __additive__ is expressed in between the parents. This analysis captures genes that are different from either parent, but still expressed at mid-parent level.

![additive 1](/assets/images/GRMZM5G826666.png)

![additive 2](/assets/images/AC194591.2.png)

And then __it gets interesting__. In the other two quadrants, there are genes that are significantly under-expressed or significantly over-expressed in the hybrid compared to the parent gene expression.

![over expressed](/assets/images/GRMZM2G140726.png)

![under expressed](/assets/images/AC149475.2.png)

## Conclusion and Remarks

There are currently lots of interesting things to investigate here. A few things not yet examined:

* disregulated genes shared across multiple crosses
* gene functions of disregulated genes
* disregulated gene clusters - how do these genes cluster
* are some crosses worse than others?
* What are the phenotypes of the hybrid crosses (this data was unavailable)

This project is definitely in its infancy, and there are multiple different layers to this analysis. But, we were excited that there did seem to be some potential in the role that disregulated genes could play in the overall success of a hybrid.
