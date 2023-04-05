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

Make a directory for packages for a specific version of R (currently 4.2.2):
```
mkdir -p ~/.local/R/$EBVERSIONR/
```
Add this path to the R_lib variable:
```
export R_LIBS=~/.local/R/$EBVERSIONR/
```
Install packages:
```
R CMD INSTALL -l '~/.local/R/4.2.2/' ggplot2_3.4.2.tar.gz
```
