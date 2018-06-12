# ChloroplastAnnotation
## Helper scripts for annotating Chloroplast sequences amplified by 16S universal primers

This pipeline is an extension to the 16S amplicon processing SOP provided at the Microbiome Helper Wiki:

https://github.com/LangilleLab/microbiome_helper/wiki/16S-Bacteria-and-Archaea-Standard-Operating-Procedure

Essentially, we utilize this SOP to annotate Chloroplast sequences using the Greengenes database with QIIME scripts. But, since the taxonomy information for chloroplast sequences is not extensive, we use an additional process of separating the Chloroplast reads from the dataset (as annotated by QIIME using Greengenes) and picking OTUs again using the PhytoRef database (http://phytoref.sb-roscoff.fr/) with the taxonomy info included therein.

## Installation
### Prerequisites
* A working QIIME version is required

For example, if you are using QIIME in a virtual environment, you need to activate it first

<code> source ~/miniconda3/bin/activate qiime1 </code>

* Perl5 with the BioPerl package

* Microbiome Helper scripts (optional)

\*Note that the path to activate QIIME would be different on your computer depending on where QIIME is installed.

### Script and Files
No installation is required to use this helper script for chloroplast annotation. If you are using the Micorbiome Helper scripts, just make sure to put the script in a location which is included in the $PATH environment variable E.g /home/<username>/bin or /usr/local/bin. 
  
In any case, the permissions for the script need to be changed to make it executable

<code> chmod u+x extract_sequences_for_OTU_ids.pl </code>

And then,

<code> cp extract_sequences_for_OTU_ids.pl /home/\<username\>/bin </code>

The PhytoRef sequences Fasta, alignment and taxonomy files need to be copied to a databases folder (if existing). Wherever these files are stored, the path to these files will need to be specified while running the QIIME scripts.

## Usage

* Run QIIME scripts to generate an OTU table as explained in the SOP at https://github.com/LangilleLab/microbiome_helper/wiki/16S-Bacteria-and-Archaea-Standard-Operating-Procedure

* Separate the Chloroplast like OTUs (those sequences annotated as Chloroplast using the GreenGenes DB in the above SOP) by using the QIIME script **filter_taxa_from_otu_table.py**

<code> filter_taxa_from_otu_table.py -i otu_table.biom -o otu_table_Chloroplast.biom -p c__Chloroplast </code>

* Convert the Chloroplast OTU table from .biom format to .txt formal using the command **biom convert**

<code> biom convert -i otu_table_Chloroplast.biom -o otu_table_Chloroplast_w_tax.txt --to-tsv --header-key taxonomy </code>

* Get a list of OTU IDs for the Chloroplast OTUs. You could use the Unix command **cut** for doing this or Alternatively, use text editors or spreadsheets for the purpose

<code> cut -f 1 otu_table_Chloroplast_w_tax.txt > Chloroplast_OTU_IDs.txt </code>

\*Note: If you use **cut**, you would need to remove the first line of the resulting file, as this contains the column header

* Extract all the reads mapping to these OTU IDs using the Perl script **extract_sequences_for_OTU_ids.pl**. This script requires the QIIME environment to be acticvated. For a full list of options to use with this script and how to use it, just type <code> extract_sequences_for_OTU_ids </code> on the command line and press Enter.

  Basically, it takes as input the following files
  * a file containing the list of OTU IDs (one OTU ID per line) that we generated above
  * a mapfile generated by the QIIME command **pick_open_reference_otus.py**; If you have followed the Microbiome Helper SOP, this file should be in the **clustering** folder generate by the command; The name is generally of the type **final_otu_map_mc1.txt**
  * A sequence Fasta file conating all the reads from all your samples; In the Microbiome Helper SOP, you can find this in a folder called **combined_fasta** with the name **combined_seqs.fna**
  * an output folder where the extracted seqeunce files and other intermediate files will be stored; Make sure that this folder actually exists before runnning

  Run the script as follows:

<code> perl extract_sequences_for_OTU_ids.pl --lst OTUIDS_list.txt --mapfile clustering/final_otu_map_mc1.txt --seqfile combined_fasta/combined_seqs.fna --out OTUlist-extracted-reads </code>

* The above command will generate a Fasta file containing the reads mapping to the OTUs in OTUID list file. The next step is to pick open reference OTUs using the QIIME script **pick_open_reference_otus.py**. You can use the template parameters file given here to instruct QIIME to use the PhytoRef Fasta, alignment and taxonomy files formatted according to the QIIME script.

<code> pick_open_reference_otus.py -i OTUlist-extracted-reads/Chloroplast_OTU_IDs.txt.extracted.fasta -o Chlaroplast_clustering/ -p Chloroplast_clustering_params.txt -m sortmerna_sumaclust -s 0.1 -v --min_otu_size 1  </code>
  
* This should result in an OTU file (in the biom format) containing OTUs picked using the PhytoRes picked. From this point on, you could use the scripts and steps mentioned in the 16S SOP to generate OTU relative abundance plots for the differnet samples, Alpha and Beta diversity plots etc. 
