# MSI QIIME2 Pipeline Tutorial
This tutorial will be based on QIIME2 Version 2018.11. Commands and formats of files may change depending on the version of QIIME2 you are using. 

_Documentation for QIIME2 Ver. 2018.11 can be found [here](https://docs.qiime2.org/2018.11/index.html)._

## Step 1: Getting Started
 ### Software Requirements 
  1) File transfer Software like FileZilla (Cyberduck, etc.)
  2) Duo Authentication
  3) Text Editor like Atom, Notepad++
  4) Terminal such as Command Prompt or Windows Powershell (Windows), Terminal (Mac)
  5) Citrix to connect to servers else where(Cisco Anyconnect, etc.)
  6) Excel (GoogleSheets, etc.)

  ### Command Line
  **Windows Users:** Navigate to the start application in the bottom left-hand side of the screen and search for either Command Prompt or Windows Powershell. Click and open this application. 
  
  **Mac Users:** Navigate to the search spotlight in the upper right-hand side of the screen and search for Terminal.app. Click and open this application.




<img src="https://github.com/StephRut/Images-for-Github/blob/main/Command%20Prompt.jpg" width=800>



This is your terminal. It might look a little different to what you're used to applications you're used to, but don't fret! Your terminal is command-line interface (CLI), meaning you type commands (and hit return/enter) to interact with your computer. 
  
