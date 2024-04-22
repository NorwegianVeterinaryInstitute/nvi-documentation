# Submission of bacterial isolate shotgun reads to the NCBI-SRA database using VIGAS

In the NVI VIGAS system it is possible to submit the shotgun reads of one or multiple isolates to the NCBI Short Read Archive (SRA) so that you can get accession numbers for these and the sequencing data becomes public. Note, that you can not submit the genome sequences to the NCBI genbank database through the VIGAS portal. To do that you have to use the NCBI submission system. 

But before you start with the submission in VIGAS, you first need to check if the sample(s) you want to submit are part of a BioProject [BIOPROJECT](https://www.ncbi.nlm.nih.gov/bioproject/) If these samples are from a project that is not yet submitted to the NCBI, we first have to create a BioProject. In that process, you also need to register BioSamples for each of the samples you want to submit. [BIOSAMPLE](https://www.ncbi.nlm.nih.gov/biosample/docs/) IDs. It is not possible to submit your shotgun sequences via VIGAS to the NCBI SRA database without these two items in place. But don't worry. The process is very easy to follow and do. Things are automatically saved when you go to the next submission step at NCBI. So you can leave the process and come back the next day to continue, and it will do so at the step where you left. 

## Selecting the type of submission.
To start the submission go to the website [https://submit.ncbi.nlm.nih.gov/](https://submit.ncbi.nlm.nih.gov/) and login or create an account at NCBI. Once you are inside the submission system, you are asked what you want to submit. 
In the question box you type "Bioproject" or "BioSample" (in case you already have a BioProject). A box will appear indicating "Bioproject and Biosample", selecting that will bring you to the first page of the tool we need to use.
That page will show you an overview of the process of creating a BioProject and what you can expect to do. We use this because we have large and long-term projects where we will be submitting samples over a longer period of time.

## Starting the submission.
At the bottom of the page you find the `Submit` button, clicking this will bring you to the page with all your previous submissions. On the next page you find a button called `New Submission`. Clicking this button will start the process and a submission ID is immediately generated. 
The first page that you see, is a page with all the details of the submitter. Those details include email addresses for communication, and the address of the institute that is submitting. Here you can decide if you submit it as a single person, or that you do it as part of a group. You can also create a group and invite your collaborators. Once they accepted the invitation, they can also modify the details of your submission. 
Once this is all set-up, than you can click "continue". 

The next page is asking for general information on your project. When you already have a BioProject, you can say "Yes" here and indicate the BioProject number. When you do not have a BioProject number, than a Bioproject will be create when selecting "No". 
The next question is asking if you already have a BioSample number. When "Yes", these numbers can be provided at a later step in the process. When the answer is "No", they will be created later in the submission process.
The final question on this page is about the release date. When should the submission be release to the public? You can select "immediately" or release on a "specific date". In the later case you have to indicate a future date. The maximum time allowed for holding back the release is exact 4 years after the day you fill in this form. I often set it at one year from now, even if I want to release it now. I do that so I can correct errors later one, befor actually releasing the sequences.
Then we click "continue".

## Creating a Bioproject SUB14394354
On the next page you have to start filling out the project title, the public description, and you indicate the primary relevance of the project, by selecting one of the options in the scroll down menu. Next you can indicate, if this is a project that needs to be linked to an umbrella project and you can specify the umbrella accession number when relevant. 
Next you can give one or more links to external websites such as the website of the main project that produced the samples you are submitting. You can also add the Grant that funded the research to sequence your samples. And finally, you can indicate if the sequencing was done by a consortium, and if there is another data provider that is different from the institute of the submitter (you indicated that earlier). 
If everything is selected and filled in, you can click "continue"

## Creating a Biosample
At the next page, we will select the right sample type. If you know the species of your bacterium, then you can use that to select the right information packages. If your organism was identified during a a disease outbreak then select "Pathogen". If your organisms was obtained during surveillance than select "One Health Enteric"species for those organisms. For the rest of organisms select the package "MIGS Cultured Bacterial/Archaeal". You should have good arguments if you want to select the package "Microbe", but we would not recommend it. 

After the package selection you will be asked how you want to fill in the Biosample attributes. For a single sample select the build-in table editor, but for multiple isolates you have to fill in an excel sheet (it is just quicker). In case of the later option you can download the excel sheet and fill in the field for all the isolates you want. Here you also need to fill in the Bioproject number for each isolate. **Note**, it is possible to indicate multiple bioprojects if an isolate is part of multiple different projects. In case of the "Pathogen" or the "One Health Enteric" options, you also have to option to add information on MIC values (https://www.ncbi.nlm.nih.gov/biosample/docs/antibiogram/).

Once you have filled in the biosample attributes in either the online table or the excel sheet you uploaded again, you continue to the next step to provide more information on the genomes.  Once that is done you are ready to go to VIGAS.

## STUFF
Is it forrle general information. Here you are asked if you have a bioproject and / or biosamples. For an entirely new project you have to say "no", and otherwise say "yes" and specify it. Also specify a release data. We suggest you pick a data far in the future, and only when everything is satisfactory you actively release it, by changing the date to something in the immediate future. And at the bottom of the page you give the submission a title and you might add some comment to the NCBI staff.

On the next page you are asked how you generated the (draft-) genome assembly. We always use assembler software, so we say "no". Then you will get questions about the ambigous bases in your sequence (the N's) and if they are used to indicate gaps. And the final question on the page asks what kind of information was used to infer linkage across gaps. In the case of assemblies done by VIGAS it is "paired-ends". But for more complicated projects you know if other methods such as fingerprinting, optical mapping or maybe even Hi-C were used.

Once you have moved to the next pages, you will fill in the details for the Bioproject. Note that the words here will become visible for the public when you release your genomes. So write a clear title and a good description. For the description you might use the abstract of your publication. Then also add a link to the webpage of your project, add the grant that was used for generating the data. If this was part of a consortium effort, than also indicate that. 
Once your done here: you w

Then you can add the publication, when there is already one, to the submission. Otherwise you can do that later, by modifing your submission.

