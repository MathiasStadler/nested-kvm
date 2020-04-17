# neted-kvm

## increase root fs with xfs format

[from here](https://access.redhat.com/articles/1190213)

1) TODO delete in fdisk the old partion and add a new one with the same start vector
2) increase the file system

```bash
sudo xfs_growfs -d /
```



 
