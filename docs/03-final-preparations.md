# Final Preparations

## Creating a limited directory layout in LFS system
- Refer to scripts to create a limited directory -> [create-limited.sh](/scripts/create-limited.sh)
- Use this script as `root` must!!!
```bash
chmod +x create-limited.sh
./create-limited.sh
```
- Still acting as root create a directory to compile with cross compiler
```bash
mkdir -pv $LFS/tools
```

## Creating a LFS user
- Finally getting into it and creating a user in our linux system
```bash
groupadd lfs
useradd -s /bin/bash -g lfs -m -k /dev/null lfs
```
- Add password
```bash
passwd lfs
```
- Refer to scripts to grant full acces to all directory under $LFS by making lfs owner:
- Script -> [make-owner.sh](/scripts/make-owner.sh)
```bash
chmod +x make-owner.sh
./make-owner.sh
```

- Switch to the lfs user you just created
```bash
su - lfs
```

## Setting up the environment
- This ensures that no
unwanted and potentially hazardous environment variables from the host system leak into the build environment.
```bash
cat > ~/.bash_profile << "EOF"
exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
EOF
```

- The new instance of the shell is a non-login shell, which does not read, and execute, the contents of the /etc/profile
or .bash_profile files, but rather reads, and executes, the .bashrc file instead. Create the .bashrc file now:

```bash
cat > ~/.bashrc << "EOF"
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/usr/bin
if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
PATH=$LFS/tools/bin:$PATH
CONFIG_SITE=$LFS/usr/share/config.site
export LFS LC_ALL LFS_TGT PATH CONFIG_SITE
EOF
```

> [!NOTE]
> set +h
> The set +h command turns off bash's hash function. Hashing is ordinarily a useful feature—bash uses a hash
> table to remember the full path to executable files to avoid searching the PATH time and again to find the same
> executable. However, the new tools should be used as soon as they are installed. Switching off the hash function
> forces the shell to search the PATH whenever a program is to be run. As such, the shell will find the newly compiled
> tools in $LFS/tools/bin as soon as they are available without remembering a previous version of the same program
> provided by the host distro, in /usr/bin or /bin.
> umask 022
> Setting the umask as we've already explained in Section 2.6, “Setting the $LFS Variable and the Umask.”
> LFS=/mnt/lfs
> The LFS variable should be set to the chosen mount point.
> LC_ALL=POSIX
> The LC_ALL variable controls the localization of certain programs, making their messages follow the conventions
> of a specified country. Setting LC_ALL to “POSIX” or “C” (the two are equivalent) ensures that everything will
> work as expected in the cross-compilation environment.
> LFS_TGT=$(uname -m)-lfs-linux-gnu
> The LFS_TGT variable sets a non-default, but compatible machine description for use when building our cross-
> compiler and linker and when cross-compiling our temporary toolchain. More information is provided by
> Toolchain Technical Notes.
> PATH=/usr/bin
> Many modern Linux distributions have merged /bin and /usr/bin. When this is the case, the standard PATH variable
> should be set to /usr/bin/ for the Chapter 6 environment. When this is not the case, the following line adds /
> bin to the path.
> if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
> If /bin is not a symbolic link, it must be added to the PATH variable.
> PATH=$LFS/tools/bin:$PATH
> By putting $LFS/tools/bin ahead of the standard PATH, the cross-compiler installed at the beginning of Chapter 5
> is picked up by the shell immediately after its installation. This, combined with turning off hashing, limits the risk
> that the compiler from the host is used instead of the cross-compiler.
> 34
> Linux From Scratch - Version 12.3
> CONFIG_SITE=$LFS/usr/share/config.site
> In Chapter 5 and Chapter 6, if this variable is not set, configure scripts may attempt to load configuration items
> specific to some distributions from /usr/share/config.site on the host system. Override it to prevent potential
> contamination from the host.
> export ...
> While the preceding commands have set some variables, in order to make them visible within any sub-shells, we
> export them.


