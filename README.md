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
</scratch/scratch_to_delete/ben xargs -d'\n' touch
```

# fix permissions
```
cd ~/projects/
chgrp -R rrg-ben rrg-ben/ben
chgrp -R def-ben def-ben/ben
chmod -R g+s rrg-ben/ben/
chmod -R g+s def-ben/ben/
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

# Downloading from SRA db
```
module load StdEnv/2020 gcc/9.3.0 'sra-toolkit/3.0.0'
fasterq-dump SRR6347841 SRR6347842 SRR6347843 SRR6347844 SRR6347845 SRR6347846 SRR6347847 SRR6347848 SRR6347849 SRR6347850
```
