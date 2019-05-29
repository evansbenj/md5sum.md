# md5sum.md
```
md5sum *
cat *.md5 >md5z
md5sum -c md5z
```
tar files

```
for dir in FILENAME; do base=$(basename "$dir"); tar -czf "${base}.tar.gz" --remove-files "$dir"; done
```
