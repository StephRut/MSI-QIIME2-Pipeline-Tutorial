# Filter, Denoise, and Visualize Single-End Data
If you are struggling with recovery with paired-end 16s analysis,  I would recommend using single-end analysis. Luckily this only requires some minor adjustments to Step 7 of the pipeline!

First we want to make sure that Qiime2 has been loaded using:
```
module load qiime2/2023.2
```
Next, ensure that you are in the proper directory (**'16s_Tutorial_Analysis'**) using `pwd`. Once you confirm you are in the proper directory, we will run the ```qiime dada2 denoise-single``` command instead of the ```qiime dada2 denoise-paired``` command. Our input file will remain the same **'trimmed-seqs.qza'**, but we will need to rename our 3 output files so as not to overwrite the files we made previously. Additionally, we will no longer use the `--p-trunc-len-r` parameter as we will only be looking at the forward reads. Initially, we will truncate our forward reads at a length of 165 (our forward truncation length for the paired-end section of the tutorial).

```
qiime dada2 denoise-single \
--i-demultiplexed-seqs trimmed-seqs.qza \
--p-trunc-len 165 \
--o-representative-sequences dada2-single-end-rep-seqs.qza \
--o-table dada2-single-end-table.qza \
--o-denoising-stats dada2-single-end-stats.qza
```

Now, we will visualize the denoising stats.
```
qiime metadata tabulate \
--m-input-file dada2-single-end-stats.qza \
--o-visualization dada2-single-end-stats.qzv
```

View the QZV stats file in QIIME 2 View, download the .tsv, and open it in Excel to calculate the recovery following the steps for paired-end data. After performing the analysis, ~90.7% of the reads passed the intial filter and ~88.5% were non-chimeric!

Using the outputs from the ```qiime dada2 denoise-single``` command we can continue on with step 8 of the [MSI-QIIME2-Pipline-Tutorial](https://github.com/StephRut/MSI-QIIME2-Pipeline-Tutorial/blob/main/README.md). Of course, we will need to substitue the single-end files for the paired-end files in the commands in step 8. I would recommend trying to do this on your own at first, but if you cannot figure it out, you can find the commands [here].
