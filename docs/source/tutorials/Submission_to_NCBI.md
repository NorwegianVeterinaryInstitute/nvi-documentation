# Submission of bacterial isolate shotgun reads to the NCBI-SRA database using VIGAS

The NVI VIGAS system gives you the possibility to submit the shotgun reads of one or multiple isolates directly to the NCBI Short Read Archive (SRA) so that you can get accession numbers for these and the sequencing data becomes public. Note, that you can not submit the genome sequences to the NCBI genbank database through the VIGAS portal. To do that, you have to use the [NCBI submission system](https://submit.ncbi.nlm.nih.gov/). 

But before you start with the submission in VIGAS, you first need to check if the sample(s) you want to submit are part of project which is already registered as a BioProject [BIOPROJECT](https://www.ncbi.nlm.nih.gov/bioproject/) at NCBI. If these samples are from a project that is already registered as a BioProject, than we can proceed to register BioSamples to obtain the [BIOSAMPLE](https://www.ncbi.nlm.nih.gov/biosample/docs/) IDs. Go to [Creating a BioSample submission](#Creating-a-Biosample-submission).

When you do not have a BioProject, we will start with that and in the process also register our samples as BioSamples. It is not possible to submit your shotgun sequences via VIGAS to the NCBI SRA database without these two items in place. But don't worry. The process is very easy to follow and do. Things are automatically saved when you go to the next submission step at NCBI. So you can leave the process at anytime and come back the next day to continue where you left. 

We will start with creating a BioProject submission, which will also result in a BioSample submission.

## Starting the submission.
To start the submission go to the website [https://submit.ncbi.nlm.nih.gov/](https://submit.ncbi.nlm.nih.gov/) and login or create an account at NCBI. Once you are on the home page of the submission system, you are asked what you want to submit. On that page you see different tabs. Select the tab called: "My submissions". On the screen that now appears you see your previous submissions, and you see a box with "Start a new submission". Click here on "__BioProject__".

## Creating a BioProject submission.
The first page that you see, is a page with all the details of the submitter. Those details include email addresses for communication, and the address of the institute that is submitting. Here you can decide if you submit it as a single person, or that you do it as part of a group. You can also create a group and invite your collaborators. Once they accepted the invitation, they can also modify the details of your submission. 
Once this is all set-up, than you can click "continue". 

The next page is asking what kind of project you are trying to register. We select here: "Raw sequencing reads", since that is what is in VIGAS that we want to submit. Next we can select what the sample scope is. You find an explanation below the box. We can select the one that fits our project in the best way. For instance, if your project only deals with _Escherichia. coli_ isolates from different samples, we can select the "Multi-isolate" option. Note that the option you select will give you a specific page in the Target tab, which is in line with the scope you selected. Now you click "continue".

In the next screen you are asked to fill in the Target details. For the "Multi-isolate" scope this requires you to fill in the species name of the organism, you can select it from a scroll down menu. You can add any kind of other information about your isolates here, but that is not mandatory. But for the future, it would be good, because more metadata makes the life of other researchers easier. We then proceed to the next page.

The first question on the tab for the general information is about the release that of your submission to the public? You can select "immediately" or release on a "specific date". In the later case you have to indicate a future date. The maximum time allowed for holding back the release is exact 4 years after the day you fill in this form. I often set it at one year from now, even if I want to release it now. I do that, so I can correct errors later, before actually releasing the sequences.

Next we start with filling out the project title, the public description, and you indicate the primary relevance of the project, by selecting one of the options in the scroll down menu. Next you can indicate, if this is a project that needs to be linked to an umbrella project and you can specify the umbrella accession number when relevant. 
On this page you can also add one or more links to external websites such as the website of the main project that produced the samples you are submitting. You can also add the Grant that funded the research to sequence your samples. And finally, you can indicate if the sequencing was done by a consortium, and if there is another data provider that is different from the institute of the submitter (you indicated that earlier). 
If everything is selected and filled in, you can click "continue". At this point all information for a new BioProject is registered.

On the fifth tab, called BioSample, we are asked to either fill in the IDs of existing BioSamples. If you only want to register a single sample, than you can do that here. In case you want to register multiple samples, than you click "continue" to finish registering the BioProject.

This will bring you to a page to register publications that are associated with the project and the samples you want to submit. Note, you can also register those later. Continue!.
And then you are at the final page of the BioProject submission. Review everything and click the submit button to generate the BioProject ID. Note, after this step, you can no longer delete the BioProject yourself, but you can modify the details using the "Manage data" box.

## Creating a BioSample submission.
To start registering BioSamples you go to the website [https://submit.ncbi.nlm.nih.gov/](https://submit.ncbi.nlm.nih.gov/) and login or create an account at NCBI. Once you are on the home page of the submission system, you are asked what you want to submit. On that page you see different tabs. Select the tab called: "My submissions". On the screen that now appears you see your previous submissions, and you see a box with "Start a new submission". Click here on "__BioSample__". That will open a screen for BioSample and it has a blue box called "New submission". click that to activate the submission.

The first page that you see, is a page with all the details of the submitter. Those details include email addresses for communication, and the address of the institute that is submitting. Here you can decide if you submit it as a single person, or that you do it as part of a group. You can also create a group and invite your collaborators. Once they accepted the invitation, they can also modify the details of your submission. 
Once this is all set-up, than you can click "continue". 

The first question on the tab for the general information is about the release that of your submission to the public? You can select "immediately" or release on a "specific date". In the later case you have to indicate a future date. The maximum time allowed for holding back the release is exact 4 years after the day you fill in this form. I often set it at one year from now, even if I want to release it now. I do that, so I can correct errors later, before actually releasing the sequences.

Next, you are asked if you are submitting a batch/multiple samples or single sample. Select what fits your submission and continue.

On the third tab, we have to select the right sample type. If you know the species of your bacterium, then you can use that to select the right information packages. If your organism was identified during a a disease outbreak then select "Pathogen". If your organisms was obtained during surveillance than select "One Health Enteric"species for those organisms. For the rest of organisms select the package "MIGS Cultured Bacterial/Archaeal", and then select from what kind of environment you sample came from. You should have good arguments if you want to select the package "Microbe", but we would not recommend it.

Now we end up on the fourth tab, which is the attributes tab. For a few samples you can use the build in editor to fill in the BioSample attributes. It will indicate the madatory columns that you need to fill in and there is support for each column describing the format of the data that can be entered.
For lots of samples you can download an excel sheet with the attributes you need to fill in. There you see the madatory columns listed in green. When done you can also upload it here again using the "choose file" option. If you made misstakes, the system will notify you, and asks for you to revise your excel sheet. Only when everything that is essential is filled in correctly, can you proceed to the final tab, which is "Review & Submit". 

When everything is correct for your BioSample registration, than you can proceed to the submit it. Depending on the time of the day, it can take either a few minutes or a bit longer, before you get your samples listed with a Biosample ID.

## Submission of the reads with VIGAS to the SRA.

