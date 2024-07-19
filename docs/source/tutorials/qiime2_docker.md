# Getting Qiime 2024.5 via container

## Setting up your `.bashrc` config for Apptainer

[Apptainer](https://apptainer.org/) is the system on SAGA that allows you to use containers.

Some configuration is necessary to make it easier to use containers. To do that, we need to modify the file `.bashrc`, which is located in your home directory.

```bash
cd ~
nano .bashrc
```
Copy those lines and put them into your `.bashrc`.
Replace what is in between `<username>` with your own username.

Add those lines to your .bashrc. Do not forget to change `<username>` with yours.
```bash
# This is for apptainer itself (for me worked without but do it)
export APPTAINER_CACHEDIR=${USERWORK}/images
export APPTAINER_LIBRARYDIR=${USERWORK}/images
export APPTAINER_HOME_MOUNT=true

# This is for the nextflow pipelines ... can do that at the same time, it will be done!
export NXF_APPTAINER_CACHEDIR=${USERWORK}/images
export NXF_APPTAINER_LIBRARYDIR=${USERWORK}/images
export NXF_APPTAINER_HOME_MOUNT=true

# This allows you to have access to those paths when you use the container. If you need more add those
export APPTAINER_BIND="$USERWORK,/cluster/projects/nn9305k/active/<username>,/cluster/projects/nn9305k/db_flatfiles"
```

## Getting the container on SAGA

Do that in your $USERWORK  in a subdirectoly called images (`/cluster/work/users/<username>/images`).

```bash
# download the container into an image (sif is the singularity format) 
apptainer pull qiime_amplicon.sif docker://quay.io/qiime2/amplicon:2024.5
```

Now, you need to create a command to make it more easy to use the container when you need it:
Interactive work eg. for testing.

Note: You need to ask ressources before starting the container.
When the container is started, you see a new promt in your shell: `Apptainer>`.
Now you can use that exactly as you did with a software that was installed eg. in conda. 

```bash
# ask for ressources eg
srun --account=nn9305k --cpus-per-task=1 --mem=4G --qos=devel --time=0:30:00 --pty bash -i
# create a command to work 
IMG="/cluster/work/users/<username>/images/qiime_amplicon.sif"
# this starts the container you will see change of prompt then
apptainer shell $IMG
qiime --h

# do you analyses
...

# exit the container 
exit 

# to chancel your job 
scancel <jobid> 
```

<!--
 Command to reproduce with apptainer 
docker run -t -i -v $(pwd):/data quay.io/qiime2/amplicon:2024.5 qiime
-v bind mount a volume
-t allocate pseudo-TTY
-i interactive (keep STDIN open even if not attached) 

> should work ok in interactive then

Possible options to add -f --compat for fakeroot and docker compatibility if problem
-->

If you need to run a slurm script, aqua not interactive work,
you need to create as script that contains the instuctions for the 
software and then run this script, eg where the command is the command to run your script.

```bash
apptainer exec qiime_amplicon.sif <command> 
# the command can be a script execution for quiime eg bash my_quiime_script.sh
```

Let us know if you have some challenges, so we can help you find solutions.
