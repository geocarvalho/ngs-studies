## Download data from link below

> **[data link](https://d396qusza40orc.cloudfront.net/gencommand/gencommand_proj1_data.tar.gz)**

1. How many chromosomes are there in the genome?

`$ grep -c ">" apple.genome`

2. How many genes?

`$ cat apple.genes | cut -f 1 | sort -u | wc -l`

3. How many transcript variants?

`$ cat apple.genes | cut -f 1 | sort -u | wc -l`

4. How many genes have a single splice variant?

`$ cut -f 1 apple.genes| uniq -c | grep " 1" | wc -l`

5. How may genes have 2 or more splice variants?

`$ cut -f 1 apple.genes| uniq -c | grep -v " 1" | wc -l`

6. How many genes are there on the ‘+’ strand?

`$ cut -f1,4 apple.genes | grep "+" | sort -u | wc -l`

7. How many genes are there on the ‘-’ strand?

`$ cut -f1,4 apple.genes | grep -v "+" | sort -u | wc -l`

8. How many genes are there on chromosome chr1?

`$ grep "chr1" apple.genes| cut -f1 | sort -u | wc -l `

9. How many genes are there on each chromosome chr2?

`$ grep "chr2" apple.genes| cut -f1 | sort -u | wc -l `

10. How many genes are there on each chromosome chr3?

`$ grep "chr3" apple.genes| cut -f1 | sort -u | wc -l `

11. How many transcripts are there on chr1?

`$ sort -u apple.genes | grep "chr1" | cut -f2 | sort -u | wc -l`

12. How many transcripts are there on chr2?

`$ sort -u apple.genes | grep "chr2" | cut -f2 | sort -u | wc -l`

13. How many transcripts are there on chr3?

`$ sort -u apple.genes | grep "chr3" | cut -f2 | sort -u | wc -l`

14. How many genes are in common between condition A and condition B?

`$ cut -f1 apple.conditionA | sort -u > apple.conditionA.genes`

`$ cut -f1 apple.conditionB | sort -u > apple.conditionB.genes`

`$ cut -f1 apple.conditionC | sort -u > apple.conditionC.genes`

`$ comm -1 -2 apple.conditionA.genes apple.conditionB.genes | wc -l`

15. How many genes are specific to condition A?

`$ comm -2 -3 apple.conditionA.genes apple.conditionB.genes | wc -l`

16. How many genes are specific to condition B?

`$ comm -1 -3 apple.conditionA.genes apple.conditionB.genes | wc -l`

17. How many genes are in common to all three conditions?

`$ comm -1 -2 apple.conditionA.sorted apple.conditionB.sorted > conditionAB`

`$ sort -u apple.conditionC > apple.conditionC.sorted`

`$ comm -1 -2 conditionAB apple.conditionC.sorted | wc -l`
