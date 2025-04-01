#  Mapping reads against a reference/assembly

## 1. What is read-mapping?

Read mapping is "placing" your reads at a locus with a matching sequence in a longer sequence.
This longer sequence can either be the isolate assembly, or an assembly of a related isolate (eg. a reference isolate for the group of isolate under study).

You could think of read mapping as a simulation of local hybridization of your reads to the genome, at a loci where it fits well enough to hybridize. Usually a few mismatches are allowed (think about the consequences), even if your reads have been "hard-clipped". Hard-clipping means that the ends of the reads are not part of the mapping and that those parts have been removed (or trimmed: eg. adapter removal with Trimmomatic). On the other hand, "Soft-clipping" allows to perform read mapping while ignoring (downweighting) the mismatches present at the endings of reads. This allows to map reads in the presence of adapters.


A portion of the reads may map to several loci in your genome (ie. in repeated regions).

Reads can be mapped as paired or single. If paired is used, then the matching regions are defined by the insert size and the length of each read


![https://commons.wikimedia.org/wiki/File:Mapping_Reads.png](https://upload.wikimedia.org/wikipedia/commons/2/2e/Mapping_Reads.png)


## 2. Reads mapping purposes

Read mapping techniques are used for several purposes.
Some examples:

1) To evaluate the quality of an assembly (or to compare different methods used to assemble your reads). Read mapping can help identifying problematic areas in your assembly process. Indeed, statistics are necessary but might not be sufficient to evaluate the quality of your assembly.

   To assess the quality of an assembly we can eg. to look at:
   - the coverage regularity (ex: some repeated regions might have increased coverage)
   - the coverage at the beginning and end of scaffolds - is it good enough?
   - are there positions where pileup of reads show polymorphism?
   - ...

2) Assembly polishing software such as `pilon` and `reapr` use mapped reads to identify potential positions where the assemblies should be improved. It is good to visualize what information they are using.

Assembly polishing softwares can eg. use variation in coverage, wrong read pairs orientation, discrepancy between expected insert size and actual insert size obtained from read mapping (ie. longer than expected) to improve assembly.

3) To identify SNPs: some methods use reads mapping against a reference genome to identify and type variants/SNPs (e.g. [snippy](https://github.com/tseemann/snippy))

## 3. Practical: mapping reads to the assembly obtained from the same reads.

We have assembled reads using `Bifrost pipeline`. The data for today's practical are found on SAGA in `/cluster/projects/nn9305k/tutorial/20210412_mapping_visualization/mapping_practical`

Description of the files in this folder:
- the raw_reads: `MiSeq_Ecoli_MG1655_50x_R{1,2}.fastq.gz`
- the trimmed-reads: `MiSeq_Ecoli_MG1655_50x_{R1,R2,S}_concat_stripped_trimmed.fq.gz`
- the assembly (polished with Pilon, output during prokka annoation): `MiSeq_Ecoli_MG1655_50x.fna` (fasta format)

Mapping can be done either with raw reads or trimmed reads. If you are interested to see if there are positions with SNPs, it is easier to use trimmed-reads (avoiding the visualization noise due to the presence of adapters)

To map reads, we use the [bwa mem] option from [bwa tools] software.

NB: Convention: means `<change_me>`


_**PRACTICAL EXERCISE:**_

In your `home` directory make a directory for today's work
and a folder called `mapping` where you will **copy** the input files

```bash
cd <home>
mkdir mapping
cd mapping

rsync -rauPW /cluster/projects/nn9305k/tutorial/20210412_mapping_visualization/mapping_practical/* .
```


You can look at the file content e.g. (one reads-file, the assembly file):
```bash
head MiSeq_Ecoli_MG1655_50x.fna
gunzip -cd MiSeq_Ecoli_MG1655_50x_S_concat_stripped_trimmed.fq.gz | head
```

The mapping tool called `bwa tools` which is installed in the **Bifrost pipeline** conda environment.

Here are options for performing the mapping:
1. You can use the raw_reads for mapping
2. You can use the trimmed reads for mapping
3. You can perform 2 mapping rounds with raw_reads and trimmed reads (2 times). This might be interesting if you want to look at the effect of trimming or not on the mapping visually.

For the tutorial, you will chose one solution 1 or 2. (Solution 3 is if you are curious and want to repeat the exercise)

You will ask for interactive ressources in saga and activate the `bifrost` environment. (We use the queue for testing purposes: devel)

```bash
srun --account=nn9305k --mem-per-cpu=4G --cpus-per-task=2 --qos=devel --time=0:30:00 --pty bash -i
conda activate bifrost
```

To map the reads to an assembly/reference we need to:
1) Index the reference: we are in this case using the assembly as reference

```bash
bwa index <reference.fna>
```

2) We map the reads (attribute the position of the reads according to the index) and sort the mapped reads by index position

