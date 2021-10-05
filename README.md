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

For now, though, we are going to ask a pretty simple question: which of the genes in a _de novo_ transcriptome are likely to be mitochondrial? In this repo, you'll find a semi-cleaned de novo transcriptome for the hydrothermal vent crab, _Bythograea thermydron_.

[grab txm from NCBI? or just give it to them?]

## Build a BLAST database of your "reference" sequences



Run makeblastdb (from folder where you would like output files to be generated)
	[PATH]/makeblastdb -in [REFERENCE_FILENAME].fasta -dbtype nucl
# Other useful flags:
#	-dbtype prot	For a database of protein sequences (default)

## Check query sequences against local reference database
# Run blastn (from folder where you would like output files to be generated)
	[PATH]/blastn -db [REFERENCE_FILENAME].fasta -query [QUERY_FILENAME].fasta -out [OUT_FILENAME].out

# Note: MANY flag options are available to change parameters of seach and output
# See: https://www.ncbi.nlm.nih.gov/books/NBK279675/
# Some useful flags:
#	-outfmt 6	Creates tabular output file (can be further customized by changing number, etc.)
#			eg, -outfmt "6 std stitle" prints default fields plus full sequence title
#	-task		blastn (default), blastn-short, megablast, dc-megablast
#	-num_alignments Only prints top X alignments
#	-evalue		Maximum e-value to return hits (default: 10, preferred: 1e-3)

