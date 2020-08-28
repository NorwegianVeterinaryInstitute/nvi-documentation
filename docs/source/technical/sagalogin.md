# Logging in on Saga and NIRD

Saga is the computing cluster that is frequently used for bioinformatics
and other activities at the Institute.

NIRD is a high capacity storage facility that is connected to Saga.

## Computer names

* Saga: saga.sigma2.no
* NIRD: login-trd.nird.sigma2.no


## First time users

### Apply for access

If you are a first time user of the cluster, you need to apply to get access
to it.

* [Apply for access here](https://www.sigma2.no/how-apply-user-account)

Use Feide user names if you have it. If not: create your own 7-8 letter
username from your first and last name. We want to be able to tell who is who,
that is not doable from VI usernames.

* Organization is Veterinærinstituttet.
* Project is nn9305k
* Resource is Saga.

Your account will be approved by Sigma2 within a day or two, and then the
allocation managers here at NVI are notified. We then approve your access.
You will then have to set your password. Use the instructions in the link
below to do that. NOTE: do not reuse either UiO or Vetinst passwords for
your Saga account!

If you are confused regarding passwords, this link will guide you on that

* [About passwords, how to get one, and how to reset it](https://documentation.sigma2.no/getting_help/lost_forgotten_password.html)

### Setting up your account for working on saga

There are a few settings we need to change to ensure that we all get a good
working experience on saga.

The instructions are to be found in this file:

`/cluster/projects/nn9305k/samplefiles/new_user.txt`

Please do what is in that file, before doing anything else on the cluster.

## How to log in to Saga

Once you have your username and password, you need to log into Saga. To do that,
you need to access something that is called a terminal. A terminal is a window
into which you can type commands. There are three main ways to get to a terminal
window:

* Use the program putty (not recommended)
* Use the program called `git bash` (recommended)
* Have a virtual machine (vm) on which you have linux, which has a terminal (other
  parts of this documentation contains information about this)

This description is based on you using either `git bash` or a vm.

* Open git bash (or a terminal window)
* Type in the following command on the command line that appears:
  `ssh saga.sigma2.no`

You are now using a program that is called `ssh` to create a tunnel through
the internet between your computer and Saga.

Once you have typed in your password, you will now be on Saga. Anything you type
in on the command line now will happen on Saga.
