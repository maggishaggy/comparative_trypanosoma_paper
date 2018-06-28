# comparative_trypanosoma_paper
Custom scripts for recreating some of the analyses from the trypanosoma genomic comparisons work.

[1. Genome Completion Pipeline](#1-genome-completion-pipeline)  
[2. Genome Annotation Pipeline](#2-genome-annotation-pipeline)  
[3. Edit Alignments for Phylogeny](#3-edit-alignments-for-phylogeny)  
[4. Percent Identity Calculation](#4-percent-identity-calculation)  
[5. Gene Cluster Analysis](#5-gene-cluster-analysis)  
[6. Copy Number Estimation](#6-copy-number-estimation)  
[7. Parse Pseudogene Predictions](#7-parse-pseudogene-predictions)  
[8. Heterozygosity Pipeline](#8-heterozygosity-pipeline)  
[9. Metabolism Gene Length Coverage](#9-metabolism-gene-length-coverage)  

## 1. Genome Completion Pipeline

### gce_tcruzi.sh

Description: Pipeline for genome completion evaluation. We designate this tool 'Genome assembly Completion and Integrity Analyzer' (GenoCIA), as it estimates assembly completion and gene calling integrity. This tool performs two tasks: (i) randomly selects 2, 4, 6, 8, 10, 20, 30, 40, 50, 60, 70, 80, 90 and 99% of the reads and performs assemblies using Newbler with these read subsets; and (ii) uses tBLASTn to determine the presence of a curated set of 2,217 kinetoplastid orthologous single copy genes at 25, 50, 75, 90 and 99% alignment lengths (merging reference gene alignment lengths over multiple contigs or genes where necessary). 

Usage: `gce_tcruzi <path to assembly fasta folder> <organism short name>`

Input  
- path to assembly fasta folder
- organism short name

Output  
- estimates of assembly completion and gene calling integrity

## 2. Genome Annotation Pipeline

### gap.pl

Description: Pipeline for genome annotation.

Usage: `perl gap.pl`

Input  
--outdir         Output directory path (REQUIRED)  
--fasta          Assembly in fasta format (REQUIRED)  
--short_name     Short Name for the organism (REQUIRED)  
--org            Type of organism, i.e., 1=Bacteria 2=Eukaryote(default)  

Output  
Results for the following options are available:  
--call_genes     Call genes using Glimmer or GeneMark  
--trna_scan      Run trnaScan-SE  
--rpsblast       rpsblast against COG/PFAM (requires called genes)  
--blastx         blast assembly against protein database (NR by default)  
--rnammer        find RNAs using rnammer-1.2  
--asgard         perform metabolic reconstruction using ASGARD  

### get_best_annotated_hit_v2.py

Description: This is a separate but complementary script to the Genome Annotation Pipeline. It searches a database of the best annotation available.

Usage: `python get_best_annotated_hit_v2.py <nr_blastp_results> <outfile> > <logfile>`

Input  
- nr BLASTp results

Output  
- Best hits for each gene

## 3. Edit Alignments for Phylogeny

### get_shorts.py

Description: Get a list of alignments that should be excluded based on containing sequences <25% of the median sequence length in the alignment.

Usage: `for f in *.gblocks.fasta.infoalign; do perl get_shorts.pl $f >> exclusion_list_difflengths;done`

Input
- infoalign output files containing sequence lengths in the alignments

Output
- List of alignments to be exluded from the percent identity and phylogeny analyses

## 4. Percent Identity Calculation

### get_perc_id_general.py

Description: Script for calculating percent identity from aligned nucleotide or amino acid sequences.

Usage: `python get_perc_id_general.py <fastafile>`

Input  
- fastafile: alignment in fasta format

Output  
- percent identity

## 5. Gene Cluster Analysis

### gen_orthofinder_stats_automated.pl

Description: Generates stats for a venn diagram of shared gene clusters across the organisms.

Usage: `perl gen_orthofinder_stats_automated.pl`

Input  
- OrthologousGroups.txt (output from OrthoFinder)

Output  
- Stats file containing cluster counts for each intersection or species specific group.

### groups2binnedcounts_orthofinder_perc_surface_prots_threshold.py

Description: Determines the frequency of various gene cluster sizes, and the percent surface or secreted proteins for each gene cluster size, within each organism.

Usage: `python groups2binnedcounts_orthofinder_perc_surface_prots_threshold.py`

Input  
- OrthologousGroups.txt (output from OrthoFinder)
- List of genes containing a positive prediction from TMHMM, KOHGPI or SignalP

Output  
- Stats .txt file

## 6. Copy Number Estimation

## 7. Parse Pseudogene Predictions

## 8. Heterozygosity Pipeline

### freebayes_vcftools_ht_pipeline.sh

## 9. Metabolism Gene Length Coverage

### blast2cov.py

Description: This script was used post-ASGARD analysis to further check whether some genes initially marked as absent were likely to be present. The script indicates whether genes are covered by a threshold level of sequencing reads across their entire length: provides one type of evidence for genomic presence of a gene.

Usage: `python blast2cov.py <tBLASTn results file> <outfile>`

Input  
- tBLASTn results for genes of interest vs. reads

Output  
- List of genes that are legitimately "FOUND" based on the reads and the given criteria (currently set to genes where at least 4 BLAST-aligned reads cover over 60% of the gene length).


