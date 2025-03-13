# Setup the host

## Check for the required packages and compatibility
- Create a version-check script, refer to [version-check.sh](/scripts/version-check.sh)
- Save the script and change the mod as executable (+x)
    ```bash
    chmod +x version-check.sh
    ./version-check.sh
    ```
- Verify the requirements

## Creating a New Partition
- Creating a new partition in already working partition can risk in corruption of data.
But still this method worked for me and `fsck` didn't complained anything which worked.
    ```bash
    lsblk #check for any mounted partitions
    ```
- If the partition you want is mounted un mount it
    ```bash
    sudo umount /dev/(your_partition) 
    ```
- Resize the partition(Shrink or Expand) !!i am using sda3 use these commands wisely!!!
    ```bash
    # check for errors
    sudo e2fsck -f /dev/sda3
    # resize the filesystem
    sudo resize2fs /dev/sda3 189G
    #replace the 189G with your desired size
    ```
- Open `cfdisk` or `fdisk` I prefer cfdisk better UI
    ```bash
    sudo cfdisk /dev/sda
    ```
- Select the free space, choose New.
- Set the size (e.g., 30GB for LFS).
- Choose Primary.
- Write changes and exit.
- Format and mount the new partition
    ```bash
    sudo mkfs.ext4 /dev/sdaX #sdaX being new partition
    # mount it
    sudo mount /dev/sdaX /mnt/lfs
    ```
- This should partition your disk and mount it as required

## Setting the $lfs variable
- Export the LFS as /mnt/lfs
    ```bash
    export LFS=/mnt/lfs
    ```
- Set file mode creation mask (umask) to `022` some distros has different default
    ```bash
    umask 022
    ```
- This provides security while creating files making it writable by only owner and readable by directories.
- Check `echo $LFS` and `umask`

## Mounting thr New partition
- We have already mounted the disk earlier but doing it second time doesnt hurt
    ```bash
    mkdir -pv $LFS
    mount -v -t ext4 /dev/sdaX $LFS
    ```
- Set the owner and permission mode of the $LFS directory (i.e. the root directory in the newly created file system for the
LFS system) to root and 755 in case the host distro has been configured to use a different default for mkfs
    ```bash
    chown root:root $LFS
    chmod 755 $LFS
    ```

## Important if you restart your computer
- You need to remount the partition everytime you restart your pc
- So put this in your fstab file `/etc/fstab`
    ```bash
    /dev/<xxx> /mnt/lfs ext4 defaults 1 1
    ```
Now we are rady to go........

