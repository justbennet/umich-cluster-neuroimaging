# Basic set of Slurm job options

This page will explain the most commonly used options for submitting jobs
on ITS ARC clusters.  These options should be the same on any Slurm cluster
not just on the ones run by ITS ARC.  You may wish to open [the example
script](basic-job-script.html) in a separate window to follow along.


The Slurm job script is a shell script, and on most Slurm clusters, the
shell will be `bash`.  This is indicated by the first line,

```
#!/bin/bash
```

which _must_ be there.

For those without experience using shell scripts, the `#` character
signifies a comment.  It is most common for comments to start in the first
column, but they can also appear after commands.  Both of the following are
valid comments.

```
# This is an example of a comment
echo "Welcome to my world"  # Print a welcoming message
```

Slurm options all begin with `#SBATCH`, and are considered comments by the
shell.  Slurm searches your job script for lines that begin that way and
reads the options until it reaches a line that is not either a comment, blank,
or an option.  It then runs the script exactly as it appears with the shell.

## Slurm options

We generally list the options as in the example.  The order you use is up to
you, however we generally recommend listing them in about the same order, as
it makes modifying and checking them easier if the order is consistent.  In
general, we recommend that options that may change often appear at the top
and options that change infrequently appear at the bottom of the preamble.

We refer to the collection of options that appear in the job script and that
tell Slurm about your job the _preamble_.  All of the Slurm options must
appear before any actual commands run by your job.  Any options that follow
a command will be ignored.

### Name and run time

The most common situation for neuroimaging jobs on the cluster is to have
a job correspond to some processing step for a subject.  That said, most
of the options will remain the same if you are preprocessing subjects
except the subject identfier.  We therefore start with the job name.  We
specified the job name with `--job-name=id14237689`.  The job name is displayed
when you use Slurm commands to inquire about the status of your job, e.g.,
is it running, in the queue (pending), complete.

```
$ sq
   JOBID PARTITION     NAME     USER  ACCOUNT ST       TIME  NODES NODELIST(REASON)
34108385  standard id142376 grundoon  support  R       0:05      1 gl3060
```

As you can see, the full name `id14237689` is truncated in the output from
`sq` and many other Slurm commands.  We generally try to have the first
characters be those that distinguish among jobs best.

In the above transcript, we use the `$` in the first column to indicate the
shell prompt to distinguish it from the output of the command that follows
the prompt.  So, in the above `$ sq` is the command and everything else is
output.

The next most common change is to the amount of time your job should be
able to run before the job manager should stop it.  The scheduler needs to
know the maximum amount of time so it can schedule other jobs around yours.
If you ask for a short time, say, under four hours and certainly under an
hour, your job will be easy to schedule; if you ask for a week, it will be
harder.

Try to get something that will always let your job finish, even if the
system is slow for some reason, but not so long that it is hard to schedule,
as that will delay when it starts.  You may also want to limit time in
case a program you are running misbehaves somehow and stops working
but does not actually end.

In this example, `--time=1:00:00` is asking for one hour, zero minutes, and
zero seconds.  You can specify days with, for example, `--time=1-2:03:04`
which asks for one day, two hours, three minutes, and four seconds.

## Job resources

_Job resources_ simply refers to all the attributes about the machinery
on which your job will run, which includes number of computers, the number
of cores (processors) on each, the amount of memory available, and in
some cases information about any desired GPUs.

### Tasks versus processes versus threads

When you run a software package, it starts a _process_.  Modern computers
typically have more than one processor, on which processes run.  Some software
can either start additional processes or use _threads_, which are like a
process but not one, to do computation on those other processors.

Most neuroimaging software can only use one physical computer, but some
can use more than one processor.  Additionally, you will almost always
want only one task.

There are basically three Slurm options you can use to ask that your job
get what it needs to run:  `--nodes`, `--ntasks-per-node`, and
`--cpus-per-task`.  As noted about, almost always you will want only one
node and one task, so to get additional processors, you would do so with
`--cpus-per-task`; so, to request four processors (CPUs), you would use

```
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=4
```

### Some software examples

#### MATLAB

MATLAB uses additional processors in two ways: automatically when it does
matrix operations and manually when you use `parfor`.

