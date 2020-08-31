# What is Saga

Saga is one of several high performance computers that is set up by a
national computing resources consortium called [Sigma2](https://www.sigma2.no/).
This consortium sets up such resources for the use of all researchers in Norway.
To gain access, each institution has to apply for access.

## Acknowledging use of Saga
Note, in order to get and maintain access it is very important to
register all scientific publications that are a result of use of Saga and other
resources. Thus, please do the following:

* [Please report the use of Sigma2 in Cristin, as described here]
(https://www.sigma2.no/reporting-usage-through-cristin)
* [Please include the following acknowledgement in your paper]
(https://www.sigma2.no/acknowledgements)
* Please report any papers, posters and other publications to the local admins.


## What is Saga used for?
The NVI mainly uses Saga for bioinformatics analyses, but Saga can also be used
for a variety of other things. Physicists, meteorologists and others use
this resource for their analyses. Researchers tend to choose to use Saga
because normal computers can either not handle the amount of data they are
analysing, and/or because the calculations take too much time otherwise. An
additional factor is that for many research fields, the tools that are in
common use are only available for the UNIX/Linux operating system platform.
This is the case for most bioinformatics related analysis.

For the NVI, Saga is a resource that gives us a place to share data and
software, and to analyse data in a fast and efficient way.

Please note, Saga is a shared resource, not only among researchers at the NVI
but among researchers throughout Norway. Thus it is important to follow the
rules that are set on the operations of Saga to ensure a smooth experience for
all users.

## Drive areas

The NVI has a shared drive area on Saga and a storage area on NIRD. These are
organized in specific ways to ensure that there is space enough for all of
us, and that we all have access to the data we need. Please see the disk drive
description for more information on which areas are available and how to use
them responsibly.


## Queueing system and software

Saga is a shared resource, and a such there has to be a system in place that
helps sort out the use of the resources. This is done through a queueing system
called `slurm`. This is a program that runs on the computer, and that accepts
requests from users to run a program. Anything that would run for more than
30 minutes, or use a bit of memory should be submitted to the queueing system.
More info can be found under information on how to use slurm.

On Saga, there are many users at any one time trying to do a lot of different
things. To accomodate the need for various kinds of software, there are systems
that help organize software. This is done to ensure that each user gets access
to the software they want, and  not by accident something else.

There are two main sources for software:
* The conda system, managed by NVI people
* The module system, managed by the cluster

More info on how to get access to software through these systems can be found
elsewhere in this manual.
