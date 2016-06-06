---
title: 'Assembling the Genome in a Bottle sequencing data with ABySS'
author: 'Shaun D Jackman'
date: '2016-06-06'
history: true
slideNumber: true
---

## Genome in a Bottle

Assembling the Genome in a Bottle sequencing data with ABySS

Shaun Jackman [\@sjackman][]

2016-06-06

[![Creative Commons Attribution License](images/cc-by.png)][cc-by]

[Fork me on GitHub!][]

[\@sjackman]: http://twitter.com/sjackman
[cc-by]: http://creativecommons.org/licenses/by/4.0/
[Fork me on GitHub!]: https://github.com/sjackman/giab-slides

## Shaun Jackman

| [BC Cancer Agency Genome Sciences Centre][]
| Vancouver, Canada
| [\@sjackman][] | [github.com/sjackman][] | [sjackman.ca][]

![](images/sjackman.jpg)

[BC Cancer Agency Genome Sciences Centre]: http://bcgsc.ca
[github.com/sjackman]: https://github.com/sjackman
[sjackman.ca]: http://sjackman.ca

Genome in a Bottle Data
================================================================================

## [GIAB Data][]

+ 7 individuals
    + Pilot individual (NA12878)
    + Ashkenazim Trio
    + Chinese Trio
+ 13 sequencing technologies
    + Illumina
    + Ion Torrent
    + 10x Genomics
    + Pacbio
    + Oxford Nanopore
    + SOLiD
    + Complete Genomics
    + BioNano Genomics

[GIAB Data]: https://github.com/genome-in-a-bottle/giab_data_indexes

## Sequencing

+ Illumina 2x150 paired-end of 600 bp
+ Illumina 2x250 paired-end of 600 bp
+ Illumina 6 kbp mate-pair
+ Illumina Whole Exome
+ Ion Proton Exome
+ SOLiD
+ Moleculo
+ 10x Genomics GemCode
+ PacBio
+ Oxford Nanopore
+ Complete Genomics
+ Complete Genomics LFR
+ BioNano Genomics Irys

Assembly Pipeline
================================================================================

## Assembly Pipeline

+ Inspect data
+ Trim adapters
+ Correct errors
+ Merge reads
+ Estimate *k*-mer abundance
+ Assemble
+ Scaffold
+ Close gaps
+ Polish
+ Align to reference
+ Calculate metrics
+ Count core genes
+ Generate reports

## Inspect data

+ Check library and sequencing quality
+ Discard bad lanes
+ Trim bad cycles

### Tools

+ [FastQC][]
+ [MultiQC][]

## Trim adapters

Remove adapter sequence from reads

### Tools

+ [Cutadapt][]
+ [Trimadap][]
+ [Trimmomatic][]
+ [NxTrim][] for Nextera mate-pair

## Correct errors

Reduce memory utilization and improve contiguity and correctness

### Tools

+ [BFC][]

## Merge reads

+ Merge overlapping reads
+ Fill the gap between paired-end reads

### Tools

+ ABySS-MergePairs
+ [Konnector][]

## Estimate *k*-mer abundance

+ Estimate memory usage prior to assembly
+ Pick a range of values for *k* to assemble

### Tools

+ [ntCard][]
* [KmerStreamer][]

## Assemble

Assemble reads into contigs

### Tools

+ [ABySS][]
+ [DISCOVARdenovo][]
+ [Minia][]
+ [SOAPdenovo][]

## Scaffold

Join contigs into scaffolds

### Tools

+ [LINKS][] for mate-pair
+ [ARCS][] for 10x Genomics

## Close gaps

+ Fill gaps of Ns with sequence
+ Improve the contig N50

### Tools

+ [Sealer][]

## Polish

Identify and remove errors from the assembly

### Tools

+ Scrubber
+ [BFC][] (off-label)
+ [Pilon][]

## Align to reference

Identify misassemblies

### Tools

+ [BWA-MEM][] `-xintractg`
+ [MUMmer][] for smaller genomes

