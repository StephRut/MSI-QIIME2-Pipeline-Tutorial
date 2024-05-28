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

What is a filepath? Good question! This is essentially the directions to the folder or directory that you are currently located in in the file system. Take a look at your current file path. This can be found in brackets [ ] immediately after x500@ln####. After connecting to MSI your file path should be `[~]`. The **"~"** symbol represents your home or user directory. 


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

<img src="https://github.com/StephRut/Images-for-Github/blob/main/16s%20home%20directory.jpg" width=600 height=400>

Your terminal should look a little something like this ⬆

To list only the file names and directory names use:

```
ls 16s_Tutorial -1 
```
<img src="https://github.com/StephRut/Images-for-Github/blob/f9366f0166d03f16fbbf818162661ab1e0b6938f/ls%20-1.png" width=300 height=400>

Copy all of the reads listed. This will be used later on!

Next, create a new directory for analysis of your data within your home directory:
   
```shell
mkdir 16s_Tutorial_Analysis
```
    


## Step 3: Create a Manifest File
_Format Dependent on QIIME Version Used_ 

A manifest file is a file that maps each raw read file to a sample-id. Essentially, the manifest file associates all raw reads together and gives each read a ID.
Your manifest file will have 3 columns, with the headers: **'sample-id'**, **'absolute-filepath'**, and **'direction'**. Manifest files can be made in whatever program you feel most comfortable with, as long as you can save the file as a .csv. This tutorial will be using Excel to make the manifest file. 
               
 ⚠️ _NOTE: Ensure that your column headers are spelled correctly, otherwise you may run into errors later on!_ 

 Your file should look like this: 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/excel_1.png" width=250 height=125>

Next, we want to get the list of reads from MSI using the terminal. In step 2, you should have listed out and copied the names of the reads in the **'16s_Tutorial'** directory in your terminal. In excel, create a new sheet 'Editing Sheet' and paste these file into the first column. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/excel%20file%20list.png" width=210 height=380>

The sample-ids can be whatever you want them to be so long as they are unique from each other (except when you have forward and reverse reads, in which case there should be two of each sample-id). In this tutorial, our samples will have both forward and reverse reads, also called paired end reads.

To isolate our sample-ids we will manipulate the text using Excel functions. In column B, type ```=TEXTBEFORE(A1,"_S") ```, which will ouput all text in cell A1 that precedes "_S". Your output should be "316D14". Click on the lower right-hand side of the box and drag down the formula to the bottom row. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/generate%20sample%20ids.png" width=240 height=380>

Copy and paste the sample-ids into the sample-ids column of your manifest file. 





The absolute-filepath is the full path beginning from the root and ending with the directory/file you are specifying. No matter which directory you are in, an absolute filepath will always specify the same directory/file. In MSI, the absolute filepath will begin with ```/home```.

Ex: ```/home/gomeza/rutsc011/16s_Tutorial```

To determine what your absolute filepath will be, first navigate to the **'16s_Tutorial'** directory in the terminal. Then use the ```pwd``` command. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/pwd.png" width=520, height=37>

Copy and paste the filepath into C1 of the Editing sheet in Excel and drag down to the last row.

<img src="https://github.com/StephRut/Images-for-Github/blob/main/file%20path%20beginning.png" width=430 height=380>

This is the first part of our absolute filepath. Now we just need to add the text from column C and column A together, separated by a '/'. In column D type ```=TEXTJOIN("/",TRUE,C1,A1)```, which joins achieves this merge. Again, pull down the formula to the bottom row. This is our absolute-filepath. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/absolute-filepath.png" width=800 height=380>

Copy and paste column D into the absolute-file column of the manifest.



Lastly, the direction of the read will be either 'forward' or 'reverse'. Again, spelling matters! Check to see if your raw reads have both R1 and R2 in their names. If so, you have both forward and reverse reads or paired-end data. R1 represents the forward read, and R2 represents the reverse.

To assign forward vs. reverse we will isolate the text to the right of 'L001_' in our read files. In column E of the Editing sheet type ```=TEXTAFTER(A1,"L001_")``` and press enter. E1 should now begin with 'R1'. Pull down this formula to the bottom row. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Read%20direction%201.png" width=880 height=380>

