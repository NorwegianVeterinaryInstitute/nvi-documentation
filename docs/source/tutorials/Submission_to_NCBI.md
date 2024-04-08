# Bacterial genome submission to NCBI with VIGAS

In the NVI VIGAS system it is possible to submit one or multiple genome sequences from isolates to the NCBI database so that you can get accession numbers for these and the genome becomes public. But before you start with the submission in VIGAS, you first need to create a [BIOPROJECT](https://www.ncbi.nlm.nih.gov/bioproject/) (in case it is not already existing) and you need to obtain [BIOSAMPLE](https://www.ncbi.nlm.nih.gov/biosample/docs/) IDs. It is not possible to submit your genome sequences to the NCBI database without these two items in place. But don't worry. The process is very easy to follow and do. Things are automatically saved when you go to the next submission step at NCBI. So you can leave the process and come back the next day to continue, and it will do so at the step where you left.

## Starting the submission.
To start the submission go to the website [https://submit.ncbi.nlm.nih.gov/](https://submit.ncbi.nlm.nih.gov/) and login or create an account at NCBI. Once you are inside the submission system, you are asked what you want to submit. There are four major categories:

1. Genome - Submit assembled complete or incomplete/draft prokaryotic and eukaryotic genomes. Not for viral, phage, or single locus sequences (for example: 16S rRNA).
2. BankIt - Submit genomic DNA, organelle, ncRNA, plasmids, other viruses, phages, mRNA and synthetic constructs.
3. Genbank - Submit ribosomal RNA (rRNA), rRNA-ITS, SARS-CoV-2, Influenza, Norovirus, Dengue, metazoan COX1 or eukaryotic nuclear mRNA.
4. SRA - SRA accepts unassembled reads from high throughput sequencing platforms. Submitted data files should generally be minimally processed and include per-base quality scores.

Since we want to submit Bacterial Genomes, we select that option. Next you get to a page that describes the entire process for the genome submission.
You can go through the different boxes called: Overview, Submission type, Annotation, Project/Sample, Metadata and Files. Those will describe the process and what is required to finish it.

At the bottom of the page you find the `Submit` button, clicking this will bring you to the page with all your previous submissions. On the next page you find a button called `New Submission`. Clicking this button will start the process and a submission ID is immediately generated. The next question you get is what is it you are submitting, a single genome or multiple genomes or haplotypes. After selecting that you start with filling int the information about who is submitting. If you have already submitted a genome before this will be filled in, otherwise you have to do this.


## Creating a Bioproject
On the next page you have to start filling out the general information. Here you are asked if you have a bioproject and / or biosamples. For an entirely new project you have to say "no", and otherwise say "yes" and specify it. Also specify a release data. We suggest you pick a data far in the future, and only when everything is satisfactory you actively release it, by changing the date to something in the immediate future. And at the bottom of the page you give the submission a title and you might add some comment to the NCBI staff.

On the next page you are asked how you generated the (draft-) genome assembly. We always use assembler software, so we say "no". Then you will get questions about the ambigous bases in your sequence (the N's) and if they are used to indicate gaps. And the final question on the page asks what kind of information was used to infer linkage across gaps. In the case of assemblies done by VIGAS it is "paired-ends". But for more complicated projects you know if other methods such as fingerprinting, optical mapping or maybe even Hi-C were used.

Once you have moved to the next pages, you will fill in the details for the Bioproject. Note that the words here will become visible for the public when you release your genomes. So write a clear title and a good description. For the description you might use the abstract of your publication. Then also add a link to the webpage of your project, add the grant that was used for generating the data. If this was part of a consortium effort, than also indicate that. 
Once your done here: you w

Then you can add the publication, when there is already one, to the submission. Otherwise you can do that later, by modifing your submission.

## Creating a Biosample
At the next step, we will select the right sample type. If you know the species of your bacterium, then you can use that to select the right information packages. If your organism was identified during a a disease outbreak then select "Pathogen". If your organisms was obtained during surveillance than select "One Health Enteric"species for those organisms. For the rest of organisms select the package "MIGS Cultured Bacterial/Archaeal". You should have good arguments if you want to select the package "Microbe", but we would not recommend it. 

After the package selection you will be asked how you want to fill in the Biosample attributes. For a single sample select the build-in table editor, but for multiple isolates you have to fill in an excel sheet (it is just quicker). In case of the later option you can download the excel sheet and fill in the field for all the isolates you want. Here you also need to fill in the Bioproject number for each isolate. **Note**, it is possible to indicate multiple bioprojects if an isolate is part of multiple different projects. In case of the "Pathogen" or the "One Health Enteric" options, you also have to option to add information on MIC values (https://www.ncbi.nlm.nih.gov/biosample/docs/antibiogram/).

Once you have filled in the biosample attributes in either the online table or the excel sheet you uploaded again, you continue to the next step to provide more information on the genomes.  Once that is done you are ready to go to VIGAS.




