# Module 3 Exam

* [Data link](https://d396qusza40orc.cloudfront.net/gencommand/gencommand_proj3_data.tar.gz)
* I had problems with the Bowtie version. The version used in ther course was 2.2.2, however the 2.2.4 version worked well
```
conda install -c bioconda bowtie2=2.2.4
```

* Rules

> **Apply to questions 1 - 5:**

Generate a bowtie2 index of the wu_0_A genome using bowtie2-build, with the prefix 'wu_0'.

> **Apply to questions 6 - 14:**

Run bowtie2 to align the reads to the genome, under two scenarios: first, to report only full-length matches of the reads; and second, to allow partial (local) matches. All other parameters are as set by default.

For the following set of questions (11 - 20), use the set of full-length alignments calculated under scenario 1 only. Convert this SAM file to BAM, then sort the resulting BAM file.

> **Apply to questions 15 - 19:**

Compile candidate sites of variation using SAMtools mpileup for further evaluation with BCFtools. Provide the reference fasta genome and use the option “-uv” to generate the output in uncompressed VCF format for easy examination.

> **Apply to questions 20 - 24:**

Call variants using ‘BCFtools call’ with the multiallelic-caller model. For this, you will need to first re-run SAMtools mpileup with the BCF output format option (‘-g’) to generate the set of candidate sites to be evaluated by BCFtools. In the output to BCFtools, select to show only the variant sites, in uncompressed VCF format for easy examination.

1. How many sequences were in the genome?
```
% cat  wu_0.v7.fas | grep -c "^>"
```

2. What was the name of the third sequence in the genome file? Give the name only, without the “>” sign.
```
% grep "^>" wu_0.v7.fas 
```

3. What was the name of the last sequence in the genome file? Give the name only, without the “>” sign. 
```
% grep "^>" wu_0.v7.fas 
```

4. How many index files did the operation create?
```
% bowtie2-build wu_0.v7.fas ./wu_0
```
 
5. What is the 3-character extension for the index files created?
bt2

6. How many reads were in the original fastq file?
```
% echo $(cat wu_0_A_wgs.fastq | wc -l)/4 | bc -l
```

7. How many matches (alignments) were reported for the original (full-match) setting? Exclude lines in the file containing unmapped reads.
```
% samtools flagstat wu_0_A_wgs.bt2.sam
```

8. How many matches (alignments) were reported with the local-match setting? Exclude lines in the file containing unmapped reads.
```
% samtools flagstat wu_0_A_wgs.local.bt2.sam
```

9. How many reads were mapped in the scenario in Question 7?
10. How many reads were mapped in the scenario in Question 8?
11. How many reads had multiple matches in the scenario in Question 7? You can find this in the bowtie2 summary; note that by default bowtie2 only reports the best match for each read.​
12. How many reads had multiple matches in the scenario in Question 8? Use the format above. You can find this in the bowtie2 summary; note that by default bowtie2 only reports the best match for each read.​
13. How many alignments contained insertions and/or deletions, in the scenario in Question 7?
```
% cat wu_0_A_wgs.bt2.sam | grep -v "^@" | cut -f6 | grep -c "[D,I]"
```

14. How many alignments contained insertions and/or deletions, in the scenario in Question 8?
```
% cat wu_0_A_wgs.local.bt2.sam | grep -v "^@" | cut -f6 | grep -c "[D,I]"
```

15. How many entries were reported for Chr3?
```
% samtools mpileup -v -u -f ./wu_0/wu_0.v7.fas wu_0_A_wgs.bt2.sorted.bam > wu_0_A_wgs.vcf
% cat wu_0_A_wgs.vcf | grep -v "^#" | grep -c "Chr3"
```

16. How many entries have ‘A’ as the corresponding genome letter?
```
% cat wu_0_A_wgs.vcf | grep -v "^#" | cut -f4 | grep -c '^A$'
```

17. How many entries have exactly 20 supporting reads (read depth)?
```
% cat wu_0_A_wgs.vcf | grep -v "^##" | cut -f8 | grep -c "DP=20;"
```

18. How many entries represent indels?
```
% cat wu_0_A_wgs.vcf | grep -v "^#" | grep -c "INDEL"
```

19. How many entries are reported for position 175672 on Chr1?
```
% cat wu_0_A_wgs.vcf | grep -v "^#" | grep "Chr1" | cut -f2 | grep -c "^175672$"
```

20. How many variants are called on Chr3?
```
% cat wu_0_A_wgs.vcf | grep -v "^#" | grep -c "^Chr3"
```

21. How many variants represent an A->T SNP? If useful, you can use ‘grep –P’ to allow tabular spaces in the search term.
```
% samtools mpileup -g -f ./wu_0/wu_0.v7.fas wu_0_A_wgs.bt2.sorted.bam > wu_0_A_wgs.bcf
% bcftools call -v -m -O v -o wu_0_A_wgs.bcf.vcf wu_0_A_wgs.bcf
% grep -v "^#" wu_0_A_wgs.bcf.vcf | cut -f 4,5 | grep -c "^A   T$"
```

22. How many entries are indels?
```
%
```

23. How many entries have precisely 20 supporting reads (read depth)?
```
%
```

24. What type of variant (i.e., SNP or INDEL) is called at position 11937923 on Chr3?
```
% grep -v "^#" wu_0_A_wgs.bcf.vcf | grep "11937923"
```
