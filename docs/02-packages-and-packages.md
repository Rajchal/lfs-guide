# Packages and Patches

## Downloading the packages
- A working directory is also required to unpack the sources and build them. $LFS/sources can be used both
as the place to store the tarballs and patches and as a working directory
- To create this directory, execute the following command, as user root, before starting the download session:
    ```bash
    mkdir -v $LFS/sources
    ```
- Make this directory writable and sticky. “Sticky” means that even if multiple users have write permission on a directory,
only the owner of a file can delete the file within a sticky directory.
    ```bash
    chmod -v a+wt $LFS/sources
    ```

> I struggled in this part as we have to download so many packages all at once and also check their MD5 sum which was such a hassle.

- This command should prolly work for you if not I will also teach you mine
    ```bash
    wget --input-file=wget-list-sysv --continue --directory-prefix=$LFS/sources
    ```
- Place it all in $LFS/sources
- There is a seperate md5sums file which helps to verify the package so sun this command
    ```bash
    pushd $LFS/sources
    md5sum -c md5sums
    popd
    ```
- Change the owners of these files to root to avoid UID issues 
    ```bash
    chown root:root $LFS/sources/*
    ```

## All packages
- You can go to my github repo for the downloading script to download all the files too
- This is the link -> [Repo](https://github.com/Rajchal/lfs_auto_download)
- Follow this:
    ```bash
    git clone https://github.com/Rajchal/lfs_auto_download.git
    cd lfs_auto_download
    chmod +x lfs_autodownload.sh
    ./lfs_auto_download.sh
    ```
- This repo also contains all the downloaded packages so you can refer to it aswell

# How it worked for me
- So for me there were few things i did wrong during the installation which caused me to waste my time 
1. Didn't checked for $LFS was exported or not
```bash
echo $LFS
```
2. Was getting wget-list-sysv doesnt exist error
```bash
wget https://www.linuxfromscratch.org/lfs/downloads/stable/wget-list
ls -l wget-list-sysv # verify that the file exists
# retry wget
wget --input-file=wget-list-sysv --continue --directory-prefix=$LFS/sources
```
3. Verify the Downloaded Packages
```bash
wget https://www.linuxfromscratch.org/lfs/downloads/stable/md5sums
pushd $LFS/sources
md5sum -c ../md5sums
popd
```
3. Verify the Downloaded Packages
```bash
wget https://www.linuxfromscratch.org/lfs/downloads/stable/md5sums
pushd $LFS/sources
md5sum -c ../md5sums
popd
```
4. Download the `md5sums` file
```bash
wget https://www.linuxfromscratch.org/lfs/downloads/stable/md5sums -O $LFS/sources/md5sums
```
5. Verify again (I dont know why but after doing this again it worked for me)
```bash
pushd $LFS/sources
md5sum -c md5sums
popd
```
6. And my stupid ass downloaded it in my main partitioned sources direcotory `/sources`
```bash
mv /sources/* /mnt/lfs/sources/
ls -l /mnt/lfs/sources/ # verify
# change the ownership
chown root:root /mnt/lfs/sources/*
```
7. If you also downloaded it somewere else try this
```bash
find / -name "sysvinit-3.14.tar.xz" 2>/dev/null
# then move it like this
mv /path/to/your/files/* /mnt/lfs/sources/
```

Now we move to final preps......
