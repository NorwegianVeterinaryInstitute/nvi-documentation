# What is conda?

Conda is a management system for software that can run on Windows, MacOS or linux system such as the SAGA cluster. We use conda to maintain software on the SAGA cluster that is of interest for the users of the Norwegian Veterinary Institute. The advantage of using conda is that we can create an `environment` that only consists of the software packages that are needed to run a certain application. In this way we can start the environment for our application and install the software easily  inside that environment. In this way, we can prevent the occurence of conflicts with other software that need slightly different versions of commenly used packages needed to run for instance Python scripts.

The people that maintain Saga also maintain software installations on the cluster. They use the module system for that. Most of the software available on Saga via the module system, is software that is usable for many of the users on Saga. 

### Before you start using conda 

Before you start with the creation of a conda environment for a specific software you want to use, it is good practice to see if you can identify if the software is available via the Saga module systems. Here are commands to use the module system on saga  to find [installed software](https://documentation.sigma2.no/software/installed_software.html).

## Setting up your conda system


