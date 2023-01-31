# Day 7

## Transcriptomics working with R studio

activating conda:

```
conda activate /home/sunam226/.conda/envs/grabseq
```
keywords in paper to find the data storage

- NCBI
- SRA
- accession

using the link related to the keywords, finding the different samples and their SRR numbers from Prasse et al. 2017:

- SRR4018517
- SRR4018516
- SRR4018515	
- SRR4018514	

get the files:

```
fasterq-dump SRR4018517 SRR4018516 SRR4018515 SRR4018514
```

downloading metadata file from Kröger et al:

```
grabseqs sra -l -t 4 -m metadata_Kroeger.csv  SRP028811
```

quality check:

```
module load fastqc

fastqc -t 4 -o fastqc_output/ *.fastq
```

merge the files:

```
multiqc -d . -o ./multiqc_output/
```

file format needed for analysis:

GFF/GTF

## important design questions

### Are more replicates or deeper sequencing better?

more replicates

### How many replicates are necessary?

depends on the money available

### What is an optimal read length?

50, 100 or 150

## Working with Reademption

activating conda reademption:

```
conda activate /home/sunam226/.conda/envs/reademption
```
creating a folder structure:

```
reademption create --project_path READemption_analysis --species salmonella="Salmonella Typhimurium"
````

alignment of the sequences:

```
module load miniconda3/4.7.12.1
source activate
conda activate /home/sunam226/.conda/envs/reademption
reademption align -p 4 --poly_a_clipping --project_path READemption_analysis
reademption coverage -p 4 --project_path READemption_analysis
reademption gene_quanti -p 4 --features CDS,tRNA,rRNA --project_path READemption_analysis
reademption deseq -l InSPI2_R1.fa.bz2,InSPI2_R2.fa.bz2,LSP_R1.fa.bz2,LSP_R2.fa.bz2 -c InSPI2,InSPI2,LSP,LSP -r 1,2,1,2 --libs_by_species salmonella=InSPI2_R1,InSPI2_R2,LSP_R1,LSP_R2 --project_path READemption_analysis
reademption viz_align --project_path READemption_analysis
reademption viz_gene_quanti --project_path READemption_analysis
reademption viz_deseq --project_path READemption_analysis
```

now with the dataset from above from Prasse et al. 2017:

We had to download the reference sequences in fasta and gff for Methanosarcina mazei from NCBI by going on the genome dataset, clicking on the RefSeq link, click on fasta, click on GenBank and download the files at "send to"

After that, a batch script can be written similar to the given one, changing the deseq names and adding "--fastq" in the align command

```
module load miniconda3/4.7.12.1
source activate
conda activate /home/sunam226/.conda/envs/reademption
reademption align --fastq -p 4 --poly_a_clipping --project_path READemption_analysis
reademption coverage -p 4 --project_path READemption_analysis
reademption gene_quanti -p 4 --features CDS,tRNA,rRNA --project_path READemption_analysis
reademption deseq -l sRNA_R1.fa.bz2,sRNA_R2.fa.bz2,wt_R1.fa.bz2,wt_R2.fa.bz2 -c sRNA,sRNA,wt,wt -r 1,2,1,2 --libs_by_species methanosarcina=InSPI2_R1,InSPI2_R2,LSP_R1,LSP_R2 --project_path READemption_analysis
reademption viz_align --project_path READemption_analysis
reademption viz_gene_quanti --project_path READemption_analysis
reademption viz_deseq --project_path READemption_analysis
conda deactivate

jobinfo

```






