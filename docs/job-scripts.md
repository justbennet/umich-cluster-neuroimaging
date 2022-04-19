# Slurm job scripts

## Job scripts are Bash scripts

As noted in the previous section on the Slurm job options, a job script is
just a Bash shell script.  You will note from the example shown there that
all the Slurm options begin with the `#` character, which makes them 
comments.  That means that every job script, if properly written, can
be run outside of a job as well as inside one.  If we remove all blank lines
and comments from the example script, we are left with only three lines:

```
module purge
my_job_header
echo "Hello, world"
```

The implication of this is twofold:  You can run a job script on a login
node to test it (within some constraints) and you can include in a job script
any command that you would use at a shell prompt.

### Diagnostic information

We recommend that you always include the `my_job_header` command to include
diagnostic information about the job in your output.  This will seldom be
needed, but when it is, it can make troubleshooting problems with your jobs
much easier.  You do not need to understand all its output, but if you contact
cluster support people, they will.

### Software your job uses

There are number of software packages for analyzing or processing MRI images.
Software is configured for use when you need it by using the `module` command.
You can locate most of the software specific to neuromimaging by using the
`module keyword mri`.  Here is an example from March, 2022.

```
$ module keyword mri
----------------------------------------------------------------------------

The following modules match your search criteria: "mri"
----------------------------------------------------------------------------

  ANTs: ANTs/2.1.0
    ANTs MRI image registration program

  ANTsR: ANTsR/2.1.0
    R with ANTsR available via site libraries.

  afni: afni/18.0.27
    AFNI suite of MRI neuroimaging tools.

  freesurfer: freesurfer/6.0.0
    FreeSurfer MRI processing and analysis software.

  fsl: fsl/5.0.11, fsl/6.0.3, fsl/6.0.5.1
    FSL suite of MRI neuroimaging tools.

  mricron: mricron/30apr2016
    Image viewer for NIfTI images and DICOM conversion

  mrtrix: mrtrix/3.0_RC3, mrtrix/3.0.3
    MRtrix provides a set of tools to perform various advanced diffusion
    MRI analyses.
```

Many packages will have only one version, but some have several.  From the
output above, you can see that `afni` has only version 18.0.27 available
whereas `fsl` has versions 5.0.11, 6.0.3, and 6.0.5.1.

There may be other packages available that are less commonly used that are
not included, especially if they are typically only used when installing
other software.  For example the ITK software packages is commonly used
for its image processing, but you must first set up your environment to
compile the software you want before it will be visible.

Some software also does not need a module to run.  The most commonly used
package like that is fMRIPrep.  An example of using fMRIPrep will be shown
in a later section.

Let's say we wish to do DTI analysis using the MRTrix3 package.  To do that
you will use 

```
$ module load mrtrix
```

where the `$` indicates the prompt at which you type the `module` command.
When you type this, or when you put it into a job script, do not include the
`$`.

To see which modules are loaded, you use

```
$ module list
```

which is one of the command run by `my_job_header`, hence why we put `module`
commands before `my_job_header`.

The `module purge` command shown in the example job script removes (purges)
all loaded modules.  By including it before any `module load` commands,
you insure that _only_ the software you need for a specific job is loaded.
That, in turn, increases the likelihood that your script will reproduce
results if used again.

### Running your desired software

Finally, in the example, the software we run just prints

```
Hello, world
```

We use this to make sure that we have got all Slurm options correct and that
we have the right software modules loaded.

In the examples coming next, we'll replace that with examples of using real
software.

### Testing your job script

You can test your job script by running it on the login node, provided that
it does not run too long, use too much memory, or use too many CPUs. Often
many of the commands in your job script will be fetching data from one place
to another and running preliminary steps that may not take long, those can be
tested without running your analytic software.  Copy the commands from
the example and put them into a file called `example.sbat` on the cluster.

You can now run it with

```
$ bash example.sbat
Hello, world
```

The file extension `.sbat` is just a local convention.  It is not required.
We use it to make grouping related files by name and distinguishing among
them by extension possible.  For example, I might have

```
dti_analysis.py
dti_analysis.sh
dti_anslysis.sbat
```

as the three files used for my DTI analysis.  You can, also, make your
script an executable program by changing it _mode_ (permissions) once,
and then using it as a command.  For example,

```
$ chmod +x example.sbat
$ ./example.sbat
Hello, world
```

The `./` is required so that the shell knows which directory the `example.sbat`
command is in (`.` indicates the current directory).

## Submitting your job script

To submit your batch script, you use the `sbatch` command.

```
$ sbatch example.sbat
```

Previous section: [Basic set of Slurm job options](basic-job-options.html)
