# Using [bcftools] : Utilities for variant calling and VCF (variant call format) files manipulation

## Background

There many variant callers, and the variants discovered differe to a certain extend between tools. 

This is due to several reasons: 
- data preparation:
  - sequencing depth / coverage and quality
  - accuracy and or variation during read mapping (eg. different algorithms can lead to different optimal mapping) to a reference genome[^1]
- different algorithms and criteria during variant calling and filtering

> You can read about some studies highlighting those variant calling differences. Some and non exhaustive examples:
> [Hwang etal. 2015](https://www.nature.com/articles/srep17875)
> [Barbitoff etal. 2022](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-022-08365-3) 
> [Andreu-Sánchez etal. 2021 ](https://www.frontiersin.org/journals/genetics/articles/10.3389/fgene.2021.648229/full) with recommendation for metagenomic samples 
> [Mey Seah etal. 2023](https://journals.asm.org/doi/10.1128/jcm.01842-22) and [Olson etal 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4493402/) for microbial genomics.


Taking control of the variant calling process and how variant files are manipulated can give you control of the whole process.
Here we will give some examples on how you can do so with bcftools. 
I can also help understand what the pipelines (or tools) using bcftools actually do. 

For example, [Snippy] the variant calling and core genome alignment sowftware that is implemented in [ALPPACA] pipeline[^2] do not use bcftools for variant calling[^3] it uses use it for several purposes: filtering variants, creating consensus, converting / compressing and indexing variant files.


[^1]: Mapping to an external or assembly from the reads you are using to call variants

[^2]: Analysis with [Snippy] is implemented in the _mapping analysis_ track in [ALPPACA]

[^3]: [Snippy] uses [FreeBayes](https://github.com/freebayes/freebayes) as variant caller.

## Examples of what can you do with [bcftools]

Some examples of what you can do:

- filter variants - eg. only calling variants that have been found in 90% of the reads of the pileup
- ?determining HqVariants (congruence between different variants callers) (intersection) ?
- ?stastistical: determining the likelihood of a genotype (application - typing?)
- consensus calling
- calculating coverage for genes (copy nb ?) 
- finding the effects of a mutation ? annotation
- determining haplotyes (eg. working with diploid / polyploid organisms)

- ? modifying headers ?
- ? providing statistics about your Variant files

[bcftools] offers a variety of modules to manipulate VCF files. Combining the different utilities in bcftools and piping them,
can help you extract the information you want from your data.

## Ressources

There are many ressources online:

- [BCFtools HowTo, with manual](https://samtools.github.io/bcftools/howtos/index.html)
- [Videos on YouTube, eg. from Bioinformatics coach channel](https://www.youtube.com/@bioinformaticscoach) (need to check those)

Other tutorials that are relevant: 
- [Galaxy training - Microbial variant calling] (with Snippy). We will use the data provided in this tutorial for the exercises. 
- [Galaxy training - Mapping]


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
We followed the [Galaxy training - Microbial variant calling] tutorial to provide a working data set you can exercise with.
> If you wish to try this tutorial, be welcome, you can train with the data made available and "try Snippy" either in Galaxy or in SAGA. 
> You can use the files generated with bcftools and / or in comparison to results provided by Snippy. 
> If you wish to skip this step, the data is available on SAGA 

<!-- I checked - We get the same variant positions as presented in the tutorial.
We can eventually include the jbrowse
--> 

1. wildtype.fna : the reference assembly. One contigs 
2. mutant sample : reads mutant_R1.fastq and mutant_R2.fastq # @mutant-no_snps.gff

You can find the raw data and data processed with [Snippy] for this tutorial in SAGA in `/cluster/projects/nn9305k/tutorial/20240226_bcftools/data`
And as you can see, we recovered the same variant position as in the galaxy tutorial

![We did it!](./bcftools_image1.png)

```bash
cd galaxy_snippy
grep ">" wildtype.fna 
#>Wildtype
```  


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
- [ ] line interesections `when performing line intersections, the desire` -> what does that mean ? 

```bash
```

## bcftools usage 
- it is possible that you will have to compress input files to be able to work with bctools


<!-- 
The utility is installed in samtools : my trial did not work So starting from scratch
```bash
cd galaxy_snippy
bgzip -r snippy.vcf # snippy.vcf.gzi
bgzip <snippy.vcf > snippy.vcf.gz
```
--> 
### Variant calling `bcftools call`


`bcftools mpileup`: 
- `Ou`output type uncompressed
- `f`format of fields ==(not sure what that does here)== 

`bcftools call`:
- `Oz`output type compressed (bgzf)
- `o`output file
-[ ] `-c` is the variant calling model. Note that `-c` the original calling method:  _consensus-caller_ WHILE the other option is `-m` is the _multiallelic-caller_ used for rare-variants and multiallelic calling
- `v`output variant sites only (this is what is a VCF)
-[ ] `--ploidy` 


<!-- caller modesl 
- `consensus-caller`: `-c`consensus caller - original model calling method
- `multiallelic-caller`: `-m` model multiallelic-caller_ used for rare-variants and multiallelic calling
--> 

<!-- Ploidy encoding : we need to understand that correctly
? 0 : expected all sites no polymorphism, 1: expected haploid, 2: expected diploid.
Its actually not organism ploidy per see but what is expected during the process of calling (so can be major/minor variants, ...)

https://www.biostars.org/p/318151/
Coding with respect to dosage of the minor allele e.g. If we have a biallelic loci with T and G, and say G is a minor allele, the 3 possible genotypes will be coded as follows: TT=0, TG=1, and GG = 2

Ok in VCF file: maximum ploidy among all samples that are used ! (so not of the organism)

--> 
Note:
**Per default bcftools assumes that the data is diploid**.
<!-- 
DOES THIS MAKE SENSE - what are we actually calling ? See p26 of VCF  - Genotype in the field 
The organism we have is haploid. But the ploidy employed in VCF format is what is expected from variant calls. 
0: Expectation of unique bases in pileup. (Haploid, no variants - eg exact clones)
1: Expectation of one majoritary base in pileup (and 1 minor) eg. (Haploid) - for calling minor variants eg. (because you have a population of organisms) 
2: Expectation of two majority bases in pileup (eg. diploid or eg. admixture of population of haploid organisms)
different encoding ... and expectations
-->
```bash
cd tutorial

# bcftools mpileup -Ou -f <ref.fasta> <mapped_reads.bam> 
# bcftools call -mv -Oz -o <called_variants>.vcf.gz
bcftools mpileup -Ou -f ../wildtype.fna ../galaxy_snippy/snippy.bam | bcftools call -v -Oz --ploidy 1 -co calls.vcf.gz
bcftools index calls.vcf.gz

```

### Consensus calling 

```bash
cat ../wildtype.fna | bcftools consensus snippy.vcf.gz > consensus.fa
```

```bash

```



<!-- REFERENCES Repeated-->

# References

[Galaxy training - Mapping]:https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/mapping/tutorial.html

[Galaxy training - Microbial variant calling]:https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/microbial-variants/
tutorial.html

[bcftools]:(https://samtools.github.io/bcftools/bcftools.html

[Snippy]: https://github.com/tseemann/snippy

[ALPPACA]:https://github.com/NorwegianVeterinaryInstitute/ALPPACA/tree/master

[FreeBayes]:https://github.com/freebayes/freebayes

<!-- Coordinates
29,439..29,515 The one in tutorial
47299
--> 