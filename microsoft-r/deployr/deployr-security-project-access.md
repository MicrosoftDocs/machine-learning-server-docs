---

# required metadata
title: " Security | DeployR 8.x | Microsoft Docs"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
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

# Project and Repository File Access Controls

DeployR enforces a consistent security model across projects and repository-managed files. This model is based on two simple precepts: ownership and access levels.

## Project Access Controls

When a project is created, it is privately owned by default, meaning it is visible only to its owner. Such user-based privacy is a central aspect of the DeployR security model.

The owner of a temporary or persistent project has, by default, full read-write access to that project and use of the full set of project-related APIs.

DeployR introduced a type of secure, temporary project called a *blackbox project*. Blackbox projects restrict access to the underlying R session. In this case, the project owner can use only a small subset of the project-related APIs, collectively known as the “Blackbox API Controls”.

If the owner of a project wants to grant read-only access to that project to other authenticated users, then the owner can set the access level for the project to `Shared`. You can change the access level on a project using the `/r/project/about/update` API call.

>Anonymous users are not permitted access to projects. For more information, refer to the [API Reference Help](deployr-api-reference.md).

## Repository File Access Controls

When a repository-managed file is created, it is privately owned by default, meaning it is visible only to its owner. Such user-based privacy is a central aspect of the DeployR security model.

The owner of a repository-managed file has full read-write access to that file and use of the full set of repository-related APIs.

If the owner of a repository-managed file wants to grant read-only access to that file to other users, then the owner can set the file’s access level to one of the following values:

+ `Private` - the default access level, the file is visible to its author(s) only.

+ `Restricted` - the file is visible to authenticated users that have been granted one or more of the roles indicated on the restricted property of the file.

+ `Shared` - the file is visible to all authenticated users when the shared property is true.

+ `Public` - the file is visible to all authenticated and all anonymous users when the published property is true.

You can change the access level on a repository-managed file using the `/r/repository/file/update` [API call](deployr-api-reference.md#repository-on-the-api) or using the [Repository Manager](deployr-repository-manager-files.md#about-file-properties).

## Repository File Download Controls

The repository file download controls provide fine-grain control over who can download repository file data. It is important to tailor the configuration of these controls in your DeployR external configuration file in order to enforce your preferred download policy for repository-managed files.

    /*
     * DeployR Repository File Download Controls
     *
     * The repository file download controls provide fine
     * grain control over user access to repository file data
     * returned on the following API call:
     *
     * /r/repository/file/download
     *
     * The default repository file download policy is shown
     * for each of the supported repository file access levels:
     *
     * [Private, Restricted, Shared, Public]
     *
     * - Files with private access can be downloaded by authors only.
     * - Files with restriced access can be downloaded by authors
     *   and by authenticated users with sufficient privileges.
     * - Files with shared access can be downloaded by authors
     *   and by authenticated users.
     * - Files with public access can be downloaded by authors
     *   and by authenticated users.
     * - Regardless of access level, by default anonymous users
     *   can not download repository files.
     *
     * Enable file.author.only.download to ensure only authors
     * can download a repository-managed file. When this
     * property is enabled the file.anonymous.download option
     * is ignored.
     *
     * Enable file.anonymous.download to allow anonymous
     * users to download files with public access.
     *
     * Note: The repository file download controls apply to
     * all repository-managed files excluding R scripts.
     *
     */
    deployr.security.repository.file.author.only.download=false
    deployr.security.repository.file.anonymous.download=false
    
## Repository Scripts Access Controls

Repository-managed R scripts can be exposed as an executable on the API. Since repository-managed R scripts are a type of repository-managed file, all information in the [previous section](#repository-file-access-controls) also applies to repository-managed scripts.

However, repository-managed R scripts deserve special mention since scripts can be managed through the Administration Console interface. Additionally, when you work with the R scripts in the Administration Console, you will likely also use and work with roles so as to impose restricted access to your R scripts.

>For information on how to use roles as a means to restrict access to individual R scripts, refer to the [Administration Console Help](deployr-admin-managing-server-policies.md#server-policy-properties).

## Repository Script Download Controls

The repository script download controls provide fine-grain control over who can download repository script data. It is important to tailor the configuration of these controls in your DeployR external configuration file in order to enforce your preferred download policy for repository-managed scripts.

``` 
/*
* DeployR Repository Script Download Controls
*
* The repository script download controls apply to
* repository-managed scripts [*.R/r] and markdown
* files [*.Rmd/rmd] only.
*
* The repository script download controls provide fine
* grain control over user access to repository script data
* returned on the following API call:
*
* /r/repository/file/download
*
* Note: Script download controls do not intefere with
* permissions to execute a script. Execution permissions are
* entirely determined by the file access level assigned to
* the script by it's author.
*
* The default repository script download policy is shown
* for each of the supported repository script access levels:
*
* [Private, Restricted, Shared, Public]
*
* - Scripts with private access can be downloaded by authors only.
* - Scripts with restriced access can be downloaded by authors
*   and by authenticated users with sufficient privileges.
* - Scripts with shared access can be downloaded by authors
*   and by authenticated users.
* - Scripts with public access can be downloaded by authors
*   and by authenticated users.
* - Regardless of access level, by default anonymous users
*   can not download repository scripts.
*
* Enable script.author.only.download to ensure only authors
* can download a repository-managed script. When this
* property is enabled the script.anonymous.download option
* is ignored.
*
* Enable script.anonymous.download to allow anonymous
* users to download scripts with public access.
*
* Enabled script.list.authenticate to prevent anonymous
* users from calling the /r/repository/script/list API.
*
*/
deployr.security.repository.script.author.only.download=false
deployr.security.repository.script.anonymous.download=false
deployr.security.repository.script.list.authenticate=false
```
    
## File Type Black List Controls

The file type black-list controls provide fine-grain control over the types of files that can be:

-   Uploaded, transferred and written into the repository or
-   Uploaded, transferred, written and loaded into the working directory of a project (R session).

These controls are particularly useful if an administrator wants to ensure malicious executable files or shell scripts are not uploaded and executed on the DeployR server.

```
/*
* DeployR File Type Black List Policy Configuration
*
* Files with the following extensions are forbidden
* on upload, transfer, and write calls on:
* 1. Project (R sesesion) directories
* 2. The Repository
* These files are also forbidden from participating on
* prelaodfile* parameters on all calls that adhere to
* the DeployR standard execution model.
*
* ADMINISTRATORs are not subject to these restrictions.
*/
deployr.file.type.black.list = [ "exe", "sh", "bat", "bash", "csh", "tcsh" ]
```