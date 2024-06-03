# Slurm Job Script 
Submitting a Slurm job script to the slurm scheduler allows you to run commands in the background instead of running them interactively (what you have been doing up until now). These scripts are shell scripts and can be made using a texteditor app like Atom. 
## Writing the Script
### Example:
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
The first line is needed for every shell script you create. It specifies which shell you will run the script on, we will use bash here. The following #SBATCH lines are for requesting resources. The #SBATCH --time line reqeusts the amount of time you need for you job to run (here it requests 8 hours). The #SBATCH --ntasks line requests 2 processor cores. The #SBATCH --mem line requests 30 gigabytes(gb) of memory, and the #SBATCH --tmp line request 30 gb of temporary space. 

The #SBATCH --mail-type line will allow you to receive an email when the job begins, completes and/or fails. The #SBATCH --mail-user specifies to whom these emails are sent.
The cd line indicates from which directory you want to run the command. You will also need to load the proper module for the command before listing the command. The last line in the script is the command you want to run.

More information for the MSI Slurm Scheduler can be found [here](https://msi.umn.edu/our-resources/knowledge-base/slurm-job-submission-and-scheduling). The partitions you can run the jobs on can be found [here](https://msi.umn.edu/our-resources/slurm-scheduler/slurm-partitions).

## Submitting the Script

To submit the script first transfer the file containing the shell script from your local computer to MSI using Filezilla. 

**Windows Users Only:**
```
dos2unix [file name]
```

Don't put the square brackets around your actual file name. Then, schedule the job using:
```
sbatch [file name]
```
To check on the status of your job, you can use:

```
squeue -u x500
```




