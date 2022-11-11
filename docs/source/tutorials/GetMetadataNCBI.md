# Get Metadata from NCBI 

Extract metadata from NCBI. 

Tool: [NCBImeta](https://github.com/ktmeaton/NCBImeta)

Example local installation
```
conda create -n ncbimeta -c bioconda ncbimeta
```

Get an API key from NCBI :
- login (or create an account)
- account > account settings : there you can create an API key you will use - copy it and keep it safe

We will need to create a configuration file (.yalm) for the software to work. 

Assume you have downloaded sequences and you have biosample ID - you want to retrieve the data for all the BioSamples that belong to 
the genus _Providencia_. 

A trick is to create a simple search in NCBI ex: 
![screenshot1](./searchfield1.png)

This search field will help you build your configuration file (.yalm) for the search data and the gathering of metadata 

Now we do a specific search for a specific sample we are interested in to get the metadata for, eg. BioSample: `SAMN12536438`. 
This page give us the information for the metadata we can have acces to and how it is structured in the database. To ease the process of creating the `.yalm` file we save this data as `xml` and open it with a web-browser so we can get the structure of the fields we want information from. ![screenshot2](./exportsearch_xml.png)

This gives: [xml_file](./biosample_result.xml).

Compare it to the [.yalm configuration file](./providencia_metadata.yaml)

The table column define what will go in the database - and from which database 

You see that you can define your own fields, that were not provided in the other examples found in the [Biosample example](https://github.com/ktmeaton/NCBImeta/tree/master/schema). 
To define those fields you need to access at which hierarchy of the `.xml` file you downloaded as helper. 
The different hierarchical levels are defined in order separated by comas eg. `Attribute, harmonized_name, <variable>`  

This [attribute documentation page at NCBI for BioSample](https://www.ncbi.nlm.nih.gov/biosample/docs/attributes/) gives you all the attribute fields that may have been defined. You see that there is the `harmonized_name`column then you know which variable name you should use to get what you want, and all the possible information that may have been registered.   

When you have created the .yalm configuration file as you want, you can try to run it. I will guide you if some fields are incorrect, so you can rectify. 

Then you can run the tool. 
```
conda activate NCBImeta
NCBImeta --config providencia_metadata.yaml --flat
```

This creates a "sqlite" database. 
- You can open it with the [sqlite browser](https://sqlitebrowser.org/) (there is an option without install for windows: portable app) 

Open the database > browse - it looks like a spreadsheet
- You can export it into a .csv file (that you can open in Excel)
- You can decide to use this database and increase it to make links with the data you already have. If you do not how: this carpentry course can help you start : [Databases and SQL](https://swcarpentry.github.io/sql-novice-survey/) 
- You can interact with the database with python or R. Eg for R, the [**dbplyr**](https://dbplyr.tidyverse.org/) library allows you to use query the database directly, using the **dplyr** syntax (with the pipes) as we have already seen in other tutorials.

