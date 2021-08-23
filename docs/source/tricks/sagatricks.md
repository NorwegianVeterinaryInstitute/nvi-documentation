## Using screen to avoid dropped connections

We are using Saga. Once we run a program on Saga (or any remote computer), that program will only keep running a long as the terminal is connected to it, that is, as long as you have the computer and the ssh connection up. This is a bit inconvenient since we’re working on laptops and might want to pack that one down without interrupting the connection. There is a program on unix computers that is called screen which will let us do that.

### How to start
1. Log onto saga
2. Type in screen. You will get a new prompt, but that’s it
Start whatever program that yo want to have running.
Depress Control-a, followed by d. Your screen will now say something about a screen being detached.
You can now end your ssh session, and your program will keep running.

### How to see and resume your sessions
You can see a list of screens (you can have multiple), type in screen -ls. You will see something along the lines of 23921.pts-123.login-0-1. The number before pts is what I refer to below. To connect to a specific screen, type in screen -DR <number>.

Note, since there are multiple login nodes on Saga. Thus, if you use screens, you might want to check which one you’re on when you start your screen (use the command hostname). Then, when you log in again, you can check again which one you’re on. If you’re on the wrong one, use ssh to log into the other one.

For more, [check out this link](https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/).
  

## Conda virtual environments
On a computer we can install a lot of different software packages. When you do that on the Saga cluster, or on your own Windows or Macintosh machines, it often happens that software requires additional software in order to function properly. These we call “dependencies”. For example, some software requires the python version 2.7 while other software requires python version 3.5 or higher.

On many computing clusters this is solved by a process called “Sandboxing”, where a system is set-up that allows one to load a specific set of software by loading a “module”. The module file contains a list on which software to load to set-up the environment is such a way that you can run for instance the SPAdes assembler.

Note however, that when you load multiple different modules, it can happen that one version loads python 2.7, while another loads 3.5. At such moments, your software becomes “confused” and tries to run a python script with the wrong python version, and it will not work. In such situations, it can be convenient when you do not have to worry about dependencies having conflicts without having to think about the settings of the system you are running. A way of solving this is to use Virtual environments. The purpose of a virtual environment is to create a space where only software is allowed that does not create internal conflicts due to differences in the dependencies needed. For instance, only python version 2.7 is allowed and not python version 3.5, or vice versa. And if you for some reason need to switch python version, it is only a matter of changing the active environment.

For more on python virtual environments check here: * [Python virtual environments](https://realpython.com/python-virtual-environments-a-primer/)
  
In recent years using virtual environments has improved and now multiple system exists that helps users to manage virtual environments. On Saga we use the conda system, and see for more here: [Conda virtual environments](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html). In order to use this it is needed to set-up the conda system so you can use it to run special software, such as the bifrost pipeline, or ncbi-genome-download.

## Obtaining multiple genomes from NCBI database

## Processing multiple datasets in parallel

## Sharing / downloading data with filesender2
