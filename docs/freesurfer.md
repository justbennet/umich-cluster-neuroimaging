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
