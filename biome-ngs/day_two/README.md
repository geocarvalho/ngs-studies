# DAY 2
sandro@neuro.ufrn.br
## *Part 1*
## About BioME projects
* Viva: software from BioME, from FASTQ to interpretation (not based in ACMG)
* Proteogenomics browser: Data integration from genomics, transcriptomics, proteomics

## Genomic revolution
### DNA sequencing evolution
* Sanger, Gilbert (Nobel 1980)
* Leroy Hood (Automatic sequencing - ABI)
* Human genome project (1990)
* International iniciative (nature) vs. Celera genomics (science)
* 454 ROCHE
* Solexa > Illumina
* SOliD ABI
* Benchtop
* ['2016 Advancements in Next-Generation Sequencing'](http://www.annualreviews.org/doi/full/10.1146/annurev-genom-083115-022413)

### [Illumina sequencing process](https://www.youtube.com/watch?v=fCd6B5HRaZ8)
* [Shomu's biology - Illumina sequencing](https://www.youtube.com/watch?v=TDD6bExnR38)

### Aplications
#### 1. Whole-Genome Sequencing (WGS)
* Re-sequencing
* Single-end or paired-end (cromossome re-arrange)
* ['2012 Performance comparison of whole-genome sequencing platforms'](www.nature.com/nbt/journal/v30/n1/full/nbt.2065.html)
* ['2016 Clinical sequencing: is WGS the better WES?'](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4757617/)
#### 2. Whole-Exome Sequencing (WES)
* [Shomu's biology - Exome sequencing](https://www.youtube.com/watch?v=lb9BlWolZqg)

#### 3. RNA-seq 

#### 4. CHIP-seq (Chromatin immuno preciptation)

#### 5. Metagenomics (taxonomic analysis)
* Shotgun or 16S rRNA

### Papers with applications
* ['Actionable diagnosis of neuroleptospirosis by NGS'](http://www.nejm.org/doi/full/10.1056/NEJMoa1401268#t=article) - Metagenomics application in neuro biopsy to search for microorganisms disease causing
* ['Analysis of protein-coding genetic variation in 60,706 humans'](http://www.nature.com/nature/journal/v536/n7616/full/nature19057.html?foxtrotcallback=true) - ExAC paper
* ['Neanderthal behaviour, diet, and disease inferred from ancient DNA in dental calculus'](https://www.nature.com/nature/journal/v544/n7650/full/nature21674.html) - Sequencing microorganism from past

## *Part 2*
### ISO
* Start '02-AulaQualidade.pdf'

### Necessary informations
* How many samples per lane?
* Pair or single end (Ion and Proton)?
* Read length expected?
* Biological and experimental replicates
* Sequencing machine and model (homopolymer problems, SOLiD give numbers instead nucleotides=csFASTA)

### FASTQ (4 lines per read)
* [FASTQ_format from wikipedia](https://en.wikipedia.org/wiki/FASTQ_format)

#### Sequence read start with '@' + Machine ID + read pair + QC filter flag can be Y=bad or N=good + barcode sequence)

#### Sequence read

#### '+' for annotations

#### Quality score (Phred +33)
* Each reaction (light) has your quality
* For number in the ASCIII table, ignore the invisible digits (real digits starts in 33), thats why you subtract 33 from the given quality value per character
* Phred score 10 indicate 1 error in 10 base calls (90%), phred score *25* (99% < x < 99.9%)

### Map reads
* Download Genome
* Sorting the chromossomes
* Index the genome

### [SAM format](https://samtools.github.io/hts-specs/) (pos alignment txt file)
* [Next_Generation_Sequencing_(NGS)/Alignment](https://en.wikibooks.org/wiki/Next_Generation_Sequencing_(NGS)/Alignment)
* [File specification](https://samtools.github.io/hts-specs/SAMv1.pdf)
* Soft clip (mask barcode sequences)
* CIGAR string
* [FLAG](https://broadinstitute.github.io/picard/explain-flags.html) and Broad discussion about that, [here](https://gatkforums.broadinstitute.org/gatk/discussion/7019/sam-flags-down-a-boat)

### BAM format
* [Explanation by illumina](https://support.illumina.com/help/BS_App_TruSightTumor15_OLH_1000000001133/Content/Source/Informatics/BAM-Format.htm)
* [By Center for statistical genetics](http://genome.sph.umich.edu/wiki/BAM)
### FastQ_Screen
* 'FastQ Screen is a simple application which allows you to search a large sequence dataset against a panel of different genomes to determine from where the sequences in your data originate' ([reference](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/fastq_screen_documentation.html)
* Confirm if what you send is what are you receiving 
* RNA-seq has alot of ribossomal mix ups

### fastqutils
* Basic statistics
* [Web site informations](http://ngsutils.org/modules/fastqutils/)

### FastQC
* 'Modern high throughput sequencers can generate hundreds of millions of sequences in a single run. Before analysing this sequence to draw biological conclusions you should always perform some simple quality control checks to ensure that the raw data looks good and there are no problems or biases in your data which may affect how you can usefully use it.' ([Web site](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/))
* Basic statistics
* Per base sequence quality (The start and end normally are bad)
* Sequence duplication levels (In Amplicon and microRNA is expected more repetitions)
* Overrepresented sequences (To eliminate it use FastQ_Screen)

### SAMstat (alignment analysis)
* 'To understand the relationship between potential protocol biases and poor mapping we wrote SAMstat, a simple C program plotting nucleotide overrepresentation and other statistics in mapped and unmapped reads in a concise html page.'
* ['2011 SAMStat: monitoring biases in next generation sequencing data'](https://academic.oup.com/bioinformatics/article/27/1/130/201972/SAMStat-monitoring-biases-in-next-generation)
* Mapping stats (before and after trimming)
* Error profile

### DynamicTrim.pl (remove bad base qualities)
* 'A read trimmer that individually crops each read to its longest contiguous segment for which quality scores are greater than a user-supplied quality cutoff (or alternately, the read segment returned by the BWA trimming algorithm)'
* 'DynamicTrim and LengthSort are typically used in combination to remove poor quality bases and/or reads from high throughput sequence data. Both programs should work on any FASTQ file (modified or unmodified, compressed or not)'.
* [Web page](http://solexaqa.sourceforge.net/)

### Trim_galore (can detect primers used)
* 'A wrapper around Cutadapt and FastQC to consistently apply adapter and quality trimming to FastQ files, with extra functionality for RRBS data.'
* [github](https://github.com/FelixKrueger/TrimGalore)

### Cutadapt (just remove adpators)
* 'Cutadapt finds and removes adapter sequences, primers, poly-A tails and other types of unwanted sequence from your high-throughput sequencing reads.'
* 'Cleaning your data in this way is often required: Reads from small-RNA sequencing contain the 3’ sequencing adapter because the read is longer than the molecule that is sequenced. Amplicon reads start with a primer sequence. Poly-A tails are useful for pulling out RNA from your sample, but often you don’t want them to be in your reads.'
* [github](https://github.com/marcelm/cutadapt)

### Starting practice ('02-Aula_pratica_SeqQualityControl.pdf')

## *Part 3*
## Alignment and variant Calling
* Start class '03-AulaVariantes.pdf'

### 1. Burrows-Wheeler Alignment ([BWA](http://bio-bwa.sourceforge.net/bwa.shtml))
* ['Question: When and why is bwa aln better then bwa mem?'](https://www.biostars.org/p/117225/)
* ['2013 Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM'](https://arxiv.org/abs/1303.3997)
* ['2013 Benchmarking short sequence mapping tools'](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-184) - 'The results show that no single tool outperforms all others in all metrics. However, Bowtie maintained the best throughput for most of the tests while BWA performed better for longer read lengths.'
* ['2016 Accuracy, speed and error tolerance of short 
DNA sequence aligners'](http://www.biorxiv.org/content/early/2016/05/16/053686) - 'These findings show that BWA mem is highly accurate and tolerant to errors including mismatches 
and indels. These features make BWA mem suited to mapping of whole genome bisulfite sequencing reads.'

### 2. SAMtools
* ['2009 The Sequence Alignment/Map format and SAMtools'](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2723002/) - 'The Sequence Alignment/Map (SAM) format is a generic alignment format for storing read alignments against reference sequences, supporting short and long reads (up to 128 Mbp) produced by different sequencing platforms. It is flexible in style, compact in size, efficient in random access and is the format in which alignments from the 1000 Genomes Project are released. SAMtools implements various utilities for post-processing alignments in the SAM format, such as indexing, variant caller and alignment viewer, and thus provides universal tools for processing read alignments.'
* 'The SAM (Sequence Alignment/Map) format (BAM is just the binary form of SAM) is currently the de facto standard for storing large nucleotide sequence alignments.' ([reference](http://davetang.org/wiki/tiki-index.php?page=SAMTools))
* SAM > BAM
* Mpileup - [interpretation](http://samtools.sourceforge.net/pileup.shtml)
* Dedup can be made by **samtools rmdup** or **picard MarkDuplicates**
> ['Question: Why Do We Need Markduplicates For Variants Detection In Gatk Processing Pipeline?'](https://www.biostars.org/p/18784/)
> ['Removing duplicates is it really necessary?'](http://seqanswers.com/forums/showthread.php?t=6854)
> ['Question: About Paired-End Sequencing'](https://www.biostars.org/p/788/)
> ['Question: Difference Between "Mate Pair" And "Pair-End"'](https://www.biostars.org/p/77293/)

**QUESTION**: How differentiate a variant caused by an error after dedup from a somatic variant?

### 3. Varscan (variant call software, normally used for somatic samples)

### 4.  SnpEff (vcf annotation - [site](http://snpeff.sourceforge.net/SnpEff_manual.html))

### 5. Variant Calling Format ([VCF](https://samtools.github.io/hts-specs/VCFv4.1.pdf))
* [Broad explanation](https://gatkforums.broadinstitute.org/gatk/discussion/1268/what-is-a-vcf-and-how-should-i-interpret-it) 
### Start tutorial '03-Aula_pratica_variantes.pdf'
* How many reads sequenced against reference? 341429
* How many reads mapped with quality above MAPQ 30? 327293
* How many correct alignment exist (properly paired)? 317774
> samtools flagstat bwa.bam


