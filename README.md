# FASTQC
### *by James Lee, Nina Yang, Matthew Lutz-Paap*


### RNA-Seq Analysis Pipeline
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/RNA-seq%20pipeline.JPG)
</br>
Before any RNA-seq analysis, we need to have a good quality sequence data in order to carry out any further analysis. Above is the RNA-Seq Analysis pipeline that shows where the quality control of sequence data is needed. 
<br/>
<br>

![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/Capture.JPG)
</br>
The most common sequence data files are FASTQ, BAM or SAM files.
FASTQ file is a text-based file that stores both biological sequences and their quality scores. 
<br/>
**FASTQC** is the software that checks the quality of these sequence files by running several tests. It takes in data from FastQ files and provide a quick overview to tell you which areas of the sequence may have problems. Each test will be flagged with either a pass, warning or fail sign depending on the results. 
</br>
The report is generated as a HTML file.
<br/>

## How to start FASTQC
In order to use FASTQC, you will first need to have the UNIX environment on your device. </br>
For Windows users, you will need to have WSL(Windows Subsystem for Linux) and *Ubuntu*. Check their [website](https://docs.microsoft.com/en-us/windows/wsl/install-win10) for further details. <br/>
Once you have the Unix environment, set up a Conda virtual environment for RNAseq Analysis. (Install Miniconda and Bioconda) </br>
You will also need to install the following packages: <br/>

```
conda install fastp=0.20.0 
conda install fastqc=0.11.8
conda install subread=1.6.4
conda install samtools=1.9
conda install htseq=0.11.2
conda install hisat2=2.1.0
conda install star=2.7.2b
```

Once these are intalled, you can use "fastqc" command to run it on your fastq files. You will need your own fastq files. Both NCBI and EBI provide already-analyzed raw sequence data. 
<br>
- [NCBI-SRA](https://www.ncbi.nlm.nih.gov/sra) - Main respositoy of raw sqeeucene data (fastq format)   


- [EBI-ENA](https://www.ebi.ac.uk/ena/browser/home) - similar database but on EBI  

<br>
Use "fastqc -h" to get the help page. 
<br>
Example below: 

```
fastqc 2cells_1.fastq  2cells_2.fastq  6h_1.fastq  6h_2.fastq 
```

Once FASTQC is done, it will generate an HTML file for each fastq file you type in. 

### 1. Basic Statistics 
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/1.%20Basic%20Statistics.JPG)
<br/>
Above is the first summary table you get from the FASTQC report. You can read the basic information about the fastq sequence data, including the encoding type, total sequence numbers, sequence flagged as poor quality, sequence length and GC content. 
</br>
<br/>

### 2. Per base sequence quality
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/2.%20Per%20base%20sequence%20quality.JPG)
</br>
Next, Per base sequence quality shows a graphical representation of the quality scores provided in the fastq file. The good quality scores stay in the green area. there may be a warning or failure sign if the median score drops to the yellow or red zone. But it’s normal to see lower qualities at the beginning and end of this plot with modern sequencing techniques.

<br/>

### 3. Per tile sequence quality 
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/3.%20Per%20tile%20sequence%20quality.JPG)
<br/>
This module only shows up on fastqc reports with data from Illumina sequencing. This figure maps physical locations on the flow cell used for sequencing onto a graph and analyzes the average performance of each tile in comparison to the overall average score of all reads. Tiles that are marked with cooler colors reported average or above average scores, while warmer colors indicate that a given tile has underperformed, reporting below average scores. This test can provide some insight into whether dubious results elsewhere in the report can be attributed to an error in sequencing itself or some other part of the pipeline.
</br>
<br/>

### 4. Per sequence quality score
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/4.%20Per%20sequence%20quality%20scores.JPG)
<br/>
This graph plots the number of appearances of each mean quality score among all sequences in the report. While a small percentage of reads are expected to have a low mean score, high quality data generally has a high concentration of high mean scores, manifesting in this graph as a fairly narrow peak over the high end of the x-axis.
</br>

### 5. Per base sequence content
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/5.%20Per%20base%20sequence%20content.JPG)
<br/>
This module reports the percentages of each base that appears in every position across all sequences in the report. For example, in the above figure, approximately 40% of sequences in this report started with G (position 1). Expected results for this test meaningfully differ between DNA-seq and RNA-seq results, although fastqc is unable to differentiate between the two, often resulting in RNA-seq data automatically failing regardless of quality. Generally, good results for this test have A% matching T% and C% matching G%, creating two sets of parallel lines over all positions. RNA-seq data is expected to deviate from this pattern for the first 10-15 bases, which often results in this test failing.
<br/>

