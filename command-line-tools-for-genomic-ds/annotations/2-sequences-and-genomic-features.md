# Command line tools for genomic data science
## Week 2 - annotations
---
### Genome

 * **A totality of genetic material in a cell** (very long sequence). It's organized in 46 chromosomes (22 pairs autossomics and 1 pair sexual);

 * In the genome we have genes organized as beads along the string in both strands of the **double-helix** genome;

 * Genes are made of two types of blocks, **exons** that are informatives and the spaces between exons that are called **introns**.

![Genomes-and-genes](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-genomes-and-genes.png?raw=true)

### Genes
> Genes formation in eukaryotic cells

![genes-expression](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-gene-expression.png?raw=true)

1. Transcription

> The **pre-mRNA** is formed based on the DNA as a single-strand

> The first modification is **capping**, where the tail is chopped off and replaced to a poli-A tail.

2. Splicing
> The most important modification is **splicing**, the exons are connected, whereas the introns are removed;

> After that you have a **mRNA**, it happened in the nucleus.

3. Translation
> The mRNA is transported to the citoplasm and translated to protein by the ribossoms (or not);

> This mRNA forms can be different for the same Gene, that's called **transcripts** or **alternative splicing**.

### Sequence representation
1. Genomic sequences:  *AGCT*
2. mRNA (gene) sequences: *AGCU*
3. Proteins: *AVFP...W*

### Generation of sequences

1. **Sanger sequence** (<2008)

* Shear DNA/RNA into fragments;
* Clone in vector (amplification);
* Sequence insert end(s): 500-800 bp;
* Slow, costly, medium-to-high throughput.

2. **Next Generation Sequencing (NGS)** (>2008)
* Shear DNA/RNA (cDNA);
* May or may not require amplification;
* Sequence fragment ends: 50-450 bp;
* Single-end or paired-end reads;
* Low cost (<%0.04/Mb), fast, very high throughput (100s millions/run).

### Assembly
> After the sequencing process, bioinformatics algorithms are responsible to reorganize the reads against a reference genome or take the common bases in the reads and put together based on graph algorithms.

### Representation
1. **Fasta/Pearson format** - single sequences
* Most common for Sanger sequences;
* The first line is the header that starts with '>' containing the information about the sequence;
* The second line is the sequence it self;
* Fast files normally came with a `.fasta.fai` which has the quality of the sequence;
* Fasta files can have more than one sequence separated by different headers, called multifasta.

2. **Fastq format**
* Format used in NGS;
* First line is the header with informations about the sequence. It starts with a "@" and finish with the read pair number;
* Second line is the sequence;
* Third line is the strand (+ or -);
* Fourth line is the sequence quality.
![fastq](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-fastq.png?raw=true)
* About the base quality scores in FASTQ files:
  * Let Pb = probability that the call at base *b* is correct;
  * Quality value: Qsanger = -10.log10Pb (integer);
  * Sanger (Phred quality scores): 0-93 (ASCII characters 33-126);
![ascii](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-ascii.png?raw=true)
  * The maximal quality value is 40;
  * Quality values below 20-25 are considered low.
---

### Genomic features
> * Determine the precise location and structure of genomic features along the genome;
> * Genes, promoters, protein binding sites, translation start/stop site, DNasel sites, etc.

* **BED** (Browser extensible data) format
  * The basic format has 3 columns (chr, start and end);
  * The extended format has *chr, start, end, name, score, strand, thick_start, thick_end and rgb* columns;
![extended-bed](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-extended-bed.png?raw=true)
  * It's 0-based (count spaces) and it means that the first nucleotide is counted as 0, not 1-based (count bases) counting as 1;
  * The extended format can have more columns like *Block count* and *Block sizes*;
![bed-more-columns](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-bed-more-columns.png?raw=true) 
  * The example above is 0-based.
  
* **GTF** (Genome transfer format) format
  * Each interval takes one line;
  * Columns 1-9 is tab separated;
  * The fields in column 9 is space separeted;
  * Coordinates are 1-based.
![gtf](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-gtf.png?raw=true)