Then, in F1 we will type ```=IF(E1 = "R1_001.fastq.gz", "forward", "reverse")``` and pull down. This function is stating that if cell E1 is 'R1_001.fastq.gz', then it will populate the cell with 'forward'. If E1 does not match 'R1_001.fastq.gz', then it will populate with 'reverse'. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/direction%20part%202.png" width=935 height=380>

Copy the F column and paste it into the direction column of the manifest.


How your Manifest file in Excel should look:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Manifest%20Example%20Trim%202.1.png" width=488>


Before uploading your final Manifest file to QIIME2, ensure that your file looks like [this](https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/blob/main/Manifest_Example2), with no extra spaces or blanks between columns. Extra blanks/spaces will lead to downstream errors. 

Hint: Make sure to check that the cells in Excel are populating correctly to avoid errors later on. 

## Step 4: Import Manifest
Open up Filezilla (or other file sharing software) and connect to the remote server. To do this, click 'file' in the upper lefthand corner of the screen, and 'site manager' in the pull down list. Your screen should look like this:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Filezilla%20Site%20Manager.png" width=900 height=450>

Verify that the protocol is SFTP, the host is mesabi.msi.umn.edu, the port is 22, the logon type is interactive and that your user is correct (x500). Press connect and enter your password used logging in to MSI. The local site (your computer) will be on the left and the remote site (MSI) will be on the right. Locate and click on the **'16s_Tutorial_Analysis'** directory on MSI and the Manifest file on your computer. Drag your manifest file over to the empty directory box on the bottom right of the screen.

Ensure that your Manifest file has been transfered to MSI by performing and ```ls``` command on the **'16s_Tutorial_Analysis'** directory.


## Step 5: Trim Primers and Visualize Sequences

Load QIIME2 in the directory you are working in.
```shell
module load qiime2/2018.11
```

Next, within the **'16s_Tutorial_Analysis'** directory, convert your Manifest file into QZA (QIIME 2 Artifact) format. The input file ' Manifest.csv' is the manifest file that we just moved into MSI and the output 'demux.qza' will be a file  of demultiplexed sequences. 
```
qiime tools import \
--type SampleData[PairedEndSequencesWithQuality] \
--input-path Manifest.csv \
--output-path demux.qza \
--input-format PairedEndFastqManifestPhred33
```

Now we want to trim the PCR Primers.



For 16S data, the process of trimming primmers is fairly straight forward. For ITS data, please view the [Trimming ITS Primer](url) file. The forward and reverse primers are the sequences that were specified for the PCR. Oftentimes, the V4 region of 16S rRNA is used, which can be isolated using the 515F - 806R primer pair with forward primer: _GTGYCAGCMGCCGCGGTAA_ and reverse primer: _GGACTACNVGGGTWTCTAAT_

Using the output file 'demux.qza' from the previous command as our input we can trim the primers. The parameter '--p-front-f' specifies the forward primer (primer used on forward reads) and '--p-front-r' specifies the reverse primer (primer used on reverse reads). Any primers can be used in the space following these parameters.

To trim these primers we type:
```
qiime cutadapt trim-paired \
--i-demultiplexed-sequences demux.qza \
--p-front-f GTGYCAGCMGCCGCGGTAA \
--p-front-r GGACTACNVGGGTWTCTAAT \
--o-trimmed-sequences trimmed-seqs.qza 
```
The output file will be 'trimmed-seqs.qza' which will contain the primer-less version of your sequences.

Next, create a visualization summary of your trimmed sequences. Again, taking the output file from our previous command, we type:


