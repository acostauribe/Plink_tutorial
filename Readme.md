# Intro to Plink
#### Juliana Acosta-Uribe RMP, UCSB 2021

#### 1. Individual based filtering

Filter individuals in or out of the analysis. -> We care about the .fam file
Open a text editor: 

```
nano
```

Paste the IDs of the individuals you want to keep. Plink always requieres 2 columns for IDS [Fam-ID, Ind-ID]

```nano
F-10262	4888-HLI-10262
F-10514	4888-HLI-10514
F-10516	4888-HLI-10516
F-10528	4888-HLI-10528
F-10571	4888-HLI-10571
```
[ctrl + o]  to save the file  
[ctrl + x] to exit 

Remember: To name a file use a dot, a dash or an underscore instead of spaces. Never use /
 
 Use Plink to keep only these 5 genomes
 
```
 [acostauribe@kosik Training_RMP]$ plink --bfile RMP --keep 5_individuals --make-bed --out RMP_5-individuals
PLINK v1.90b6.7 64-bit (2 Dec 2018)            www.cog-genomics.org/plink/1.9/
(C) 2005-2018 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to RMP_5-individuals.log.
Options in effect:
  --bfile RMP
  --keep 5_individuals
  --make-bed
  --out RMP_5-individuals

32007 MB RAM detected; reserving 16003 MB for main workspace.
12493355 variants loaded from .bim file.
30 people (12 males, 18 females) loaded from .fam.
30 phenotype values loaded from .fam.
--keep: 5 people remaining.
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 0 founders and 5 nonfounders present.
Calculating allele frequencies... done.
Total genotyping rate in remaining samples is 0.999244.
12493355 variants and 5 people pass filters and QC.
Among remaining phenotypes, 5 are cases and 0 are controls.
--make-bed to RMP_5-individuals.bed + RMP_5-individuals.bim +
RMP_5-individuals.fam ... done.
```

What does plink mean by founders and non founders?

```
Before main variant filters, 0 founders and 5 nonfounders present
```

When I do any type of "alelic frequency analysis" plink will only use founders
F-10601	4888-HLI-10601	0	0	1	2 --> Founder; has 0 for dad and mom id
F-10611	4888-HLI-10611	10611XY	10611XX	2	2 --> nonfounder, had dad and mom ids


#### 2. Position based filtering

Get a region of interest based on the genomic coordinates:

```
acostauribe@kosik Training_RMP]$ plink --bfile RMP_5-individuals --chr 1 --from-bp 1 --to-bp 1000000 --recode tab --out RMP_5-individuals.chr1_1-1000000
PLINK v1.90b6.7 64-bit (2 Dec 2018)            www.cog-genomics.org/plink/1.9/
(C) 2005-2018 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to RMP_5-individuals.chr1_1-1000000.log.
Options in effect:
  --bfile RMP_5-individuals
  --chr 1
  --from-bp 1
  --out RMP_5-individuals.chr1_1-1000000
  --recode tab
  --to-bp 1000000

32007 MB RAM detected; reserving 16003 MB for main workspace.
9856 out of 12493355 variants loaded from .bim file.
5 people (3 males, 2 females) loaded from .fam.
5 phenotype values loaded from .fam.
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 0 founders and 5 nonfounders present.
Calculating allele frequencies... done.
Total genotyping rate is 0.992573.
9856 variants and 5 people pass filters and QC.
Among remaining phenotypes, 5 are cases and 0 are controls.
--recode ped to RMP_5-individuals.chr1_1-1000000.ped +
RMP_5-individuals.chr1_1-1000000.map ... done.
```

Get a region of interest based on the SNP identifiers.
First, make a lisyt of the identifiers of the variants you want e.g.

```
rs1455322520
rs1161275668
rs1431522956
rs533630043
rs1369025799
rs868389710
rs866988825
```

Use plink to extract those seven variants and recode it as a .ped file