* **GFF** (Genomic feature format) format
  * One line for each exon.
![gff3](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-gff3.png?raw=true)
---

### Alignment
> A mapping between the letters of the two sequences, with some spacers (indels);

> Can take into account differences such as polymorphisms and sequencing errors, and introns (for genes)

* Substitutions - different letters between the reference and the read (polymorphism, sequence errors);
* Insertion and deletions - gain or loss of letters in reference or reads.
* Spliced alignments occur frequently in mRNA alignmnets (showed as continuous in the mRNA), when the reads have spaces to alignm when compared to the genome;
* NGS alignments - Properly paired or concordant according to *d'* (distance between 5'-3' primers). But sometimes it's non-concordant.
* **SAM** format
  * SAM format can be compressed in BAM format (binary alignment);
  * The first few lines represent the header (most of them if "@" in the begining);

> HQ = header, SQ = sequence line, PG = program (command) or how it was made.

![sam](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-sam.png?raw=true)
  * After header we have the alignments reads metrics per read

![sam-metrics](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-sam-metrics.png?raw=true)
  * *Mate chr* = Where is the mate of this particular field, "=" if is the same chr, "chr?" another chr if is different or "*" isn't mate;
  * *Edit distance to reference (NM)* = how many of edit operations (substitutions, insertions and deletions) we need to include to equalize to do the mapping between the query and the corresponding reference;
  * *Hit index for this alignment (HI)* = if we have multiple matches and each one has different quality or alignment score, what's the order, the position of this alignment within that particular hierarchy (list)

* SAM **FLAGs**
![sam-flag](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-sam-flag.png?raw=true)
  * Based on binary notation (0 = false; 1 = true)
  * Read from right to left
  * **0x1** = paired (1) or not (0); **0x2** = proper paired (1) or not (0); **0x4** = unmapped (1) or mapped (0); **0x8** = mate not mapped (1) or mapped (0);
  * **0x10** = reverse complement (1) or foward (0); **0x20** = mate is reverse complemented (1) or foward (0); **0x40** = first in pair (1) or not (0); **0x80** = second (last) in pair (1) or not (0);
  * **0x100** = secondary alignment (1) or not (0); **0x200** = not passed quality check (1) or passed (0) (based in quality values low or too many ends); **0x400** = PCR or optical duplicate (1) or not (0); **0x800** = sup. alignment (1) or not (0).

* SAM **CIGAR**
![sam-cigar](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-sam-cigar.png?raw=true)
  * The second example represents a splice alignment.
---
### Repositories
* GenBank - Nucleotide, SRA, GEO
* UCSC Table Browser
* Ensembl

