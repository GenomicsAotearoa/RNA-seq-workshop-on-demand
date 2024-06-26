
# Quality control of the sequencing data.

!!! info "Objectives"

    - Assess the quality of your data
    - Use FastQC package to do quality check
    - Use MultiQC to view our analysis results


<center>
![image](./theme_images/qc_image.png){width="350"}
</center>


---

Several tools available to do quality assessment. For this workshop, we will use `fastqc`.

First, it is always good to verify where we are:

```bash
cd ~
pwd
```

```
    /home/shared/TRAINING1
# Instead of "TRAINING1" you should see the training username you used to log in. 
```

Checking to make sure we have the Raw files for the workshop.

```bash
ls
```
```
    RNA_seq ...
```

Creating a directory where to store the QC data:

```bash
cd RNA_seq
ls
```
```
    Genome  Raw  rsmodules.sh  yeast_counts_all_chr.txt
```

```bash
mkdir QC
```

NOTE: If you are familiar with working on the NeSI HPC, you will be used to loading modules (software). With the OnDemand system we do not need to load modules. 





We start with quality control:

```bash
fastqc -o QC/ Raw/*
```
You will see an automatically updating output message telling you the progress of the analysis. It will start like this:

```
    Started analysis of SRR014335-chr1.fastq
    Approx 5% complete for SRR014335-chr1.fastq
    Approx 10% complete for SRR014335-chr1.fastq
    Approx 15% complete for SRR014335-chr1.fastq
    Approx 20% complete for SRR014335-chr1.fastq
    Approx 25% complete for SRR014335-chr1.fastq
    Approx 30% complete for SRR014335-chr1.fastq
    Approx 35% complete for SRR014335-chr1.fastq
```

The FastQC program has created several new files within our ***~/RNA_seq/QC/*** directory.

```bash
ls QC
```
```bash
    SRR014335-chr1_fastqc.html  SRR014336-chr1_fastqc.zip   SRR014339-chr1_fastqc.html  SRR014340-chr1_fastqc.zip
    SRR014335-chr1_fastqc.zip   SRR014337-chr1_fastqc.html  SRR014339-chr1_fastqc.zip   SRR014341-chr1_fastqc.html
    SRR014336-chr1_fastqc.html  SRR014337-chr1_fastqc.zip   SRR014340-chr1_fastqc.html  SRR014341-chr1_fastqc.zip
```

## Viewing the FastQC results


![image](./Images/fqc1_2.png)

## Working with the FastQC text output

Now that we’ve looked at our HTML reports to get a feel for the data, let’s look more closely at the other output files. Go back to the tab in your terminal program that is connected to NeSI and make sure you’re in our results subdirectory.

```bash
cd ~/RNA_seq/QC
ls
```
```
    SRR014335-chr1_fastqc.html  SRR014336-chr1_fastqc.zip   SRR014339-chr1_fastqc.html  SRR014340-chr1_fastqc.zip
    SRR014335-chr1_fastqc.zip   SRR014337-chr1_fastqc.html  SRR014339-chr1_fastqc.zip   SRR014341-chr1_fastqc.html
    SRR014336-chr1_fastqc.html  SRR014337-chr1_fastqc.zip   SRR014340-chr1_fastqc.html  SRR014341-chr1_fastqc.zip
```
Let's unzip the files to look at the FastQC text file outputs.

```bash
for filename in *.zip
do
unzip $filename
done
```

Inside each unzipped folder, there is a summary text which shows results of the statistical tests done by FastQC

```
ls SRR014335-chr1_fastqc
```
```
    fastqc_data.txt  fastqc.fo  fastqc_report.html	Icons/	Images/  summary.txt
```

Use less to preview the summary.txt file

```
less SRR014335-chr1_fastqc/summary.txt
# Use "q" to exit (quit) out of the window when you are done.
```

We can make a record of the results we obtained for all our samples by concatenating all of our summary.txt files into a single file using the cat command. We’ll call this fastqc_summaries.txt.

```
cat */summary.txt > ~/RNA_seq/QC/fastqc_summaries.txt 
```

* Have a look at the ***fastqc_summaries.txt*** and search for any of the samples that have failed the QC statistical tests.

---

## MultiQC -  multi-sample analysis

!!! quote ""

     - The FastQC analysis is applied to each sample separately, and produces a report for each.
     - The application MultiQC provides a way to combine multiple sets of results (i.e., from MANY 
     different software packages) across multiple samples.
     - To generate `multiqc` results, run the following command in the directory with the output files you want to summarise (e.g., fastqc reports generated above):
    
```bash
cd ~/RNA_seq/
mkdir MultiQC
cd MultiQC
cp ../QC/* ./
multiqc .
ls -F
```
```bash
    multiqc_data/  multiqc_report.html
```
The html report shows the MultiQC summary

![image](./Images/MQC1.png)

- - - 

<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/RNA-seq-workshop/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>