```
[acostauribe@kosik Training_RMP]$ plink --file RMP_5-individuals.chr1_1-1000000 --extract 7_SNPs --recode tab 12 --out RMP_5-individuals.chr1_1-1000000.7_snps
PLINK v1.90b6.7 64-bit (2 Dec 2018)            www.cog-genomics.org/plink/1.9/
(C) 2005-2018 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to RMP_5-individuals.chr1_1-1000000.7_snps.log.
Options in effect:
  --extract 7_SNPs
  --file RMP_5-individuals.chr1_1-1000000
  --out RMP_5-individuals.chr1_1-1000000.7_snps
  --recode tab 12

32007 MB RAM detected; reserving 16003 MB for main workspace.
Possibly irregular .ped line.  Restarting scan, assuming multichar alleles.
.ped scan complete (for binary autoconversion).
Performing single-pass .bed write (9856 variants, 5 people).
--file: RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bed +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bim +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.fam written.
9856 variants loaded from .bim file.
5 people (3 males, 2 females) loaded from .fam.
5 phenotype values loaded from .fam.
--extract: 7 variants remaining.
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 0 founders and 5 nonfounders present.
Calculating allele frequencies... done.
7 variants and 5 people pass filters and QC.
Among remaining phenotypes, 5 are cases and 0 are controls.
--recode ped to RMP_5-individuals.chr1_1-1000000.7_snps.12.ped +
RMP_5-individuals.chr1_1-1000000.7_snps.12.map ... done.
```

the output should look like this :

```
[acostauribe@kosik Training_RMP]$ more  RMP_5-individuals.chr1_1-1000000.7_snps.ped
F-10262	4888-HLI-10262	10262XY	10262XX	2	2	G G	G G	T T	G G	C C	G G	G G
F-10514	4888-HLI-10514	10514XY	10514XX	1	2	G G	G G	T T	G G	C C	G G	G G
F-10516	4888-HLI-10516	10516XY	10516XX	2	2	G G	G G	T T	G G	C C	G G	G G
F-10528	4888-HLI-10528	6072	10528XX	1	2	G G	G G	T T	G G	C C	G G	G G
F-10571	4888-HLI-10571	10571XY	10571XX	1	2	G G	G G	T T	G G	C C	G G	G G

[acostauribe@kosik Training_RMP]$ more  RMP_5-individuals.chr1_1-1000000.7_snps.map
1	rs1455322520	0	15527
1	rs1161275668	0	15566
1	rs1431522956	0	15584
1	rs533630043	0	15585
1	rs1369025799	0	15589
1	rs868389710	0	15599
1	rs866988825	0	15616
```

You can use --recode 12 to do a 1/2 recoding for allele1/allele 2.
It would look like this:

```
[acostauribe@kosik Training_RMP]$ plink --file RMP_5-individuals.chr1_1-1000000 --extract 7_SNPs --recode tab 12 --out RMP_5-individuals.chr1_1-1000000.7_snps
PLINK v1.90b6.7 64-bit (2 Dec 2018)            www.cog-genomics.org/plink/1.9/
(C) 2005-2018 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to RMP_5-individuals.chr1_1-1000000.7_snps.log.
Options in effect:
  --extract 7_SNPs
  --file RMP_5-individuals.chr1_1-1000000
  --out RMP_5-individuals.chr1_1-1000000.7_snps
  --recode tab 12

32007 MB RAM detected; reserving 16003 MB for main workspace.
Possibly irregular .ped line.  Restarting scan, assuming multichar alleles.
.ped scan complete (for binary autoconversion).
Performing single-pass .bed write (9856 variants, 5 people).
--file: RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bed +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bim +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.fam written.
9856 variants loaded from .bim file.
5 people (3 males, 2 females) loaded from .fam.
5 phenotype values loaded from .fam.
--extract: 7 variants remaining.
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 0 founders and 5 nonfounders present.
Calculating allele frequencies... done.
7 variants and 5 people pass filters and QC.
Among remaining phenotypes, 5 are cases and 0 are controls.
--recode ped to RMP_5-individuals.chr1_1-1000000.7_snps.12.ped +
RMP_5-individuals.chr1_1-1000000.7_snps.12.map ... done.
```

