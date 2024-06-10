```
#!/bin/bash -l
#SBATCH --time=3:00:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=20
#SBATCH --mem=60g
#SBATCH --tmp=60g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=rutsc011@umn.edu
cd ~/16s_Tutorial_Analysis
module load qiime2/2023.2
qiime feature-classifier classify-sklearn --i-classifier 2022.10.backbone.v4.nb.qza --i-reads dada2-paired-end-rep-seqs.qza --p-n-jobs 20  --o-classification taxonomy.qza
```
Here we use #SBATCH --cpus-per-task=12 to perform a multithreaded job. This will allow for parallel processing as requested by the --p-threads 20 option (20 threads). Using more than one thread, when possible will decrease the amount of time it takes to run your command.
