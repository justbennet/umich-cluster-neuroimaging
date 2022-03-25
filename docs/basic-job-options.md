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
