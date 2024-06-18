# ITS QIIME2 Analysis
Processing ITS (internal transcribed sequences) of fungi does differ slightly from 16S rRNA V4 region analysis for bacteria. Namely, ITS data have different primers, filtering methods and a different database for reference sequences.

Trim primers (different primer pairs specifically ITS2)
```
qiime cutadapt trim-paired --i-demultiplexed-sequences demux.qza --p-front-f TCGATGAAGAACGCAGCG --p-front-r TCCTCCGCTTATTGATATGC --p-error-rate 0.225 --o-trimmed-sequences demux-trimmed3.qza
```


Because the ITS2 fungi sequences tend to vary in length more so than the 16S rRNA V4 region, it is recommended to filter the sequences by both -p-max-ee and -trunc-q instead of trimming/truncating at a specified length. 


Unite database

