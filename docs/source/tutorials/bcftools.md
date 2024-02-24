# Using [bcftools] : Utilities for variant calling and VCF (variant call format) files manipulation

## Introduction
### Background

There many variant callers, and the variants discovered differe to a certain extend between tools.

This is due to several reasons:

- data preparation:
  - sequencing depth / coverage and quality
  - accuracy and or variation during read mapping (eg. different algorithms can lead to different optimal mapping) to a reference genome[^1]
- different algorithms and criteria during variant calling and filtering

> You can read about some studies highlighting those variant calling differences. Some and non exhaustive examples:
[Hwang etal. 2015](https://www.nature.com/articles/srep17875),
[Barbitoff etal. 2022](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-022-08365-3),
[Andreu-Sánchez etal. 2021 ](https://www.frontiersin.org/journals/genetics/articles/10.3389/fgene.2021.648229/full),
with recommendation for metagenomic samples,
[Mey Seah etal. 2023](https://journals.asm.org/doi/10.1128/jcm.01842-22) and
[Olson etal 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4493402/) for microbial genomics.

Taking control of the variant calling process and how variant files are manipulated can give you control of the whole process.
It can help understanding what complex tools and pipelines actually do.
Here we will give some examples on how you can do so with bcftools.

[bcftools] utilities are assotiated or part of the [Samtools] suite of programs.
Bcftools is for example used in [Snippy] the variant calling and core genome alignment sowftware that is implemented in [ALPPACA] pipeline[^2].
Snippy do not use bcftools for variant calling[^3], but it uses it for several purposes:
filtering variants, creating consensus, converting, compressing and indexing variant files.
Bbcftools offers a variety of commands/modules to manipulate VCF files. Combining those in different way,
can help you extract the information you want from your data.

### Examples of what can you do with [bcftools] and [Samtools]

Some examples of what you can do:

- Call a consensus (using a reference and a variant call)
- Variant calling, which includes:
  - choosing between two different variant calling models : `consensus-caller` model and `multiallelic-caller` model
  - determining the likelihood of a genotype (understand of a variant in the pipelup), accounting for base and mapping quality
  - filtering variants by different criteria (eg. only calling variants that have been found in 90% of the reads of the pileup)
- Calculating copy number variation (CNV) calling
- Help ensure quality of your dataset by providing utilities to help detect sample concordance, swaps and contamination
- Detect the number of chromosomal copies (polysomy)

It can also work working with haplotypes, determine consequences of variations (eg Frame shift), and in association to specific mutation annotations (eg. as with [SnpEff]) as done in [Snippy] it can eg. help determine the effects of a mutation.

It also provide some utilities for files manipulations, and can provide statistics on variant calls, and much more. It seems eg. that it can help determining Hq variants by allowing to extract variants congruent between different variants callers (intersection).

### Ressources

There are many ressources online:

- [BCFtools HowTo, with manual](https://samtools.github.io/bcftools/howtos/index.html) and more generally [Samtools] website. A list of articles describing the algorithms and usages of [bcftools] is provided in [Samtools documentation](https://www.htslib.org/doc/#manual-pages).
- Videos on YouTube, eg. from [Bioinformatics coach channel](https://www.youtube.com/@bioinformaticscoach) <!-- TODO not sure how good they are-->

Other tutorials that are relevant:
- [Galaxy training - Microbial variant calling] with [Snippy]. **We will use the data provided in this tutorial for the exercises.** 
- [Galaxy training - Mapping]

You might have to consult the description of file formats. The standards for the following file formats can be found [here](https://samtools.github.io/hts-specs/).

- Sequence Alignment Map format (SAM)
- Variant Call Format (VCF)
- The BED format <!-- TODO is this the one used to define the intervals to analyze ? -->

### Terms to become familar with
<!-- TODO Some need to be understood more-->

Computer Science:

- Binary VS non-binary variant files: BCF VS VCF
- Working on stream
- Indexing (def). 
> Note that Some operations only work on indexed files in [bctools], and Working with multiple VCF files will require indexing.
- Standard vs non standard indexes <!-- what is that -->

bcftools / variant manipulation: 
- line interesections <!-- not sure what they mean : "when performing line intersections, the desire"  -> what does that mean ? is it the comlumn pipleup at the variant? --> 
- phasing
- mapQ : The mapping quality - as provided by samtools
- BAQ: per base alignment quality

Be-aware that different aligners used mapping quality in a different way.
You can read more in [this blog](https://medium.com/@shilparaopradeep/samtools-guide-learning-how-to-filter-and-manipulate-with-sam-bam-files-2c28b25d29e8) and [here](https://www.acgt.me/blog/2014/12/16/understanding-mapq-scores-in-sam-files-does-37-42). If you use other mapping tools, you might consider how the mapQ is used to adjust parameters accordingly.
<!-- need to check if is used then in the likelihood computation - which I guess so - which means different
mapper then risk of need of adjusting quality for likelihood -->


---

## Tutorial

### Getting the data used for this tutorial

We followed the [Galaxy training - Microbial variant calling] tutorial to provide a working data set you can exercise with. (Feel free to do the same).

In this tutorial we will recreate some steps used by [Snippy] to lear how [bcftools] works.
This will also help us to better understand [Snippy]. We will eg. compare files generated with
bcftools manually to what was generated during the [Snippy] run on galaxy.

<!-- Note: I have checked: 
We get the same variant positions as presented in the tutorial.
We can eventually include the jbrowse / IGV if people want to look

Dataset : Staphylococcus aureus
1. wildtype.fna : the reference assembly. One contigs - complete , with annotations
2. mutant sample : reads mutant_R1.fastq and mutant_R2.fastq - PE - illumina
-->
The raw data and results from galaxy processing with [Snippy] for this tutorial is available in SAGA in:
`/cluster/projects/nn9305k/tutorial/20240226_bcftools/data`

And as you can see, we recovered the same variant position as in the galaxy tutorial:
![This is reproducible !](./bcftools_image1.png)

### Getting started with [bcftools] in SAGA

Activate conda.
> If you havent had set conda, please look at
[Thomas tutorial](https://nvi-documentation.readthedocs.io/en/latest/tools/setting_up_conda.html)

```bash
minconda 
conda activate bcftools

bcftools --help # Version: 1.19 (using htslib 1.19.1)
# OR 
bcftools --version
```

> Note: The most recent version of VCF tools is not compatible with `samtools <= 0.1.19`.
If you need to use VCF file produced by samtool prior to this version, you will need
to convert to a more recent format.

To see the list of commands and plugins (aka what you can do with bcf tools do:)
```bash
# give you the list of default commands
bcftools 
# give you an overview of additional pluggins
bcftools plugin -lv
```

---
### Note on bcftools usage

It is possible that you will have to compress input files to be able to work with [bcftools].
We will see an example on how to do so during consensus calling.

---

### Variant calling

Variant calling requires **two phases**:
- The first step is the **computation of the likelihood of each genotype** (understand the likelihood that each detected variant is true). This is done with `bcftools mpileup`.
- The seconda step is the actuall **call of variants**. This is done with `bcftools call`.

To start the process of variant calling, you need an indexed reference.
If the reference is not indexed yet, you need to create an index. This is done with samtools:
```bash
samtools faidx ../wildtype.fna
```

<!-- Ploidy encoding : we need to understand that correctly
? 0 : expected all sites no polymorphism, 1: expected haploid, 2: expected diploid.
There are options for poliploids. And how to use the ploidy eg. according to detection minor/major variants? 
Is it actually not organism ploidy per see but what is expected during the process of calling (so can be major/minor variants, ...)

https://www.biostars.org/p/318151/
Coding with respect to dosage of the minor allele e.g. If we have a biallelic loci with T and G, and say G is a minor allele, the 3 possible genotypes will be coded as follows: TT=0, TG=1, and GG = 2

Ok in VCF file: maximum ploidy among all samples that are used ! (so not of the organism)
 
DOES THIS MAKE SENSE - what are we actually calling ? See p26 of VCF  - Genotype in the field 
The organism we have is haploid. But the ploidy employed in VCF format is what is expected from variant calls. 
0: Expectation of unique bases in pileup. (Haploid, no variants - eg exact clones)
1: Expectation of one majoritary base in pileup (and 1 minor) eg. (Haploid) - for calling minor variants eg. (because you have a population of organisms) 
2: Expectation of two majority bases in pileup (eg. diploid or eg. admixture of population of haploid organisms)
different encoding ... and expectations
-->

Options used for computation of likelihood:
```bash
bcftools mplieup
```

-  **mplieup** options you might want to consider adjusting:
  - `-d OR --max-depth <YourDepth>` Per default the maximum depth of the pileup that is used is 250. If you have sequenced deeper, adjust this.
  - `-q, --min-MQ INT` which allows to filter out alignments that have a quality (mapQ) lower than a certain threshold
  - `-Q, --min-BQ INT` to avoid accounting for bases with a quality lower than a certain threshold
  - `-r` list OR `-R` file to select certain regions of the reference only. This can eg. allow you
to avoid calling variants in repeated regions.  <!-- TODO need eventually to find out how>
   - There are options to ajust how the likelihood is computed (we keep those for now as we have little experience, per default Illumina profile is activated)
   - `--seed INT` allows you to set the seed - this can be usefull if you try to adjust options
- Options used here:
  - `Ou`output type uncompressed
  - `f`format of fields ==(not sure what that does here)== <!-- TODO find out-->

> Note that at this stage the likelihood is computed assuming a diploid organism. You will have to adjust
the filter to organism ploidy during variant calling.

Options used for variant calling:
```bash
bcftools call
```

- **call** options you might want to consider: 
  - `-r` list OR `-R` file to select certain regions of the reference only
  - options to adjust usage of the likelihood that was calculated
- used here:
  - `Oz`output type compressed (bgzf) or `Ou`output uncompressed
  - `o`output file
  - `-c` OR `-m` is the variant calling model chosen (pick one).
  - `v`output variant sites only (this is what is a VCF)
  - `--ploidy` <!-- TODO understand that better -->

Note:
> `-c` _consensus-caller_ model is the originally developped model, whiile
`-m`, the _multiallelic-caller_ which is used for rare-variants and multiallelic calling is NOW the default method used in bcfools.


Tutorial:
```bash
cd tutorial
# bcftools mpileup -Ou -f <ref.fasta> <mapped_reads.bam>  "the result of this is piped to:"
# bcftools call -mv -Oz -o <called_variants>.vcf.gz
bcftools mpileup -Ou --max-depth 500  -Q, -f ../wildtype.fna ../galaxy_snippy/snippy.bam  -Ou -o result_mpileup.vcf
bcftools call -mv -Ou --ploidy 1 -o calls.vcf result_mpileup.vcf

# OR in one command 
bcftools mpileup -Ou -f ../wildtype.fna ../galaxy_snippy/snippy.bam | bcftools call -v -Oz --ploidy 1 -co calls.vcf.gz


bcftools index calls.vcf.gz

```
There are several other options that might be of use to adjust the way you call variants. 
To view the detailed list of available options do: 



### Inspiration: what does [Snippy] uses [bcftools] for ?

You can have a look at the commands used by [Snippy] 
in the tutorial data`/cluster/projects/nn9305k/tutorial/20240226_bcftools/` subfolder `data/galaxy_snippy/snippy.txt` file.

#### Consensus calling (OK ! )

[Snippy] command[^5] to create the consensus sequence
`bcftools consensus --sample mutant_R1_fastq -f reference/ref.fa -o snps.consensus.fa snps.vcf.gz` 

`-o`is the consensus output file - fasta format
`snps.vcf.gz` is the compressed form of the final and annotated variant file 
`ref.fa`is the wild strain reference (it only differs by encoding in upper vs lower cases)
`mutant_R1_fastq` is the sample name

<!-- ? can we try to recreate -> yes working ! --> 


1. We create a copy of the VCF file and reference in a separate directory to be able to follow our work
2. We need to index and compress the VCF file (bgzip). The compressing utility is installed with samtools/bctools
3. We recreate the consensus
4. We compare our consensus to Snippy consensus.
Because in theory those sequences should be identical (obtained by the same means, but it might have been some headers reformating), we can concatenate "our" consensus and Snippy consensus and pretend this is an alingnment and detect SNPs differences between those consensus files. 

Note: We used the final files provided by galaxy to demonstrate equivalence

```bash
# 1. Create dir, copy of the VCF and ref files
cd data && mkdir consensus && cd consensus 
cp ../galaxy_snippy/snippy.vcf .
cp ../wildtype.fna .
ls

# 2. Compress and index the VCF file 
conda activate bcftools
bgzip -ik snippy.vcf 
# there is a problem with the index that is given as output (bug) we fix that 
bcftools index snippy.vcf.gz
# Creating the consensus
bcftools consensus --samples mutant_R1_fastq --fasta-ref wildtype.fna --output our_consensus.fasta snippy.vcf.gz
conda deactivate

# Comparison to snippy consensus
# In theory those sequences should be identical (except maybe header) 
# create a dummy alignment
cd ..
cat galaxy_snippy/snippy_consensus.fasta consensus/our_consensus.fasta > dummy.fasta

conda activate snp-dists
snp-dists -a dummy.fasta # no differences were detected
conda deactivate
```
### Variant filtering

Ex. why 
![3 variants, one filtered out because lack depth](./bcftools_image2.png)

Ex. variant filtering and final results differences
- unfiltered variant call from [Snippy] (called with FreeBayes) in the tutorial data subfolder
`data/galaxy_snippy/snippy_zip/mutant_R1_fastq/snps.raw.vcf` vs the filtered variant call `data/galaxy_snippy/snippy_zip/mutant_R1_fastq/snps.filt.vcf`
and the final annotated variant[^4] are provided in `data/galaxy_snippy/snippy_zip/mutant_R1_fastq/snps.vcf`. 

> Note: This last one is equivalent to the one exported as `data/galaxy_snippy/snippy.vcf`. 
> You can test this using `diff file1 file 2`

[^4]: Filtered variants are annotated with [SnpEff]
<!--  -->
The filtering command used in [Snippy] is: 
`bcftools view --include 'FMT/GT="1/1" && QUAL>=100.0 && FMT/DP>=10 && (FMT/AO)/(FMT/DP)>=0.9' snps.raw.vcf  | vt normalize -r reference/ref.fa - | bcftools annotate --remove '^INFO/TYPE,^INFO/DP,^INFO/RO,^INFO/AO,^INFO/AB,^FORMAT/GT,^FORMAT/DP,^FORMAT/RO,^FORMAT/AO,^FORMAT/QR,^FORMAT/QA,^FORMAT/GL' > snps.filt.vcf`

Can we reproduce this type of filtering? 
? detail understanding of what is the filtereding 

Lets decompose : 1 -> 2 -> 3 -> 4
1. `bcftools view --include 'FMT/GT="1/1" && QUAL>=100.0 && FMT/DP>=10 && (FMT/AO)/(FMT/DP)>=0.9' snps.raw.vcf`
2. `vt normalize -r reference/ref.fa - `
3. `bcftools annotate --remove '^INFO/TYPE,^INFO/DP,^INFO/RO,^INFO/AO,^INFO/AB,^FORMAT/GT,^FORMAT/DP,^FORMAT/RO,^FORMAT/AO,^FORMAT/QR,^FORMAT/QA,^FORMAT/GL'`
4. write filtered variants to file: `> snps.filt.vcf`

```bash
# ? can we reproduced this result
--- test 

# check difference between filtered variant file and our results 
#? will that work here ?
diff <our result> snps.filt.vcf
```

# END

<!-- Footnotes -->

[^1]: Mapping to an external or assembly from the reads you are using to call variants

[^2]: Analysis with [Snippy] is implemented in the _mapping analysis_ track in [ALPPACA]

[^3]: [Snippy] uses [FreeBayes](https://github.com/freebayes/freebayes) as variant caller.

[^5]: probably earlier version of [bcftools]

<!--References -->

[Galaxy training - Mapping]:https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/mapping/tutorial.html

[Galaxy training - Microbial variant calling]:https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/microbial-variants/tutorial.html

[bcftools]:https://samtools.github.io/bcftools/bcftools.html

[Samtools]:https://www.htslib.org/

[Snippy]: https://github.com/tseemann/snippy

[ALPPACA]:https://github.com/NorwegianVeterinaryInstitute/ALPPACA/tree/master

[FreeBayes]:https://github.com/freebayes/freebayes

[SnpEff]:https://pcingola.github.io/SnpEff/

<!-- Coordinates of interest in our dataset:

29,439..29,515 The one in tutorial
47299 - other one suggested in tutorial
102660 - a low zone coverage that needs to be filtered out 
102968 - 103048 - zone with 2 variants  
[138877-138954] - 2 variants + 1 filtered out because not enough coverage
103378 deletion , 114540
106602 variant - 109833, 129881
106722 insertion

There is a whole section missing the mutated before 165204 
Not sure what we could use for the gene coverage ... maybe we need another example or just gamble

Possible region with duplicates : 36434 - 38778
-->

<!-- 
https://www.htslib.org/doc/samtools.html 

How a guide here ! https://medium.com/@shilparaopradeep/samtools-guide-learning-how-to-filter-and-manipulate-with-sam-bam-files-2c28b25d29e8

Fix index possibilities
 zcat snippy.vcf.gz | bgzip -c > snippy.new.vcf.gz && tabix snippy.new.vcf.gz
--> 


>