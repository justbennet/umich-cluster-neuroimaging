# An introduction to cluster computing

Cluster computing is a specific kind of _batch_ computing, which simply
means that you put the commands you wish to run into a file, then
instruct the computer to read the file and run the commands.  One or
more commands can be included in the file; if there are several, then
the commands, taken together, are consider a _batch of commands_, hence
the terminology.  The file that contains the commands to run is called
a _job script_.  The commands are the same ones you would use if typing
them at a command line prompt.

Each job is run by a _job manager_, which is told when to run each job by
a _job scheduler_.  In the case of ITS ARC clusters, both of these are
part of a software package called Slurm.  See the bottom of this page for
a link to the online Slurm documentation.

Please see the page [When to use ARC
clusters](https://docs.support.arc.umich.edu/help/use/) for a some
examples of when and why you might want to use a cluster.

## References

[Slurm documentation](https://slurm.schedmd.com/)

[Cluster terminology (a glossary)](https://docs.support.arc.umich.edu/terms/)



Previous section: [Contents](index.html)
Next section:  [Basic set of Slurm job options](basic-job-options.html)
