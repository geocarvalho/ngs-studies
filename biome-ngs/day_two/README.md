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

### [Illumina sequencing process](https://www.youtube.com/watch?v=fCd6B5HRaZ8)
* [Shomu's biology - Illumina sequencing](https://www.youtube.com/watch?v=TDD6bExnR38)

### Aplications
#### 1. Whole-Genome Sequencing (WGS)
* Re-sequencing
* Single-end or paired-end (cromossome re-arrange)

#### 2. Whole-Exome Sequencing (WES)

#### 3. RNA-seq 

#### 4. CHIP-seq (Chromatin immuno preciptation)

#### 5. Metagenomics (taxonomic analysis)
* Shotgun or 16S rRNA

### Papers
* ['Actionable diagnosis of neuroleptospirosis by NGS'](http://www.nejm.org/doi/full/10.1056/NEJMoa1401268#t=article) - Metagenomics application in neuro biopsy to search for microorganisms disease causing
* ['Analysis of protein-coding genetic variation in 60,706 humans']
* *(http://www.nature.com/nature/journal/v536/n7616/full/nature19057.html?foxtrotcallback=true) - ExAC paper
* ['Neanderthal behaviour, diet, and disease inferred from ancient DNA in dental calculus'](https://www.nature.com/nature/journal/v544/n7650/full/nature21674.html) - 

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
* [File specification](https://samtools.github.io/hts-specs/SAMv1.pdf)
* Soft clip (mask barcode sequences)
* CIGAR string
* [FLAG](https://broadinstitute.github.io/picard/explain-flags.html) and Broad discussion about that, [here](https://gatkforums.broadinstitute.org/gatk/discussion/7019/sam-flags-down-a-boat)

### BAM format
* [Explanation by illumina](https://support.illumina.com/help/BS_App_TruSightTumor15_OLH_1000000001133/Content/Source/Informatics/BAM-Format.htm)
* [By Center for statistical genetics](http://genome.sph.umich.edu/wiki/BAM)
### FastQ_Screen
* Confirm if what you send is what are you receiving 
* RNA seq has alot of ribossomal mix ups

### fastqutils
* Basic statistics

### FastQC
* Basic statistics
* Per base sequence quality (The start and end normally are bad)
* Sequence duplication levels (In Amplicon and microRNA is expected more repetitions)
* Overrepresented sequences (To eliminate it use FastQ_Screen)

### SAM stats (alignment analysis)
* Mapping stats (before and after trimming)
* Error profile

### DynamicTrim.pl (remove bad base qualities)

### Trim_galore (can detect primers used)

### Cutadapt (just remove adpators)

### Starting practice ('02-Aula_pratica_SeqQualityControl.pdf')

## *Part 3*
## Variant Calling
* Start class '03-AulaVariantes.pdf'

### 1. SAMtools

* SAM > BAM
* Mpileup - [interpretation](http://samtools.sourceforge.net/pileup.shtml)
* Dedup can be made by **samtools rmdup** or **picard MarkDuplicates**

**QUESTION**: How differentiate a variant caused by an error after dedup from a somatic variant?

### 2. Varscan (variant call software, normally used for somatic samples)

### 3.  SnpEff (vcf annotation - [site](http://snpeff.sourceforge.net/SnpEff_manual.html))

### 4. Variant Calling Format ([VCF](https://samtools.github.io/hts-specs/VCFv4.1.pdf))
* [Broad explanation](https://gatkforums.broadinstitute.org/gatk/discussion/1268/what-is-a-vcf-and-how-should-i-interpret-it) 
### Start tutorial '03-Aula_pratica_variantes.pdf'
* How many reads sequenced against reference? 341429
* How many reads mapped with quality above MAPQ 30? 327293
* How many correct alignment exist (properly paired)? 317774
> samtools flagstat bwa.bam


