# Lab kraken2

**[ Kraken 2](https://github.com/DerrickWood/kraken2/wiki/About-Kraken-2)** is one of the most popular tools used for assigning taxonomic labels to short DNA sequences.  

At the core of [Kraken](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2014-15-3-r46) is a database that contains records consisting of a k-mer and the LCA of all organisms whose genomes contain that k-mer. This database, built using a user-specified library of genomes, allows a quick lookup of the most specific node in the taxonomic tree that is associated with a given k-mer. Sequences are classified by querying the database for each k-mer in a sequence, and then using the resulting set of LCA taxa to determine an appropriate label for the sequence. Sequences that have no k-mers in the database are left unclassified by Kraken. 

![alt text](https://github.com/2021-environmental-bioinformatics/Lab_kraken2/blob/main/images/Kraken1.png)
*The Kraken sequence classification algorithm. To classify a sequence, each k-mer in the sequence is mapped to the lowest common ancestor (LCA) of the genomes that contain that k-mer in a database. The taxa associated with the sequence’s k-mers, as well as the taxa’s ancestors, form a pruned subtree of the general taxonomy tree, which is used for classification. In the classification tree, each node has a weight equal to the number of k-mers in the sequence associated with the node’s taxon. Each root-to-leaf (RTL) path in the classification tree is scored by adding all weights in the path, and the maximal RTL path in the classification tree is the classification path (nodes highlighted in yellow). The leaf of this classification path (the orange, leftmost leaf in the classification tree) is the classification used for the query sequence.*


[Kraken 2](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1891-0) improved upon Kraken 1 by reducing memory usage by 85%, allowing greater amounts of reference genomic data to be used, while maintaining high accuracy and increasing speed fivefold. Kraken 2 also introduces a translated search mode, providing increased sensitivity in viral metagenomics analysis.

![alt text](https://media.springernature.com/full/springer-static/image/art%3A10.1186%2Fs13059-019-1891-0/MediaObjects/13059_2019_1891_Fig1_HTML.png?as=webp)
*Differences in operation between the two versions of Kraken. a Both versions of Kraken begin classifying a k-mer by computing its ℓ bp minimizer (highlighted in magenta). The default values of k and ℓ for each version are shown in the figure. b Kraken 2 applies a spaced seed mask of s spaces to the minimizer and calculates a compact hash code, which is then used as a search query in its compact hash table; the lowest common ancestor (LCA) taxon associated with the compact hash code is then assigned to the k-mer. In Kraken 1, the minimizer is used to accelerate the search for the k-mer, through the use of an offset index and a limited-range binary search; the association between k-mer and LCA is directly stored in the sorted list. c Kraken 2 also achieves lower memory usage than Kraken 1 by using fewer bits to store the LCA and storing a compact hash code of the minimizer rather than the full k-mer. d Impact on speed, memory usage, and prokaryotic genus F1-measure in Kraken 2 when changing k with respect to ℓ (ℓ = 31, s = 7 for all three graphs). e Impact on prokaryotic genus sensitivity and positive predictive value (PPV) when changing the number of minimizer spaces s (k = 35, ℓ = 31 for both graphs). In d and e, the data are from our parameter sweep results in Additional file 1: Table S2, and the default values of the independent variables for Kraken 2 are marked with a circle.*

## Install kraken2
```
conda create -n kraken2
conda activate kraken2
conda install -c bioconda kraken2 
```
## Kraken 2 Database
The kraken2 [PlusPF](https://benlangmead.github.io/aws-indexes/k2) database (contains archaea, bacteria, viruses, plasmids, human, UniVec_Core, protozoa and fungi) is located in `/vortexfs1/omics/env-bio/collaboration/databases/`.

You can find all the information about the database:
``` 
kraken2-inspect --db /vortexfs1/omics/env-bio/collaboration/databases/kraken2db_pluspf/
```

## Assigning taxonomic information to paired-end reads
We will use the small subsets of reads we used for the assembly (located in `/vortexfs1/omics/env-bio/collaboration/sequences/megahit_example/`).

Check the kraken2 help page. Remember to start an `srun` before executing the command. Request 4 nodes and 80Gb of memory.