### 6. Per sequence GC content
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/6.%20Per%20sequence%20GC%20content.JPG)
<br/>
This module measures the GC content of each sequence in a file and compares it to a modelled normal distribution of GC content. Warnings and failures in this test are based on the total deviation between the theoretical and actual distributions. There are a wide variety of reasons for this test to potentially be marked with a warning or failure, even with otherwise good data, making it difficult to draw effective conclusions from this test. In this example specifically, there are several very narrow peaks that deviate strongly from the theoretical distribution, indicating that there are some potentially overrepresented sequences with distinct GC contents.
<br/>

### 7. Per base N content
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/7.%20Per%20base%20N%20content.JPG)
<br/>
“N” is a placeholder base call, specifically used when read quality is too low to make a confident base call for a given position. Per base N content measures the percentage of N calls for every position across all sequences read. Because early and late positions tend to have lower scores due to current technologies, it is not uncommon for N to be called early or late in sequences. Aside from that, N calls should make up a very small proportion of base calls in high quality data, with 5% N content being the default value to raise a warning for this test in fastqc.
<br/>

### 8. Sequence Length Distribution
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/8.%20Sequence%20length%20distribution.JPG)
<br/>
Sequence length distribution module reports if all sequences have the same length or not, generating a graph showing the distribution of fragment sizes in the file that was analysed. In many cases this will produce a simple graph showing a peak only at one size, but for variable length FastQ files this will show the relative amounts of each different size of sequence fragment. In this graph a peak was shown at the location of 100bp. This module will raise a warning if all sequences are not the same length, and will raise an error if any of the sequences have zero length. For some sequencing platforms it is entirely normal to have different read lengths so warnings here can be ignored.
<br/>

### 9. Sequence Duplication Levels
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/9.%20Sequence%20Duplication%20Levels.JPG)
<br/>
This module counts the degree of duplication for every sequence in a library and creates a plot showing the relative number of sequences with different degrees of duplication.
There are two lines on the plot:
-The blue line takes the full sequence set and shows how its duplication levels are distributed.
-The red line displays the proportions of the sequences that are deduplicated which come from different duplication levels in the original data.
</br>
It will issue a warning if non-unique sequences make up more than 20% of the total, and will issue an error if non-unique sequences make up more than 50% of the total. <br/>
A warning or error in this module is simply a statement that you have exhausted the diversity in at least part of your library and are re-sequencing the same sequences. In a supposedly diverse library this would suggest that the diversity has been partially or completely exhausted and that you are therefore wasting sequencing capacity. However in some library types you will naturally tend to over-sequence parts of the library and therefore generate duplication and will therefore expect to see warnings or errors from this module.
</br>
<br/>

### 10. Overrepresented Sequence
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/10.%20Overrepresented%20Sequences.JPG)
<br/>
This module lists all of the sequences which make up more than 0.1% of the total. To conserve memory only sequences which appear in the first 100,000 sequences are tracked to the end of the file. Therefore, it is possible that a sequence which is overrepresented but doesn't appear at the start of the file for some reason could be missed by this module. <br/>
The sequence are truncated to 50bp for the purpose of analysis and the program will look for matches of common contaminants and will report the best hit it finds. Even if some hits were found, it doesn't necessarily mean that this is the source of the contamination, but may show you a possible direction. Hits must be at least 20 bp in length and have no more than 1 mismatch. </br>
A warning is issued if any sequence is found to represent more than 0.1% of the total. A failure is issued if any sequence is found to represent more than 1% of the total. 

<br/>

### 11. Adapter Content
![alt text](https://github.com/jameshyojaelee/FASTQC/blob/main/11.%20Adapter%20Content.JPG)
<br/>
This chart shows a cumulative percentage of the proportion of the library in which each of the adapter sequences at each position has been detected. Once a sequence has been detected in a read, it is counted as being present right through to the end of the read so the percentage increases as the read length continues. In this plot,  X-axis shows the position in read, where the Y-axis shows the amount of adapter content. <br/>
A warning is issued if any sequence is present in more than 5% of all reads. A failure is issued if any sequence is present in more than 10% of all reads. </br>
This module indicates if the sequences will need to be trimmed for adapters before proceeding with any downstream analysis. 

<br/>
 <br/>



### Reference 

[FastQC Github Page](https://github.com/s-andrews/FastQC/) 
<br/>
[FastQC Manual by University of Missouri](https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf)
<br/>
[FastQC Manual by Babraham Institute](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/)
<br/>
[BENG183 Github Page](https://github.com/Irenexzwen/BIOE183/blob/master/Tutorial1_Preparation.md)




