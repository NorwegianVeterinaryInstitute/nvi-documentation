# How to do a Blast job on Saga

The example files for this tutorial are stored in our project directory, the PATH is:

```
/cluster/projects/nn9305k/
```
in the subfolders:

```
/samplefiles/fasta_files/
```

from your home area we go to this directory.

```
cd /cluster/projects/nn9305k/samplefiles/fasta_files/
```

Now we create a directory to do our analysis, on our working area: `/cluster/work/users/YOUR_USERNAME`
A shortcut for that working area is a variable called: `$USERWORK`. We can use that instead of the PATH to that directory.

let's create a temporary directory on our work directory `$USERWORK`
```
mkdir $USERWORK/blast_tutorial
```

Now we copy the example file to the new directory.
```
rsync -rauWP 16S_rRNA_sequences_20.fasta $USERWORK/blast_tutorial
```

Let us follow where that file went.
```
cd $USERWORK
```

This folder is located at:
```
pwd
```
The location should be this: `/cluster/work/users/YOUR_USERNAME`.

let us go to the blast_tutorial, and check if the protein sequences are there.

```
cd blast_tutorial

ls -la
```

To do Blast of these 20 rRNA sequences we need two items
1. A blast database suited for microbial 16S rRNAs. For instance a 16S rRNA database 
2. The Blast software

Now the software is loaded. and we now need a database to blast our sequence against it.
The current ncbi-NR database is more than 35 Gb, so that takes sometime to download. But that is not needed on Saga.

On saga the NCBI-NR database is present. The location of the database is:

```
/cluster/shared/databases/blast/latest/
```
Go to that folder to check the databases that are present.
```
ls 
```
This is not helpful when there are so many files. So let's use a bash trick to sort get only the essential things. 

```
ls |cut -f1 -d "." | uniq 
```
To understand the command above you can run part of the command before a vertical slash.

Now we can see a bunch of databases. And yes there we find a 16S rRNA microbial database, which we can use to blast our 16S sequences.

the location of that database is then:


```
/cluster/shared/databases/blast/latest/16SMicrobial
```

let's go back to our tutorial folder

```
cd $USERWORK/blast_tutorial
```

Before we can run our blast job we need to ask for computing time. We do that by first starting a screen.
This to make sure our job is running, even when our connection is broken to Saga
``` 
screen
``` 
Here you can find more on screen: [How to use screen](https://linuxize.com/post/how-to-use-linux-screen/)

Once the screen is up and running, we can ask for computing time and resources for an interactive job. 
For testing small commands use the [development queue](https://documentation.sigma2.no/jobs/job_scripts/saga_job_scripts.html)

```
srun --account=nn9305k --mem=40G --cpus-per-task=10 --time=1:0:0 --pty bash -i
```
or when using the develop queue

```
srun --account=nn9305k --mem=40G --cpus-per-task=10 --time=1:0:0 --qos=devel --pty bash -i
```
Once a job is allocated we can load the blast software. The blast software can be loaded using the [module system on saga](https://documentation.sigma2.no/software/installed_software.html).

Check the modules present
```
module avail
```
This shows all available software. To show only the available blast software type this:

```
module avail blast
```

Before we load the module, we want to remove any module that could interfere with the software we want to use:

```
module purge
```
And then we load the module of our choice:

```
module load BLAST+/2.8.1-intel-2018b
```

Now we can test if the nucleotide blast is working:

```
blastn -h

or

blastn -help
```

now we make the command to run our 16S rRNA sequences against this 16SMicrobial database.

```
blastn -db /cluster/shared/databases/blast/latest/16SMicrobial -query 16S_rRNA_sequences_20.fasta -outfmt 0 -evalue 0.001 -out blast_16S_results.txt -num_threads 2

```
This should take about 30 seconds.

When you want to create a tabular format output style, use: `-outfmt 6`.
```
blastn -db /cluster/shared/databases/blast/latest/16SMicrobial -query 16S_rRNA_sequences_20.fasta -outfmt 6 -evalue 0.001 -out blast_16S_results.txt -num_threads 2
```

When you want to create a custom tabular output file:

```
blastn -db /cluster/shared/databases/blast/latest/16SMicrobial -query 16S_rRNA_sequences_20.fasta -outfmt "6 qseqid saccver pident length mismatch gapopen qstart qend sstart send
   evalue bitscore" -evalue 0.001 -out blast_16S_results.tab -num_threads 2
```

When you only want to show the 5 best hits
```
blastn -db /cluster/shared/databases/blast/latest/16SMicrobial -query 16S_rRNA_sequences_20.fasta -outfmt 6 -evalue 0.001 -out blast_16S_results.txt -num_threads 2 -max_target_seqs 5
```


### A final tip

When using a different database, say the NCBI-NR or NT databases, than you need much more memory to run a blast job. Ask for a minimum of 10 cpus for your job.
Since the loading of the databases costs a lot of time, it is better to use a slurm script to blast many sequences against such large databases.



