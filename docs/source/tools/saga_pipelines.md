# NVI pipelines on saga

The NVI has developed several pipelines that run on Saga. These pipelines are
the tools that can be used for more large scaled analyses. This page details how
they are used, and gives pointers to where more documentation about them can be
found. 

## Pipeline benefits

All of the pipelines are written in a language that is called
[Nextflow](https://www.nextflow.io/). This is a language that conveys several
benefits to the user:
- several tools can be chained together
- once one tool is done with a dataset the nextflow system hands the data over
  to the next tool that will do something
- the nextflow system keeps track of where files are, and all of their paths
- a specific set of output can be detailed from each tool, meaning that the most
  useful data will be put into a specified folder
- if parts of the analysis has already been done, or the pipeline has been
  changed, the system will only run on what has been changed or that is left to
  do
- it takes care of sending jobs to the saga queueing system

All of this means that the user can run a large set of data through an analysis
pipeline without having to worry about data management or how to set up a
massive set of slurm jobs. 

## Pipeline usage

All of the pipelines at the NVI have a command line that follow the basic format:

```bash
name_of_runscript.sh name_of_nfscript.nf configfile.conf profile output_dir
```
The parts are:

- name_of_runscript.nf: each pipeline has a runscript. This is a bash script where some settings are set. In addition, in most of the pipelines this script also saves the version of the scripts, and the config file in a separate directory
- name_of_nfscript.nf: each pipeline has its own nextflow file. This is the name of that script.
- configfile.conf: each pipeline needs a way of figuring out which reads to run on, which options to give to programs, etc. That is specified in this file. This is in most cases the ONLY file you need to edit to run the pipeline.
- profile: this is an option to the pipeline, which tells the pipeline which file to read to understand which computer settings to use. This is a file that lives inside of the pipeline that the user does not need to worry about.
- output_dir: this is the name of the output directory that the results will be run in.

## How does this system work?

All the pipelines live in this directory:

```bash
/cluster/projects/nn9305k/vi_src
```

Each pipeline has its own runscript. 

When a pipeline is run, the input to the pipeline has to be described in the configfile. All of the pipelines use the same setup for this: this is a directory where all of the raw data is stored. All of the data inside of this directory will be analyzed. The best way of setting this up is by having the raw data stored in the designated raw data directory, and then symlinking it into your analysis directory. This method makes it easy to edit the dataset that is used for an analysis.

All of the pipelines use a location that is called $USERWORK to run the analyses in. This is a special directory that is only available to the user running the analysis. Note: this is a location that does not have backup, and data will be auto-deleted after a set period of time (anytime between 21-45 days after data generation).

## Available pipelines

This is a list of the pipelines that are available:

- [Bifrost](https://github.com/NorwegianVeterinaryInstitute/Bifrost) - whole genome assembly pipeline 
- [Bulk RNA-seq](https://github.com/NorwegianVeterinaryInstitute/RNA-seq) - transctipome analysis pipeline
- [Ellipsis](https://github.com/NorwegianVeterinaryInstitute/Ellipsis) - plasmid assembly and annotation pipeline
- [ALPPACA](https://github.com/NorwegianVeterinaryInstitute/ALPPACA) - phylogeny pipeline
