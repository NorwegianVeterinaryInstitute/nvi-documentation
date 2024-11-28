# First time on Saga

## Learning objectives:

* Know what Saga is and what to use it for
* Know how to log in to Saga from your own computer
* Do first-time setup to get the right configuration needed for Saga
* Know how to move around on saga
* Know what is where on Saga
* Know how to set up a slurm job for saga

## What is Saga

Please read the [What is Saga](../technical/saga_basic_info.md) guide to learn
more about what saga is and how we use it.

## How to log in, and first time setup

To use Saga, you first need to log on to the computer. During your first
time on the computer, there are certain tasks that should be completed.
Please go through the [Logging in on Saga and NIRD](../technical/sagalogin.md)
to do the necessary things.

Saga is a Linux system. This means that you use linux (or unix) commands to
move around. See the [External Tutorials](external_tutorials.md) guide for
links to a linux tutorial. Meanwhile, the most important commands to know are:

* pwd - 'print working directory' - to see where in the file system you are
* ls - to look at the contents of the current directory
* cd - to change directory
    * to change into a directory that is underneath your current, do
    `cd directoryname`
    * to change into the directory above your current, do `cd ..`
* less filename - to look at a file. Scroll with the arrows, quit with `q`


## Where is what

There are several different directories that you should be aware of. See
the [Drive overview](../technical/drives_saga.md) for more info.

##  How to set up a basic slurm job

To run things that take a bit of time, we need to submit it to the queue system
so that it can run the program for us. In order to make things a bit easier
for us, we have created several template files that we can use. These can
be found here:

`/cluster/projects/nn9305k/samplefiles`

To try out how this works, copy one of the example slurm scripts to your
active area on saga. Then, try running it to see what happens.

There is also more information to be found on how to use the queueing system
in the [Slurm usage](../technical/slurm_usage.md) guide.
