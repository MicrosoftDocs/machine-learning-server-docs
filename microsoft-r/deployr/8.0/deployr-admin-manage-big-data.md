# Managing External Directories for Big Data

## Introduction

Whenever the data files with which your users need to work are too big to be copied from the Web or copied from their local machines to the server, those files can be stored in 'big data' external directories on the DeployR main server. It is your responsibility as the DeployR administrator to configure and manage these 'big data' external directories and the data files within them.

Once you put the big data files in the external directories, data scientists can use [the `deployrExternal` function](#addfiles) from the `deployrUtils` package in their R code to reference these files in such a way that they can be found whether the R code is run locally or on the DeployR server. Additionally, these external directories can also be referenced in R code without the overhead of replicating the big data itself for each individual project.

## Setting up NFS Setup

>[!IMPORTANT]
>NFS configuration for external directories is required only if your DeployR environment has multiple grid nodes. DeployR Open administrators can skip this step.

To benefit from external directory support in a multi-grid node DeployR environment, you (the administrator) must install and configure a Network File System (NFS) shared directory on the DeployR main server as well as any grid node from which they want to access to this big data files.

### Configure NFS on Windows

1.  NFS mount the `<INSTALL_DIR>/deployr/external` directory on both the DeployR main server and each grid node.
    `<INSTALL_DIR>` is the full path to directory into which you installed DeployR, which is `C:\Program Files\Microsoft\DeployR\8.0\` by default.

2.  Open the Windows firewall ports as described in the Windows NFS documentation for your platform.

### Configure NFS on Linux

>[!IMPORTANT]
>If you do not have access to the Internet, you’ll have to copy the install files to this machine using another method.

1. Log into the operating system as `root`.

1. Install NFS.

	> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
	> ```Redhat
	> yum install -y nfs-utils nfs-utils-lib
	> ```
	> ```SLES
	> zypper install -y nfs-kernel-server
	> ```
       
1. Open the file `/etc/sysconfig/nfs` and add these lines to the end of the file:

        RQUOTAD_PORT=10000
        LOCKD_TCPPORT=10001
        LOCKD_UDPPORT=10002
        MOUNTD_PORT=10003
        STATD_PORT=10004
        STATD_OUTGOING_PORT=10005
        RDMA_PORT=10006

1. In IPTABLES, open the following ports for NFS for external directory support:

    -   `111`
    -   `2049`
    -   `10000:10006`

1. Restart IPTABLES.

        service iptables restart

1. Set up the automatic start of NFS and `portmap/rpcbind` at boot time. At the prompt, type:

   -   For both the main server and any grid machines on Redhat 5, type:

        /sbin/chkconfig nfs on 
        /etc/init.d/portmap start 
        /etc/init.d/nfs start

   -   For both the main server and any grid machines on Redhat 6, type:

        /sbin/chkconfig nfs on 
        /etc/init.d/rpcbind start 
        /etc/init.d/nfs start

   -   For the main server on SLES, type the following at the prompt:

        /sbin/chkconfig nfsserver on 
        /etc/init.d/rpcbind start 
        /etc/init.d/nfsserver start

    -   For a grid machine on SLES, type the following at the prompt:

        /sbin/chkconfig nfs on 
        /etc/init.d/rpcbind start 
        /etc/init.d/nfs start

1. To create a new NFS directory share, run the following commands. To update an existing NFS share, see the next step instead.

    -   On the DeployR main server:

        1.  Add a line to the file `/etc/exports`.

                /home/deployr-user/deployr/8.0.0/deployr/external/data *(rw,sync,insecure,all_squash,anonuid=48,anongid=48)

        1.  Broadcast the new directory. At the prompt, type:

                exportfs -r

    -   On the DeployR grid node machine:

        1.  Add a line to the end of the fstab file, where `<DeployR_server_ip>` is the IP or hostname of the machine on which you installed DeployR,

                <deployr_server_ip>:/home/deploy-user/deploy-install/deployr/external/data /home/deploy-user/deploy-install/deployr/external/data nfs rw 0 0"

        1.  Attempt to mount the contents of the file and verify the NFS points to the correct address. At the prompt, type:

                mount -a
                df -k

1. To use an existing NFS directory share, do the following for the main server and each grid node.

    1.  Add the following line to `/etc/fstab`, where `<nfs_share_hostname>` and `<nfs_share_directory>` is the IP or hostname and directory of the machine where the NFS share site exists:

            <nfs_share_hostname>:/<nfs_share_directory>   /deployr/external/data  nfs  rw   0   0

    1.  Try to mount the contents of the file and verify the NFS points to the correct address. At the prompt, type:

            mount -a
            df -k

## External Directory Structure

While it suffices for users to store their local copy of the data in the current working directory of their R session, the external directory structure in the DeployR server environment is not transparent to the users. For this reason, you need to [put the files into the external directories for them](#addfiles).

The external directory structure is really a tree of public and private directories hanging off a common base directory managed by you, the DeployR administrator.

### For Linux:

For example, if the base directory is `/home/deployr-user/deployr/8.0.0/deployr/external/data`, then the base external directory structure might look:
-   `/home/deployr-user/deployr/8.0.0/deployr/external/data/public`
-   `/home/deployr-user/deployr/8.0.0/deployr/external/data/testuser`
-   `/home/deployr-user/deployr/8.0.0/deployr/external/data/user1`
-   `/home/deployr-user/deployr/8.0.0/deployr/external/data/{private_user_dir}`

### For Windows:

For example, if the base directory on the main server is `C:/Program Files/Microsoft/DeployR/8.0/deployr/external/data/`, then the base external directory structure might look:
-   `C:/Program Files/Microsoft/DeployR/8.0/deployr/external/data/public`
-   `C:/Program Files/Microsoft/DeployR/8.0/deployr/external/data/testuser`
-   `C:/Program Files/Microsoft/DeployR/8.0/deployr/external/data/user1`
-   `C:/Program Files/Microsoft/DeployR/8.0/deployr/external/data/{private_user_dir}`

### For Mac OS X:

For example, if the base directory is `/Users/deployr-user/deployr/8.0.0/deployr/external/data`, then the base external directory structure might look:
-   `/Users/deployr-user/deployr/8.0.0/deployr/external/data/public`
-   `/Users/deployr-user/deployr/8.0.0/deployr/external/data/testuser`
-   `/Users/deployr-user/deployr/8.0.0/deployr/external/data/user1`
-   `/Users/deployr-user/deployr/8.0.0/deployr/external/data/{private_user_dir}`

&nbsp;

Two directories are automatically created by the server the first time the corresponding user logs into the system. The first directory is called `/public` and contains the files that can be read by any authenticated users on the server. The second directory is the user’s private directory. Each private external directory can be found at `<DeployR_Install_Dir>/external/data/{deployr_username}`. Files in each private directory are available only to that authenticated user during a live session. The administrator can also manually create these subdirectories in advance. The names of the directories are case-sensitive.

For example, the user `testuser` will have access to the files under:

-   The private external directory, `<DeployR_Install_Dir>/external/data/testuser`
-   The `/public` directory, `<DeployR_Install_Dir>/external/data/public`.

It is up to the administrator to inform each user of which files are available in the `/public` directory and which files are in their private directory.

## Adding Files to External Directories

A file can be added to the external directories in one of two ways:

1.  You can place the file, on the user's behalf, into his or her private external directories or into the `/public` external directory so that it can be access when running R code.
2.  A user can execute R code that will write a file directly into his or her private external directory.

Ultimately, the administrator is responsible for moving and managing files in the external directories as well as informing users that the files exist and their whereabouts.

To reference one of these files in the code, a user must declare and, therefore, know if the given file is stored in the `/public` external subdirectory or the user's private subdirectory. This declaration is done using the `deployrExternal` function in the `deployrUtils` package. This function provides read and write access to big data files in a portable way whether the R code is run locally or in the remote DeployR server environment. For more information, point your users to the [Writing Portable R Code](https://deployr.revolutionanalytics.com/documents/dev/scientist-portable-code/#fileaccessport) guide and the package help for this function (`??deployrUtils::deployrExternal`).

>Currently there is no user interface to display the names of the files in this directory. Additionally, due to the potentially large size of the data, we do not expose any API or other facility for moving data into the external directories.