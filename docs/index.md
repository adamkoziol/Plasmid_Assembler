# What is Plasmid Extractor?

Plasmid Extractor is a command-line pipeline to search raw reads for plasmids and reconstruct the sequences of any plasmids found. To do this, it uses a fully customizable database of **_over 9000_** plasmid
sequences from RefSeq, enabling plasmid finding with higher sensitivity and specificity than other published tools.

# How Does Plasmid Extractor Work?

ADD A PICTURE HERE ONCE PIPELINE WORKFLOWS ARE FINALIZED

First, PlasmidExtractor looks at the input directory specified by the user to find paired reads and generates a list of samples. For each sample found, the following workflow is followed:

1. Quality trim reads using BBDuk. 
2. Bait out reads from the trimmed read set that have matches to the plasmid database, again using BBDuk.
3. Perform a preliminary screen for plasmid content using Mash.
4. For each potential plasmid found from the Mash screen, find out exactly how well it's represented by splitting the plasmid reads generated in step 2 and the plasmid sequence itself into 31-mers and finding what proportion of plasmid sequence 31-mers are covered by plasmid read 31-mers.
5. Filter out confirmed plasmid hits that have a high degree of similarity to one another, as the database may contain extremely similar plasmids. This step also faciliates the creation of custom databases, 
as it means that when adding new plasmids to the database it isn't necessary to check if that plasmid is already represented. In each group of similar plasmids, the plasmid hit with the highest score (proportion of plasmid sequence kmers that are covered by read kmers) is taken as a definitive hit.
6. A consensus sequence is created for each definitive hit is generated by mapping plasmid reads to reference sequences using BBMap, .

Once these 6 steps have been followed for each sample, the following post-analysis steps are taken:

1. Compare total plasmid content across samples using sourmash.
2. Identify AMR genes present on plasmid sequences using BLAST and ??? database.
3. Identify plasmid incompatibility group for each plasmid using BLAST and ??? database.
4. Identify pathogenicity genes present on each plasmid using BLAST and ??? database.

IMPORTANT NOTE: While AMR, incompatibility group, and pathogenicity gene detection have been implemented,
they have not been thoroughly tested. At this point, if you are interested in these features, it is recommended you do a search
of your own.
