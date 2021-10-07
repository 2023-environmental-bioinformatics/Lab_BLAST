# Lab_BLAST

BLAST is a many-splendored thing; it has a lovely online interface at GenBank, but it's also helpfully a command line program. If you want to do a quick check on a couple of sequences, online is great. But for a whole lot of BLASTing, you want `blast+` (command-line blast), which can be gloriously parallelized on the HPC.

## Set up conda environment

Get a fresh new conda environment set up for blasting:

```
conda create --name blast
conda activate blast
conda install -c bioconda blast
```

Ultimately, when you blast anything you are searching a query sequence (stuff you want to know about) to a set of reference sequences (stuff you presumably already know). You can use almost any collection of sequences as a reference as long as they're in fasta format. Genome, transcriptome, proteins, you name it. If you want to go big (and your HPC admins are OK with this!) you can even download the **entire** nr database from NCBI.

For now, though, we are going to ask a pretty simple question: which of the genes in a _de novo_ crab transcriptome are likely to be from a common microparasite of crabs? In this repo, you'll find a semi-cleaned, downsampled _de novo_ transcriptome for the hydrothermal vent crab, _Bythograea thermydron_. I built this transcriptome using four tissue types from two individual crabs from the East Pacific Rise (9N). We know surprisingly little about most parasites, but we know they are very very common, so let's search our transcriptome for contigs matching a common haplosporidian parasite to see if we can find it in the depths.

## Build a BLAST database of your "reference" sequences

In order to search efficiently, blast needs to have your reference sequences turned into a special database format. Use the `-dbtype nucl` flag for a DNA reference, or `-dbtype prot` for a protein reference.

```
makeblastdb -in Hepatospora_eriocheir_genome.fasta -dbtype nucl
```

Check out the contents of your directory. What happened?

## Check query sequences against local reference database


Run blastn from the folder where you would like output files to be generated.
```
blastn -db Hepatospora_eriocheir_genome.fasta -query Bt_multi_txm_10p.fasta -out Bytho_vs_hepato.out
```

This is a really basic alignment - there are tons of ways you can customize your similarity search. You can also use different types of blast - for example, we used `blastn` to align nucleotide to nucleotide, but we could also use `tblastx` to translate both reference and query before searching. If you had a protein database, you'd want to use the protein-based options like blast p (protein query - protein reference) or blastx (translated nucleotide query - protein reference).

MANY flag options are available to change parameters of seach and output  
See: https://www.ncbi.nlm.nih.gov/books/NBK279675/

Some useful flags:  
-outfmt 6	Creates tabular output file (can be further customized by changing number, etc.) eg, -outfmt "6 std stitle" prints default fields plus full sequence title  
-task		blastn (default), blastn-short, megablast, dc-megablast  
-num_alignments Only prints top X alignments  
-evalue		Maximum e-value to return hits (default: 10, preferred: 1e-3)

> Play around with these parameters for a few minutes, and use your command-line skills to parse the output. How many of your "crab" sequences do you think are really microsporidian parasite sequences?