## Calculate metrics

for contiguity, correctness, completeness

### Tools

+ [QUAST][] for smaller genomes
+ [ABySS-samtobreak][]
+ [htsbox abreak][] (no scaffold metrics)

## Count core genes

Estimate assembly completeness without a reference

### Tools

+ [BUSCO][]
+ [CEGMA][]

## Generate reports

Generate tables and figures of

contiguity, correctness, completeness

### Tools

+ [QUAST][]
+ [RMarkdown][]

[ABySS-samtobreak]: https://github.com/bcgsc/abyss/blob/master/Misc/samtobreak.hs
[ABySS]: http://www.bcgsc.ca/platform/bioinfo/software/abyss
[ARCS]: https://github.com/bcgsc/arcs
[BFC]: https://github.com/lh3/bfc
[BUSCO]: http://busco.ezlab.org
[BWA-MEM]: https://github.com/lh3/bwa
[CEGMA]: http://korflab.ucdavis.edu/datasets/cegma/
[Cutadapt]: https://github.com/marcelm/cutadapt
[DISCOVARdenovo]: https://www.broadinstitute.org/software/discovar/blog/
[FastQC]: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
[htsbox abreak]: http://lh3.github.io/2014/07/07/abreak-evaluating-de-novo-assemblies/
[KmerStreamer]: https://github.com/pmelsted/KmerStream
[Konnector]: http://www.bcgsc.ca/platform/bioinfo/software/konnector
[LINKS]: http://www.bcgsc.ca/platform/bioinfo/software/links
[Minia]: http://minia.genouest.org
[MultiQC]: http://multiqc.info
[MUMmer]: http://mummer.sourceforge.net
[ntCard]: https://github.com/bcgsc/ntCard
[NxTrim]: https://github.com/sequencing/NxTrim
[Pilon]: http://www.broadinstitute.org/software/pilon/]
[QUAST]: http://quast.sourceforge.net/quast
[RMarkdown]: http://rmarkdown.rstudio.com
[Sealer]: http://www.bcgsc.ca/platform/bioinfo/software/sealer
[SOAPdenovo]: http://soap.genomics.org.cn/soapdenovo.html
[Trimadap]: https://github.com/lh3/trimadap
[Trimmomatic]: http://www.usadellab.org/cms/?page=trimmomatic

Results
================================================================================

## Correct errors using [BFC][]

+ Reduce memory utilization from 975 GB to 418 Gb
+ Improve scaffold NG50 from 3.7 Mbp to 4.9 Mbp
+ Improve correctness (? no data)

## Contigs

![Assemble contigs using [ABySS][]](images/contig-NG50.png)

## Scaffolds

![Scaffold using [ABySS][]](images/scaffold-NG50.png)

## LINKS

Improve the scaffold

+ N50 from 5.6 to 8.2 Mbp
+ NG50 from 4.6 to 6.8 Mbp
+ NGA50 from 4.4 to 5.3 Mbp

## Contiguity vs correctness

![Scaffold NGA50 vs breakpoints](images/scaffold-NGA50-breakpoints.png)

fin
================================================================================

## Links

[ABySS-samtobreak][] | [ABySS][] | [ARCS][] | [BFC][] | [BUSCO][] | [BWA-MEM][] | [CEGMA][] | [Cutadapt][] | [DISCOVARdenovo][] | [FastQC][] | [htsbox abreak][] | [KmerStreamer][] | [Konnector][] | [LINKS][] | [Minia][] | [MultiQC][] | [MUMmer][] | [ntCard][] | [NxTrim][] | [Pilon][] | [QUAST][] | [RMarkdown][] | [Sealer][] | [SOAPdenovo][] | [Trimadap][] | [Trimmomatic][]

## Shaun Jackman

| [BC Cancer Agency Genome Sciences Centre][]
| Vancouver, Canada
| [\@sjackman][] | [github.com/sjackman][] | [sjackman.ca][]

![](images/sjackman.jpg)
