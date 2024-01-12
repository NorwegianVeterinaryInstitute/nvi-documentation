# SAGA drive organization

We have access to several drive locations on SAGA and NIRD. This document
explains which areas we have access to and how to use them. SAGA is used
for compute (calculations) and NIRD for long term storage. 

## A note on backup

As of fall 2021, there is a 6 months snapshot (a kind of backup) function on SAGA. This means that
the disk areas on SAGA are backed up to NIRD. Please note that any changes after
6 months cannot be recovered.

## NIRD and SAGA

It is important to remember that NIRD and SAGA are two different computers.
However, in every day use, we only access SAGA as such. The drives that
are on the NIRD server are available on SAGA. However, since they are not
actually on SAGA, we should **use rsync and not copy/move when shifting data**
**to and from NIRD.**

## Drive locations on SAGA and NIRD: overview

These are different locations that we work with in regarding SAGA and NIRD.
Users normally get access to those two areas.  (Contact: Karin Lagesen).

- `$USER` is your username 
- See also [SIGMA2 Documentation: Storage areas on HPC clusters](https://documentation.sigma2.no/files_storage/clusters.html)

| SAGA |  | NIRD |
| ---- | ---- | ---- |
| Compute |  | Storage |
| saga.sigma2.no |  | login.nird.sigma2.no |
| /cluster/project/nn9305K | Main NVI Bioinfo project area | /project/NS9305K |

Overview: 

| SAGA                                              | Usage                                                                                                                           | Backed-up | Access         | Comments                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------- | -------------- | ------------------------------------------------------------------------------------------- |
| **main organization**                             |                                                                                                                                 |           |                |                                                                                             |
| /cluster/home/$USER                               | Your login area: where you end up after login                                                                                   | Yes       | All            | **Do not store any NVI data here!**                                                         |
| /cluster/projects/nn9305k                         | Main Bioinformatic location and working area                                                                                    | yes       | All            |                                                                                             |
| /nird/projects/NS9305K                            | Mount point for NIRD NNVI project (= NIRD)                                                                                      | _NA_      | All            | Long term storage                                                                           |
| /cluster/work/users/$USER                         | Analysis area                                                                                                                   | no        | All            | Data is removed after carence time if spaces is limited. = `$USERWORK`                      |
|                                                   |                                                                                                                                 |           |                |                                                                                             |
| **Subfolders organization**                       |                                                                                                                                 |           |                |                                                                                             |
| /cluster/projects/nn9305k/active/$USER_OR_project | Your **active** working area, shared active projects                                                                            | yes       | All            | Where all your work happens                                                                 |
| /cluster/projects/nn9305k/db_flatfiles            | various references files                                                                                                        | yes       | All            | eg. ariba MLST, AMR, and virulence db; chewbbaca schemes;  adapter files for trimmomatic.   |
| /cluster/projects/nn9305k/genome_references       | Reference genome data and databases created by reference genomes                                                                                                                                 | yes       | all            | (eg. annotations db) |
| /cluster/projects/nn9305k/samplefiles             | Template files and procedures for new users                                                                                                                                | yes       | all            | (eg.  slurm scripts, bashrc and bash_profile)   |
| **Other NVI Projects**                            |                                                                                                                                 |           |                |                                                                                             |
| /cluster/projects/nn9305k/nn10070k                | NVI Metagenomics project area                                                                                                   | yes          | specific users | Contact : Håkon Kaspersen. See specific documentation for usage                                                                  |
|                                                   |                                                                                                                                 |           |                |                                                                                             |
| **Shared ressources on SAGA**                     |                                                                                                                                 |           |                |                                                                                             |
| /cluster/shared                                   | Location for shared/common databases                                                                                            | no        |                |                                                                                             |
| /cluster/shared/vetinst                           | Location for NVI shared **active** raw data*                                                                                    | no        | all            |                                                                                             |
| /cluster/shared/biobases                          | Location for shared classification databases (eg. kraken)                                                                       | no        | all            | Contact : Thomas Haverkamp                                                                  |
| /cluster/shared/databases                         | Location for shared databases (eg. blast)                                                                                       | no        | all            | Contact : Thomas Haverkamp                                                                  |
| Detail : datasets on **/cluster/shared/vetinst**  | `datasets` - **copies** of currently **active = worked on** datasets. Divided into `wgs`, `transcriptomics` and `metagenomics`. | no        | all            | Ensure that raw data is stored on NIRD. **Can be deleted if space is required !**           |
|                                                   |                                                                                                                                 |           |                |                                                                                             |
|                                                   |                                                                                                                                 |           |                |                                                                                             |
Special explanations 
- Shared folders usually contain subfolders and REAME file describing what the databases contains. Read those. 
- `/cluster/shared/vetinst` - Main raw data directory. All data used for calculations should be stored there. Unpack and softlinked the data in to your active directories to work on them. Compressed archives should be removed.  This folder is **not backed up**,   therefore you must ensure that all datasets are first deposited on NIRD for safe long-term storage and then copied from NIRD to this folder.
- `/cluster/work/users/$USERNAME` - A folder that should be used for analyses, ie. if analyses create many temporary files. It is used as such in eg. BIFROST and ALPPACA pipelines. There is **no backup** of this folder, files are automatically deleted after 21-42 days. Remember copying your results to your dedicated results folder if you use this area.

| NIRD | Usage | Backed-up | Access | Commments |
| ---- | ---- | ---- | ---- | ---- |
| projects/NS9305K | Long term storage | yes | all | Main Bioinformatic storage area. NVI research projects row data and storage of analysis |
| projects/NS9305K/datasets | Long term storage datasets | yes | all | Archives of row datasets. Sub-directories  `wgs`, `transcriptomics` and `metagenomics`. |
| /projects/NS9305K/home/$USER<br> | Long term storage | yes | all | Directory where you can archive eg. results of analyses.  |
| /projects/NS9305K/home/<nb_project> | Long term storage | yes | all | Long term storage for results |

## Usage and activities

### Long time storage of raw data

All long term storage of raw data should happen on NIRD, where there is
backup. In the NIRD folder, there is a datasets folder, same as in SAGA.
Sequencing done at NVI will put the data there. Make sure you use rsync to
get the data from the NIRD drives to the drives on SAGA.

If your data is not sequenced at NVI, you need to ensure yourself that your raw
data is on NIRD.

### Long time storing of results

All long time storage should happen on the NIRD drives. There are two ways of
doing long time storage there:
- `Shared project` (in /projects/NS9305K) if you are working with someone else on this project, create
  a top level directory on the NIRD drive, and keep the results there.
- `Personal project`  (in /projects/NS9305K/home/$USER). If you are not collaborating with someone on the project,   put it in your NIRD home folder.

### Data analysis 

All active analyses should happen in the active folder on SAGA. This allows
us to monitor the data usage on the server. For analyses, we suggest the
following way of organizing things.

- Create a folder inside your active folder, with this format:
  `year_projectname`
- Inside this folder, create a `README file` that explains to you and anybody
  else what this project is about
- Then, create the following folders:
  - `rawdata` - softlinklink in your dataset here
  - `scripts` - any scripts, including slurm scripts go here
- You might have several subtasks that you will do. Create a separate directory
  for each of them, using the format year_month_purpose (for instance
  2020_09_phylogeny). Remembering when things were done is usually easier than
  many other things. You should still have a README file for each subdirectory.
- We also suggest you keep a log file in text format with your analysis. In this
  file we recommend that you not only write down what you did, but why you did
  it. The why is usually the most difficult to remember.
