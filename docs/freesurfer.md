## Running FreeSurfer on the cluster

In this example, we will set up a job script to run one subject from
sample data available on the UM Great Lakes cluster.  FreeSurfer's
`recon-all -all` runs for a long time (6 &ndash; 10 hours, typically),
so we will only run `recon-all -auto-recon1` when we show the example
with multiple subjects.

The procedure we recommend you follow when starting is to run `recon-all`
from a login node on one subject to make sure you have the proper syntax,
then run the same subject from a job.  Check that all the options you
need have been specified and that all the output you expect is generated.

Then we will modify the single-subject job script to be used with more
than one subject and submit a group of subjects.

An exercise is provided for you to convert to using a different data set,
and if you complete that, you should be in good shape to modify the
job setup to work with your own data.

---
**NOTE**

The following section is under construction; check back soon for more updates.

---

### Running the Analysis

To analyze a subject using FreeSurfer's `recon-all`, let's modify the sample 
script that was provided earlier, and save it as `myJobScript.sbat`:


```
#!/bin/bash

#---------------------------------------------
# Slurm preamble

#SBATCH --job-name=id14237689
#SBATCH --time=12:00:00

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
module 

#---------------------------------------------
# Print useful diagnostic information to the job output file
my_job_header

#---------------------------------------------
# Commands to run during the job
cd nfs/turbo/lsa-ajahn/FreeSurfer_Demo
SUBJECTS_DIR=`pwd`
recon-all -s sub-101 -i sub-101_ses_BL_T1s.nii.gz -all
```

Note that we changed the total time to 12 hours, to provide enough time 
for recon-all to complete. In rare cases it may take longer than that, but
for most purposes it finish before that time limit. If the job runs longer
than the time limit, it will terminate and return an error in the output
slurm script.

We can then submit this job by typing:

```
sbatch myJobScript.sbat
```

And you will see a line of text returned that says that the job was submitted,
along with an ID. You can check on the status of the job by typing `squeue` and
piping it into a grep command to find your username. For example, if my username
is `ajahn`, I can check on the status of my job by typing `squeue | grep ajahn`.
This will return a list of any jobs submitted under my username, along with their
status and how long they have been running.

As the job runs, the output will be continously written to a file called
`slurm.xxxxxxx.out`, with the x's representing the ID of the submitted job. You
can check the output from time to time using a text editor, or you can monitor
the output in real-time by typing `tail -F slurm.xxxxxxx.out`.
