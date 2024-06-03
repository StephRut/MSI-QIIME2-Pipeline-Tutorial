```
#!/bin/bash -l
#SBATCH --time=1:00:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --mem=15g
#SBATCH --tmp=15g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=rutsc011@umn.edu
cd ~/16s_Tutorial_Analysis
module load qiime2/2023.2
qiime greengenes2 non-v4-16s --i-table dada2-single-end-table.qza --i-sequences dada2-single-end-rep-seqs.qza --i-backbone 2022.10.backbone.full-length.fna.qza --p-threads 12 --o-mapped-table single-feature-table.biom.qza --o-representatives gg2-single-rep-tips.fna.qza
```
Here we use #SBATCH --cpus-per-task=12 perform a multithreaded job. This will allow for parallel processing with shared me
