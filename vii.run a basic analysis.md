# RUN A BASIC ANALYSIS

# A. BACTERIAL GENOME DATA
We will now run some sample code.

First, let’s check our tools:
```
which bwa
```
Output shows where bwa is installed.
```
which samtools
```
Output shows where samtools is installed.

## Basic Bacterial Genome Sequence Analysis

1. Get a reference sequence:
```
mkdir -p /tmp/outbreaks/SG-M1
```
```
cd /tmp/outbreaks/SG-M1
```
```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/
```
```
001/275/545/GCF_001275545.2_ASM127554v2/GCF_001275545.2_ASM127554v2_genomic.fna.gz
```
```
gunzip GCF_001275545.2_ASM127554v2_genomic.fna.gz
mv GCF_001275545.2_ASM127554v2_genomic.fna SG-M1.fna
```
2. Map and call SNPs:
> NOTE: For an annotation of the programs used below and other bioinformatics tools, check out our course github page.
```
bwa index SG-M1.fna
```
```
bwa mem SG-M1.fna /tmp/fastq/SRR6327950/SRR6327950_1.fastq.gz /tmp/fastq/SRR6327950/SRR6327950_2.fastq.gz | samtools view -bS - > SRR6327950.bam
```
```
samtools sort SRR6327950.bam -o SRR6327950-sort.bam
```
```
samtools index SRR6327950-sort.bam
```
```
lofreq faidx SG-M1.fna
```
```
lofreq call -f SG-M1.fna -r NZ_CP012419.2:400000-500000 SRR6327950-sort.bam > SRR6327950-400k.vcf
```
Mapping takes ~5 min on a t2.medium. Sorting takes ~2 min. Running lofreq on this limited section of the genome takes ~1 min.

3. Assembly (runs ~4 min then will run out of RAM if you’re on a t2.medium):
```
spades.py -t 2 -1 /tmp/fastq/SRR6327950/SRR6327950_1.fastq.gz -2 /tmp/fastq/SRR6327950/SRR6327950_2.fastq.gz -o SRR6327950_spades
```
> NOTE: This assembly above will complete on a t3a.large and takes about 5 hours.

Excellent! This is a pretty routine task that can easily be run on an AWS EC2 instance. As experienced when conducting the assembly in Step 3, selecting the right machine for the job is incredibly important. If you run out of RAM or space on your disk, your job may quit. Luckily, these can be easily addressed by changing your instance type or by attaching another EBS volume to your machine.

---

# B. LONG-READ RNA-SEQ DATA
Long read RNA sequencing, Nanopore sequencing, has been widely adopted in different areas. The ability to produce full-length reads of RNAs has been a great improvement to the previous sequencing technologies. This is shown to have the potential to uncover the full genome picture, same as for transcriptomics, where varying isoforms can be discovered in full, fusion isoforms even.

Here we use this long read RNA sequencing analysis tool, Bambu, a R package that can easily process multiple samples in one command that does both transript discovery and quantification. Comparing to other softwares, this software is comparably more efficient while being more sensitive and precise in transcript discovery, which is very important in transcript quantification as well.
```
mkdir -p /tmp/RNASeq
cd /tmp/RNASeq
```
The analysis is run with built-in R, a different language from what you have been using in the rest of the tutorial (bash) and those commands will not work. This session will require a AWS machine of 8GB RAM.

To start an R session
```
R
```
A workspace image keeps track of all the variables you have stored and can be reopened at a later point. For this part of the tutorial it is not needed

To load the bambu packages, which is already pre-installed:
```
library(bambu)
```
Now we will load in a small test dataset including aligned reads, a genome, and annotations. This will be explained in further detail on day 2.
```
test.bam <- system.file("extdata", "SGNex_A549_directRNA_replicate5_run1_chr9_1_1000000.bam", package = "bambu")
  
fa.file <- system.file("extdata", "Homo_sapiens.GRCh38.dna_sm.primary_assembly_chr9_1_1000000.fa", package = "bambu")

gtf.file <- system.file("extdata", "Homo_sapiens.GRCh38.91_chr9_1_1000000.gtf", package = "bambu")
```
Lets try and detect if we can find any novel transcripts in this long-read data and quantify their expression
```
bambuAnnotations <- prepareAnnotations(gtf.file)

se <- bambu(reads = test.bam, annotations = bambuAnnotations, genome = fa.file)
```
You can explore the data with a few commands but we will go into more detail with this in a later tutorial
```
se
head(rowData(se))
head(rowRanges(se))
head(assays(se))
head(assays(se)$counts)
```
The next command will produce a plot showing the different transcripts found for the gene ENSG00000107104 and their expression. We will show you later in the tutorial on how to download this plot to your local machine so you can view it
```
pdf(file = "ENSG00000107104_CPM.pdf", width = 6, height = 8)
plotBambu(se, type = "annotation", gene_id = "ENSG00000107104")
dev.off()
```
> NOTE: To leave the R session at any point once you have opened it. Do not type this in until you have completed the rest of the steps or else you will need to reopen R.
```
quit()
# type in 'n' in response to "Save workspace image?
```
Once you have left R you an see if the file was made
```
ls
You should see a file called ENSG00000107104_CPM.pdf.
```
Congratulations, you now know how to run transcript discovery and quantification using R on an AWS EC2 instance. Next, we’ll look at how to make an AMI from your own machine.