# Mycobacterium tuberculosis example dataset


## Table of contents
1. [Introduction & Aims](#introduction)
2. [Using conda to download tools](#conda)
3. [Run FASTQC](#exercise1)
4. [Nextclade](#exercise2)
5. [Pangolin](#exercise3)
6. [Exploring more data in Nextclade](#exercise4)

## 1. Introduction <a name="introduction"></a>

The goal of this exercise is look at basic QC data from Illumina sequencing reads and to use a trimming tool to remove adapter sequences and poor quality bases.

First move into the TB dataset folder:
```bash
cd TB_module
```
If you list the contents of this directory `ll` it should appear like the following:
![](figures/TB_fig1.png)

We are working with two samples that have been sequenced with Illumina via paired-end sequencing. Therefore, each sample has a forward and reverse read. The forward read is usually denoted by  `SAMPLE-NAME_1.fastq.gz` and the reverse is usually denoted by `SAMPLE-NAME_2.fastq.gz`

 [↥ **Back to top**](#top)

 ******
## Using `conda` to install and download tools <a name="conda"></a>


 ******
## Run FASTQC <a name="exercise1"></a>

The program `fastqc` is widely used to assess the quality metrics of Illumina sequencing data. Here we will be running `fastqc` on both of the TB samples before and after we perform read and adapter trimming.

As with nearly all programs you can get see how to run the program by either just typing the program name, in this case `fastqc` or typing `fastqc -h`.

`fastqc` in default mode simply expects fastq files after the program name like so : `fastqc SAMPLE_1.fastq.gz`. We have to run the program for each the forward and the reverse reads per sample -- so a total of 4 files. Thankfully we can list all the files at one time.

Run `fastqc` on each of the files:
```bash
fastqc TBsample1_1.fastq.gz TBsample1_2.fastq.gz TBsample2_1.fastq.gz TBsample2_2.fastq.gz
```
![](figures/TB_fig2.png)

When this has finished, we are now going to use a related program called `multiqc` to combine and visualize the output of `fastqc`.

Run `multiqc` here and don't forget the period at the end of this command! The period simply designates run "here" in the current folder
```bash
multiqc .
```
![](figures/TB_fig3.png)

Type `ls` to see all the new files we have generated
![](figures/TB_fig4.png)

| File type | Description |
| ------------- |:-------------:
| .fastq.gz     | original fastq data
| _fastqc.zip      | data folder for output of fastqc compressed as zip file
| _fastqc.html      | fastqc results per file that can be viewed in web browser
| multiqc_data     | multiqc combines the data across all `fastqc` and stores them in this folder
| multiqc_report.html     | results of multiqc that can be viewed in web browser

The main output for us to view from `multiqc` is the `multiqc_report.html` file. We can either open this file by using the file browser and clicking on the html file, or we can open a web browser from the command line.

To view results type:
```bash
firefox multiqc_report.html
```
This will open firefox and load the file into an interactive session.

![](figures/TB_fig5.png)

There are many different statistics and QC analysis that is output by `fastqc / multiqc`. Here we are going to highlight the most important ones, but I encourage you to look over all of them and use the nice built-in descriptions to learn more.

### **General Statistics Table**

The first is the nice **General Statistics** table at the very start. This is s nice summary per file (remember it is a separate file for both forward and reverse reads per sample)
![](figures/TB_fig6.png)

### **Sequence quality histograms**
**This is THE most important plot.** This plot shows you the average quality score per base across summarized across the read length.

Sequences with good quality scores across the read are colored green.

Sequences with poor quality scores across the read are colored red.

![](figures/TB_fig7.png)

##### *Questions*
1. Which sample has good quality scores?
2. Which sample has bad?
3. What is happening at the end of the reads in terms of quality?

A related plot is the per sequence quality scores histogram which gives the total number of reads that have a given mean quality score.

![](figures/TB_fig8.png)

### GC histogram
Another important plot is to look at the mean GC content of the reads. This plot often gives us an indication of whether or not we have contamination. If you are sequencing a single organism in your sample(s), the GC content should follow a normal 'bell' distribution. If there happened to be contaminating DNA from another organism it will likely have a slightly different mean GC and this shows up a 'bump' in the distribution.
![](figures/TB_fig9.png)


There are other useful plots and analyses that are in the  fastQC / multiQC report. I encourage you to look at these and explore the data!


[↥ **Back to top**](#top)
*******

## Trimming reads <a name="exercise2"></a>

Next, we will be using the  tool **fastp** to trim (ie remove) poor quality data and contaminating adapter sequences

```bash
fastp -i TBsample1_1.fastq.gz -I TBsample1_2.fastq.gz -o TBsample1_1_trim.fastq.gz -O TBsample1_2_trim.fastq.gz
```

Now run **fastp** for the second sample:
```bash
fastp -i TBsample2_1.fastq.gz -I TBsample2_2.fastq.gz -o TBsample2_1_trim.fastq.gz -O TBsample2_2_trim.fastq.gz
```

Now we need to run **FASTQC** on the trimmed fastq files:
```bash
fastqc *trim.fastq.gz
```
Here we utilized a "shortcut" by using the `*trim.fastq.gz` which allows us to match all files that have the ending `trim.fastq.gz`, instead of us having to type each file individually.  
