# From contigs to bins and quality MAGs

- clustering sequences
- reference-basend or reference-free
- methods the program uses:
  - kmers
  - GC content
  - coverage data in multiple samples
  
- contamination
- completeness
- number of contigs
- heterogeneity

[link](linkitself.de) hyperlink

adding an image:

- put image in github synched folder
- ![image](imagepath)


cd /work_beegfs/sunam229/day3/fastp

for i in `ls *_R1.fastq.gz`;
do
    second=`echo ${i} | sed 's/_R1/_R2/g'`
    bowtie2 --very-fast -x /work_beegfs/sunam229/day3/mapping/contigs.anvio.fa.index -1 ${i} -2 ${second} -S "$i".sam 
done


bowtie2 --very-fast -x c/work_beegfs/sunam229/day3/mapping/ontigs.anvio.fa.index -1 /work_beegfs/sunam229/day3/fastp/BGR_130305_R1.fastq.gz -2 /work_beegfs/sunam229/day3/fastp/BGR_130305_R2.fastq.gz -S BGR_130305.sam



module load miniconda3/4.7.12.1

conda activate anvio

module load samtools
cd /work_beegfs/sunam229/day3/mapping/4_mapping
for i in *.sam; do samtools view -bS $i > "$i".bam; done
