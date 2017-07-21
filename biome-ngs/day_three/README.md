## DAY 3
### Part 1 - '04-AulaRNA-seqI.pdf'
* Focus on coding genes, not in microRNA
* [Help slide](http://www.ic.unicamp.br/~zanoni/orientacoes/mestrado/lucas/SlidesTeseLucas.pdf)
#### FASTQC - quality
* Remember to search for ribosomal to clean (use FastQ_Screen to search)
#### [Trimmomatic: A flexible read trimming tool for Illumina NGS data](http://www.usadellab.org/cms/?page=trimmomatic)
* Remove adapters
* Remove low quality reads in the start and end
* Remove low base quality
* Remove short reads
#### Tophat2 - alignment
* Reference genome and annotations (gtf or gff)
* Bowtie indirectly (reads not mapped, remap with default bowtie)
#### Cufflinks
* Assembly reads
#### Cuffmerge
* Create an archive with paths to gtf or gff
#### Cuffdiff - differential expression (more than one tissue)
* Use results from cuffmerge
* Normalization is made by default (RPKM for single-end and FPKM for pair-end)
* archive.diff - For each gene it show the expression for each tissue(Binomial negative)
#### MicroArray vs RNAseq
* In microarray you have to know the reference probe

---
### Part 2 - '05-AulaRNA-seqII.pdf'
* Focus on microRNA
* Transcriptoma
* Differential expression by tissue (some mutations in important genes have to analyze the tissue expression carefully)
* coding RNA (is extracted based on RNA poli A+)
* Non coding RNA (split reads based on databases of ncRNAs)
> They are classified based on their structure (snRNA, snoRNA, tRNA)

> Can be in intron, intergenic regions (has promoters, pseudogenes?), exons

* Project idea 1: Search for ncRNA in open mRNA data from NCBI or others
* Project idea 2: What mRNAs are expressed in specific cancers (breast, prostate cancer) and organize a database

#### Cutadapt

#### mapper

#### miRDeep2

#### Bowtie or tophat2

#### Differential expression
* DESeq, EdgeR, SAMSeq
#### Pipelines
* sRNAMapper, ncRNAAnnotator, CompaRNA, ncRNAPredictor

---
### Part 3 - '04-Aula_pratica_RNAseqI.pdf' and '05-Aula_pratica_RNAseqII.pdf'
* Start practice
