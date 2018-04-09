# CLT4GDS - Week 4 - Tools for transcriptomics
## Command line tools for genomic data science
---

### Overview
* We have genes and each gene can produce multiple transcripts or isoforms or splice variants;
![isoforms](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-isoforms.png)

* 90% of the human genes are alternative spliced to form two or more isoforms up to potentially thousands;
* Many of aberrant isoforms has been associated with diseases;
* Some questions/problems addressed in transcriptome analysis:
![questions](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-questions.png)
---

### RNAseq
* Refers to analyzing NGS data of cellular RNA
![rnaseq](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-rnaseq.png)
1. RNA-seq
  * Generates short reads between 50 and 250 bp.
2. Mapping (alignment)
  * The reads are mapped (realigned) to the genome.
**3. Transcript assembly and quantification**
  * The alignment is clustered in gene-oriented groups to form a basic structure for gene and transcripts;
  * The number of transcripts are decoded from the structure and quantified (rates are assigned to them);
![quantification](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-quantification.png)
  * Exonas are nodes and edges are introns;
  * Example of algorithms to assigning reads. EM = expectation maximization; LP =linear program;
  * Identified which is the transcript is possible calculate and express the level for the transcript.

4. Differential analysis (expression splicing)
  * With multiple conditions or replicates the analysis continue with a differential analysis at the expression level (splicing level) between the two conditions (test x control)
  * In the end is selected the variants most likely to be expressed in that particular sample.

### RNAseq analysis workflow
![workflow](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-workflow.png)
* [Differential gene and transcript expression analysis of RNA-seq experiments with TopHat and Cufflinks | Nature Protocols](https://www.nature.com/articles/nprot.2012.016)
* [Benchmarking transcript expression quantification | Nature Reviews Genetics](https://www.nature.com/articles/nrg.2016.62)
* [A survey of best practices for RNA-seq data analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/)
* [RNA-seq Tutorial- STAR, StringTie and DESeq2 - 1 Learning Materials - CyVerse Wiki](https://pods.iplantcollaborative.org/wiki/display/TUT/RNA-seq+Tutorial-+STAR%2C+StringTie+and+DESeq2)
---

### Tophat
* Performs alignment
* It's necessary use the Bowtie index
```
#!/bin/bash

OUTPATH=/path/Tophat/Sample
ANNOT=/path/allmix_nonpseudi.nonredund.gff3
ANNOTIDX=/path/allmix_nonpseudor_nonredund_bt2index/
BT2IDX=/path/hg38c
FASTQ1=/path/*R1*gz
FASTQ2=/path/*R2*gz

mkdir -p $OUTPATH/

tophat2 -o $OUTPATH -p 10 --max-multihits=10 \
  -G $ANNOT --transcriptome-index $ANNOTINDX \
  -r 120 --mate_std-dev 30 \
  $BT2IDX $FASTQ1 $FASTQ2
```

* Results:
  * The primary alignment part that will be used to perform transcript assembly = `accepted_hits.bam`
  * BED files showing deletions, insertions and junctions = `deletions.bed insertions.bed junctions.bed`
  * Summary file with basics informations about the mapping base = `align_summary.txt`
  ![tophat-summary](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-tophat-summary.png)
---

### Cufflinks
* Performs assemly into transcripts;
* It uses an overlap graph, in order to represent in a compact way, a gene. Then it finds the minimum number of pass through the overlap graph that can explain all the weights or rather all the splicing paths.
* It uses the alignment file from Tophat (but can be from STAR, Hisat,...)
```
#!/bin/bash

THDIR=/path/Tophat/
WORKDIR=/path/Cufflinks

mkdir -p $WORKDIR/Sample
cd $WORDIR/Sample

cufflinks2 -L GeneLabel -p 8 $THDIR/Sample/accepted_hits.bam
```

* Results:
  * The main file is `transcripts.gtf`, it contains the assembled transcripts
  ![transcripts-gtf](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-transcripts-gtf.png);
  * FPKM = estimated expression level;
  * The `skipped.gtf` file contains a small number of transcripts that couldn't be processed, because there was some sort of exception (many reads at the logs).
  * The `.fpkm_*` gives expression estimates
  ![gene-fpkm](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-gene-fpkm.png)
  * "How many genes or transcripts we have in the `transcripts.gtf` file?"
  ```
  cut -f9 transcripts.gtf | cut -d ' ' -f2 | sort -u | wc -l
  cut -f9 transcripts.gtf | cut -d ' ' -f4 | sort -u | wc -l
  ```
---

### Cuffmerge
* Reconcile the gene structure acroos the samples, combine and compare it with the reference annotation;
* Is necessary create a file (`GTFs.txt`) with the GTF files that will be merged;
```
#!/bin/bash
WORDIR=/path/Cuffmerge
ANNOT=/path/allmix_nonpseudo.nonredund.gff3

mkdir -p $WORKDIR

cuffmerge -g $ANNOT -p 8 -o $WORKDIR $WORKDIR/GTFs.txt
```

* The result is a `merged.gtf` file with all samples merged that is the input for **Cuffdiff**.
![merged](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-merged.png)

### Cuffdiff
* Performs differential expression and splicing analysis between two conditions.
```
#!/bin/bash
WORDIR=/path/Cuffdiff
MERGEDIR=/path/Cuffmerge
THDIR=/tophat/path/
ANNOT=/path/allmix_nonpseudo.nonredund.gff3

mkdir -p $WORKDIR

cuffdiff2 -o $WORKDIR -p 10 $MERGEDIR \
          $THDIR/Sample1/accepted_hits.bam,$THDIR/Sample2/accepted_hits.bam,$THDIR/Sample3/accepted_hits.bam \
          $THDIR/Ctrl1/accepted_hits.bam,$THDIR/Ctrl2/accepted_hits.bam,$THDIR/Ctrl3/accepted_hits.bam
```

* The results: 
  * The most used file is `gene_exp.diff` file;
  ![cuffdiff-exp](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-cuffdiff-exp.png)
  * To obtain the list of genes that are differentially expressed:
  ```
  grep yes gene_exp.diff | wc -l
  ``` 
  ![cuffdiff-yes](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-cuffdiff-yes.png)
  * The commando above can be used to the `isoform_exp.diff` and others;
  * There's one difference when looking at splicing pattern differences, looking at the distribution of the values as percentage of the total distribution for a gene. eg.: We might have three different transcripts and the relative FPKM (expression values) might be 50%, 30%, 20% in one case and 80%, 10%, 10% in the control data set. 
  ![example-cuffdiff](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-example-cuffdiff.png)
---

### Integrated Genomics View (IGV)

* Open BAM or GTF files, but it has to be sorted and indexed;
* To sort and index transcripts GTF files in small pieces:
```
cat /path/Cufflinks/Sample1/transcripts.gtf | awk '{if ($1=="chr9") print $_;}' > Sample1.chr9.gtf
igvtools sort Sample1.chr9.gtf Sample1.chr9.sorted.gtf
igvtools index Sample1.chr9.sorted.gtf
```

* To sort and index the transcripts BAM file in small pieces:
```
samtools -b /path/Tophat/Sample1/accepted_hits.bam "chr9" > Sample1.chr9.bam
samtools sort Sample1.chr9.bam Sample1.chr9.sorted.bam
samtools index Sample1.chr9.sorted.bam
```

* With the IGV program is possible to open the sorted files (GTF and BAM) to analyze
![igv-gtf](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-igv-gtf.png)
![igv-gtf-bam](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/4-igv-gtf-bam.png)