## Step 2: Connect to MSI Server and Locate the Data
**For Beginners, use a linux [Cheat Sheet](https://phoenixnap.com/kb/wp-content/uploads/2022/03/linux-commands-cheat-sheet-pnap.pdf)** 
### Connecting to MSI
 _Take note of File Commands and Directory Navigation_

If you are working remotely, i.e, not connected to the eduroam WiFi on campus, you're going to need to use a VPN to acsess the MSI servers. This is where Cisco AnyConnect comes in!

Open up your Cisco AnyConnect application and connect to the U of M Full-Tunnel VPN. Once connected, navigate to your terminal application type:

```
ssh x500@mesabi.msi.umn.edu
```

Don't forget to press enter!

The system will then ask you to enter your password. No text will show up on the screen when entering in your password. Enter your password as normal, and hit enter again. This should take you to a Duo Authentication screen. The first time you connect to MSI, the system will ask if you want to cache the server fingerprint. Type 'yes' and then hit enter.

_See the MSI [Connecting to HPC](https://www.msi.umn.edu/content/connecting-hpc-resources#connecting-to-msi-with-windows-os) page for more information._


<img src="https://github.com/StephRut/Images-for-Github/blob/main/succesful_connection.jpg" width=800 height=300>

### Finding the Sequencing Data

Locate the raw read files (look for files ending in .fastq or .fastq.gz). In this tutorial, the files will be found at the file path: `/home/gomeza/shared/GitHub_Tutorial/16s_data/Gomez_Project_036`

What is a filepath? Good question! This is essentially the directions to the folder or directory that you are currently located in in the file system. Take a look at your current file path. This can be found in brackets [] immediately after x500@ln####. After connecting to MSI your file path should be `[~]`. The **"~"** symbol represents your home or user directory. 


The **"/"** symbols are used for separation between directories. The **'Gomez_Project_036'** directory is within the **'16s_data'** directory which is in the **'GitHub_Tutorial'** directory, and so on. The last listed directory in the file path, **'Gomez_Project_036'** in this case, is the directory that you are currently in, a.k.a. the parent directory. To navigate to the **'Gomez_Project_036'** directory listed in the file path above, type the following command in your terminal and press enter:

```
cd /home/gomeza/shared/GitHub_Tutorial/16s_data/Gomez_Project_036
```
Make sure to have a space between **cd** and the file path. To return to your home directory, you can always type: `cd ~` 

Again ensure you have a space between **cd** and **~**.

Let's copy the **'Gomez_Project_036'** directory (and all of its contents) into a new directory within our home directory. We'll call this directory **'16s_Tutorial'**. In order to copy the **'Gomez_Project_036'** directory we will need to go up one directory. This can be achieved by entering the following command: `cd ..`


To copy the **'Gomez_Project_036'** directory into a new directory within our home directory named **'16s_Tutorial'**, enter the following command:

```shell
cp -r Gomez_Project_036 ~/16s_Tutorial
```

To check that you've successfully made the new directory in your home directory, return to your home directory. Once you're in your home directory, enter: `ls -l`
to list the directories and files within your home directory. Make sure you see the **'16s_Tutorial'** directory! Next, check that the **'16s_Tutorial'** directory contains the raw reads (.fastq.qz) by entering:

```
ls -l 16s_Tutorial
```

Next, create a new directory for analysis of your data within your home directory:
   
```shell
mkdir 16s_Tutorial_Analysis
```
    


## Step 3: Create a Manifest File
_Format Dependent on QIIME Version Used_

A manifest file is a file that maps each raw read file to a sample-id. Essentially, the manifest file associates all raw reads together and gives each read a ID.
Your manifest file will have 3 columns, with the headers: **'sample-id'**, **'absolute-filepath'**, and **'direction'**. Manifest files can be made in whatever program you feel most comfortable with, as long as you can save the file as a .csv. For an more information on creating a manifest file in Excel, click [here](url).
               
  _NOTE: Ensure that your column headers are spelled correctly, otherwise you may run into errors later on!_

The sample-ids can be whatever you want them to be so long as they are unique from each other (except when you have forward and reverse reads, in which case there should be two of each sample-id).
The absolute-filepath is the full path beginning from the root and ending with the directory/file you are specifying. No matter which directory you are in, an absolute filepath will always specify the same directory/file. In MSI, the absolute filepath will begin with ```/home```.

Ex: ```/home/gomeza/shared/GitHub_Tutorial/16s_data/Gomez_Project_036```

To determine what your absolute filepath will be, use the ```pwd``` command. The direction of the read will be either 'forward' or 'reverse'. Again, spelling matters! Check to see if your raw reads have both R1 and R2 in their names. If so, you have both forward and reverse reads or paired-end data.
R1 represents the forward read, and R2 represents the reverse.

How your Manifest file in Excel should look:


<img width="488" alt="Manifest_Example_Screenshot" src="https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/assets/125623174/ef7e8705-0fd5-4eaa-9ce2-a5adde515e63">

Before uploading your final Manifest file to QIIME2, ensure that your file looks like [this](https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/blob/main/Manifest_Example), with no extra spaces or blanks between columns. Extra blanks/spaces will lead to downstream errors. This tutorial will be working with paired-end data.

## Step 4: Import Manifest
Open up Filezilla (or other file sharing software) and connect to the remote server. Locate your Manifest file and transfer it over to the remote server in the directory you made previously for analysis. 

**(Put in how to connect and interact wiht filezilla here as well as a screen shot)**

Load QIIME2 on the remote server.
```shell
module load qiime2/2018.11
```

Next, convert your Manifest file into a QZA (QIIME 2 Artifact) format.
```
qiime tools import \
--type SampleData[PairedEndSequencesWithQuality] \
--input-path Manifest_Example.csv \
--output-path demux.qza \
--input-format PairedEndFastqManifestPhred33
```
Learn more about the ```qiime tools import``` command [here](url).

Visualize a summary of your QZA file of demultiplexed sequences by converting it into a QZV (QIIME 2 Visualization) file. 

```
qiime demux summarize \
--i-data demux.qza \
--o-visualization demux.qzv
```
Learn more about the ```qiime demux summarize``` command [here](url).

Download the output QZV file onto your computer and upload the file into [QIIME 2 View](https://view.qiime2.org/). This visualization is similar to the FastQC reports, except it takes into account all of your raw reads when determining the quality scores. 

<img width="1007" alt="Demux Visualization Screenshot" src="https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/assets/125623174/d5e3d6ba-3cc9-4d8d-a81e-5e465ce7d5b3">

Try to get familiar with the interactive quality plots in this visualization.


## Step 5: Trim Primers
For 16S data, the process of trimming primmers is fairly straight forward. For ITS data, please view the [Trimming ITS Primer](url) file. The forward and reverse primers are the same sequences that were specified for PCR. Oftentimes, the V4 region of 16S rRNA is used, which can be isolated using the 515F - 806R primer pair with forward primer: GTGYCAGCMGCCGCGGTAA and reverse primer: GGACTACNVGGGTWTCTAAT
```
qiime cutadapt trim-paired \
--i-demultiplexed-sequences demux.qza \
--p-front-f GTGYCAGCMGCCGCGGTAA \
--p-front-r GGACTACNVGGGTWTCTAAT \
--p-cores 20 \
--o-trimmed-sequences trimmed-seqs.qza 
```
Learn more about the ```qiime cutadapt trim-paired``` command [here](url). 

Next, create a visualization summary of your trimmed sequences/reads.

```
qiime demux summarize \
--i-data trimmed-seqs.qza \
--o-visualization trimmed-seqs.qzv
```

## Step 6 : Analyze FastQC

Analyzing the quality of your raw data can be crucial during the QIIME2 pipeline as poor quality reads might necessitate tweaking the steps you take in the pipeline. If you have a FastQC report on your samples, please take the time to assess the overall quality of your reads. 

[FastQC documentation](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), [Good FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html), [Bad FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)

Generally, the quality score of a base will decrease as the base position increases. It's also common to see the first 5-10 bases have a lower quality score than the bases following. 

FOR MSI USERS: Within the directory that you found your raw Fastq files in, follow the filepath ```analysis/illumina-basicQC```. Look over the file "report.html" This will take you to an Illumina BasicQC report. You should also view a few of the FastQC reports generated, which can be found by following ```analysis/illumina-basicQC/fastqc``` again, starting from the directory containing your raw Fastqc files.

For more information on FastQC files, view the [FastQC Basics](url) file.



## Step 8: Visualize Trimmed Reads

Upload your trimmed sequences QZV file to QIIME 2 View to look at the quality of your reads. This visualization should look similar to the previous QIIME 2 visualization. The key difference is that your sequences should now be shorter in length. 

<img width="758" alt="before and after trimming length" src="https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/assets/125623174/971e2434-1e2e-4c69-8db5-00b27124f859">

In 16S data, the quality of your reads will determine the lengths where you should trim and truncate the sequences. In ITS data, it is generally recommended to filter your data using different metrics, see Filtering Step. 

## Step 9: Filter the Sequences
If you are filtering ITS data, see [Filtering ITS](url) page.

For 16S rRNA data, especially paired-end reads, it is important to know the length of the sequences you are targeting. According to Katiraei et al. [(2022)](https://doi.org/10.1007/s00284-022-02956-9), the V4 region of 16S rRNA is approximately 250 base pairs in length. The ```qiime dada2 denoise-paired``` command used to truncate and merge the forward and reverse reads requires an overlap of at least 20 base pairs. Therefore, you will want to set truncation lengths such that a 20 base pair overlap is likely to occur. In other words, don't truncate too short if you want to merge the paired-end reads!

If the quality scores are too low on the ends of your reads, the data is less likely to be an accurate representation of what species are present in your sample. If your paired-end reads must be trimmed or truncated short due to low quality reads towards the ends of the sequences, it might be best to analyze the forwards sequences as single-end reads instead. The key here is to find the sweet spot where you keep as much data as you can, without sacrificing the quality of the data.

Ideally, we want to keep the median quality score of the sequences equal to or above 30 or Q30. Meaning when we look at our last QIIME 2 Visualization, we will trim right before the first location in the sequence where the median is < 30. 

To truncate and merge the paired-end reads, use the ```qiime dada2 denoise-paired``` command.

```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs trimmed-seqs.qza \
--p-trunc-len-f 228 \
--p-trunc-len-r 180 \
--o-representative-sequences dada2-paired-end-rep-seqs.qza \
--o-table dada2-paired-end-table.qza \
--o-denoising-stats dada2-paired-end-stats.qza
```
Here we truncated the forward reads at 228 and the reverse at 180.

Learn more about the ```qiime dada2 denoise-paired``` command [here](url).

**What is a Quality Score?**

According to Illumina, a quality score represents the probability that the base "read" in the sequence is erroneous, i.e., not accurately representive of what the actual base of the biological sequence is.[<sup>Illumina</sup>](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/technote_understanding_quality_scores.pdf#:~:text=A%20high%20quality%20score%20implies%20that%20a%20base,call%20in%201%2C000%20is%20predicted%20to%20be%20incorrect.) The higher the quality score, the lower the probability of the base being an error. 

 <img width="471" alt="quality score illumina" src="https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/assets/125623174/3403e7c8-1582-4cd2-88fa-64ab06123954">

**Image Source:** [https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/technote_understanding_quality_scores.](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/technote_understanding_quality_scores.pdf#:~:text=A%20high%20quality%20score%20implies%20that%20a%20base,call%20in%201%2C000%20is%20predicted%20to%20be%20incorrect.)

Keeping median quality scores greater than or equal to 30 means that we are keeping the median at a 99.9% probabilty of it accurately representing the biological sequence. 

(ITS:
8) explain EE
9) explain p-trunc-q)

## Step 10: Visualize the Denoising Stats
A crucial step in this pipeline is visualizing the denoising stats.

```
qiime metadata tabulate \
--m-input-file dada2-paired-end-stats.qza \
--o-visualization dada2-paired-end-stats.qzv
```
Learn more about the ```qiime metadata tabulate``` command [here](url).

View the QZV stats file in QIIME 2 View, download the .tsv, and open it in Excel to calculate the recovery. 
Your .tsv file should look similar to this: 

<img width="325" alt="denoise stats" src="https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/assets/125623174/eb09e44c-f702-41a5-88ca-d707652512b2">

To calculate recovery, find the sum of each numerical column. Then determine the percentage of reads that made it through each stage by taking the sum of the individual columns C-F over the sum of the input column.  Ideally, you want at least a 70% recovery, or 70% of the reads are non-chimeric. 

For further reference, view the [Denoise Stats Example](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/Denoise%20Example%20Data.csv).

If not enough reads are passing the filtering step, consider reducing the trunc length or other filtering options such as [maxEE](url) or [truncQ](url). 
If not enough reads are passing the merging step, your reads may not be long enough to have a 20 base pair overlap. Consider analyzing single-end reads instead. If a large percentage of reads do not make it past the non-chimeric reads, this may be a sign that the primers have not been fully removed from your reads. 

## Step 11: Training the Classifier
 1) Download the Classifier from either green genes or silva 16s or Unite ITS
 3) make characters uppercase to avoid downstream errors
 4) 99 sequences .fasta to .qza
 5) 99 taxonomy .txt to .qza
 6) train classifier code
 7) Learn the classifier (i believe 16s you trim to V4 region) but in ITS its best not to do trimming
## Step 12: Building ASV Table with Taxonomy
 1) create taxa feature table (ASV)
 2) convert taxa table into .txt
 3) open in excel
 4) analysis



