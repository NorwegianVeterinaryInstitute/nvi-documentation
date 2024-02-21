# Using bcftools : Utilities for variant calling and VCF (variant call format) files manipulation

## Examples of what can you do with bcftools

Some examples of what you can do
- filter variants - eg. only calling variants that have been found in 90% of the reads of the pipelup
- consensus calling
- calculating coverage for genes

bcftools offers many tools to manipulate VCF files. Combining the different utilities in bcftools and piping them,
can help you extract the information you want from your data.

## Background

There many variant callers, and the variants discovered differe to a certain extend between tools. 
This is due to several reasons: 
- data preparation: how reads are mapped to a reference genome (external or assembly from the reads you are using to call variants)
- different algorithms and filtering criteria used during variant calling
- ...

- [ ] some refs papers

Taking control of the variant calling process and how variant files are manipulated can give you control of the whole process.
Here we will give some examples on how you can do so with bcftools. 

## Ressources

There are many ressources online:

- [BCFtools HowTo, with manual](https://samtools.github.io/bcftools/howtos/index.html)
- [Videos on YouTube, eg. from Bioinformatics coach channel](https://www.youtube.com/@bioinformaticscoach) (need to check those)

Other tutorials that are relevant: 
- [Galaxy training - Microbial variant calling (with Snippy)](https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/microbial-variants/tutorial.html). We will use the data provided in this tutorial for the exercises


## bcftools in SAGA 

Activate conda. 
> If you havent had [set conda, please look at Thomas tutorial](https://nvi-documentation.readthedocs.io/en/latest/tools/setting_up_conda.html)

```bash
minconda 
conda activate bcftools

bcftools --help # Version: 1.19 (using htslib 1.19.1)
# bcftools --version
```

## Getting test data 
We get the data provided in the Galaxy tutorial - Microbial variant calling.

_Getting the data example in SAGA_

```bash
cd <folder of your choice> 
```  


# Getting started with VCF tools 

Note: The most recent version of VCF tools is not compatible with `samtools <= 0.1.19`. If you need to use samtool, check which version was used.

## Terms to understand
- [ ] I think we need to define some voc so we can be sure we understand the manual correctly
- Binary VS non-binary variant files: BCF VS VCF
- Working on stream
- Indexing (def). 
> Some operations only work on indexed files. Working on multiple variant files require indexing.

- Standard vs non standard indexes (how to recognize the,)


```bash
```

## Variant calling

- 2 models of variant callling 
- [ ] find out what they do 
- `consensus-caller`
- `multiallelic-caller`