# SV_calling

Downloading the reads and decompressing the files:
```

```
Install NGMLR and sniffles:
```
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda create -n sniffles_and_NGMLR
conda activate sniffles_and_NGMLR
conda install sniffles=2.4
conda install bioconda::ngmlr
conda deactivate
```
Install samtools/1.9 (Newer samtools versions did not work directly with the output of NGMLR):
```
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda create -n samtools_1.9
conda activate samtools_1.9
conda install samtools=1.9
conda deactivate
```

Align the reads using NGMLR (you could also use minimap2):
```
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda activate NGMLR

ngmlr -x ont -t 100 -r YOURGENOME.fasta -q READS.fastq.gz -o alignment.sam
```
Convert to bam, sort, and index:
```
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda activate samtools_1.9

samtools view -@20 -bS alignment.sam > alignment.bam
samtools sort -@20 -o alignment.sorted.bam alignment.bam
samtools index alignment.sorted.bam
conda deactivate
```
Call structural variants using sniffles (for more than one sample):
```
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda activate sniffles_and_NGMLR

module load samtools

sniffles --input alignment1.sorted.bam --threads 60 --snf alignment1.snf #--output-rnames 
sniffles --input alignment2.sorted.bam --threads 60 --snf alignment2.snf #--output-rnames 
#sniffles --input alignment1.snf alignment2.snf --vcf multisample.vcf --threads 60 
conda deactivate

```
