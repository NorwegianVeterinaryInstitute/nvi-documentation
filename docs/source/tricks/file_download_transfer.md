## Obtaining multiple genomes from NCBI database

Downloading one genome from the NCBI database is relatively easy and can be done with help of a webbrowser and the following webpages: 

* [https://www.ncbi.nlm.nih.gov/genome/microbes/](https://www.ncbi.nlm.nih.gov/genome/microbes/)
* [https://www.ncbi.nlm.nih.gov/genome/browse#!/prokaryotes/](https://www.ncbi.nlm.nih.gov/genome/browse#!/prokaryotes/)

However, in order to download multiple genome from the NCBI webpages takes more effort. See also the section above on [Conda virtual environments](#Conda-virtual-environments). Conda needs to be set-up correctly for this to work.

* [How to download multiple genomes](genome_downloads.md)

## Downloading data from filesender2

In a lot of projects it is important to be able to share sequence data. The University of Oslo has a system in place that can help you share data between yourself and collaborators in the rest of the world. You find it at: [https://filesender2.uio.no/](https://filesender2.uio.no/). It is very easy to use and the filesize seems unlimited. What is not so easy to do is to get the data that was uploaded to filesender, downloaded straight to Saga. But you need Feide credentials to ues them.

Here is my recipe for when people send me large sequence data and I want to use it on Abel.

1. Login to filesender2 and send out a request for data, unclick the box to allow for more uploads per link (in case your collaborators fail to put everything in one tar file.
2. Collaborator uploads one or more files to the fileserver2 and you get a notification in your mailbox.
3. In the notification mail that data is uploaded, Follow the link to the data for download in your webbrowser. It opens a page on the filesender webpage. 
4. Go to the file you want to download and "right" click the download box and copy the link address.
5. In your Terminal, go to abel and the location where you want to store the data. Somewhere on `/projects/nn9305k/`:
6. Then type in the terminal: `wget -O myfile.data` and paste the link address from 4 at the end but within quotations marks. like this: ``` wget -O large_genome.gz "YOURLINK" ``` 
7. Than the data is downloaded to a file with the filename given in 6.

Note, that you can download multiple file in one go as a zip file by scrolling to the bottom of the file list and copy the link produced by the box: `Download as single (.zip) file`
