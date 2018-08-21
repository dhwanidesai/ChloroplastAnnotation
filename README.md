# ChloroplastAnnotation
Here, we describe some protocols and workflows for identifying and classifying chloroplast sequences from 16S amplicon sequencing runs and visualizing the phylogenetic placements of these sequences on a reference chloroplast tree constructed from the Phytoref database (http://phytoref.sb-roscoff.fr/). Check out our Wiki page for more details:
https://github.com/dhwanidesai/ChloroplastAnnotation/wiki

We start off with one using the Operational Taxonomic Unit concept of QIIME1 where we utlize the GreenGenes 16S database to identify putative chloroplast sequence OTUs and then use the Phytoref database as a second pass for re-annotating chloroplast OTUs with acccurate taxonomy from the Phytoref database. More details here:
https://github.com/dhwanidesai/ChloroplastAnnotation/wiki/Chloroplast-amplicon-sequence-annotation-using-QIIME1

We also describe protocol for chloroplast annotation using the current QIIME2 package, where, instead of OTUs, we identify actual denoised Sequence Variants with the Deblur method (https://github.com/biocore/deblur). This protocol is essentially based on the SOP described at the Microbiome Helper Wiki (Comeau et al., 2017) page:
https://github.com/LangilleLab/microbiome_helper/wiki/Amplicon-SOP-v2
More details here:
https://github.com/dhwanidesai/ChloroplastAnnotation/wiki/Chloroplast-amplicon-sequence-annotation-using-Deblur-in-QIIME2
