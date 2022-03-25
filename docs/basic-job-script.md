# Very basic Slurm job script

The following is a very basic Slurm job script that is intended to be a model
for job scripts on the ITS ARC clusters that use Slurm.

```
#!/bin/bash

#---------------------------------------------
# Slurm preamble

#SBATCH --job-name=id14237689
#SBATCH --time=1:00:00

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=5g

#SBATCH --account=example
#SBATCH --partition=standard

#SBATCH --mail-type=NONE

#---------------------------------------------
# Make sure only and all the software needed for the job is loaded
module purge

#---------------------------------------------
# Print useful diagnostic information to the job output file
my_job_header

#---------------------------------------------
# Commands to run during the job
echo "Hello, world"
```
