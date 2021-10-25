# Nanopore sequence data tutorial

This is a tutorial to do quality control of the Nanopore sequence data. The data we use at our institute is mainly minION sequence data gnerate for the assembly of bacterial genomes. This tutorial uses Guppy version 5 for basecalling, demultiplexing and quality score filtering.  The tutorial is run a HIGH PERFORMANCE COMPUTING system that uses a SLURM system for job allocation. If you have your own server, just skip the srun step, but make sure to have Guppy, Nanoplot and NanoFilt installed.

_This tutorial is a work in progress, and will be updated in the coming periode, with assembly steps as well._

To follow this tutorial you need to have:

* Guppy version 5 installed that uses the GPU nodes on your environment. I would not advice using cpu's only, since it takes for ever.
  * I have it installed as a conda environment. (if you want to know how to make that, please contact me: `thomas.haverkamp at vetinst.no`)
* A recent dataset from a nanopore sequencer with the fast5 extention.
* Information of the flowcell and sequencing kits used.

The Nanopore sequence data for this tutorial can be found in SAGA folder, or use your own data:

```
 /cluster/projects/nn9305k/tutorial/20211025_nanopore_analysis
 ```

This consist of 2 fast5 files, but you can find the complete dataset (12 datasets) in the SAGA folder:

```
 /cluster/projects/nn9305k/datasets/wgs/nanopore_test_data/np_test_data.tar.gz
```
The tutorial dataset consist of 8000 sequences.

### Setting up the analysis

For this tutorial we are going to create a directory on our user directory: $USERWORK. which contains a directory with fast5 files.
```
cd $USERWORK

mkdir nanopore_tutorial

cd nanopore_tutorial

rsync -rauWP /cluster/projects/nn9305k/tutorial/20211025_nanopore_analysis/*.fast5 ./fast5

```
Now we have a tutorial folder, that is temporary, and it contains the fast5 files.

Now we need the information for Guppy, which selects the correct basecalling model for the data we have here.

| type ID | ID |
| ----- | ----- |
| Flowcell ID | FLO-MIN106  |
| Sequencing kit ID | SQK-RBK004 |
| Barcoding KIT ID |  RBK004    |

### running Guppy basecalling

Let's try and see if we can run Guppy, without blocking each other access to the GPU nodes.

first let's active a `screen`. We call this screen : NANOPORE, so it is easy to find back if our connection is gone, and we have to log on to the system again. It is also easy to use when you open new screens all the time.

```
screen -S NANOPORE
```
and now we start an active node on SAGA that has GPU chips.
```
srun --account=nn9305k --mem=64G --partition=accel --gres=gpu:1 --cpus-per-task=8 --time=2:0:0 --pty bash -i
```
asking for the software to be loaded:
```
conda activate guppy_gpu_v5
```

To see which flowcells and kits are supported by this version of Guppy we can do:

```
guppy_basecaller --print_workflows
```

and then running guppy on the fast5 files, which takes about 2 minutes to run.

```
guppy_basecaller --flowcell FLO-MIN106 --kit SQK-RBK004 \
        -x "cuda:all" \
        --gpu_runners_per_device 16 \
        --num_callers 16 \
    --records_per_fastq 0 \
    --compress_fastq \
    -i fast5 \
    -s fastq
```
as a single line
```
guppy_basecaller --flowcell FLO-MIN106 --kit SQK-RBK004 -x "cuda:all" --gpu_runners_per_device 16 --num_callers 16 --records_per_fastq 0 --compress_fastq -i fast5 -s fastq
```
Note that the minimum quality score cut-off is: 9

This produces the following folders and files:
```
folders:
fail
pass

files:
sequencing_summary.txt  
sequencing_telemetry.js
guppy_basecaller_log-2021-10-25_10-34-10.log
guppy_basecaller_log-2021-10-25_10-31-13.log
```

We can also run it with demutliplexing, and quality trimming

Some options from Guppy:

```
--disable_qscore_filtering : Disable filtering of reads into PASS/FAIL folders based on min qscore
--min_qscore : Minimum acceptable qscore for a read to be filtered into the PASS folder
--barcode_kits : Space separated list of barcoding kit(s) or expansion kit(s) to detect against. Must be in double quotes.
--trim_barcodes : Trim the barcodes from the output sequences in the FastQ files.
--num_barcode_threads: Number of worker threads to use for barcoding
--disable_pings: stop ping the nanopore server.
```

So let us add the demultiplexing and reduce minimun quality score

```
guppy_basecaller --flowcell FLO-MIN106 --kit SQK-RBK004 \
        -x "cuda:all" \
        --gpu_runners_per_device 16 \
        --num_callers 16 \
    --records_per_fastq 0 \
    --compress_fastq \
    --min_qscore 8 \
    --barcode_kits "SQK-RBK004" \
    --trim_barcodes \
    -i fast5 \
    -s fastq_demultiplexed
```

Now we have enough datasets produced with the GPU nodes, so now we can stop the srun job. on saga.
```
conda deactivate
exit
```
### getting some statistics.

Now we start the nanoplot conda environment to generate a report of our nanopore data
Nanoplot can be found at this website: https://github.com/wdecoster/NanoPlot
```
conda activate nanoplot

cd fastq
```
Let's run NanoPlot on the file `sequencing_summary.txt`

```
NanoPlot --summary sequencing_summary.txt --loglength -o summary-plots-log-transformed
```
That generates a folder called:  "summary-plots-log-transformed"

let's take a look at the files. If you are on a cluster, you should download the folder to your own personal computer.

You can also use Nanoplot on the fastq files that were produced. Lets do that for now on the fail and passed reads.

```
NanoPlot -t 2 --fastq fail/*.fastq.gz --loglength -o failed_summary_plots

NanoPlot -t 2 --fastq pass/*.fastq.gz --loglength -o passed_summary_plots

```
We can clearly see that the first Guppy analysis removed all the low quality reads.

### Filtering reads
Let us do some more filtering on the passed reads with a tool called NanoFilt (https://github.com/wdecoster/nanofilt)
NanoFilt needs the fastq files to be unzipped. So we do that in a single command.

```
zcat *.fastq.gz |NanoFilt -q 10 -l 1000 --headcrop 50 | gzip >> High_quality_reads.fastq.gz

```
I added the headcrop, because nanopore reads can be of low quality in the first bit of the reads. It could improved your assemblies later on.

Checking the quality of the filtered reads with Nanoplot.
```
conda activate NanoPlot
NanoPlot -t 2 --fastq High_quality_reads.fastq.gz --loglength -o ../High_quality_reads.summary.txt
```

### The end for now

This tutorial will be update with more options on how to do nanopore sequence quality control.


### Links to software
 
* Guppy: Sorry Nanopore only gives that out, so you have to ask them.
* NanoPLot: https://github.com/wdecoster/NanoPlot
* NanoFilt: https://github.com/wdecoster/nanofilt