Output

```
[acostauribe@kosik Training_RMP]$ more RMP_5-individuals.chr1_1-1000000.7_snps.12.ped 
F-10262	4888-HLI-10262	10262XY	10262XX	2	2	2 2	2 2	2 2	2 2	2 2	2 2	2 2
F-10514	4888-HLI-10514	10514XY	10514XX	1	2	2 2	2 2	2 2	2 2	2 2	2 2	2 2
F-10516	4888-HLI-10516	10516XY	10516XX	2	2	2 2	2 2	2 2	2 2	2 2	2 2	2 2
F-10528	4888-HLI-10528	6072	10528XX	1	2	2 2	2 2	2 2	2 2	2 2	2 2	2 2
F-10571	4888-HLI-10571	10571XY	10571XX	1	2	2 2	2 2	2 2	2 2	2 2	2 2	2 2

[acostauribe@kosik Training_RMP]$ more RMP_5-individuals.chr1_1-1000000.7_snps.12.map 
1	rs1455322520	0	15527
1	rs1161275668	0	15566
1	rs1431522956	0	15584
1	rs533630043	0	15585
1	rs1369025799	0	15589
1	rs868389710	0	15599
1	rs866988825	0	15616
```
*Look that the map files are identical!*


You can also use the --recode funtion to create a vcf file

```
[acostauribe@kosik Training_RMP]$ plink --file RMP_5-individuals.chr1_1-1000000 --extract 7_SNPs --recode vcf --out RMP_5-individuals.chr1_1-1000000.7_snps
PLINK v1.90b6.7 64-bit (2 Dec 2018)            www.cog-genomics.org/plink/1.9/
(C) 2005-2018 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to RMP_5-individuals.chr1_1-1000000.7_snps.log.
Options in effect:
  --extract 7_SNPs
  --file RMP_5-individuals.chr1_1-1000000
  --out RMP_5-individuals.chr1_1-1000000.7_snps
  --recode vcf

32007 MB RAM detected; reserving 16003 MB for main workspace.
Possibly irregular .ped line.  Restarting scan, assuming multichar alleles.
.ped scan complete (for binary autoconversion).
Performing single-pass .bed write (9856 variants, 5 people).
--file: RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bed +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.bim +
RMP_5-individuals.chr1_1-1000000.7_snps-temporary.fam written.
9856 variants loaded from .bim file.
5 people (3 males, 2 females) loaded from .fam.
5 phenotype values loaded from .fam.
--extract: 7 variants remaining.
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 0 founders and 5 nonfounders present.
Calculating allele frequencies... done.
7 variants and 5 people pass filters and QC.
Among remaining phenotypes, 5 are cases and 0 are controls.
--recode vcf to RMP_5-individuals.chr1_1-1000000.7_snps.vcf ... done.

[acostauribe@kosik Training_RMP]$ more RMP_5-individuals.chr1_1-1000000.7_snps.vcf 
##fileformat=VCFv4.2
##fileDate=20210628
##source=PLINKv1.90
##contig=<ID=1,length=1000000>
##INFO=<ID=PR,Number=0,Type=Flag,Description="Provisional reference allele, may not be based on real reference genome">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	F-10262_4888-HLI-10262	F-10514_4888-HLI-10514	F-10516_4888-HLI-10516	F-10528_4888-HLI-10528	F-10571_4888-HLI-10571
1	15527	rs1455322520	G	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15566	rs1161275668	G	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15584	rs1431522956	T	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15585	rs533630043	G	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15589	rs1369025799	C	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15599	rs868389710	G	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
1	15616	rs866988825	G	.	.	.	PR	GT	0/0	0/0	0/0	0/0	0/0
```
