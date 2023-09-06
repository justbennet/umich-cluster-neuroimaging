# An introduction to cluster computing

A compute cluster is a collection of computers that have specific
hardware, networking, storage, and software to enable many computers
to be treated as one big machine.  For a simple diagram of a generic
cluster, please see the entry for ***cluster*** on the [cluster
glossary](https://docs.support.arc.umich.edu/terms/) page.

Cluster computing is a specific kind of _batch_ computing, which simply
means that you put the commands you wish to run into a file, then
instruct the computer to read the file and run the commands.  One or
more commands can be included in the file; if there are several, then
the commands, taken together, are consider a _batch of commands_, hence
the terminology.  The file that contains the commands to run is called
a _job script_.  The commands are the same ones you would use if typing
them at a command line prompt.

There is also a web interface to the ARC clusters, called Open OnDemand,
or just OnDemand for short.  That can be used to start a job that opens
a graphical interface to select applications on the cluster.  Each time
you start a session with a graphical interface, you are actually starting
a job, so the limits are the same as for jobs run in batch.

One question that most people ask when first told about clusters is,
Why should I use a cluster?  Please see the page [When to use ARC
clusters](https://docs.support.arc.umich.edu/help/use/) for a some
examples of when and why you might want to use a cluster.

Each job is run by a _job manager_, which is told when to run each job by
a _job scheduler_.  In the case of ITS ARC clusters, both of these are
part of a software package called Slurm.  See the bottom of this page for
a link to the online Slurm documentation.

# Why cluster computing not high performance computing?

Many clusters started life as high performance computing (HPC) clusters, and
some still are.  However, as the 2010s came to a close, more and more of
users of clusters were not concerned with the high performance aspect.  Instead,
they were concerned with high job volume (throughput) and transferring
their computing load from computers they bought and maintained to the cluster.
That enables people to do work on their own computers that is not easily

Given modern computers, it makes most sense for neuroimaging to think of
jobs as being defined by subject, and that also makes it quite convenient
to move back and forth between researcher-owned computers and the cluster.
done on the cluster, most notably visualization.

So, this site is mostly about how to use the cluster to run many jobs, not
about how to make one job go faster or get bigger.  The most common
neuroimaging use of clusters is for preprocessing -- that is, all the
processing that is done to a single subject without reference to any other
subjects.  This is sometimes called level one processing (SPM and FSL), and
possibly also level two (FSL).  FreeSurfer's `recon-all` is an excellent
and common candidate.

## References

[Official Slurm documentation](https://slurm.schedmd.com/)
<br>[Cluster terminology (a glossary)](https://docs.support.arc.umich.edu/terms/)



Previous section: [Contents](index.html)
Next section:  [Intro to Slurm jobs](job-intro.html)
