# Batch Script 
Explanation of What a Batch Script is/ Does
## Example:
``` shell script
#!/bin/bash -l
#SBATCH --time=8:00:00
#SBATCH --ntasks=2
#SBATCH --mem=30g
#SBATCH --tmp=30g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=rutsc011@umn.edu
cd /path/to/your/directory/here
module load qiime2/2018.11
qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads unite-ver9-99-seqs-29.11.2022.qza --i-reference-taxonomy unite-ver9-99-tax-29.11.2022.qza --o-classifier unite-ver7-99-classifier-29.11.2022.qza
```
## Explanation of Each Line
