# An introduction to cluster computing

Cluster computing is a specific kind of _batch_ computing, which simply
means that what you want to run is submitted in a batch and the computer then
takes what you submit and does what you asked.  The batch could consist of
just one specific thing, called a _job_, or it could hundreds of things.
The 'things' you want done are typically specified in a _job script_,
which is just a shell script that will run on the cluster.

Each job is run by a _job manager_, which is told when to run each job by
a _job scheduler_.  In the case of ITS ARC clusters, both of those are
from software called Slurm.  See the bottom of this page for a link to
the online Slurm documentation.

You may find opening our [example job script](basic-job-script.html)
in another window (or tab) helpful, as you can then follow along in the
script with the explanations of the options in what follows.

## 

## References

Slurm documentation:  https://slurm.schedmd.com/