1. **Using GenBank to retrieve FASTA file**
* [National Center for Biotechnology Information](https://www.ncbi.nlm.nih.gov/);
* To retrieve the gene sequence as example, search for "IL-2 homo sapiens mRNA" using the database "Nucleotide";
* In the top select "Transcripts";
* Select "Homo sapiens interleukin 2 (IL2), mRNA";
* Click in "FASTA";
* Copy it or Send as FASTA format.

2. **Using GenBank to retrieve FASTQ  file**
* Let's get the strawberry RNA-seq FASTQ, search for "strawberry RNA-seq" using the database "SRA";
* In "Results by taxon, select "*Fragaria vesca*";
* Select "illumina sequencing of floral transcriptomes in woodland strawberry" with around 15 mb to download it fast;
* Click in the run "SRR1107997" and download the data
```
% wget ftp://ftp-trace.ncbi.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR110/SRR1107997/SRR1107997.sra
```

* Transform `.sra` format to `.fastq` format using `fastq-dump`
```
% wget --output-document sratoolkit.tar.gz http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz
```

* During the download install `fastq-dump`([HowTo: Binary Installation · ncbi/sra-tools Wiki · GitHub](https://github.com/ncbi/sra-tools/wiki/HowTo:-Binary-Installation))
```
% gunzip sratoolkit.tar.gz
% tar -vxzf sratoolkit.tar.gz
% export PATH=%PATH:%PWD/sratoolkit.2.4.0-1.mac64/bin
% which fastq-dump
% fastq-dump --help
```

* Now run `fastq-dump`
```
% nohup fastq-dump SRR1107997.sra & 
```

* For pair-end files
```
% nohup fastq-dump --split-3 fileName.sra &
```
---

### Genomic features

* [UCSC Table Browser](https://genome.ucsc.edu/cgi-bin/hgTables)

![table-1](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-table-1.png?raw=true)
* Save it as "RefSeq.gtf".

![table-2](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-table-2.png?raw=true)
* After that select "Whole gene";
* Save it as "RefSeq.bed".

![table-3](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-table-3.png?raw=true)
* In "filter" add to the "repName" = "Alu*";
* Download the "Whole gene";
* Save as "Alus.bed.txt".
---

### SAMtools

1. flagstat
```
% samtools flagstat example.bam
```

![flagstat](https://github.com/geocarvalho/ngs-studies/blob/master/command-line-tools-for-genomic-ds/annotations/images/2-samtools-flagstat.png?raw=true)

2. sort
```
% samtools sort example.bam example.sorted
```

3. index
```
% samtools index example.sorted.bam
```

4. merge
```
% samtools merge example.bam example_1.bam example_2.bam
```

5. view
* To see the alignment
```
% samtools view example.bam | more
% samtools view -h example.bam | more
% samtools view -h example.bam > example.sam
% samtools view -H example.bam | more
```
> -h you will see also the header (BAM -> SAM), while -H you see just the header

* To convert from SAM to BAM

```
% samtools view -bT /path/ref.fa example.sam > example.sam.bam
```

* To extract a range size of the BAM

```
% samtools view example.bam "chr22:240000000-25000000"
% samtools view -L example.bed example.bam
``` 
> Use -L to use a bed file (chr22\t24000000\t25000000)
---

### BEDtools
1. intersect
* How many exons transcripts in the reference contains Alus?

```
% bedtools intersect -wo -a RefSeq.gtf -b Alus.bed | cut -f9 | cut -d " " -f2 | sort -u | wc -l
```

> My GTF wasn't formated well, so it didn't work to me.

```
% bedtools intersect -split -wo -a RefSeq.bed -b Alus.bed | wc -l
```

> For my data `-wo` didn't work

```
% bedtools intersect -split -wao -a RefSeq.bed -b Alus.bed | wc -l
```

> It shows all the features in A with overlap or not (it worked in the new version). Follow by a *NULL* entry in the Alu file, represented by `. -1 -1 .`

2. bamtobed

* Transform a BAM file to BED file with a CIGAR column

```
% bedtools bamtobed -cigar -i NA12814.bam | more
```

* Using `-split` option for a BED12 it will split by exon

```
% bedtools bamtobed -split -i NA12814.bam | more
```

3. bedtobam

```
% bedtools bedtobam -i RefSeq.gtf -g /path/hg38.hdrs > refseq.gtf.bam
```
> The `-g` is a header file. The command above separate by exon
> Use `samtools view` to see the result file

```
% bedtools bedtobam -i RefSeq.bed -g /path/hg38.hdrs > refseq.bed.bam
```
> The command above separate by gene (one line by gene, and the CIGAR don't show the introns)

```
% bedtools bedtobam -bed12 -i RefSeq.gtf -g /path/hg38.hdrs > refseq.gtf.bam
```
> With `-bed12` parameter it shows a realistic view of the gene (exon/intron)

4. getfasta

* Alows to get the FASTA sequence for some intervals, usefull for transcriptomic analysis for example.

```
% bedtools getfasta -fi /path/hg38c.fa -bed RefSeq.gtf -fo refseq.gtf.fasta
```
> The command above gives one line for exon

```
% bedtools bedtobam -i RefSeq.bed -g /path/hg38.hdrs > refseq.bed.bam
```
> It gives one contig by gene, with exons/introns

```
% bedtools bedtobam -split -i RefSeq.bed -g /path/hg38.hdrs > refseq.bed.bam
```
> The `-split` indicate the BED with multiple blocks, each genes has the correct range


