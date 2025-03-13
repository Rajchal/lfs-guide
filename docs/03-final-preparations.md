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


