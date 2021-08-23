# Creating gap tracks

We use a little python script from [sequencetools repository](https://github.com/lexnederbragt/sequencetools). NB: If you use this script in an article you need to proprely aknowledge Lex Nederbragt.


For ease we copied this script and placed it in the SAGA folder: `/cluster/projects/nn9305k/tutorial/20210412_mapping_visualization/scripts`. The script is now called `scaffoldgap2bed_py3.py`.


Gap tracks can be useful to insert gap locations: where scaffolds are interupted, into a file. This eg. ease locating the start and ending of scaffolds (which are potentially problematic regions of an assembly), when visualizing genome with software such as IGV.

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
