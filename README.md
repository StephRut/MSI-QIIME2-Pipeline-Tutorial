# MSI_QIIME2_Pipeline_Tutorial
## Step 1: Software Requirements
  1) FileZilla (Cyberduck, etc.)
  2) Text Editor 
  3) Citrix (Cisco Anyconnect, etc.)
  4) Excel

## Step 2: Connect to MSI Server
 1) locate raw read files and copy directory into home directory
 2) create new directory for analysis
 3) linux commands link
4)  find [batch files](https://github.com/StephRut/MSI_QIIME2_Pipeline_Tutorial/blob/main/Batch%20Script.md) and Slurm scheduler
5)  link to MSI OnDemand
## Step 3: Creating a Manifest File
  1) Dependent on QIIME Version Used
  2) Make Sure no Extra Space in Column
  3) .csv file
  4) Example of End Product
## Step 4: Load QIIME2 on MSI
## Step 5: Import Manifest
 1) import manifest and get sequences in .qza format
 2) visualize imported reads Qiime  2 View
## Step 6: Trim Primers
 1) trimming forward and reverse primers
 2) visualize results of trimming primers, look at the quality of your reads... this is how you will determine the trim and trunc lengths
## Step 7: Filter the Sequences
 4) know the length of the sequence you are targeting, if you trim too short, then there might not be enough overlap to merge the reads, in which case single end analysis would likely give better results
 5) ideally, keep the median above q 30
 6) explain what a quality score is 30
7) explain EE
8) explain p-trunc-q
9) visualize the dada stats... make sure you aren't loosing too many reads ideally 70% recovery
## Step 8: Training the Classifier
 1) Download the Classifier from either green genes 16s or Unite ITS
 2) Link both of them on the page
 3) make characters uppercase to avoid downstream errors
 4) 99 sequences .fasta to .qza
 5) 99 taxonomy .txt to .qza
 6) train classifier code
 7) Learn the classifier (i believe 16s you trim to V4 region) but in ITS its best not to do trimming
## Step 9: Taxonomy
 1) create taxa feature table (ASV)
 2) convert taxa table into .txt
 3) open in excel
 4) analysis

