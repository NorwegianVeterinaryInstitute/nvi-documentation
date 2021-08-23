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

## How to set up conda for project nn9305K
In order to access the installed conda environments within the conda project you need to modify a file located in your “home” area on Saga, e.g. the directory where you are when you log into Saga.

Check if you have one of the following files in your login directory on Saga (/cluster/home/YOUR_USERNAME) :

.bash_login or .bash_profile.

The command to use is: ls -a

When you have one of these files (and only one is needed !!!) , then open the file with nano to add the lines indicated in point 4, else go to point 3.
When you do not have one of these files in your login directory, then copy the example file to your login directory. Make sure you are in your login directory on abel before running the following command:

rsync /cluster/projects/nn9305k/samplefiles/bash_profile .bash_profile
Then add the following lines when they are not present in the file with the command: `nano .bash_login

	export PATH="/cluster/projects/nn9305k/src/miniconda/bin/:$PATH"
	
	. /cluster/projects/nn9305k/src/miniconda/etc/profile.d/conda.sh
  
save and close the file.
log out of Saga and login again.
To check if conda is present in memory type: conda info, that should give output on screen that starts with the line:

    active environment : None The rest of that overview is a summary of the current settings for the conda environment.
	
To see which conda environments are present type: conda env list.
Loading the ncbidown environment to download genomes from the NCBI databases:

 conda activate ncbidown  This should give you a prompt that starts with the following:  ```(ncbidown) ```.
Deactivate the environment: conda deactivate.

Now one last check, in some cases it is needed to activate your conda environment in a slightly different manner, so type now on the command line: source activate ncbidown.

If this again activates the ncbidownenvironment, than your conda set-up was correct. You can deactivate this environment with either: source deactivate or conda deactivate.

Now you are all set to use the different conda environments available on the nn9305k project.
  
  
## Obtaining multiple genomes from NCBI database
	
Downloading one genome from the NCBI database is relatively easy and can be done with help of a webbrowser and the following webpages:

* https://www.ncbi.nlm.nih.gov/genome/microbes/
* https://www.ncbi.nlm.nih.gov/genome/browse#!/prokaryotes/

However, in order to download multiple genome from the NCBI webpages takes more effort. See also the section above on Conda virtual environments. Conda needs to be set-up correctly for this to work.

### How to download multiple genomes
This page is based on the work of Kai Bling: https://github.com/kblin/ncbi-genome-download. We recommend that you visit his github repository for seeing an extended list of examples on how to download microbial genomes from the NCBI web page.

Here we describe how you can use it on the abel cluster, when using the Veterinary Institute installation of the NCBI-genome-download scripts.

In order to be able to download bacterial genomes to your directory of choice, you first need to start the conda environment that contains the scripts.

#### Activating the environment
	conda activate ncbidown

that will create a prompt on your screen that looks something like this:

	(nbcidown) thhaverk@login-0-0

#### Downloading all Staphylococcus epidermidis genomes from refseq

	ncbi-genome-download --genus "Staphylococcus epidermidis" bacteria

This would download for all 482 genomes the genbank flatfiles (extension: *.gbff.gz). However, many of these genomes are not finished genomes. If you want to only have genomes that are complete and only in FASTA format (e.g. without any annotations), than use the following command:

	ncbi-genome-download --format fasta --assembly-level complete --genus "Staphylococcus epidermidis" bacteria

#### Downloading multiple species

	It could be interesting to compare the pangenomes of Staphylococcus aureus with Staphylococcus epidermidis. Thus we need to download the genomes of both species. We can use the taxon ids for both species, which are found here:

	ncbi-genome-download --taxid 1280,1282  --assembly-level complete bacteria

For more examples please visit the page of the program [ncbi-genome-download](https://github.com/kblin/ncbi-genome-download)
	
## Processing multiple datasets in parallel

### The problem
You have several hundred datasets that you want to analyze using a method of choice. Examples of these are:

* Assembly of shotgun datasets from several hundred bacterial isolates
* Hundreds of metagenomic samples that need to be quality controlled and classified.
* Iteration of parameters for testing a model you are developing.
* etc…

There are many situations where you have many samples and you can make a script that processes them one after the other. When you have a stand-alone computer that is the only option you have. But doing for instance bacterial genome assemblies for several hundred isolates will take a lot of time, even with a fast computer.

However, when you have access to a Computing cluster such as Abel, then you can harness the power of the cluster by spreading out your assembly jobs, by running the assemblies in parallel. That substantially reduces the waiting time for yourself.

#### NOTE Each user on Saga can only use 400 cpu’s simultaneously, that is 200 jobs using 2 CPUs per job. If you want to run more jobs, any job after 400 cpu’s are started has to wait until one of the earlier jobs has finished.

### The solutions.
There are three possible methods where you can use parellel processing to analyze many samples.

Start slurm jobs with a for loop, where each iteration uses a different dataset.
Use the SBATCH command: #SBATCH --array=0-99
Use the Arrayrun command.
Below you will find an explanation of the three methods and a link to a small example script to run fastqc on your illumina sequence data. There are also pros

### 1. Start slurm jobs with a “for loop”
This method is the easiest to setup. What is needed is that you create a slurm job that can take an input file. Inside the script that input file is called using the following variable ${1}.

So then the command to start the slurm jobs becomes:

	for file in *.fastq; do
		sbatch fastqc_script.slurm $file
	done

When you have X files than this loop will request X jobs in one go. This is good when you have few files to analyze. There is however a little danger here, when using a many, many files it could be that you load in one go many jobs onto Abel, that will keep other peoples jobs from starting. That is not social behaviour. So think about it.

Example script: for_loop_example_script.slurm

### 2. Use #SBATCH –array
This Sbatch command is added to the slurm script, and it will start as many jobs as requested. For instance:

	#SBATCH --array=0-89

This will start 90 jobs in the following manner: request 10 jobs and then sleep several minutes. Then check if all 10 have started. If so request another 10. If not, request as many so that only 10 jobs are waiting. It will continue with this until all 90 jobs are started. This a more social behaviour to request resources on Abel.

Q: What happens if the number of requested jobs is not equal to the number of input datasets?
Inside the Slurm script you define which datasets are analyzed for each job that is requested. A possible method on how to do that is shown here:

	#Generate array of unique datasets
	FILES=($(ls -1))

	# counting number of datasets
	echo "Number of samples:" ${#FILES[*]}

	#Generate name of current sample
	#[$SLURM_ARRAY_TTASK_ID] is the element index. Increases with one per job, i.e. one new sample for every job)
	CURRENT_FILE=${FILES[$SLURM_ARRAY_TASK_ID]}

	echo "Current input-file name is" $CURRENT_FILE

With the variable $CURRENT_FILE you can then start for instance a job to run Fastqc

### 3. Use the Arrayrun command
This latest command is used in a masterscript and it will start an X amount jobs using a workerscript to analyze a dataset. If functions in a similar way as the previous example, but the set-up is slightly different.

For instance:

	# running the arrayrun command with tasks id's ranging from 0-MAX_ID
	arrayrun 0-$MAX_ID workscript_fastqc_example.slurm

In place of $MAX_ID, you could also define a number like in the SBATCH --array script, however, when using the variable $MAX_ID the master script automatically knows how many datasets to analyze.

Here is how you set it up:

MASTER script:

	# collecting all the PAIRED dataset files into an array called FOLDERS
	FILES=($(ls -1))

	# Than we count the number of files to determine the number of tasks
	NUM_TASKS=${#FILES[*]}

	# determine the maximum number of tasks
	MAX_ID=$((NUM_TASKS - 1))

	# running the arrayrun command with tasks id's ranging from 0-MAX_ID
	arrayrun 0-$MAX_ID ../slurm_scripts/workscript_fastqc_example.slurm
	
The worker script also needs to “know” which datasets to analyze. So there you also need to identify which dataset is used in the current job that is started.

WORKER script:

	# The variable FILES contains all the files for analysis
	FILES=($(ls -1))

	#create a variable using the filename and combine it with the TASK_ID from arrayrun
	CURRENT_FILE=${FILES[$TASK_ID]}

	echo current dataset is $CURRENT_FILE

This set-up allows one to analyze an unknown amount of datasets. At least you do not have to determine it yourself.

Here are the example files in Saga: 
	/cluster/projects/nn9305k/samplefiles/ 
	
## Sharing / downloading data with filesender2
