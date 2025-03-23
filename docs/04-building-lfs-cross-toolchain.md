# Insatllations

>[!IMPORTANT]
>Here is a synopsis of the build process.
>1. Place all the sources and patches in a directory that will be accessible from the chroot environment,
>such as /mnt/lfs/sources/.
>2. Change to the /mnt/lfs/sources/ directory.
>3. For each package:
>a. Using the tar program, extract the package to be built. In Chapter 5 and Chapter 6, ensure you are
>the lfs user when extracting the package.
>Do not use any method except the tar command to extract the source code. Notably, using the cp
>-R command to copy the source code tree somewhere else can destroy timestamps in the source
>tree, and cause the build to fail.
>b. Change to the directory created when the package was extracted.
>c. Follow the instructions for building the package.
>d. Change back to the sources directory when the build is complete.
>e. Delete the extracted source directory unless instructed otherwise.
