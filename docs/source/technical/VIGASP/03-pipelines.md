# Pipeline overview 
AS of January 2024, VIGAS has 12 pipelines. 8 of them are developed in house at NVI. Project managers can set up automated pipelines with customized parameters for each project. By clicking on the "shopping cart" symbol in top right corner will take you to list of pipelines available.
<br><br>

<img width="1268" alt="Pipeline Overview" src="images/Pipeline_list.png">

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

<img width="918" alt="Choosing Samples" src="images/Choosing_samples.png"> <br> <br>

Once the samples are added, you have to click on the "shopping cart" on the top right corner to see all the pipelines available. <br><br>

<img width="1671" alt="Added Samples" src="images/Added_samples.png">
<br><br>

Then, you can choose the pipeline you want to run by  clicking the "Select" button under the pipeline. 
<img width="1671" alt="Executing Pipeline" src="images/Execute_pipeline.png">
<br><br>

