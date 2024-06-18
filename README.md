# Minnesota Supercomputing Institute (MSI) QIIME2 Pipeline Tutorial
This tutorial will be based on QIIME2 Version [2023.2](https://docs.qiime2.org/2023.2/index.html). Commands and formats of files may change depending on the version of QIIME2 you are using. 

## Step 1: Getting Started
 ### Software Requirements 
  1) File transfer Software like [FileZilla](https://filezilla-project.org/) ([Cyberduck](https://cyberduck.io/download/), etc.)
  2) Duo Authentication
  3) Text Editor like [Atom](https://atom-editor.cc/), Notepad++
  4) Terminal such as Command Prompt or Windows Powershell (Windows), Terminal (Mac)
  5) [Cisco](https://software.cisco.com/download/home/286330811/type/282364313/release/5.1.3.62) to connect to servers elsewhere
  6) Excel (GoogleSheets, etc.)

  ### Command Line
  **Windows Users:** Navigate to the start application in the bottom left-hand side of the screen and search for either Command Prompt or Windows Powershell. Click and open this application. 
  
  **Mac Users:** Navigate to the search spotlight in the upper right-hand side of the screen and search for Terminal.app. Click and open this application.

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Command%20Prompt.jpg" width=800>

This is your terminal window ⬆️.  It might look a little different to what you're used to or applications you're used to, but don't fret! Your terminal is command-line interface (CLI), meaning you type commands (and hit return/enter) to interact with your computer. 
  