```
qiime demux summarize \
--i-data trimmed-seqs.qza \
--o-visualization trimmed-seqs.qzv
```
We will take the output file 'trimmed-seq.qzv' and transfer it over to our local computer using Filezilla, in the same way as earlier but reverse. Drag the 'trimmed-seq/qzv' file over from the **'16s_Tutorial_Analysis'** directory in MSI to a folder on your local computer. To view this file, upload the it into [QIIME 2 View](https://view.qiime2.org/). To view the quality plots, click on the tab in the top left corner of the screen named _Interactive Quality Plot_. Your screen should look a little something like this:


<img src="https://github.com/StephRut/Images-for-Github/blob/main/Trimmed%20Seq%20Vis.png">

You can zoom in on the plots by clicking and dragging a rectangle over the area you want to see closer. Try to get familiar with the interactive quality plots in this visualization.
Our data was trimmed to 301 nucleotides when we received it. Therefore, if the primers were trimmed, the sequences lengths' should be ~280 nucleotides long. Verify this by checking the sequence length summary at the bottom of the interactive quality plot page.

For 16S data, the quality of your reads will determine the lengths where you should trim (removing bases from the start) and truncate (removing bases from the end) the sequences. For ITS data, it is generally recommended to filter your data using different metrics, see Filtering step.

## Step 6 : Analyze FastQC

Analyzing the quality of your raw data can be crucial during the QIIME2 pipeline as poor quality reads might necessitate tweaking the steps you take in the pipeline. If you have a FastQC report on your samples, please take the time to assess the overall quality of your reads. 

[FastQC documentation](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), [Good FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html), [Bad FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)

Generally, the quality score of a base will decrease as the base position increases. It's also common to see the first 5-10 bases have a lower quality score than the bases following. 

FOR MSI USERS: Within the directory that you found your raw Fastq files in, follow the filepath ```analysis/illumina-basicQC```. Look over the file "report.html" This will take you to an Illumina BasicQC report. You should also view a few of the FastQC reports generated, which can be found by following ```analysis/illumina-basicQC/fastqc``` again, starting from the directory containing your raw Fastqc files.

For more information on FastQC files, view the [FastQC Basics](url) file.


## Step 7: Filter, Denoise and Visualize the Sequences
### Filtering and Denoising the Sequences
If you are filtering ITS data, see [Filtering ITS](url) page.

For 16S rRNA data, especially paired-end reads, it is important to know the length of the sequences you are targeting. According to Katiraei et al. [(2022)](https://doi.org/10.1007/s00284-022-02956-9), the V4 region of 16S rRNA is approximately 250 base pairs in length. The ```qiime dada2 denoise-paired``` command used to truncate and merge the forward and reverse reads requires an overlap of at least 12 base pairs. Therefore, you will want to set truncation lengths such that a 12 base pair overlap is likely to occur. In other words, don't truncate too short if you want to merge the paired-end reads!

If the quality scores are too low on the ends of your reads, the data is less likely to be an accurate representation of what species are present in your sample. If your paired-end reads must be trimmed or truncated short due to low quality reads towards the ends of the sequences, it might be best to analyze the forwards sequences as single-end reads instead. The key here is to find the sweet spot where you keep as much data as you can, without sacrificing the quality of the data.

Ideally, we want to keep the median quality score of the sequences equal to or above 30 or Q30. Meaning when we look at our last QIIME 2 Visualization, we will trim right before the first location in the sequence where the median is < 30. 


To truncate and merge the paired-end reads, use the ```qiime dada2 denoise-paired``` command. Our input is 'trimmed-seq.qza' and we will have 3 outputs. After reviewing the interactive quality plots, the forward reads are $\geq$ Q30 until base number 231 and the reverse reads are $\geq$ Q30 until 185, so we will trim at these locations.

Note: You may now have to load in Qiime2/2023.2 using the ```module load``` command.

```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs trimmed-seqs.qza \
--p-trunc-len-f 231 \
--p-trunc-len-r 185 \
--o-representative-sequences dada2-paired-end-rep-seqs.qza \
--o-table dada2-paired-end-table.qza \
--o-denoising-stats dada2-paired-end-stats.qza
```



#### **What is a Quality Score?**

According to Illumina, a quality score represents the probability that the base "read" in the sequence is erroneous, i.e., not accurately representive of what the actual base of the biological sequence is.[<sup>Illumina</sup>](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/technote_understanding_quality_scores.pdf#:~:text=A%20high%20quality%20score%20implies%20that%20a%20base,call%20in%201%2C000%20is%20predicted%20to%20be%20incorrect.) The higher the quality score, the lower the probability of the base being an error. 

 <img width="471" alt="quality score illumina" src="https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/assets/125623174/3403e7c8-1582-4cd2-88fa-64ab06123954">

Image Source:[Illumina](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/technote_understanding_quality_scores.pdf#:~:text=A%20high%20quality%20score%20implies%20that%20a%20base,call%20in%201%2C000%20is%20predicted%20to%20be%20incorrect.)

Keeping median quality scores $\geq$ 30 means that we are keeping the median at a 99.9% probabilty of it accurately representing the biological sequence. 


### Visualizing the Stats
A crucial step in this pipeline is visualizing the denoising stats. We can achieve this by typing:

```
qiime metadata tabulate \
--m-input-file dada2-paired-end-stats.qza \
--o-visualization dada2-paired-end-stats.qzv
```

View the QZV stats file in QIIME 2 View download the .tsv, and open it in Excel to calculate the recovery. 


To calculate recovery, find the sum of the input, filtered, denoised, merged and non-chimeric columns. Then determine the percentage of reads that made it through each stage by taking the sum of the individual columns (e.g., filtered, denoised, merged or non-chimeric) over the sum of the input column. One can also average the percentage columns to get an estimate of the recovery for each stage (whichever method you prefer). Ideally, you want at least a 70% recovery, or 70% of the total input reads passed through the non-chimeric stage. 
Your .tsv file should look similar to this: 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Denoise%20stats_1.png">

For further reference, view the [Denoise Stats Example](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/Denoise%20Example%20Data.csv).

If not enough reads are passing the filtering step, consider reducing the trunc length or other filtering options such as [maxEE](url) or [truncQ](url). 
If not enough reads are passing the merging step, your reads may not be long enough to have a 12 base pair overlap. Consider analyzing single-end reads instead. 
If a large percentage of reads do not make it past the non-chimeric reads, this may be a sign that the primers have not been fully removed from your reads or that the truncation lengths are too long. 

In our case, only ~ 53.8% of the input reads were non-chimeric. Looking at each stage of the filtering and denoising process, a majority of the reads were lost due to the intial filter with only approximately 58.9% passing through. Therefore, we will reduce the trunc length for both the forward and reverse reads to increase our recovery. After several filtering iterations, a forward trunc length of 165 and a reverse trunc length of 104 produced the best recovery results. With these parameters, ~75.1% passed the initial input filter, ~71.5% merged, while ~66.7% of the reads were non-chimeric. At this point, if you would like a greater recovery, I would recommend analyzing single-end reads. [HOW TO DO SINGLE-END ANALYSIS**]

## Step 9: Training the Classifier
Now we will download the Greengenes2 database. Green genes 2 is a relatively new bacterial database, but has been found to increase reproducibility between studies. [Greengenes2 unifies microbial data in a single reference tree](https://www.nature.com/articles/s41587-023-01845-1)
First we must install the greengenes2 Qiime2 plugin.

First , we will download greengenes 13_5 database using:

``` wget https://gg-sg-web.s3-us-west-2.amazonaws.com/downloads/greengenes_database/gg_13_5/gg_13_5_otus.tar.gz```

Then to untar the tar file we use:
```tar -xf gg_13_5_otus.tar.gz```

This has already been done for you within the /home/gomeza/shared/GitHub_Tutorial directory. The untared file is now the 'gg_13_5_otus' directory. 

```qiime tools import --type 'FeatureData[Sequence]' --input-path ~/../shared/GitHub_Tutorial/gg_13_5_otus/rep_set/99_otus.fasta --output-path ~/Gomez_Project_036/99_otus.qza```

qiime tools import --type 'FeatureData[Taxonomy]' --input-format HeaderlessTSVTaxonomyFormat --input-path ~/Gomez_Project_036/gg_13_5/gg_13_5_otus/taxonomy/99_otu_taxonomy.txt --output-path ~/Gomez_Project_036/ref-taxonomy.qza

qiime feature-classifier extract-reads --i-sequences ~/Gomez_Project_036/99_otus.qza --p-f-primer GTGYCAGCMGCCGCGGTAA --p-r-primer GGACTACNVGGGTWTCTAAT --o-reads ~/Gomez_Project_036/ref-seqs.qza

qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads ~/Gomez_Project_036/ref-seqs.qza --i-reference-taxonomy ~/Gomez_Project_036/ref-taxonomy.qza --o-classifier ~/Gomez_Project_036/classifier.qza

qiime feature-classifier classify-sklearn --i-classifier ~/Gomez_Project_036/classifier.qza --i-reads ~/Gomez_Project_036/rep-seqs.qza --o-classification ~/Gomez_Project_036/taxonomy.qza



 1) Download the Classifier from either green genes or silva 16s or Unite ITS
 3) make characters uppercase to avoid downstream errors
 4) 99 sequences .fasta to .qza
 5) 99 taxonomy .txt to .qza
 6) train classifier code
 7) Learn the classifier (i believe 16s you trim to V4 region) but in ITS its best not to do trimming
## Step 10: Building ASV Table with Taxonomy
 1) create taxa feature table (ASV)
 2) convert taxa table into .txt
 3) open in excel
 4) analysis



