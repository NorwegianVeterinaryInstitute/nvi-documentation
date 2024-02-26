# Overview: 
VIGAS/P stands for Veteriary Institute Genome Analysis System/Platform. VIGASP uses IRIDA for data management, data access control, analysis and storing the results. IRIDA uses Galaxy to execute the pipelines. Also, the fastq files can be uploaded to NCBI SRA.

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
3. **MLST Pipeline NVI - Modified by NVI** <br>
   Under Test: "DO NOT USE THIS" pipeline. Pipepline performs Shovill assembly and QUAST assembly quality assessment. And, uses checkm_analyze and checkm_qa for 
   assessing the quality of genomes.<br>
   
4. **SISTR Pipeline v1.1.1 (NVI) - Modified by NVI**
5. **Species Abundance Pipeline - Modified by NVI**
6. **Assembly_QC Pipeline - NVI Developed**
7. **Phylo-CoreGenomeSNP Pipeline - NVI Developed**
8. **Reads_QC Pipeline - NVI Developed**
9. **ResPointFinder - NVI Developed**
10. **ResPointFinder2 (EFSA 2022) - NVI Developed**
11. **ResPointFinder3 (EFSA 2023) - NVI Developed** 
12. **SeroTypeFinder Pipeline - NVI Developed**
13. **Virulence_Finder - NVI Developed**
14. **spaTyper Pipeline - NVI Developed**

# Analyses

## Assemblies

# LineList

# Data transfer and Automated Pipeline
Sequenced data from NVI sequencing lab are semi-automatically(so far) transferred to VIGAS from SAGA/NIRD.    

# Cititig VIGASP
