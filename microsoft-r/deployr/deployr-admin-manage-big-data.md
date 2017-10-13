---

# required metadata
title: "Managing External Directories for Big Data - DeployR 8.x "
description: "Big Data in DeployR - Managing External Directories for Big Data"
keywords: "big data, DeployR"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "deployr"
#ms.custom: ""

---

# Managing External Directories for Big Data

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../whats-new-in-r-server.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../what-is-operationalization.md).

There may be times when your DeployR user community needs access to genuinely large data files, or big data. These data files might be too big to be copied from the Web or copied from their local machines to the server.

When such files are stored in the DeployR Repository or at any network-accessible URI, the R code executing on the DeployR server can load the desired file data on-demand. However, physically moving big data is expensive both in terms of bandwidth and throughput.

To alleviate this overhead, the DeployR server supports a set of NFS-mounted directories dedicated to managing large data files. We refer to these directories as 'big data' external directories. 

As an administrator, you can enable this service by:

1.  [Configuring](#setting-up-nfs-setup) the big data directories within your deployment.

2.  Informing your DeployR users that they must use [the `deployrExternal` R function](#deployrexternal) from the `deployrUtils` package in their R code to reference the big data files within these directories. 

It is the responsibility of the DeployR administrator to configure and manage these 'big data' external directories and the data files within them.

<br />
## Setting up NFS Setup

>NFS configuration for external directories is required only if your DeployR environment has multiple grid nodes. 

To benefit from external directory support in a multi-grid node DeployR environment, you (the administrator) must install and configure a Network File System (NFS) shared directory on the DeployR main server as well as any grid node from which they want to access to this big data files.

<br />
### Configure NFS on Windows

1.  NFS mount the `<INSTALL_DIR>/deployr/external` directory on both the DeployR main server and each grid node.

    `<INSTALL_DIR>` is the full path to directory into which you installed DeployR, which is `C:\Program Files\Microsoft\DeployR-<version>\` by default.

2.  Open the Windows firewall ports as described in the Windows NFS documentation for your platform.

<br />
### Configure NFS on Linux

>If you do not have access to the Internet, you’ll have to copy the install files to this machine using another method.

1. Log into the operating system as `root`.

2. Install NFS.

	For Redhat:

		yum install -y nfs-utils nfs-utils-lib

	For SLES:

		zypper install -y nfs-kernel-server
       
3. Open the file `/etc/sysconfig/nfs` and add these lines to the end of the file:

        RQUOTAD_PORT=10000
        LOCKD_TCPPORT=10001
        LOCKD_UDPPORT=10002
        MOUNTD_PORT=10003
        STATD_PORT=10004
        STATD_OUTGOING_PORT=10005
        RDMA_PORT=10006

4. In IPTABLES, open the following ports for NFS for external directory support:

    -   `111`
    -   `2049`
    -   `10000:10006`
<br />

5. Restart IPTABLES.

        service iptables restart

6. Set up the automatic start of NFS and `portmap/rpcbind` at boot time. At the prompt, type:

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

7. To create a new NFS directory share, run the following commands. To update an existing NFS share, see the next step instead.

    -   On the DeployR main server:

        1.  Add a line to the file `/etc/exports` as follows where <VERSION> is the installed version of DeployR.

                /home/deployr-user/deployr/<VERSION>/deployr/external/data *(rw,sync,insecure,all_squash,anonuid=48,anongid=48)

        2.  Broadcast the new directory. At the prompt, type:

                exportfs -r

    -   On the DeployR grid node machine:

        1.  Add a line to the end of the fstab file, where `<DeployR_server_ip>` is the IP or hostname of the machine on which you installed DeployR,

                <deployr_server_ip>:/home/deploy-user/deploy-install/deployr/external/data /home/deploy-user/deploy-install/deployr/external/data nfs rw 0 0"

        1.  Attempt to mount the contents of the file and verify the NFS points to the correct address. At the prompt, type:

                mount -a
                df -k

8. To use an existing NFS directory share, do the following for the main server and each grid node.

    1.  Add the following line to `/etc/fstab`, where `<nfs_share_hostname>` and `<nfs_share_directory>` is the IP or hostname and directory of the machine where the NFS share site exists:

            <nfs_share_hostname>:/<nfs_share_directory>   /deployr/external/data  nfs  rw   0   0

    2.  Try to mount the contents of the file and verify the NFS points to the correct address. At the prompt, type:

            mount -a
            df -k

## External Directory Structure

While it suffices for users to store their local copy of the data in the current working directory of their R session, the external directory structure in the DeployR server environment is not transparent to the users. For this reason, you need to [put the files into the external directories for them](#adding-files-to-external-directories).

The external directory structure is really a tree of public and private directories hanging off a common base directory managed by you, the DeployR administrator.

The default base directory for the external directory structure might be:
+ On Windows, `C:/Program Files/Microsoft/DeployR-<version>/deployr/external/data/`
+ On Linux, `/home/deployr-user/deployr/<version>/deployr/external/data`
+ On Mac OS X (DeployR Open only), `/Users/deployr-user/deployr/<version>/deployr/external/data`

Two external subdirectories are automatically created by the server the first time the corresponding user logs into the system. The first directory is called `/public` and contains the files that can be read by any authenticated users on the server. 

The second directory is the user’s private directory. Each private external directory can be found at `<DeployR_Install_Dir>/external/data/{deployr_username_private_directory}`. Files in each private directory are available only to that authenticated user during a live session. The administrator can also manually create these subdirectories in advance. The names of the directories are case-sensitive.

For example, the user `testuser` would have access to the files under:
+ The private external directory, `<DeployR_Install_Dir>/deployr/external/data/testuser`
+ The `/public` directory, `<DeployR_Install_Dir>/deployr/external/data/public`

It is up to the administrator to inform each user of which files are available in the `/public` directory and which files are in their private directory.

<a name="deployrexternal"></a>
## Adding Files to External Directories

A file can be added to the external directories in one of two ways:

+ You can place the file, on the user's behalf, into his or her private external directories or into the `/public` external directory so that it can be access when running R code.

+ A user can execute R code that will write a file directly into his or her private external directory.

Ultimately, the administrator is responsible for moving and managing files in the external directories as well as informing users that the files exist and their whereabouts.

To reference one of these files in the code, a user must declare and, therefore, know if the given file is stored in the `/public` external subdirectory or the user's private subdirectory. This declaration is done using the `deployrExternal` function in the `deployrUtils` package. 

This function provides read and write access to big data files in a portable way whether the R code is run locally or in the remote DeployR server environment. For more information, point your users to the [Writing Portable R Code](deployr-data-scientist-write-portable-r-code.md#portable-access-to-data-files) guide and the package help for this function (`??deployrUtils::deployrExternal`).

>Currently there is no user interface to display the names of the files in this directory. Additionally, due to the potentially large size of the data, we do not expose any API or other facility for moving data into the external directories.