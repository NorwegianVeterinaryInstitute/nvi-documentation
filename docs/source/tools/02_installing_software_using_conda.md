# Installing software using conda.

The conda system makes it very easy to install software. With the command `conda create -n $ENV_NAME` you can set-up an environment and install any software you like. That can be packages already present at the [anaconda.org](anaconda.org) or any other conda channel. OR, you can install your own software in the bin folder of the environment. 

This however, does not make it reproducible and for us scientists, that means that we do not document clearly what we are installing and how we do it. For software that you install yourself into the bin folder of the environment, it is important that you keep track of how you do it. If not, you might not be able to recreate the exact same environment.

For software that is available on eg. [anaconda.org](anaconda.org), we need to do a little bit more work than just running the create command, but then the installation process is well documented and reproducible.

Here we show how to set-up your own environment. As an example, we will use the tool [dorado](https://github.com/nanoporetech/dorado) and the version we want is the version that uses GPU's. That tool is available for conda installation here: https://anaconda.org/hcc/dorado-gpu. 

The place on Saga where we write the instructions that allow to create the reproducible environment is done in the folder: `/cluster/projects/nn9305k/src/conda_env_creator`.

In that folder you find: a README file with instructions, a bash script called: `create_env.bash` and a folder called: `yaml`.

### Creation of the yml file
The first thing we need to do is create a yml file in the folder yaml.
The yml file contains the necessary instructions to create a reproducible environment. It is a text file with a special formatting (yaml) that contains the set of instructions that conda need to create a reproducible environment. 

Here we create the file: `dorado_gpu_0.4.2.yml`. In that file we indicate the name of the environment. The name of the environment should contain the version of the software you install. In addition we specify the channels that conda will use to find the software and the dependencies required by software. The order in which the channels are mentionned is important. Finally we indicate the software we want to install (and the required dependencies). We must specify which version of the software and which version of dependencies we want to set-up. 

That contains this
```
name: dorado_gpu_0.4.2
channels:
 - hcc
 - conda-forge
 - bioconda
 - defaults
dependencies:
 - dorado-gpu=0.4.2
```

Note that I have a channel called `hcc`, this is the specific channel at anaconda.org where the dorado software was placed. For most software we do not need this channel, but make sure the channel for your software of choice is listed. 

### Installing the software on Saga.

Once we have create the yml file and saved that in the yaml folder, we can then use the bash script `create_env.bash`.

The general command for that is: 

```
bash create_env.bash toolname_version yaml/yaml_file.yml
```

For our example that becomes:

```
bash create_env.bash dorado_gpu_0.4.2 yaml/dorado_gpu_0.4.2.yml
```

This will then install the environment for dorado_gpu_0.4.2 in the folder:
`/cluster/projects/nn9305k/src/miniconda/envs`

Once this bash script has finished, it will print a message on how to activate the environment.

For questions and help you can contact the NVI bioinformatics team on the internal workplace channel.
