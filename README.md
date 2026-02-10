# Reheader for freebayes
```
samtools view -H fem_pygm_ELI1682_sorted_rg.bam > temp.sam
sed -i 's/ID:4/ID:fem_pygm_ELI1682/g' temp.sam
samtools reheader temp.sam fem_pygm_ELI1682_sorted_rg.bam > fem_pygm_ELI1682_sorted_rg_.bam
```

# Parsetab after freebayes

Need to swap out missing genotypes that look like this './' for ones that look like this './.'
```
sed -i 's/\.\/      /\.\/\. /g' cliv_WGS_mapped_to_clivref_multi_sample_results.vcf.gz.tab
```

# samtools quickcheck
```
samtools quickcheck -v *.bam > bad_bams.fofn   && echo 'all ok' || echo 'some files failed check, see bad_bams.fofn'
```


# md5sum.md
```
md5sum *
cat *.md5 >md5z
md5sum -c md5z
```
# tar files

```
for dir in FILENAME; do base=$(basename "$dir"); tar -czf "${base}.tar.gz" --remove-files "$dir"; done
```

# rsync files
```
rsync -axvH --no-g --no-p /archive/ben/z*.tar.gz ben@gra-dtn1.computecanada.ca:/nearline/def-ben/archive/
```

# remove a site using a tab delimted file (chrXXX posXX). The carrot before the file name means it will be removed.

```
bcftools filter -T ^chr02a_99399031.exclude FandM_chr02a_BSQR_jointgeno_allsites_filtered_SNPsonly.vcf2.gz -o FandM_chr02a_BSQR_jointgeno_allsites_filtered_SNPsonly_no_chr02a_99399031.vcf.gz
```

# touch files 
```
cp /scratch/scratch_to_delete/ben temp
sed -i 's/\/lustre04//g' temp
cut -d"," -f1 temp > tempp
sed -i 's/"//g' tempp
<tempp xargs -d'\n' touch
```
On beluga:
```
cut -f1 -d "," /scratch/to_delete/ben > temp.txt
sed -i -e 's/"//g' temp.txt
<temp.txt xargs -d'\n' touch
```


# fix permissions
```
cd ~/projects/
chgrp -R rrg-ben rrg-ben/ben
chgrp -R def-ben def-ben/ben
chmod -R g+s rrg-ben/ben/
chmod -R g+s def-ben/ben/
```
# install perl modules
I think this works now:
```
module load StdEnv/2023 perl/5.36.1
cpan install List::MoreUtils
```
this is the old version:
```
module load StdEnv/2020 perl/5.30.2
cpan
o conf mbuildpl_arg "--install_base ~/projects/rrg-ben/ben/perl5"
o conf makepl_arg "PREFIX=~/projects/rrg-ben/ben/perl5"
o conf prefs_dir "~/projects/rrg-ben/ben/.cpan/prefs"
o conf commit
install File::ShareDir::Install
```
and now just this:
```
cpan install ExtUtils::MakeMaker
```
# Install perl modules on laptop
```
sudo cpan Statistics::Descriptive
```

# Install R packages from git
I've had a few problems with pat requirements; try this first
```
gitcreds::gitcreds_delete()
```
and then this in RStudio:
```
remotes::install_github("https://github.com/clauswilke/colorblindr")
```

# Install R packages locally on computecanada

load R:
```
module load  StdEnv/2020 r/4.3.1
```
Make a directory for packages for a specific version of R (currently 4.3.1):
```
mkdir -p ~/.local/R/$EBVERSIONR/
```
Add this path to the R_lib variable:
```
export R_LIBS=~/.local/R/$EBVERSIONR/
```
Install packages:
```
R CMD INSTALL -l '~/.local/R/4.3.1/' ggplot2_3.4.2.tar.gz
```
or within R:
```
install.packages('tidyverse', dependencies=TRUE, type="source")
```

# Downloading from SRA db
```
module load StdEnv/2020 gcc/9.3.0 'sra-toolkit/3.0.0'
fasterq-dump SRR6347841 SRR6347842 SRR6347843 SRR6347844 SRR6347845 SRR6347846 SRR6347847 SRR6347848 SRR6347849 SRR6347850
```
# get max of column 4 in a file (file) with a header on the first line:
```
sed '1d' file |  awk '$4 > max { max = $4; output = $4 } END { print output }'
```
# Install python modules using pip
```
# to run pip
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
deactivate
```
# getting quota for nearline
```
lfs quota -hg rrg-ben /nearline
```

# Uploading files to NCBI using aspera

Best is now ncbi supports sftp. So we can do this directly from nibi. Navigate to a directory with all the fq.gz files and only these files Then type this (you will need the password provided by NCBI when the metadata were uploaded):
```
sftp subftp@sftp-private.ncbi.nlm.nih.gov
sftp> cd uploads/evansb_mcmaster.ca_4GYExljV
sftp> mkdir SUB123456_related_data
sftp> cd SUB123456_related_data
sftp> put *
```

I previously have been using Fetch and Cyberduck but this takes forever because one needs to download files locally and then upload them to NCBI

Instead best to use Aspera, which can be installed and run directly on computecanada:

Easiest to use a module 

```
 module load ascp/3.5.4
```
Or
```
https://docs.alliancecan.ca/wiki/Transferring_files_with_Aspera_Connect/ascp
```
This command worked for me, but only when I was logged into gra-dtn1 (and not graham):

```
ascp -i /home/ben/projects/rrg-ben/ben/aspera/aspera.openssh -QT -l100m -k1 -d /home/ben/projects/rrg-ben/ben/aspera/Silurana/ subasp@upload.ncbi.nlm.nih.gov:uploads/evansb_mcmaster.ca_QL5zT65r
```

# Problems with group ownership

First identify problem files:
```
lfs find ~/projects/*/ -group $USER
```
then for each one type this:
```
chown -h -R $USER:rrg-ben path_and_filename
```
where path_and_filename are the problem ones reported in the first command.  Additional details are here:
https://docs.alliancecan.ca/wiki/Frequently_Asked_Questions#sbatch:_error:_Batch_job_submission_failed:_Socket_timed_out_on_send/recv_operation

# Paste tab in terminal
To temporarily allow pasting tabs in bash, run:
```
bind '"\t":self-insert'
```
To re-enable tab completion, run:
```
bind '"\C-i":complete'
```

# Pip install with numpy
```
[~] $ module load StdEnv/2023 python/3.13.2
[~] $ virtualenv /home/$USER/my_venv
[~] $ source /home/$USER/my_venv/bin/activate
(my_venv) [~] $ pip install --config-settings="--prefix=/home/ben/projects/rrg-ben/ben/2025_allo_PacBio_assembly/" git+https://github.com/deeptools/deepTools.git
```
# Convert ora to fastq  
```
module load orad/2.6.1
orad --ora-decompress true --ora-input <input_file>.fastq.ora --ora-reference /path/to/ora-reference --output-directory <output_directory>
```
