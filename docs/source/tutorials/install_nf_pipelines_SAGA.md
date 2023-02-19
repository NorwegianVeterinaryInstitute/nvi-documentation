# Install nextflow pipelines on SAGA and configure
> Example with [bioanalyzer](https://github.com/avantonder/bovisanalyzer) which is a nextflow pipeline build AS an`nf-core`pipeline (but not directly downloadable from nf-core).
> See also : [Nextflow documentation](https://www.nextflow.io/docs/latest/index.html), documentation to run [nf-core pipelines](https://nf-co.re/usage/installation)  and the [nf-core tools documentation](https://nf-co.re/tools/): for tools installed in `nf-core` conda environment


## 1.  Check requirements : which nextflow  version
- [ ] download appropriate version if not have - and put in here: 

```bash
cd /cluster/projects/nn9305k/bin
wget <nextflow_link>
module load Java/17.0.2 
nextflow --version 
```

- [ ] rename with the  appropriate version number - as done with other version

Note: If it is a `nf-core` pipeline there is a `nf-core`conda environment that contains java and nf-core that allow to download appropriate nextflow (& pipeline)
```bash
cd /cluster/projects/nn9305k/bin
conda activate nf-core 
```


## 2. Download all other requirements (eg. databases dependencies)

- [ ] Download databases we need if they are not already existing

Check here:
```bash
/cluster/shared/biobases
/cluster/shared/vetinst
/cluster/projects/nn9305k/db_flatfiles
```

If not in - choose the most appropriate location 

Note:  `nf-core` pipelines do not necessarily need to be installed, they can be run directly from github. 
But then you will always use the last version, without it be reported to you, **unless** you specifically requiire github to use a specific version of the pipeline.
Take this in account for reproducibility of your workflow. 

We decided to avoid this risk and then installed the pipeline

## 3. Installing the pipeline 
- [ ] Where to install? 

```bash
# For External pipelines
/cluster/projects/nn9305k/src
# For NVI developped pipelines
/cluster/projects/nn9305k/vi_src
```

- [ ]  If using its a `nf-core` pipeline we can do that in conda, instructions to use the tool [here](https://nf-co.re/tools/)

```bash
cd <install_directory>
conda activate nf-core
nf-core list
nf-core download <pipeline>
```

- [ ] If NOT `nf-core` pipeline we download the pipeline: 


```bash
cd <install_directory>
git clone <http_adress> 
```

## Configuration of the pipeline to work on SAGA
> NB: Described according to state of the art to make configuration files, but depending on the maturity of the pipeline, you might also need to explore all configuration files.
 
### Explore how the pipeline has bee build
- [ ] go to new pipeline directory
- [ ] **Look at the `nextflow.config` file**.  It helps understand the parameters, which version it require and how it runs.  Look at the `max_memory` defined, `max_cpus`and `max_time`. This helps understand parameters, which version are required and how it runs
	- [ ] `max_`parameters are usually adjusted to run the pipeline installation test github pages (they are adjusted usually to avoid being billed $ by github). It might also provide an indication of running requirements.
	- [ ] `env` settings - are important for when using containers, including containers that run `R`, contain eg. the definition of profiles paths that will allow interacting with other profiles
	- [ ] `manifest` is important, it allow the pipeline to be run directly from github  (automatically fetch the specific version)

- [ ] Look at the specific base configuration: `dir/conf/base.config`. This is the default configuration that is run (aka `local`: everything specified as additional configuration, provided by `-c`will overwrite this base configuration ).

- [ ] Look at the `dir/conf/modules.config`. This is where the output of the processes are configured. This eg. allow some same modules to be used to do for different things (eg. by providing different arguments)- 


### Adapt the base configuration file to run with SAGA
- [ ] Copy the base configuration to folder `/cluster/projects/nn9305k/nextflow/configs` 
- [ ] rename this base config (see other examples in the directory) so eg. it can avoid to reconfigurate everything after any update of the pipeline. 

```bash
cp /cluster/projects/nn9305k/src/bovisanalyzer/conf/base.config  /cluster/projects/nn9305k/nextflow/configs/saga_bovisanalyser.config
cd /cluster/projects/nn9305k/nextflow/configs
```

- [ ] Scan the new base configuration for `check_max`  function. There are currently some issues on SAGA (that we do not know how to fix yet) so we usually do not use it. You can eg. ask Håkon if this has been fixed. 
NB: The `check_max` time, memory, cpu : make that the SLUM processes will be killed if the resources asked exceed to run the pipeline exceed  those mask (defensive programming towards resource usage). 

- [ ] scan for other parameters defined to understand how the pipeline is built: 
	- ex: in BovisAnalyzer is there is a `test` run profile, with the test parameters that is already installed in the pipeline, and they have a test profile for development

>Note : there are 2 ways to supply `nf-core` type pipelines profiles
>- either some parameters/arguments are defined in the `process` run : how to run the same module in different ways -> see `modules.config 
>	- look f.eks. in the BCFTOOLS filter module : there is a way to specify when the pipelines uses conda and when the pipeline uses containers what it does

- [ ] note database paths, version of the pipeline

- [ ] Modify the copied base.config  to run on SAGA and define SLURM as executor 
	- [ ] check the requirements for : cpus, memory, time (note that in nf-core type pipelines, those labels are standardized)
	- [ ] remove the `check_max` function statements
	- [ ] add `slurm`as executor 
	- [ ] add the clusterOptions:  `'--job-name=Saga_nxf --account=nn9305k'`
	- [ ] specify the `queueSize`  `24`. The queue size defines how many jobs are sent to the queue at the same time (when one jobs is removed from the queue it sends a new one, so there are always eg. 24 jobs requested)

```
# Edit the base condif (nano, vim)
nano saga_bovisanalyser.config
```


Note: if we want to run a pipeline locally on saga (without slurm), then its only to run the default base.config. But then we have to load java and run the pipeline in a `srun`

Note: should you have high requirement for memory: look at examples in the `saga_ampliseq.config`, there are examples how to ask for more that 150GB mem and then how to specify that the specific processes should be run on `big mem`nodes (to do that we need to overwrite the normal cluster options). 

### Test run the pipeline

- [ ] We use singularity containers in SAGA. Check if you have the default path defined for singularity images in your `~/.bashrc`
	- [ ] If not add the line to your .bashrc : `export SINGULARITY_CACHEDIR="$USERWORK/images"`.  NB: Here are additional nextflow [environment variables](https://www.nextflow.io/docs/latest/config.html#environment-variables) you can use in .bashrc eg. I now added `export NXF_WORK="$USERWORK/NF_WORK"` and `export NXF_HOME="$USERWORK/NF_HOME"` so that everything goes on $USERWORK. Remember to source .bashrc 
- [ ] look at the test config (it provides an example of what memory does - this can be configured/used on github actions, it is possible that some modules wont run in the test eg. because they would require too much memory to be run on github actions, without being billed ...)
	- [ ] First a test run local (could give some cpu memory problems - eg 1cpu used instead of 2 cpus required (so this can be adapted in the test profile requirements), short, just to see if can run)
	- [ ] Test run using the config adapted to run on SAGA with SLURM as exectutor
		- [ ] check the queue
		- [ ] check that the pipeline is running



```
# test run command should look somehow like that
# <path/required_nf_version> run <pipeline/main.nf>  -profile test,singularity -c path/config/saga/my_base.config --out_dir <mytestdir>

# Example with BovisAnalyzer
NF_EXECUTABLE="/cluster/projects/nn9305k/bin/nextflow_22.10.6"
PIPELINE_MAIN="/cluster/projects/nn9305k/src/bovisanalyzer/main.nf"
BASE_CONFIG="/cluster/projects/nn9305k/nextflow/configs/saga_bovisanalyser.config"
OUTDIR="$USERWORK/test"

# Test Run - Local 
srun --account=nn9305k --mem-per-cpu=4G --qos=devel --time=0:30:00 --pty bash -i 
module load Java/17.0.2
$NF_EXECUTABLE run $PIPELINE_MAIN -profile test,singularity --out_dir $OUTDIR

# Test Run SLURM executor
module load Java/17.0.2
## I had the release as it seems otherwise this fails
$NF_EXECUTABLE run $PIPELINE_MAIN -profile test,singularity -c $BASE_CONFIG --out_dir $OUTDIR
```

Note: nextflow will predownload the images based on local profile, only the actual job is sent to SLURM, so not a requirement of pre-downloading because of lack of access to the net on compute nodes. 

NB:  `srun` still have access to internet, but not when running in SLURM

