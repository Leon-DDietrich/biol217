# Day2

## From raw reads to configs

- Metagenome whole genomes, not amplicons

- quality control to remove errors and contaminants

    - adapters
    - barcodes
    - phiX174_virus
    - host genome (p.e. human)

- 

## Assembly

- overlap assembly
- de Bruijn graph

will be learned the next three days

for i in list; do command; done

## Qualitiy check and trimming

```
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=10G
#SBATCH --time=1:00:00
#SBATCH --job-name=fastp
#SBATCH -D ./
#SBATCH --output=fastp.out
#SBATCH --error=fastp.err
#SBATCH --partition=all
#SBATCH --reservation=biol217

module load miniconda3/4.7.12.1

conda activate anvio

#fastqc 

#module load fastqc

#for i in *.gz; do fastqc $i; done

#fastp
cd /work_beegfs/sunam229/day2/

mkdir ./clean_reads

for i in `ls *_R1.fastq.gz`;
do
    second=`echo ${i} | sed 's/_R1/_R2/g'`
    fastp -i ${i} -I ${second} -R _report -o ./clean_reads/"${i}" -O ./clean_reads/"${second}" -t 6 -q 20

done
```
## Megahit assembly

```
cd /work_beegfs/sunam229/day2/clean_reads
                                    
megahit -1 BGR_130305_R1.fastq.gz -1 BGR_130527_R1.fastq.gz -1 BGR_130708_R1.fastq.gz -2 BGR_130305_R2.fastq.gz -2 BGR_130527_R2.fastq.gz -2 BGR_130708_R2.fastq.gz --min-contig-len 1000 --presets meta-large -m 0.85 -o /work_beegfs/sunam229/day2/3_coassembly -t 20 
```
