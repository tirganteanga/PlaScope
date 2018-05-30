<p align="center"><img src="/PlaScope_supernova.png" width="50%"></p>

# PlaScope


A targeted approach to assess the plasmidome of bacteria.

If you use our approach, please cite :

## Tell me more about PlaScope


This method enables you to classify contigs from a WGS assembly according to their location (i.e. plasmid or chromosome). It is based on a smart tool called Centrifuge (https://github.com/infphilo/centrifuge), initially developped as a metagenomic classifier.
We proposed here an application on *E. coli* plasmidome, with a specific database build on one hand on completely finished genomes of *E. coli* from the NCBI, and on the other hand on a custom plasmid database. In fact 3 databases of plasmid have been merged together : plasmids used to create plasmidfinder (http://aac.asm.org/content/58/7/3895.long), plasmids proposed by Orlek *et al.* (https://www.sciencedirect.com/science/article/pii/S2352340917301567?via%3Dihub) and plasmids from the RepliColScope project (http://www.agence-nationale-recherche.fr/Project-ANR-10-GENM-0012 / https://www.ebi.ac.uk/ena/data/view/PRJEB24625)

However we think that this method can easily be applied to other bacterial species since you have got enough reference data.


## Dependencies

You must install these dependencies before you start :

SPAdes 3.10.1 or later if you want to run the assembly (= mode 1) (header of contigs must be the same as in version 3.10.1) (http://bioinf.spbau.ru/spades)

Centrifuge 1.0.3 (https://github.com/infphilo/centrifuge)


## Classification of contigs according to their location

You can choose between two modes:
⋅⋅* Mode 1: SPAdes assembly then contig classification
⋅⋅* Mode 2: contig classification only (if you already assembled your genome with SPAdes)

```
./plaScope.sh -h

usage: plaScope.sh [OPTIONS] [ARGUMENTS]

General options:
  -h, --help		display this message
  -t			number of threads[OPTIONAL] [default : 8]
  -o			output directory [OPTIONAL] [default : current directory]
  --sample		Sample name [MANDATORY]
  --db_dir		path to centrifuge database [MANDATORY]
  --db_name		centrifuge database name [MANDATORY]  
  
Mode 1: SPAdes assembly + contig classification 
  -1			forward paired-end reads [MANDATORY]
  -2			reverse paired-end reads [MANDATORY]
  
  
Mode 2: contig classification of a fasta file (only if you already have your SPAdes assembly!)
  --fasta		SPAdes assembly fasta file [MANDATORY]



Example mode 1:
plaScope.sh -1 my_reads_1.fastq.gz -2 my_reads_2.fastq.gz -o output_directory  --db_dir path/to/DB --db_name chromosome_plasmid_db --sample name_of_my_sample

Example mode 2:
plaScope.sh --fasta my_fastafile.fasta -o output_directory --db_dir path/to/DB --db_name chromosome_plasmid_db --sample name_of_my_sample
````

## *E. coli* database

To download *E. coli* database, please download the 3 required files on Zenodo: http://doi.org/10.5281/zenodo.1245664 

See Reference_chromosome.tab for the list of *E. coli* chromosome used in our exemple
See Reference_plasmid.tab for the list of plasmids and their related database.

## Create your own database

You can also design your own database as explained On Centrifuge website https://ccb.jhu.edu/software/centrifuge/manual.shtml#custom-database.

You need to prepare four files files:

-database.fna : a multifasta file of your database  
-nodes.dmp : an artificial taxonomy (*i.e.* root, chromosome, plasmid)  
-seqid_to_taxid.map : a mapping file between the sequences and their taxonomic assignment  
-names.dmp : a file mapping taxonomy IDs to a name  

Then, build your database as follow:

```
centrifuge-build -p 10 --conversion-table seqid_to_taxid.map --taxonomy-tree nodes.dmp --name-table names.dmp database.fna chromosome_plasmid_db
```
