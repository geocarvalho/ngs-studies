## DAY 3
* [BCBio pipelines for RNA-seq](https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#rna-seq)
* [Informatics for RNA-seq: A web resource for analysis on the cloud](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004393) - [wiki](https://github.com/griffithlab/rnaseq_tutorial/wiki)

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

### Papers to study
* ['Errors in RNA-Seq quantification affect genes of relevance to human disease'](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-015-0734-x)
> 'We apply 12 common methods to estimate gene expression from RNA-Seq data and show that there are hundreds of genes whose expression is underestimated by one or more of those methods. Many of these genes have been implicated in human disease, and we describe their roles. We go on to propose a two-stage analysis of RNA-Seq data in which multi-mapped or ambiguous reads can instead be uniquely assigned to groups of genes. We apply this method to a recently published mouse cancer study, and demonstrate that we can extract relevant biological signal from data that would otherwise have been discarded.'

* 

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

#### Papers to study
* ['Evaluation of quantitative miRNA expression platforms in the microRNA quality control (miRQC) study'](http://www.nature.com/nmeth/journal/v11/n8/full/nmeth.3014.html?foxtrotcallback=true) - [github](https://github.com/lpantano/mypubs/blob/master/srnaseq/mirqc/ready_report.md)
> '..In this study, we systematically compared 12 commercially available platforms for analysis of microRNA expression. We measured an identical set of 20 standardized positive and negative control samples, including human universal reference RNA, human brain RNA and titrations thereof, human serum samples and synthetic spikes from microRNA family members with varying homology. We developed robust quality metrics to objectively assess platform performance in terms of reproducibility, sensitivity, accuracy, specificity and concordance of differential expression..'

* [isomiRs](https://github.com/lpantano/isomiRs)
* [DAVID](https://david.ncifcrf.gov/)
* [GSEA](http://software.broadinstitute.org/gsea/index.jsp)
* [DIANA TOOLS](http://diana.imis.athena-innovation.gr/DianaTools/index.php)
* R-Peridot - software from BioME instead edgeR

---
### Part 3 - '04-Aula_pratica_RNAseqI.pdf' and '05-Aula_pratica_RNAseqII.pdf'
* Start practice
