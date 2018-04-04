# Module 2 Exam

* [Data link](https://d396qusza40orc.cloudfront.net/gencommand/gencommand_proj2_data.tar.gz)
* Some rules:

> **Questions 1 - 5:**

> For the original set of alignments (file ‘athal_wu_0_A.bam’):

> **Questions 6 - 10:**

> Extract only the alignments in the range “Chr3:11777000-11794000”, corresponding to a locus of interest. For this alignment set:

> **Questions 11 - 15:**

> Determine general information about the alignment process from the original BAM file.

> **Questions 16 - 20:**

> Using BEDtools, examine how many of the alignments at point 2 overlap exons at the locus of interest. Use the BEDtools ‘-wo’ option to only report non-zero overlaps. The list of exons is given in the included ‘athal_wu_0_A_annot.gtf’ GTF file.
---

1. How many alignments does the set contain?
> First, a couple of introductory comments. A BAM file contains alignments for a set of input reads. Each read can have 0 (none), 1 or multiple alignments on the genome. These questions explore the relationships between reads and alignments.

The number of alignments is the number of entries, excluding the header, contained in the BAM file, or equivalently in its SAM conversion. To find the number of alignments, we can apply (‘%’ denotes the terminal prompt):

```
% samtools flagstat athal_wu_0_A.bam
```

> which will list the number of alignments on line 1 (total). An alternate method would be to count the number of lines in the converted SAM file (header excluded):
```
% samtools view athal_wu_0_A.bam | wc -l
```

> Note that, if the file was created with a tool that includes unmapped reads into the BAM file, we would need to exclude the lines representing unmapped reads, i.e. with a ‘*’ in column 3 (chrom):
```
% samtools view athal_wu_0_A.bam | cut –f3 | grep –v ’*’ | wc -l
```

2. How many alignments show the read’s mate unmapped?
> An alignment with an unmapped mate is marked with a ‘*’ in column 7. Note that the question asks for alignments, not reads, so we simply count the number of lines in the SAM file with a ‘*’ in column 7:
```
% samtools view athal_wu_0_A.bam | cut -f7 | grep -c "*" 
% samtools view athal_wu_0_A.bam | grep -c unknown 
```

3. How many alignments contain a deletion (D)?
> Deletions are be marked with the letter ‘D’ in the CIGAR string for the alignment, shown in column 6:

```
% samtools view athal_wu_0_A.bam | cut -f6 | grep -c "D"
```

4. How many alignments show the read’s mate mapped to the same chromosome?
```
% samtools view athal_wu_0_A.bam | cut -f7 | grep -c "="
```

5. How many alignments are spliced?
```
% samtools view athal_wu_0_A.bam | cut -f6 | grep -c "N"
```

6. How many alignments does the set contain?
```
% samtools view athal_wu_0_A.bam "Chr3:11777000-11794000" | wc -l
```

7. How many alignments show the read’s mate unmapped?
```
% samtools view athal_wu_0_A.bam "Chr3:11777000-11794000" | cut -f7 | grep -c "*"
```

8. How many alignments contain a deletion (D)?
```
% samtools view athal_wu_0_A.bam "Chr3:11777000-11794000" | cut -f6 | grep -c "D"
```

9. How many alignments show the read’s mate mapped to the same chromosome?
```
% samtools view athal_wu_0_A.bam "Chr3:11777000-11794000" | cut -f7 | grep -c "="
```

10. How many alignments are spliced?
```
% samtools view athal_wu_0_A.bam "Chr3:11777000-11794000" | cut -f6 | grep -c "N"
```

X11. How many sequences are in the genome file?
> This information can be found in the header of the BAM file. Starting with the original BAM file, we use samtools to display the header information and count the number of lines describing the sequences in the reference genome:
```
% samtools view -H athal_wu_0_A.bam | grep -c "SN:"
```

X12. What is the length of the first sequence in the genome file?
> The length information is stored alongside the sequence identifier in the header (pattern ‘LN:seq_length’):
```
% samtools view –H athal_wu_0_A.bam | grep “SN:” | more
% samtools view -H athal_wu_0_A.bam | grep "SN:" | head -1 
```

13. What alignment tool was used?
> Used the name in ID:name
```
% samtools view -H athal_wu_0_A.bam | grep "@PG"
```

14. What is the read identifier (name) for the first alignment?
```
% samtools view athal_wu_0_A.bam | head -1 | cut -f1
```

15. What is the start position of this read’s mate on the genome? Give this as ‘chrom:pos’ if the read was mapped, or ‘*” if unmapped.
```
% samtools view athal_wu_0_A.bam | head -1 | cut -f3,4
```
> Chr3:11700332

16. How many overlaps (each overlap is reported on one line) are reported?
> We start by running BEDtools on the alignment set restricted to the specified region (Chr3:11777000-11794000) and the GTF annotation file listed above. To allow the input to be read directly from the BAM file, we use the option ‘-abam’; in this case we will need to also specify ‘-bed’ for the BAM alignment information to be shown in BED column format in the output:
```
% bedtools intersect -abam athal_wu_0_A.bam -b athal_wu_0_A_annot.gtf -bed -wo > overlaps.bed
```
> This will create a file with the following format: Columns 1-12 : alignment information, converted to BED format Columns 13-21 : annotation (exon) information, from the GTF file Column 22 : length of the overlapAlternatively, we could first convert the BAM file to BED format using ‘bedtools bamtobed’ then use the resulting file in the ‘bedtools intersect’ command. To answer the question, the number of overlaps reported is precisely the number of lines in the file (because only entries in the first file that have overlaps in file B are reported, according to the option ‘-wo’):
```
% wc -l overlaps.bed
```
17. How many of these are 10 bases or longer?
> The size of the overlap is listed in column 22 of the ‘overlaps.bed’ file. To determine those longer than 10 bases, we extract the column, sort numerically in decreasing order, and simply determine by visual inspection of the file the number of such records. For instance, in ‘vim’ we search for the first line listing ‘9’ (‘:/9’), then determine its line number (Ctrl+g). Alternatively, one can use grep with option ‘-n’ to list the lines and corresponding line numbers:

```
% cat overlaps.bed | cut -f22 | sort -nrk1 | grep "[1-9][0-9]" | wc -l
```
> The last command you have to know if is possible any number > 99, if is you add another [0-9] besides the regex

18. How many alignments overlap the annotations?
```
% cut -f1-12 overlaps.bed | sort -u | wc -l
```
19. Conversely, how many exons have reads mapped to them?
> Columns 13-21 define the exons:
```
% cut -f13-21 overlaps.bed | sort -u | wc -l
```
20. If you were to convert the transcript annotations in the file “athal_wu_0_A_annot.gtf” into BED format, how many BED records would be generated?
> This question simply asks for the number of transcripts in the annotation file, since the BED format would represent each transcript on one line. This information can be obtained from column 9 in the GTF file:
```
% cut -f9 athal_wu_0_A_annot.gtf | cut -d ' ' -f1,2 | sort -u | wc -l
```
