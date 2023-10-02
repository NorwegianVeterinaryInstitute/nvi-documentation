# MetaMicro Documentation (nn10070k)
This page holds the documentation for the nn10070k-project on saga.

## Description
MetaMicro is the project name for nn10070k, a saga project used for the analysis of metagenomic- and amplicon data. 
The project was created due to the increasing number of metagenomics-related research projects at NVI, which requires a much higher storage capacity than comparative genomics. Thus, to alleviate the storage capacity on nn9305k, and to provide a platform for collaboration and reproducibility, nn10070k was created.

Project heads:
- Håkon Kaspersen
- Thomas Haverkamp

Due to the structural difference on nn10070k compared to nn9305k, please read this documentation thoroughly and email us for access to the project.

## Structure
MetaMicro is structured to allow better collaboration across users on the same sub-project. This is mainly achieved by using a project-based structure rather than user-based.
Below follows a detailed description of the directory structure in MetaMicro.

### Adm
This directory holds the scripts, templates, and logs for the projects at nn10070k. Scripts users can use to generate new projects can be found under `~/scripts`.
The remaining directories are only for administrative use.

### Databases
The databases directory holds databases used for the analysis of sequence data, such as smaller Kraken databases, taxonomic databases, and so on.

NOTE: Very large databases should not be placed here, they should be located in `/cluster/shared/databases`.

Note on nomenclature for tools and databases:
All databases and tools need to be tagged with a version number in the directory they are placed. If the database or tool in question do not have a specific version number, use the date of download as the version.

Examples:
- `kraken2_pluspfp_05.06.2023`
- `taxdump_25.09.2023`

### Pipelines
The pipelines directory holds software used for analysis of sequencing data, such as nf-core, nextflow, or other pipelines. This includes in-house pipelines. All pipelines should be under version control with git, and updates to pipelines requires a change in the pipeline directory name, so that the version is clearly visible, and that older versions are still available.
All `nf-core` pipelines should be updated with the `nf-core` tool, available as a conda environment under `src/conda/envs`.

### Projects
The projects directory holds the analyses results for each separate project we are working on. The structure of each directory is as follows:

```
projects
|
|
|-- project_name1
|       |
|       |--sub_project1
|	|	|
|	|	|-- sandbox
|	|	|
|	|	|-- data
|	|	|
|	|	|-- results
|	|	|
|	|	|-- scripts
|	|	|
|	|	|-- README	
|       |
|       |--sub_project2
|
|-- project_name2
        |
        |--sub_project1
```

Where the `project_name1` etc. represents the overall project (such as CIRCLES, turkeybiom, etc), and the `sub_project1` etc. represent analyses done for individual papers within each project.
How you structure your analysis within these sub-projects is up to you, but a README stating the structure and purpose of the project is mandatory, in both the `project_name` and `sub_project` levels.

Here follows a description of the sub-directories within each sub-project.
- `sandbox`: This is a testing directory, holding analyses results from testing pipelines and programs. This directory is deleted before archiving, and should not hold results of importance.
- `data`: This holds symbolic links to raw data used in this project, or subsets thereof. Do not put actual read files or other data here, symbolic links should point to the `nn10070k/rawdata` directory.
- `results`: This holds the main analysis results, and is included when archiving the project. 
- `scripts`: This holds SLURM scripts or other scripts used in the project.

New sub-projects should be generated with the script `generate_subproject.bash` to make sure that it adheres to the structure described above. Please read the "Starting a new project" section for more information.

### Rawdata
All sequencing data used for the projects on nn9305k should be placed here. The reads are temporarily rsynced from NIRD to this directory. After the analyses have completed, the reads in question are deleted from this directory.
NOTE: Only unpacked files are allowed in this directory. After transferring reads from NIRD, unpack the tarball and remove it to save space. Tarballs that have not been unpacked may be subject to immediate deletion if we need space!

Long-term storage of reads and archived projects is done on NIRD, project number NS9305K. Please see "Archiving projects" for more information.

### Src
The src directory is used for conda environments, saving conda yaml files, and for stand-alone tools not installed as a module or with conda.
Subdirectories include `~/bin` for stand-alone tools, and `~/conda` for conda environments and yaml file archives.
To create a new conda environment for a tool, the `create_env.bash` script should be used. First, copy an existing `*.yml` file from the `yaml` directory, rename it to `<tool>_<version>.yml`, and fill in the name of the tool, and the dependencies with `<tool>=<version>`. Then, run the `create_env.bash` script as follows:

```
bash create_env.bash <tool_name>_<version> path/to/yaml/<tool_name>_<version>.yml
```

The second argument will become the name of the conda environment, so make sure to adhere to the `<tool_name>_<version>` structure. The script will make sure that the environment is installed in the correct directory.
To use the conda environments made, use the following recipe:

```
module load Miniconda3/22.11.1-1
source ${EBROOTMINICONDA3}/bin/activate
conda activate <path_to_environment>
```

For this to work, no conda-related things should be in your `.bashrc` file (as we had from the old nn9305k shared conda system).
NOTE: Before installing a tool, please check if it is already available as a module by using `module avail <name>`. If it is not available, check if it is possible to install by using conda by searching on anaconda.org. If it is not available there either, install it in the `bin` directory as instructed by the authors of the tool, but adhere to the `<tool_name>_<version>` structure of the directory name.

## Creating a new project
When creating a new project or sub-project, please follow the steps described below:
1. Run the `create_project.bash` script under `~/adm/scripts` to generate the main project directory (see below)
2. Fill in the `<project_name>/README` with the necessary information
3. Run the `create_subproject.bash` script under `~/adm/scripts` whenever you need to create a sub-project (see below)
4. Fill in the necessary information in the `~/<project_name>/<sub_project_name>/README`
5. Initiate git in the sub-project directory: `git init`
6. Connect the local git directory to the NorwegianVeterinaryInstitute github site
7. Whenever you want to create a new sub-project, run the `create_subproject.bash` script and repeat steps 3-6.

```
bash create_project.bash <project_name>
bash create_subproject.bash <project_name> <sub_project_name>
```

Users may also check the `CHECKLIST` file generated in each sub-project to check if they have done all steps necessary for each sub-project.

## Archiving projects
Whenever a sub-project is complete and need archiving, please follow the steps below to ensure a safe packing and transfer of the files.

1. Delete the `~/sandbox` directory
2. Make sure that any unnecessary intermediate files in `~/results` are deleted. If you are unsure about what to remove, please contact the administrators
3. Make sure that the directory is up to date with git before archiving
4. Pack the entire sub-project directory with `tar -czvf <sub_project_name>.tar.gz <sub_project_name>`
5. Make sure that a project directory exists under the NIRD home area `/nird/projects/NS9305K/<project_name>`
5. Move the tarball to NIRD under the respective project directory: `rsync -rauPW <sub_project_name>.tar.gz /nird/projects/NS9305K/<project_name>`
6. Run the rsync command in step 5 again to make sure that the tarball has been safely transferred
7. Delete the tarball and sub_project directory on Saga