NB: Here `-` means that the output of the pipe `|` is used as input in samtools. And `\` is used to indicate that your code (instructions) continues on the next line

- for paired reads (PE) or paired trimmed reads:

```bash
bwa mem -t 4 <reference.fna> <in1:read1.fq.gz> <in2:read2.fq.gz> \
| samtools sort -o <out:PE_mapped_sorted.bam> -
```

- If you used trimmed reads (follow below) you also have unpaired reads (called S here) that we can map as such:

```bash
bwa mem -t 4 <reference.fna> <in:S_reads.fq.gz> \
| samtools sort -o <out:S_mapped_sorted.bam> -
```

- You can merge merged the paired trimmed reads and S reads as such:

```bash
samtools merge <out:all_merged.bam> <in1:S_mapped_sorted.bam> <in2:PE_mapped_sorted.bam>
```

- Now we need to be sure merged reads are still sorted by index: we resort

```bash
samtools sort -o <out:final_all_merged.bam> <in:all_merged.bam>
```

**OPTIONAL**

NB: Some software like `Pilon` need an updated index after merging bam files). You do that by running:

```bash
samtools index <final_all_merged.bam>
```

## 4. Understanding the mapping file format: the [sam/bam file format]

`.bam` files are in a compressed binary format. We need to transform the `.bam` (to a `.sam` file) to be able to see how mapped-reads are represented in the file.

Also have a look at [Samtools article] and at [Samtools manual]


_**PRACTICAL EXERCISE:**_

To decompress: chose f.eks. `PE_mapped_sorted.bam` that we did in the first step:

```bash
samtools view -h -o <out.sam> <in.bam>
```
Look at your `.sam` file using:

```bash
less <filename.sam>
```

## 5. Looking at how the reads maps against the reference/assembly with [Samtools] `tview` module

- looking at the reads pileup with SAMtools
  - use `-p <position>` if you want to see a specific position
  - type `?` to view the navigation help while samtools is running
  - type `q` to quit


_**PRACTICAL EXERCISE:**_

```bash
samtools tview  <final_all_merged.bam> --reference <assembly>
```

## Going further
Suggested next lesson:  [Visualizing assembly and associated reads pileup with IGV](./Visualisation_assembly_reads_pileup_IGV.md)

You can also look here at the [Uio course] for more details or if you want to do things slightly differently. We reuse some of their [scripts](https://inf-biox121.readthedocs.io/en/2017/Assembly/practicals/Sources.html).

[Uio course]:https://inf-biox121.readthedocs.io/en/2017/Assembly/practicals/03_Mapping_reads_to_an_assembly.html

[bwa mem]:http://bio-bwa.sourceforge.net/bwa.shtml

[bwa tools]:http://bio-bwa.sourceforge.net/

[Samtools article]:https://academic.oup.com/bioinformatics/article/25/16/2078/204688

[Samtools manual]:http://www.htslib.org/doc/samtools.html

[Samtools](http://www.htslib.org/doc/samtools.html)
