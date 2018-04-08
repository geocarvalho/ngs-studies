# CLT4GDS - Week 3 - Alignment & Sequence variation
## Command line tools for genomic data science
---

### Sequence variation
> Each individual is unique, the differences in the DNA sequence are ultimately responsible for differences in observable traits (height, eye color) and the hidden ones (inherited diseases, cancers).
* Polymorphisms
> A variant that occurs in 1% of the population

### Projects
* [HapMap](https://www.ncbi.nlm.nih.gov/probe/docs/projhapmap/)
* [1000 Genomes | A Deep Catalog of Human Genetic Variation](http://www.internationalgenome.org/)
* Others
  * [Exome Variant Server](http://evs.gs.washington.edu/EVS/)
  * [ExAC Browser](http://exac.broadinstitute.org/)
  * [gnomAD browser](http://gnomad.broadinstitute.org/)
  * [Genome 10K Project | Genome 10K Project](https://genome10k.soe.ucsc.edu/)
  * [Home - The Cancer Genome Atlas - Cancer Genome - TCGA](https://cancergenome.nih.gov/)
  * [COSMIC | Catalogue of Somatic Mutations in Cancer](https://cancer.sanger.ac.uk/cosmic)
  * [ABraOM: Brazilian genomic variants](http://abraom.ib.usp.br/)

### How to identify?
* Sample
* Library
* Sequencing
* Alignment algorithms
* Depth of coverage
![sequence-flow](http://www.micronautomata.com/assets/bioinformatics_shotgun-fb6aba1ff232328a621b80285995f0fea32defab5a3192955b17187268142365.png)
* Whole Exome Shotgun (WES) vs. Whole Genome Shotgun (WGS)
---

### SAM/BAM format
* Review of week 2

### mpileup format (SAMtools)
> Represent variants
![mpileup](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/3-mpileup.png)
* Each line represents one position along the genome with reads aligned;
* NRDS = Number or reads at the position (depth);
* BASES = String with info about the bases of each read at that position (like CIGAR string):
  * **"."** = match in the foward direction;
 <http://www.internationalgenome.org/wiki/Analysis/vcf4.0> * **","** = match, but the read is reverse complement;
  * **"{T,A,C,G}"** = mismatch in the foward direction;
  * **"{t,a,c,g}"** = mismatch in the reverse complement;
  * **"^"** = beginning of the read;
  * **"$"** = end of the read;
  * **"+..."** = insertion in the reference (eg.: +3ACC);
  * **"-..."** = deletion in the reference (eg.: -2GG)
  * **">"** = reference skip (jumping over a portion of this sequence).
* QUALITIES = Qualities of those bases in the corresponding reads;
* READPOS (optional) = Gives the position of that particular letter in the read (`-O`)

### VCF/BCF format
* Variant Call Format - Describe information about sequence variantion [VCF (Variant Call Format) version 4.0 | 1000 Genomes](http://www.internationalgenome.org/wiki/Analysis/vcf4.0);
* It can be compressed into BCF format;
* Contains a header that starts with "#" and the variants;
* The header is contains legends about the informations available for each variant.
![vcf](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/3-vcf.png)
  * The beging has information about the format version;
  * Has information about the genome reference used (preamble);
  * Has information about the command used;
  * INFO = info about the algorithms used to variant calling;
  * FORMAT = scores reproduced in variant calling.
* VCF entries call examples
![vcf-variants](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/3-vcf-variants.png)
* Translating the VCF
![vcf-decoding](/home/george/Git/ngs-studies/command-line-tools-for-genomic-ds/annotations/images/3-vcf-decoding.png)

### Genomic tools for alignment

#### Bowtie
* Search for alignments with the small number of mismatchs
1. Create an index to your reference genome
```
% bowtie2-build /path/genome-fasta/genome.fasta /path/genome-fasta/
```
2.1. Map your FASTQ to the genome using the end-to-end option (default)
> End-to-end means global alignments 
```
% bowtie2 -p 4 -x /path/genome-fasta/genome sample.fastq -S sample.bt2.sam
% samtools view -bT /path/genome-fasta/genome.fasta sample.bt2.sam > sample.bt2.bam
```
> `-p 4` means 4 threads

2.2. Map your FASTQ to the genome using the `--local` option
> Local alignments only match a portion of the input read
```
% bowtie2 -p 4 --local -x /path/genome-fasta/genome sample.fastq -S sample.local.bt2.sam
```

#### BWA 
* Search for alignments with the small number of mismatchs
1. Create an index to your reference genome
```
% bwa index /path/genome-fasta/genome.fasta
```
2. Map your FASTQ to the genome using `mem` algorithm
```
% bwa meme -t 4 /path/genome-fasta/genome.fasta sample.fastq > sample.bwa.sam
% samtools view -bT sample.bwa.sam > sample.bwa.bam
```
> `-t 4` means 4 threads

### Genomic tools for variant calling

#### SAMtools
1.1. Create the mpileup file
```
% samtools sort sample.bam > sample.sorted.bam
% samtools index sample.sorted.bam
% samtools mpileup -f /path/genome-fasta/genome.fasta sample.sorted.bam > sample.mpileup
```
> The BAM file has to be sorted and indexed

1.2. Or create the VCF file directly
```
% samtools mpileup -v -u -f /path/genome-fasta/genome.fasta sample.sorted.bam > sample.vcf
```
> `-v -u` means VCF output and uncompressed respectively

1.3. Or create a BCF file
```
% samtools mpileup -g -f /path/genome-fasta/genome.fasta sample.sorted.bam > sample.bcf
```
> `-g` means BCF output

#### BCFtools
* Can be used to transform BCF files into VCF
```
% bcftools view sample.bcf > sample.vcf
```

1. Call variants using mpileup package
```
% bcftools call -v -m -O z -o sample.vcf.gz sample.bcf
```
> `-v` means to write only variants; `-m` to use the multiallelic model; `-O` to specify the output format (b|u|z|v = bcf.gz|bcf|vcf.gz|vcf)

#### All together - simple pipeline

1. Sort and index your BAM file
```
% samtools sort sample.bam > sample.sorted.bam
% samtools index sample.sorted.bam
```

2. Call the variants
```
% samtools mpileup -g -f /path/genome-fasta/genome.fasta sample.sorted.bam > sample.bcf
% bcftools call -v -m -O z -o sample.vcf.gz sample.bcf
```

3. Visualize variants
```
% zcat sample.vcf.gz
% samtools tview -p 17:7579600 sample.bam /path/genome-fasta/genome.fasta | more
```
