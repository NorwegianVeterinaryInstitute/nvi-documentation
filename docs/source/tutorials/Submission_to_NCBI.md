# Submission of bacterial isolate shotgun reads to the NCBI-SRA database using VIGAS

The NVI VIGAS system gives you the possibility to submit the shotgun reads of one or multiple isolates directly to the NCBI Short Read Archive (SRA) so that you can get accession numbers for these and the sequencing data becomes public. Note, that you can not submit the genome sequences to the NCBI genbank database through the VIGAS portal. To do that you have to use the [NCBI submission system](https://submit.ncbi.nlm.nih.gov/). 

But before you start with the submission in VIGAS, you first need to check if the sample(s) you want to submit are part of project which is already registered as a BioProject [BIOPROJECT](https://www.ncbi.nlm.nih.gov/bioproject/) at NCBI. If these samples are from a project that is already registered as a BioProject, than we can proceed to register BioSamples to obtain the [BIOSAMPLE](https://www.ncbi.nlm.nih.gov/biosample/docs/) IDs. 
When you do not have a BioProject, we will start with that and in the process also register our samples as BioSamples. It is not possible to submit your shotgun sequences via VIGAS to the NCBI SRA database without these two items in place. But don't worry. The process is very easy to follow and do. Things are automatically saved when you go to the next submission step at NCBI. So you can leave the process at anytime and come back the next day to continue where you left. 

We will start with creating a BioProject submission, which will also result in a BioSample submission.

## Starting the submission.
To start the submission go to the website [https://submit.ncbi.nlm.nih.gov/](https://submit.ncbi.nlm.nih.gov/) and login or create an account at NCBI. Once you are on the home page of the submission system, you are asked what you want to submit. On that page you see different tabs. Select the tab called: "My submissions". On the screen that now appears you see your previous submissions, and you see a box with "Start a new submission". Click here on "BioProject".

## Creating a BioProject submission.
The first page that you see, is a page with all the details of the submitter. Those details include email addresses for communication, and the address of the institute that is submitting. Here you can decide if you submit it as a single person, or that you do it as part of a group. You can also create a group and invite your collaborators. Once they accepted the invitation, they can also modify the details of your submission. 
Once this is all set-up, than you can click "continue". 

The next page is asking what kind of project you are trying to register. We select here: "Raw sequencing reads", since that is what is in VIGAS that we want to submit. Next we can select what the sample scope is. You find an explanation below the box. We can select the one that fits our project in the best way. For instance, if your project only deals with _Escherichia. coli_ isolates from different samples, we can select the "Multi-isolate" option. Note that the option you select will give you a specific page in the Target tab, which is in line with the scope you selected. Now you click "continue".

In the next screen you are asked to fill in the Target details. For the "Multi-isolate" scope this requires you to fill in the species name of the organism, you can select it from a scroll down menu. You can add any kind of other information about your isolates here, but that is not mandatory. But for the future, it would be good, because more metadata makes the life of other researchers easier. We then proceed to the next page.

The first question on the tab for the general information is about the release that of your submission to the public? You can select "immediately" or release on a "specific date". In the later case you have to indicate a future date. The maximum time allowed for holding back the release is exact 4 years after the day you fill in this form. I often set it at one year from now, even if I want to release it now. I do that, so I can correct errors later, befor actually releasing the sequences.

Next we start with filling out the project title, the public description, and you indicate the primary relevance of the project, by selecting one of the options in the scroll down menu. Next you can indicate, if this is a project that needs to be linked to an umbrella project and you can specify the umbrella accession number when relevant. 
On this page you can give also add one or more links to external websites such as the website of the main project that produced the samples you are submitting. You can also add the Grant that funded the research to sequence your samples. And finally, you can indicate if the sequencing was done by a consortium, and if there is another data provider that is different from the institute of the submitter (you indicated that earlier). 
If everything is selected and filled in, you can click "continue". At this point all information for a new BioProject is registered.

On the fifth tab, called BioSample, we are asked to either fill in the IDs of existing BioSamples. If you only one to register a single sample, than you can do that here. In case you want to register multiple samples, than you click "continue" to finish registering the BioProject.

This will bring you to a page to register publications that are associated with the project and the samples you want to submit. Note, you can also register those later. Continue!.
And then you are at the final page of the BioProject submission. Review everything and click the submit button to generate the BioProject ID. Note, after this step, you can no longer delete the BioProject yourself, but you can modify the details using the "Manage data" box.

## Creating a BioSample submission.

Next, you are asked if you are submitting a single sample or multiple samples. Select what fits your submission.

On the third tan, we have to select the right sample type. If you know the species of your bacterium, then you can use that to select the right information packages. If your organism was identified during a a disease outbreak then select "Pathogen". If your organisms was obtained during surveillance than select "One Health Enteric"species for those organisms. For the rest of organisms select the package "MIGS Cultured Bacterial/Archaeal". You should have good arguments if you want to select the package "Microbe", but we would not recommend it.


In the question box you type "Bioproject" or "BioSample" (in case you already have a BioProject). A box will appear indicating "Bioproject and Biosample", selecting that will bring you to the first page of the tool we need to use.
That page will show you an overview of the process of creating a BioProject and what you can expect to do. We use this because we mostly have large and long-term projects where we will be submitting samples over a longer period of time.

## Starting the submission.
At the bottom of the page you find the `Submit` button, clicking this will bring you to the page with all your previous submissions. On the next page you find a button called `New Submission`. Clicking this button will start the process and a submission ID is immediately generated. 










## Creating a Biosample
At the next page, we are asked to fill in the BioSample IDs for the samples we want to submit. In the case you do not have those, you can click continue, and you are transferred to the system for the submission of BioSamples. On the first page, you have to indicate the release date for your BioSample. Note that this can be later than the release date of the BioProject you registered previously.  

After the package selection you will be asked how you want to fill in the Biosample attributes. For a single sample select the build-in table editor, but for multiple isolates you have to fill in an excel sheet (it is just quicker). In case of the later option you can download the excel sheet and fill in the field for all the isolates you want. Here you also need to fill in the Bioproject number for each isolate. **Note**, it is possible to indicate multiple bioprojects if an isolate is part of multiple different projects. In case of the "Pathogen" or the "One Health Enteric" options, you also have to option to add information on MIC values (https://www.ncbi.nlm.nih.gov/biosample/docs/antibiogram/).

Once you have filled in the biosample attributes in either the online table or the excel sheet you uploaded again, you continue to the next step to provide more information on the genomes.  Once that is done you are ready to go to VIGAS.

## STUFF
Is it forrle general information. Here you are asked if you have a bioproject and / or biosamples. For an entirely new project you have to say "no", and otherwise say "yes" and specify it. Also specify a release data. We suggest you pick a data far in the future, and only when everything is satisfactory you actively release it, by changing the date to something in the immediate future. And at the bottom of the page you give the submission a title and you might add some comment to the NCBI staff.

On the next page you are asked how you generated the (draft-) genome assembly. We always use assembler software, so we say "no". Then you will get questions about the ambigous bases in your sequence (the N's) and if they are used to indicate gaps. And the final question on the page asks what kind of information was used to infer linkage across gaps. In the case of assemblies done by VIGAS it is "paired-ends". But for more complicated projects you know if other methods such as fingerprinting, optical mapping or maybe even Hi-C were used.

Once you have moved to the next pages, you will fill in the details for the Bioproject. Note that the words here will become visible for the public when you release your genomes. So write a clear title and a good description. For the description you might use the abstract of your publication. Then also add a link to the webpage of your project, add the grant that was used for generating the data. If this was part of a consortium effort, than also indicate that. 
Once your done here: you w

Then you can add the publication, when there is already one, to the submission. Otherwise you can do that later, by modifing your submission.

