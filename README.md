# SV_calling
Install NGMLR and sniffles:
'''
module load anaconda3/2024.03_deb12
source /mnt/nfs/clustersw/Debian/bullseye/anaconda3/2024.03_deb12/activate_anaconda3_2024.03_deb12.txt
conda create -n sniffles_and_NGMLR
conda activate sniffles_and_NGMLR
conda install sniffles=2.4
conda install bioconda::ngmlr
'''