## Step 2: Connect to MSI Server and Locate the Data
**For Beginners, use a linux [Cheat Sheet](https://phoenixnap.com/kb/wp-content/uploads/2022/03/linux-commands-cheat-sheet-pnap.pdf)** 

 _Take note of File Commands and Directory Navigation within the CheatSheet_

### Connecting to MSI


If you are working remotely, i.e, not connected to the eduroam WiFi on campus, you're going to need to use a VPN to acsess the MSI servers. This is where Cisco AnyConnect comes in!

Open up your Cisco AnyConnect application and connect to the U of M Full-Tunnel VPN. Once connected, navigate to your terminal application and type:

```
ssh x500@agate.msi.umn.edu
```

Don't forget to press enter!

The system will then ask you to enter your password. No text will show up on the screen when entering in your password. Enter your password as usual, and hit enter again. This should take you to a Duo Authentication screen. The first time you connect to MSI, the system will ask if you want to cache the server fingerprint. Type 'yes' and then hit enter.

You should see a screen like this ⬇️ if you connected successfully.


<img src="https://github.com/StephRut/Images-for-Github/blob/main/succesful_connection.jpg" width=800 height=300>

_See the MSI [Connecting to HPC](https://www.msi.umn.edu/content/connecting-hpc-resources#connecting-to-msi-with-windows-os) page for more information._

### Finding the Sequencing Data

Locate the raw read files (look for files ending in .fastq or .fastq.gz). In this tutorial, the files will be found at the filepath: /home/gomeza/shared/GitHub_Tutorial/16s_data/Gomez_Project_036

What is a filepath? Good question! This is essentially the directions to the folder or directory that you are currently located in within the file system. Take a look at your current filepath. This can be found in brackets [ ] immediately after x500@####. After connecting to MSI your filepath should be '~'. The tilde symbol represents your home or user directory. 


<img src="https://github.com/StephRut/Images-for-Github/blob/main/homedir2.png" width= 250 height=35>

The '/' symbols are used for separation between directories. The **'Gomez_Project_036'** directory is within the **'16s_data'** directory which is in the **'GitHub_Tutorial'** directory, and so on. The last listed directory in the filepath, **'Gomez_Project_036'** in this case, is the directory that you are currently in, a.k.a. the parent directory. To navigate to the **'Gomez_Project_036'** directory listed in the filepath above, type the following command in your terminal and press enter:

```
cd /home/gomeza/shared/GitHub_Tutorial/16s_data/Gomez_Project_036
```
Make sure to have a space between 'cd' and the filepath. To return to your home directory, you can always type: `cd ~` 

_Remember you can always check the [cheatsheet](https://phoenixnap.com/kb/wp-content/uploads/2022/03/linux-commands-cheat-sheet-pnap.pdf) if you need more help!_

Let's copy the **'Gomez_Project_036'** directory (and all of its contents) into a new directory within your home directory. We'll call this directory **'16s_Tutorial'**. In order to copy the **'Gomez_Project_036'** directory we will need to go up one directory. This can be achieved by entering the following command: `cd ..`


To copy the **'Gomez_Project_036'** directory into a new directory within your home directory named **'16s_Tutorial'**, enter the following command:

```
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

Copy all of the reads listed. This will be used later on! Next, create a new directory for analysis of your data within your home directory:
   
```
mkdir 16s_Tutorial_Analysis
```
    

## Step 3: Create a Manifest File
_Format Dependent on QIIME Version Used_ 

A manifest file is a file that maps each raw read file to a sample-id. Essentially, the manifest file associates all raw reads together and gives each read a ID.
Your manifest file will have 3 columns, with the headers: **'sample-id'**, **'absolute-filepath'**, and **'direction'**. Manifest files can be made in whatever program you feel most comfortable with, as long as you can save the file as a .csv. This tutorial will be using Excel to make the manifest file. 
               
 ⚠️ _NOTE: Ensure that your column headers are spelled correctly, otherwise you may run into errors later on!_ 

 Your file should look like this: 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/excel_1.png" width=250 height=125>

Next, we want to get the list of reads from MSI using the terminal. In step 2, you should have listed out and copied the names of the reads in the **'16s_Tutorial'** directory in your terminal. In Excel, create a new sheet 'Editing Sheet' and paste these files into the first column. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/excel%20file%20list.png" width=210 height=380>

The sample-ids can be whatever you want them to be so long as they are unique from each other (except when you have forward and reverse reads, in which case there should be two of each sample-id: R1, R2). In this tutorial, our samples will have both forward (R1) and reverse (R2) reads, also called paired end reads. To isolate our sample-ids we will manipulate the text using Excel functions. In column B, type ```=TEXTBEFORE(A1,"_S") ```, which will ouput all text in cell A1 that precedes "_S". Your output should be '316D14'. Click on the lower right-hand side of the box and drag down the formula to the bottom row. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/generate%20sample%20ids.png" width=240 height=380>

Copy and paste the sample-ids into the **'sample-ids'** column of your manifest file. 

The absolute filepath is the full path beginning from the root and ending with the directory/file you are specifying. No matter which directory you are in, an absolute filepath will always specify the same directory/file. In MSI, the absolute filepath will begin with '/home'.

Ex: /home/gomeza/rutsc011/16s_Tutorial

To determine what your absolute filepath will be, first navigate to the **'16s_Tutorial'** directory in the terminal. Then use the ```pwd``` command. 

 <img src="https://github.com/StephRut/Images-for-Github/blob/main/pwd.png" width=520, height=37>

Copy and paste the filepath into C1 of the 'Editing sheet' in Excel and drag down to the last row.

<img src="https://github.com/StephRut/Images-for-Github/blob/main/file%20path%20beginning.png" width=430 height=380>

This is the first part of our absolute filepath. Now we just need to add the text from column C and column A together, separated by a '/'. In column D type ```=TEXTJOIN("/",TRUE,C1,A1)```, which achieves this merge. Again, pull down the formula to the bottom row. This is our absolute filepath. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/absolute-filepath.png" width=800 height=380>

Copy and paste column D into the **'absolute-filepath'** column of the manifest.

Lastly, the direction of the read will be either 'forward' or 'reverse'. Again, spelling matters! Check to see if your raw reads have both R1 and R2 in their names. If so, you have both forward and reverse reads or paired-end data. R1 represents the forward read, and R2 represents the reverse. To assign forward vs. reverse we will isolate the text to the right of 'L001_' in our read files. In column E of the 'Editing sheet' type ```=TEXTAFTER(A1,"L001_")``` and press enter. E1 should now begin with 'R1'. Pull down this formula to the bottom row. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Read%20direction%201.png" width=880 height=380>

Then, in F1 we will type ```=IF(E1 = "R1_001.fastq.gz", "forward", "reverse")``` and pull down. This function is stating that if cell E1 is 'R1_001.fastq.gz', then it will populate the cell with 'forward'. If E1 does not match 'R1_001.fastq.gz', then it will populate with 'reverse'. 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/direction%20part%202.png" width=935 height=380>

Copy column F and paste it into the **'direction'** column of the manifest.

How your Manifest file in Excel should look:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Manifest%20Example%20Trim%202.1.png" width=488>


Before uploading your final Manifest file to QIIME2, ensure that your file looks like [this](https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/blob/main/Manifest_Example2), with no extra spaces or blanks between columns in a text editor app (atom, notepad++, etc.). Extra blanks/spaces will lead to downstream errors. 

⚠️ _NOTE: Make sure to check that the cells in Excel are populating correctly to avoid errors later on._

## Step 4: Import Manifest
Open up Filezilla (or other file sharing software) and connect to the remote server. To do this, click 'file' in the upper lefthand corner of the screen, and 'site manager' in the pull down list. Your screen should look like this:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Filezilla%20Site%20Manager2.png" width=850 height=450>

Verify that the protocol is SFTP, the host is agate.msi.umn.edu, the port is 22, the logon type is interactive and that your user is correct (x500). Press connect and enter your password used logging in to MSI. The local site (your computer) will be on the left and the remote site (MSI) will be on the right. Locate and click on the **'16s_Tutorial_Analysis'** directory on MSI and the Manifest file on your computer. Drag your manifest file over to the empty directory box on the bottom right of the screen.

Ensure that your Manifest file has been transfered to MSI by performing and ```ls``` command on the **'16s_Tutorial_Analysis'** directory.

## Step 5: Trim Primers and Visualize Sequences

Load QIIME2 into the directory you are working in.
```
module load qiime2/2023.2
```

Several steps within this pipeline may take hours to run. Unfortunatly, you can only run commands from the command line interactively (everything you've done up until this point) if they take less than 15 minutes. If the command takes over 15 minutes, it will fail partway through. We can both get around this 15 minute wall-time and decrease the amount of time it takes to run a job by using slurm scripts!

### Slurm Job Scripts
Submitting a Slurm job script to the slurm scheduler allows you to run commands in the background instead of running them interactively. These scripts are shell scripts and can be made using a text editor app like Atom. You can also use vim if you are familiar with it. 

#### Writting the Script
Let's create a shell script for the next command in the pipeline. Within the **'16s_Tutorial_Analysis'** directory, convert your Manifest file into QZA (QIIME 2 Artifact) format. The input file **'Manifest.csv'** is the manifest file that we just moved into MSI and the output **'demux.qza'** will be a file of demultiplexed sequences. 

```
#!/bin/bash -l
#SBATCH --time=1:00:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --mem=30g
#SBATCH --tmp=30g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=rutsc011@umn.edu
cd ~/16s_Tutorial_Analysis
module load qiime2/2023.2
qiime tools import --type SampleData[PairedEndSequencesWithQuality] --input-path Manifest.csv --output-path demux.qza --input-format PairedEndFastqManifestPhred33
```

The first line is needed for every shell script you create. It specifies which shell you will run the script on, we will always use bash.

The following #SBATCH lines are for requesting resources and are known as sbatch commands. The #SBATCH --time command reqeusts the amount of time you need for your job to run (here it requests 1 hour). You can always add/subtract time from this parameter depending on how long you expect a job to run for. 

The #SBATCH --ntasks command requests 1 processor core. Generally, all commands within this pipeline will only use 1 processor core, therefore this line does not need to be changed between jobs.

The #SBATCH --cpus-per-task command requests 8 CPUs to be used for job threading, i.e., 8 CPU's instead of 1 CPU will be used to run the command. This should decrease the total amount of time it takes for a command to run. You can add/subtract then number of CPUs request for each job. 

The #SBATCH --mem command requests 30 gigabytes(gb) of memory, and the #SBATCH --tmp command requests 30 gb of temporary space. You can again add/subtract gigabytes depending on how many CPUs you request. 

The #SBATCH --mail-type command will allow you to receive an email when the job begins, completes and/or fails. 

The #SBATCH --mail-user command specifies to whom these emails are sent. The cd line indicates from which directory you want to run the command. You will also need to load the proper module (qiime2/2023.2) for the command before listing the command.

The last line in the script is the command you want to run: ```qiime tools import```. Save the file to your local computer as **'manifest.sh'**


#### Submitting the Script

To submit the script, first transfer **'manifest.sh'** from your local computer to MSI using Filezilla. 

**Windows Users Only:**
```
dos2unix manifest.sh
```

Then, submit the job to the Slurm Scheduler using:
```
sbatch manifest.sh
```
To check on the status of your job, you can use:

```
squeue -u x500
```

More information for the MSI Slurm Scheduler can be found [here](https://msi.umn.edu/our-resources/knowledge-base/slurm-job-submission-and-scheduling). The partitions you can run the jobs on can be found [here](https://msi.umn.edu/our-resources/slurm-scheduler/slurm-partitions).

### Trimming the Primers
Now we want to trim the PCR primers. For 16S rRNA data, the process of trimming primmers is fairly straight forward. The forward and reverse primers are the sequences that were specified for the PCR. Oftentimes, the V4 region of 16S rRNA is used, which can be isolated using the 515F - 806R primer pair with forward primer: _GTGYCAGCMGCCGCGGTAA_ and reverse primer: _GGACTACNVGGGTWTCTAAT_

Using the output file **'demux.qza'** from the previous Slurm job as our input, we can trim the primers. The parameter ```--p-front-f``` specifies the forward primer (primer used on forward reads) and ```--p-front-r``` specifies the reverse primer (primer used on reverse reads). Any primers can be used in the space following these parameters.

To trim these primers we type:
```
qiime cutadapt trim-paired \
--i-demultiplexed-sequences demux.qza \
--p-front-f GTGYCAGCMGCCGCGGTAA \
--p-front-r GGACTACNVGGGTWTCTAAT \
--o-trimmed-sequences trimmed-seqs.qza 
```
The output file will be **'trimmed-seqs.qza'**, which will contain the primer-less version of your sequences. 
### Visualizing the Trimmed Sequences
Next, create a visualization summary of your trimmed sequences. Again, taking the output file from our previous command, we type:

```
qiime demux summarize \
--i-data trimmed-seqs.qza \
--o-visualization trimmed-seqs.qzv
```
We will take the output file **'trimmed-seq.qzv'** and transfer it over to our local computer using Filezilla, in the same way as earlier but in reverse. Drag the **'trimmed-seq.qzv'** file over from the **'16s_Tutorial_Analysis'** directory in MSI to a folder on your local computer. To view this file, upload it into [QIIME 2 View](https://view.qiime2.org/). To view the quality plots, click on the tab in the top left corner of the screen named _Interactive Quality Plot_. Your screen should look a little something like this:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Trimmed%20Seq%20Vis.png">

These graphs are actually box and whisker plots. You can zoom in on the plots by clicking and dragging a rectangle over the area you want to see closer. Below is a zoomed in version of the box and whisker plot.

<img src="https://github.com/StephRut/Images-for-Github/blob/main/box%20and%20whisker.png" width=500 height=300> 


The data was trimmed to 301 nucleotides when we received it. Therefore, if the primers were trimmed, the lenghts of the sequences should be ~ 280 nucleotides long. Verify this by checking the sequence length summary at the bottom of the interactive quality plot page.

For 16S rRNA data, the quality of your reads will determine the lengths where you should trim (removing bases from the start) and truncate (removing bases from the end) the sequences. For ITS data, it is generally recommended to filter your data using different metrics, see [ITS Analysis](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/ITS%20QIIME2%20Analysis.md).

## Step 6 : Analyze FastQC

Analyzing the quality of your raw data is crucial during the QIIME2 pipeline as poor quality reads might necessitate tweaking the steps you take in the pipeline. If you have a FastQC report on your samples, please take the time to assess the overall quality of your reads. 

[FastQC documentation](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), [Good FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html), [Bad FastQC data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)

Generally, the quality score of a base will decrease as the base position increases. It's also common to see the first 5-10 bases have a lower quality score than the bases following. 

Within the  **'16s_Tutorial_Analysis'** directory, follow the filepath ```analysis/illumina-basicQC```. Look over the file **'report.html'**. This will take you to an Illumina BasicQC report. You should also view a few of the FastQC reports generated, which can be found by following ```analysis/illumina-basicQC/fastqc``` again, starting from the  **'16s_Tutorial_Analysis'** directory.

## Step 7: Filter, Denoise, and Visualize the Sequences
### Filtering and Denoising the Sequences

For 16S rRNA data, especially paired-end reads, it is important to know the length of the sequences you are targeting. According to [Katiraei et al.](https://pubmed.ncbi.nlm.nih.gov/35907023/), the V4 region of 16S rRNA is approximately 250 base pairs in length.<sup>1</sup> The ```qiime dada2 denoise-paired``` command used to truncate and merge the forward and reverse reads requires an overlap of at least 12 base pairs. Therefore, you will want to set truncation lengths such that a 12 base pair overlap is likely to occur. In other words, don't truncate too short if you want to merge the paired-end reads!

If the quality scores are too low on the beginning or end of your reads, the data is less likely to be an accurate representation of what species are present in your sample. If your paired-end reads must be trimmed or truncated short due to low quality reads, it might be best to analyze the forwards sequences as single-end reads instead. The key here is to find the sweet spot where you keep as much data as you can, without sacrificing the quality of the data.

Ideally, we want to keep the median quality score of the sequences equal to or above 30 or Q30. Meaning when we look at our last QIIME 2 Visualization, we will trim right before the first location in the sequence where the median is < 30. 

After reviewing the interactive quality plots, the forward reads are $\geq$ Q30 until base number 231 and the reverse reads are $\geq$ Q30 until 185, so we will truncate at these locations.

<img src="https://github.com/StephRut/Images-for-Github/blob/main/vismarked.png">


To truncate and merge the paired-end reads, use the ```qiime dada2 denoise-paired``` command. Our input is **'trimmed-seq.qza'** and we will have 3 outputs. 


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

 <img width="471" alt="quality score illumina" src="https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/assets/125623174/3403e7c8-1582-4cd2-88fa-64ab06123954">

<sup>Image Source: Illumina</sup>

According to Illumina, a quality score represents the probability that the base "read" in the sequence is erroneous, i.e., not accurately representive of what the actual base of the biological sequence is.<sup>2</sup> The higher the quality score, the lower the probability of the base being an error. Keeping median quality scores $\geq$ 30 means that we are keeping the median at a 99.9% probabilty of it accurately representing the biological sequence. 


### Visualizing the Stats
A crucial step in this pipeline is visualizing the denoising stats. We can achieve this by typing:

```
qiime metadata tabulate \
--m-input-file dada2-paired-end-stats.qza \
--o-visualization dada2-paired-end-stats.qzv
```

To view the QZV stats file in QIIME 2 View, download the tsv and open it in Excel to calculate the recovery. 


To calculate recovery, find the sum of the input, filtered, denoised, merged and non-chimeric columns. Then determine the percentage of reads that made it through each stage by taking the sum of the individual columns (e.g., filtered, denoised, merged or non-chimeric) over the sum of the input column. One can also average the percentage columns to get an estimate of the recovery for each stage (whichever method you prefer). Ideally, you want at least a 70% recovery, or 70% of the total input reads passing through the non-chimeric stage. 
Your tsv file should look similar to this: 

<img src="https://github.com/StephRut/Images-for-Github/blob/main/Denoise%20stats_1.png">



If not enough reads are passing the filtering step, consider reducing the truncation length or other filtering options such as maxEE or truncQ. 
If not enough reads are passing the merging step, your truncated reads may not be long enough to have a 12 base pair overlap. First try to increase your truncation lengths. If that doesn't work, then consider analyzing single-end reads instead. 
If a large percentage of reads do not make it past the non-chimeric step, this may be a sign that the primers have not been fully removed from your reads or that the truncation lengths are too long. 

In our case, only ~53.8% of the input reads were non-chimeric. Looking at each stage of the filtering and denoising process, a majority of the reads were lost due to the intial filter with only approximately 58.9% passing through, which is a consequence of poor quality data. Therefore, we will reduce the truncation length for both the forward and reverse reads to increase our recovery. After several filtering iterations, a forward truncation length of 165 and a reverse of 104 produced the best recovery results. With these parameters, ~75.1% passed the initial input filter, ~71.5% merged, while ~66.7% of the reads were non-chimeric. At this point, if you would like a greater recovery, you may consider analyzing single-end reads. However, with single-end analysis, the taxonomic annotations for the biological data are less likely to be accurate.

[HOW TO DO SINGLE-END ANALYSIS](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/Single-End-16s-Analysis.md)

## Step 8: Add Taxonomy and Analyze
### Greengenes2
Now we will download the Greengenes2 database. Greengenes2 is a relatively new bacterial database, but has been found to increase reproducibility between studies.<sup>3</sup>
First you must install the Greengenes2 Qiime2 plugin within your home directory **'~'**. Ensuring that you have qiime2/2023.2 loaded in your environment, we type:
```
pip install q2-greengenes2
```
Next, we want to download the pre-trained classifier for the 16S rRNA v4 region using the following command in your **'16s_Tutorial_Analysis'** directory:
```
wget http://ftp.microbio.me/greengenes_release/current/2022.10.backbone.v4.nb.qza
```

⚠️ _NOTE: All of the following qiime commands were ran using a slurm job script._

The commands in Greengenes2 uses closed-reference OTU-picking, but we want to use open-reference. Therefore, we will be using the ```qiime feature-classifier classify-sklearn```.
This method uses open-reference OTU-picking where the sequences will be compared to our reference dataset (greengenes2) for clustering. Any sequences that do not cluster with a reference sequence will be retained and clustered against themselves. To perform open-reference OTU-picking we type:
```
qiime feature-classifier classify-sklearn \
--i-classifier 2022.10.backbone.v4.nb.qza \
--i-reads dada2-paired-end-rep-seqs.qza \
--p-n-jobs 20  \
--o-classification taxonomy.qza
```
<sup> **[Slurm script](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/Qiime-Slurm-Script.md)** used for the above command.</sup>

Here, we input the representative sequences from the ```qiime dada2 denoise-paired``` command completed previously. The ```--i-classifier``` will be the pre-trained classifier downloaded.
The outputs will be taxonomy **'taxonomy.qza'** associated with the representative sequences.

Next we will use the table from the ```qiime dada2 denoise-paired``` command and the **'taxonomy.qza'** as inputs into the ```qiime taxa collapse``` command to associate the taxonomy with the table. 
```
qiime taxa collapse \
--i-table dada2-paired-end-table.qza \
--i-taxonomy taxonomy.qza \
--p-level 7 \
--o-collapsed-table collapsed_table.qza
```
This will give us the ASV table! Now we just have to adjust the format a bit.
First, we want to unpack the contents of the file **'collapsed_table.qza'**:
```
unzip collapsed_table.qza
```
This should create a directory of the unzipped contents from the **'collapsed_table.qza'** file. The directory will have a long and random name, so feel free to rename it by typing:
```
mv [insert long random directory name here] collapsed_table
```
When putting the long directory name in the command above, do not place brackets around it. This will rename your directory to **'collapsed_table**'.

Navigate to the **'collapsed_table'** directory and then the **'data'** directory within. You will see a **'feature-table.biom'** file. We need to convert this file to a tsv format so that it is readable by programs like Excel. This can be achieved by typing:

```
biom convert -i feature-table.biom -o feature_table.txt --to-tsv
```
Now the output file **'feature_table.txt'** is ready to be moved to your local computer using Filezilla! When opened in Excel, **'feature_table.txt'** should look like this:

<img src="https://github.com/StephRut/Images-for-Github/blob/main/ASV_Table.png">

This is your ASV Table! 

### Analysis with Excel

First, we will create a new sheet in Excel and name it 'Analysis'. From the ASV table, copy the sample-ids (316D14,316D21,...) in row 2 and transpose it (a type of paste) in the first column of the 'Analysis' sheet. 
Let's start by summing up the number of reads per sample. Then we will sum up the number of reads per sample by adding up the numerical values for each column with a sample ID. After summing up column B (316D14), drag the function over to column M. Copy these sums and paste them as values below. Copy these values and transpose them into the 2nd column of the 'Analysis' sheet. Adding the numerical values of column 2 together will give you your final recovery of about 172000. Looking back at the dada2_paired_end_stats, the number of initial sequences before any filtering was 258000. Thus, our final recovery is ~ 66.7%. 

A final recovery of 66.7% is okay and unsurprising with lower quality reads. If your recovery is significantly lower, you can consider running [single-end data analysis](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/Single-End-16s-Analysis.md) to increase your recovery depth.



#### References

1.   Katiraei, S. _et al._ Evaluation of Full-Length Versus V4-Region 16S rRNA Sequencing for Phylogenetic Analysis of Mouse Intestinal Microbiota After a Dietary Intervention. _Curr Microbiol_ **79**, 276 (2022).
2.   Illumina. Understanding Illumina Quality Scores. (2014).
3.   McDonald, D. _et al._ Greengenes2 unifies microbial data in a single reference tree. _Nat Biotechnol_ **42**, 715–718 (2024).
