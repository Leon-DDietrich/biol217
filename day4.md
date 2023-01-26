# From bins to species and abundance estimation

bins=MAGs

QC raw data assembly fasta sam binning fasta

errors: - misassembly, mis-binning (major source)
        - contaminations in the lab

- CSS
- RSS 
  

## Tutorial

process configs
  ```
  conda activate /home/sunam225/miniconda3/miniconda4.9.2/usr/etc/profile.d/conda.sh/envs/anvio-7.1

anvi-gen-contigs-database -f /work_beegfs/sunam229/day3/mapping/contigs.anvio.fa -o /work_beegfs/sunam229/day3/anvio_profiles/contigs.db -n 'biol217'

```
 gene comparison with loaded database

```
anvi-run-hmms -c /work_beegfs/sunam229/day3/anvio_profiles/contigs.db
```
sorting
```
cd mapping/4_mapping/

for i in *.bam; do anvi-init-bam $i -o /work_beegfs/sunam229/day3/anvio_profiles/"$i".sorted.bam; done
```
merging configs
```
anvi-merge /work_beegfs/sunam229/day3/anvio_profiles/5_anvio_profiles/BGR_130305/PROFILE.db /work_beegfs/sunam229/day3/anvio_profiles/5_anvio_profiles/BGR_130527/PROFILE.db /work_beegfs/sunam229/day3/anvio_profiles/5_anvio_profiles/BGR_130708/PROFILE.db -o /work_beegfs/sunam229/day3/anvio_profiles/5_anvio_profiles/merged_profiles/ -c /work_beegfs/sunam229/day3/anvio_profiles/contigs.db --enforce-hierarchical-clustering
```
Now binning with two different programs
first one: METABAT
```
anvi-cluster-contigs -p /work_beegfs/sunam229/day3/5_anvio_profiles/merged_profiles/PROFILE.db -c /work_beegfs/sunam229/day3/5_anvio_profiles/contigs.db -C METABAT --driver metabat2 --just-do-it --log-file log-metabat2
```
second one: CONCOCT
```
anvi-summarize -p /work_beegfs/sunam229/day3/5_anvio_profiles/merged_profiles/PROFILE.db -c /work_beegfs/sunam229/day3/5_anvio_profiles/contigs.db -o SUMMARY_METABAT -C METABAT
```
DASTool is then used to choose the best bins
```
anvi-interactive -p merged_profiles/PROFILE.db -c /PATH/TO/contigs.db -C YOUR_COLLECTION
```
