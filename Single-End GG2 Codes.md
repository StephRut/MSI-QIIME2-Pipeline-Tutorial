# Single-End Greengenes2 Analysis Codes
Use the ```non-v4-16s``` option to cluster the sequences.

```
qiime greengenes2 non-v4-16s \
--i-table dada2-single-end-table.qza \
--i-sequences dada2-single-end-rep-seqs.qza \
--i-backbone 2022.10.backbone.full-length.fna.qza \
--p-threads 12 \
--o-mapped-table single-feature-table.biom.qza \
--o-representatives gg2-single-rep-tips.fna.qza
```
Assigning taxonomy:
```
qiime greengenes2 taxonomy-from-table \
--i-reference-taxonomy 2022.10.taxonomy.asv.nwk.qza \
--i-table single-feature-table.biom.qza \
--o-classification gg2-single-taxonomy.qza
```
Adding taxonomy to the clustered table:
```
qiime taxa collapse \
--i-table single-feature-table.biom.qza \
--i-taxonomy gg2-single-taxonomy.qza \
--p-level 7 \
--o-collapsed-table collapsed_single_table_.qza
```
Unzip the table:
```
unzip collapsed_single_table.qza
```
Rename the output folder:
```
mv [insert long random directory name here] collapsed_single_table
```
Navigate to: collapsed_single_table/data/        

Convert from .biom to tsv:

```
biom convert -i feature-table.biom -o feature_table.txt --to-tsv
```
Transfer feature_table.txt to your local computer using Filezilla. This is your ASV table.