You give MATLAB additional processors for use automatically as shown above
by increasing the `--cpus-per-task`.

If your MATLAB program or toolbox uses the MATLAB command `parfor`, which
starts additional MATLAB processes, you need to increase the number of tasks
(at least as of March 26, 2022).  Each of the additional _tasks_ will get
its own processor.  This is done by the Slurm job manager, which is
configured to assign tasks to specific processors when they are started and
keep them there.  That configuration is subject to change on ITS ARC
clusters, and it may be different at other sites.  You should ask about it
if you have a toolbox that uses `parfor` or if you use it yourself.

#### R

R itself can only use one processor.  However, R has many, many installable
libraries (or packages) that can be installed.  Some of those compile
programs of their own, and when you use them, R passes data and instructions
for processing them to those programs, and those may be able to use more
than one processor.  So, if you know that a library you are using can use
multiple processors, you should increase the `--cpus-per-task`.

Only in rare cases would you ever need to change the number of tasks.
If something in your R code refers to _MPI_, that may want additional tasks.
It is best to ask ahead for help figuring out whether that is true and/or
needed.

#### Python

Python also typically only uses one processor, but it, too, has many,
many libraries (packages) that can be installed and that may be able to
use more than one processor.  So, if you know that a library you are using
can use multiple processors, you should increase the `--cpus-per-task`.
If your Python program contains an `import mpi4py` (or any variation
thereof), then it may need additional tasks, and you should ask ahead for
help figuring out whether that is true and/or needed.

### Memory

In addition to requesting processors, you need to request memory for them
to work with.  There are two Slurm options for requesting memory:  `--mem`
and `--mem-per-cpu`.  In general, we recommend that you use `--mem`, which
requests the total amount of memory needed on each physical machine by all
the processes combined.  We recommend you always request in units of a
gigabyte; for example,

```
#SBATCH --mem=5gb
```

On the ITS ARC clusters, the number of CPUs is related to the amount of
memory requested when billing occurs, but they are functionally independent.
As of March, 2022, each CPU requested on an ITS ARC cluster includes in its
charges 7 GB of memory.  Note, though, that does **not** mean that there
is actually 7 GB of memory for every CPU in a machine.  You should check
the configuration page for the cluster you are using to see the maximum
memory requestable. For the ITS ARC clusters, see the [Great Lakes
cluster](https://arc.umich.edu/greatlakes/) page or the
[Armis2](https://arc.umich.edu/armis2/) page, on which there should be a
link to that cluster's configuration and rates.

#### Why not use memory per CPU?

Regardless which program you use, if it can use more than one CPU, the amount
of memory needed by each may not be evenly distributed.  We have seen
instances where one processor needs a disproportionate amount of memory, and
when that one processor goes over the limit, the job is cancelled by the
job manager.  We have not seen this when memory is allocated as a pool
and only the total amount of used memory is monitoried by the job manager.

### Cluster partitions

The machines in the clusters are grouped together into what are called
_partitions_ based on the attributes of the machines.  The largest partition
is called `standard` and contains the largest number of machines and should
be used unless you know you have a need for a different machine.

The two most common reasons are you need a machine with a GPU or you need
a machine that has extra memory.  There are some neuroimaging programs that
can use a GPU; notable examples are MRtrix3 and some of the FSL programs
(`eddy`, `bedpostx`, `probtrackx2`, `xfibres`, et al.).

In general, you will save money and find more machines available to run your
jobs if you separate the steps that use a GPU from those that do not.  That
may mean, for example, that you submit three jobs for a DTI analysis rather
than one.

### Billing

The ITS ARC clusters have cost-recovery rates, and someone must pay for
usage.  In some cases this will be to an account set up by a faculty member
or other principal investigator.  The colleges of LSA and Engineering both
provide accounts for general, unfunded use that their affiliates can use
without charge to themselves.  The _Slurm account_, which is associated with
the payment source, is specified with the `--account` option.  When you
are granted access to use an account, you will be given the account name
to use with this option.

NOTE:  The account, `example`, in the example script **must** be changed
before you submit the job or your job will be rejected with an error.

Next section:  [Slurm job scripts](job-scripts.html)
