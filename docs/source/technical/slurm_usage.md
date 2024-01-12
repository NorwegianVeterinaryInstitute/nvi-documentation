# How to use slurm

On the SAGA cluster all compute resources are managed by a system called [Slurm](https://slurm.schedmd.com/documentation.html). If we want to analyze data on Saga we need to request resources and if those are accepted a job is started. We do this because we can not run large analysis on the login nodes. If you try to run a large job anyway, it will be killed automatically after 30 minutes. 

For a very detailed overview of using the slurm queue system on saga you can check out the pages linked here: [Sigma2 - running jobs](https://documentation.sigma2.no/jobs/overview.html)

There are two ways of running analysis on SAGA using the slurm system
1. using a slurm script
2. using a interactive command.

## How to write and use a slurm script
Before diving into this it is good to know that on Saga we have example slurm scripts ready in the folder:
/cluster/projects/nn9305k/samplefiles

A slurm script is a simple text file written in bash which alwasy starts with the line: 
```
#!/bin/bash
```
To create a slurm file we using commands that are called `#SBATCH` commands. The most important one is the line that specifies which project allocation is used for the job.

```
#!/bin/bash

#SBATCH --account=nn9305K   ## The billed account
```

Then we can add additional lines that are helpfull as well:
```
#!/bin/bash

#SBATCH --account=nn9305K   ## The billed account
#SBATCH --job-name Test   ## Name of the job
#SBATCH --output slurm-%j.out   ## Name of the output-script (%j will be replaced with job number)
```
The above four lines however, do not give you a job, since you need to specify also how much time your you can use, how many memory it needs and how much cpus it can use for the job.
If we add those lines than we get a little script like this:

```
#!/bin/bash

#SBATCH --account=nn9305K   ## The billed account
#SBATCH --job-name Test   ## Name of the job
#SBATCH --output slurm-%j.out   ## Name of the output-script (%j will be replaced with job number)
#SBATCH --time=00:01:00   ## The maximum time the job can use
#SBATCH --mem=30Gb   ## Memory allocated to a task in Gb
#SBATCH --cpus-per-task=8   ## Number of CPUs allocated for each task / job
```

If we however want to use the large memory nodes, or the GPU nodes on sage we also need to specificy the partition. For instance the large memory partition is called 'bigmem'. We can add that here.
```
#!/bin/bash

#SBATCH --account=nn9305K   ## The billed account
#SBATCH --job-name Test   ## Name of the job
#SBATCH --output slurm-%j.out   ## Name of the output-script (%j will be replaced with job number)
#SBATCH --partition=bigmem   ## Selected partition
#SBATCH --time=00:01:00   ## The maximum time the job can use
#SBATCH --mem=30Gb   ## Memory allocated to a task in Gb
#SBATCH --cpus-per-task=8   ## Number of CPUs allocated for each task / job
```
__Note !__: that a single CPU on the bigmem node has 30.000 Mb memory by default, while a CPU on a normal node only has 4.096 Mb of memory.

Now we can add commands to the script for any thing that you want to do one Saga.

```
#!/bin/bash

#SBATCH --account=nn9305K   ## The billed account
#SBATCH --job-name Test   ## Name of the job
#SBATCH --output slurm-%j.out   ## Name of the output-script (%j will be replaced with job number)
#SBATCH --partition=bigmem   ## Selected partition
#SBATCH --time=00:01:00   ## The maximum time the job can use
#SBATCH --mem=30Gb   ## Memory allocated to a task in Gb
#SBATCH --cpus-per-task=8   ## Number of CPUs allocated for each task / job

### This is a comment line
### below this you write your commands

<Your command here>   ## Command to be run
```

For more help on writting slurm scripts, you can use the [Slurm script generator](https://open.pages.sigma2.no/job-script-generator/)


## Setting up a interactive job

There are moments when you want to try out stuff on the commandline, without running an actual script. At such moments you can write a single command to request compute resources on Saga. 

The command is a one liner of the SBATCH commands from the script.

```
srun --account=9305K --mem=30G --cpus-per-task=8 --time=00:01:00 --pty bash -i
```
The last bit `--pty bash -i ` will allow you to have an interactive job. If you replace `bash -i` with `Your command ` than you will get a job and the command is run immediately.

For more advances jobs like array jobs, GPU jobs or Big Mem jobs take a look at the sample files we have on Saga.




