# Creating a file to visualize gaps in genomes

When you attempt to visualize genome assemblies with software such as IGV, beeing able to easily locate starting/and ending scaffold locations is very usefull. This, because those positions are potentially problematic regions of an assembly. A gap file (refered to as `.bed` file) provides the locations where scaffolds are interupted. Those file also also to rapidely navigate through the different scaffolds when provided to visualization softwares.


We use a little python script from [sequencetools repository](https://github.com/lexnederbragt/sequencetools) to create `bed`files. NB: If you use this script in an article you need to proprely aknowledge Lex Nederbragt.

For ease we copied this script and placed it in the SAGA folder: `/cluster/projects/nn9305k/tutorial/20210412_mapping_visualization/scripts`. The script is now called `scaffoldgap2bed_py3.py`.


To generate a `.bed` file with this script, Biopython needs to be accessible.

- On Saga: we can use biopython that is installed in the bifrost environment

<div style="background-color:silver">

_**PRACTICAL EXERCISE:**_

```bash
path_script="path"
conda activate bifrost
python scaffoldgap2bed_py3.py -i <assembly.fna>   >    <gap_file>.bed
```
</div>

</br>

You can now use this file eg. in the tutorial [Visualizing assembly and associated reads pileup with IGV](./Visualisation_assembly_reads_pileup_IGV.md)
