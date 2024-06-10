# Single-End Greengenes2 Analysis Codes
Use the ```qiime feature-classifier classify-sklearn``` option to determine taxonomy in the representative sequences.

```
qiime feature-classifier classify-sklearn \
--i-classifier 2022.10.backbone.v4.nb.qza \
--i-reads dada2-single-end-rep-seqs.qza \
--p-n-jobs 20  \
--o-classification taxonomy_single.qza
```
Adding taxonomy to the table:
```
qiime taxa collapse \
--i-table dada2-single-end-table.qza \
--i-taxonomy taxonomy_single.qza \
--p-level 7 \
--o-collapsed-table collapsed_table_single.qza
```
Unzip the table:
```
unzip collapsed_table_single.qza
```
Rename the output folder:
```
mv [insert long random directory name here] collapsed_table_single
```
Navigate to: collapsed_single_table/data/        

Convert from .biom to tsv:

```
biom convert -i feature-table.biom -o feature_table.txt --to-tsv
```
Transfer feature_table.txt to your local computer using Filezilla. This is your ASV table.

