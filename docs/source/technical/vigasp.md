# Overview: 
VIGAS/P stands for Veteriary Institute Genome Analysis System/Platform. VIGASP uses IRIDA for data management, data access control, analysis and storing the results. IRIDA uses Galaxy to execute the pipelines. 
### Features of VIGASP:
1. Browser based user friendly platform accessible from VI PC/Laptop or using VI VPN.
2. All data (fastq files, genome assemblies and metadata) and results are stored in an organized (Genus, sample_id and project) way with proper data access control.
3. Possible to search by genus, sample_id, file_name and results (F. ex. MLST type, seroptype, amr genes, etc,.)
4. As of March 2024, we have 11 pipelines
5. Each project/Genus can set automated pipelines every time when there is a new data with specific parameters for that Genus 
6. Fastq files from VIGASP can be uploaded to NCBI SRA directly.

# Accessing VIGAS and logging in
VIGAS is accessible only from official VI stationary computers and Laptops or through VI-VPN.

Those who want to get user names has to send email to Jeevan (bioinf-group@vetinst.no) with following details: 
1. Explanation: Why do you need access to VIGAS
2. Preferred user name and mobile number
3. List of species data do you want to have access for. We wil talk to the managers of those projects in VIGAS and give you access to those data/metadata/results.
4. Also, the person will have to meet one of the bioinformatics team members in person and to show the NVI ID card due to the data confidentiality agreements.

# Data management and access control 
In IRIDA, all data (Fastq files, meta data and results) are organized as projects (species bucket). No data can stand alone. F. ex. All salmonella data are part of "Salmonella spp" project and all Listeria data are part of "Listeria spp" project. Only the members of the project can see, access, analyze and share the data and the results. Users can create projects where they can share the data from main project to do further project specific analysis. 

## Projects in VIGASP
Once the you have logged-in, you will see the projects you are part of like in the image below. <br><br>   

<img width="1656" alt="Screenshot 2024-02-26 at 15 26 42" src="https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/15940041/5956ca0a-a722-423b-a7d1-d537c125dad2">
<br><br>

By clicking on the project names, you can see and access the data
<br><br>
<img width="1643" alt="Screenshot 2024-02-26 at 15 28 34" src="https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/15940041/cd8d4770-0b98-40b4-8c87-6a69f7389a69">
<br><br>

By clicking on the sample name, you can see more information, files (fastq, assembly), metadata and analyses. 
Assembly files will be available only after running "Assembly and Annotation" pipeline. Metadata can either be added one by one manually or using a metadata file (csv or excel) with sample_id. Metadata also contains "selected-formatted" results from analyses using VIGASP pipelines. 

<br><br>
<img width="1654" alt="Screenshot 2024-02-26 at 15 29 57" src="https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/15940041/fc159b42-bb41-4fc4-b198-477902bf881b"><br><br>

<br><br>
# Pipeline overview 
AS of January 2024, VIGAS has 12 pipelines. 8 of them are developed in house at NVI. Project managers can set up automated pipelines with customized parameters for each project. By clicking on the "shopping cart" symbol in top right corner will take you to list of pipelines available.
<br><br>

<img width="1268" alt="Screenshot 2024-02-26 at 15 51 00" src="https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/15940041/705dd6ef-f4f8-4bee-b731-15f625812ea4">

<br><br>

# List of pipelines: 
1. **Assembly and Annotation Pipeline** <br> 
   Shovill assembly, Prokka annotation and QUAST assembly assessment.<br>
   
2. **MLST Pipeline NVI - Modified by NVI** <br>
   Pipepline performs Shovill assembly and QUAST assembly quality assessment. And, uses checkm_analyze and checkm_qa for 
   assessing the quality of genomes.<br>
   
3. **SISTR Pipeline v1.1.1 (NVI)**<br>
   Scan contig files against PubMLST typing schemes.<br>
   
4. **Species Abundance**<br>
   Estimate the relative abundance of reads from each species present in a sample. Taxonomic classification by kraken2 followed by re-estimation by bracken.<br>
   
5. **Assembly_QC**<br>
   Pipepline performs Shovill assembly and QUAST assembly quality assessment. And, uses checkm_analyze and checkm_qa for assessing the quality of genomes.<br>
   
6. **Phylo-CoreGenomeSNP**<br>
   Phylogenetic analyses of prokaryotic isolates. Suitable only for datasets where the samples are expected to be very closely related. Here, multiple sequence 
   alignment is generated with ParSNP, which takes the whole genome into account.This pipeline is similar to ALPPACA on saga, but not identical.<br>

7. **Reads_QC**<br>
    Runs Kraken2/Bracken for species confirmation/contamination-check and fastqc for basic quality control parameters.<br>
    
8. **ResPointFinder**<br>
    Uses ResFinder and PointFinder tools to map the Fastq reads to ResFinder and PointFinder databasses and produces a combined formatted output as well.<br>
    
9. **ResPointFinder2 (EFSA 2022)**<br>
    Takes Fastq pairedend reads as input and compares against ResFinder and PointFinder database. LineList header prefix is "RPF2305". ResPointFinder tool version: 
    EFSA_2022, ResFinder DB version: EFSA_2022, PointFinder DB version: EFSA_2022.<br>
    
10. **ResPointFinder3 (EFSA 2023)**<br>
    Takes Fastq pairedend reads as input and compares against ResFinder and PointFinder database. LineList header prefix is "RPF2403". ResPointFinder tool version: 
    4.4.2, ResFinder DB version: 2.2.1, PointFinder DB version: 4.0.1 DisinFinder DB version 2.0.1.<br>
    
11. **SeroTypeFinder**<br>
    SerotypeFinder identifies the serotype in total or partial sequenced isolates of E. coli.<br>
    
12. **Virulence_Finder**<br>
    Takes fastq files and compares against Virulence finder database(DTU).<br>
    
13. **spaTyper Pipeline**<br>
    Generates spa type identification<br>

# Analyses 
In order to analyse a sample (or more than one sample), you have to select the samples in the project and click "Add to cart" button to send the fastq files for annalyses. <br> <br>

<img width="918" alt="Screenshot 2024-02-26 at 16 13 55" src="https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/15940041/e16a9702-ee26-4b99-9af9-b03873c8d14e"> <br> <br>

Once the samples are added, you have to click on the "shopping cart" on the top right corner to see all the pipelines available.

# LineList

## Assemblies

# Data transfer and Automated Pipeline
Sequenced data from NVI sequencing lab are semi-automatically(so far) transferred to VIGAS from SAGA/NIRD.    

# Cititig VIGASP
