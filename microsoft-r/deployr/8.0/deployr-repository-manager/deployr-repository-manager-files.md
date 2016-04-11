# Working with Files

The Files tab is the main tab in the Repository Manager and where all work begins. This tab gives you access to all of the repository-managed files you own as well as any other users' repository-managed files to which you have [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights).

The set of file management tasks and interactions available to you depend not only on whether you [own](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties-owners.htm) the file or whether you have [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights), but also on the [policies](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/a-permissions-policies.htm) governing the server.

## The Files You Own

When you own a file you are permitted to do almost anything to that file, including:

- [Download](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-download.htm) files from directories
- [Copy](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-copy.htm) and [move](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-move-dir.htm) files across directories
- [Open](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-open.htm) files for viewing or [editing](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties-edit.htm)
- [Share files and collaborate](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-share.htm) with others
- [View](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm), [compare](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-compare.htm), and [revert](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-revert.htm) versions in the file's history
- [Test scripts](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-script-test.htm) in a live debugging environment (R scripts only)
- [Close](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-close.htm) files
- [Delete](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-delete.htm) files from directories

## The Files You Don't Own

If you do not own a file and do not have [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) to it either, then you will never be able to see or interact with that file.

If you have [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) to a particular file you don't own, then you will see that file in one of directories under Other Files in the Files tab. Despite the fact that you do not own the file, you can still access the file in read-only mode. Possible operations include:

- [Open](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-open.htm) files and [review the file properties](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm) of the Latest version
- [Test R scripts](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-script-test.htm), including the review of [source code*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-script-test-1-code.htm), [loading*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-manipulate-sess-load-data.htm) of files into the R session, and testing of [modified input values*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-script-test-3-inputs.htm) in a debug environment (R scripts only)*
- [Close](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-close.htm) files

*Certain [server policies ](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/a-permissions-policies.htm)can affect your ability to see, download, and load files that aren't yours.

## Uploading Files

>[!IMPORTANT]
>See the [Writing Portable R Code](http://deployr.revolutionanalytics.com/documents/dev/scientist-portable-code/) guide on to learn how to make your code run consistently both locally and on the DeployR server.

While small revisions are possible through the Repository Manager, the bulk of your script development should take place outside of DeployR prior to uploading. Once ready, you can use the Repository Manager to upload the files needed for your applications to the DeployR server repository. To upload several files, first put them in a zip file and then upload the zip.

If you attempt to upload a file into a directory containing a file by the same name, then a new version of the file will be created in the repository. For example, let's say you decide to upload file test.R from your local file system to the directory Samples, but the directory Samples already has a file by that same name, test.R. In that case, upon upload the contents of your local test.R becomes the new [*Latest*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) version of test.R in the directory Samples. The owners, keywords, and description of the file test.R remain unchanged. To see the contents of file test.R as they were before the upload, look in the [version history](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm).

>[!NOTE]
>Since you cannot rename a file upon upload, make any changes to the name of a file before uploading.

To upload a file:

1. Go to the **Files** tab.
2. Click the **Actions** menu drop list.
3. From the menus, choose **Upload File**. The **Upload** dialog appears.
4. Select the file you want to upload from your local machine using the **Browse** button. Choose an individual file or a zip of several files.
5. Select the directory under **My Files** in which you want to store your file. If that directory doesn't exist yet, then [create the directory](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/b-dir-create.htm) before coming back here to upload the file.
6. If you are uploading a zip file, choose whether to extract files upon upload. Choices are:
	-  **Extract files into directory**. All files, regardless of the directory structure in the zip file, will be extracted into a single directory.
	-  **Upload the zip file without extracting files**. The zip file will be uploaded as a zip file.
7. Click **Upload**.

## Downloading Files

You can download any [version](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) of a file you own. When you download a file, whether it is the Latest version or previous version, a file is created on your local file system with a copy of the contents of the selected file version.

>[!NOTE]
>The [properties of the file](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm) (access rights, keywords, and so on), which are managed by the repository, are not included in the download.

To download the Latest version of a file you own:

1. In the **Files** tab of the Repository Manager, select the checkbox to the left of the filename(s) you want to download. The **Manage** dropdown menu appears.
2. Select any other files you want to download.
3. From the **Manage** menu, choose **Download**. A file is downloaded to your local file system through your default browser settings. If multiple files were selected, a zipped file of those files is downloaded.

To download a prior version of a file you own:

1. In the **Files** tab of the Repository Manager, identify the file whose version you want to download.
2. Click the icon in the **History** column for that file. The **Version History** table appears in the **Files** tab.
3. Click the number to the left of the version you want to download. A new file tab opens.
4. In the Manage dropdown menu on the far right, choose Download.

## Copying Files

You can make a copy of any file you own. When you copy a file, whether it is the [*Latest*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) version or previous version, a new file is created in the specified user directory in the repository. You can then see that file in the **Files** tab. The contents of your new file is a duplicate of the contents of the version of the file you copied. Your new file will have the same file properties as the version of the file being copied except you will be its sole owner. And, since this is a new file, the file will not have any previous versions.

If you copy a file into a directory in which a file by the same name already exists, then a copy of the Latest version of the selected file becomes the Latest version of the file in the destination directory. For example, let's say you own two files of the same name in different directories (directory X and directory Y). You decide to copy the file from directory X and save it in directory Y, but directory Y already has a file by that same name. In that case, the contents and file properties (access rights, keyword, description) of the Latest version of the file from directory X become the new Latest version of the file by the same name in directory Y. The file is directory X remains unchanged. If you want to see the contents of the file in directory Y before the move, you would now need to look in the [version history](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) to find it. In this case, all of the history for the file that was originally in directory X is also kept in the history of the file in directory Y.

To copy a file:

1. [Open](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-open.htm) the file in the Repository Manager.
2. In the **File Properties** page, choose **Copy** from the **File** menu. The **Copy File** dialog opens.
3. Enter a name for the file in the **Name** field. If a file by that name already exists, this will become the new Latest version of that file.
4. Select a directory in the repository in which the file should be stored from the **Directory** drop down list.
5. Click **Copy** to make the copy. The dialog closes and the **Files** tab appears on the screen.

## Moving Files

You can move the files under **My Files** for which you are the sole owner from one user directory to another. If there are multiple users on a file, it cannot be moved.

If you move a file into a directory in which a file by the same name already exists, then the Latest version of the moved file becomes the [*Latest version*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) of the file in the destination directory and the file histories are merged. For example, let's say you own two files of the same name in different directories (directory X and directory Y). You decide to move the file from directory X to directory Y, but directory Y already has a file by that same name. In that case, the contents and file properties (access rights, keyword, description) of the Latest version of the file from directory X become the new Latest version of the file by the same name in directory Y. If you want to see the contents of the file in directory Y before the move, you would now need to look in the [version history](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) to find it. In this case, all of the history for the file that was originally in directory X is also kept in the history of the file in directory Y.


>[!IMPORTANT]
>You cannot change the directory of files you co-own with other users. Files for which you are the sole owner can be moved to another directory; however, files owned by multiple users must remain under the original directory for as long as there are multiple owners.

To move a file to another directory:

1. From the **Files** tab, select the file in the table.The **Manage** dropdown menu appears.
2. In the **Manage** dropdown menu, choose **Move**. The **Move File** dialog appears.
3. In the dialog, select the user directory into which you want to move the file(s). If you are not the sole owner, then you cannot move the file.
4. Click **Move**. The dialog closes and the file is moved.

## Opening Files

If you see a file in the Files tab, you can open it. Your right to access, edit, and interact with files is determined in part by [ownership and access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) and also by [server policies](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/a-permissions-policies.htm).

>[!IMPORTANT]
>If two owners are editing a file at the same time, you will be writing over each other's work. It is essential to coordinate with the [other owners](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties-owners.htm).

To open the Latest version of a file:

1. In the **Files** tab of the Repository Manager, navigate to the directory containing the file you want to open.
2. Click the name of the file. A new file tab opens and displays the [file properties](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm).

To open a prior version of a file:

1. In the **Files** tab of the Repository Manager, navigate to the directory containing the file whose version you want to open.
2. In the table, identify the file whose version you want to open.
3. Click the icon in the **History** column for that file. The **Version History** table appears in the **Files** tab.
4. Click the number to the left of the version you want to open. A new file tab opens and displays the [file properties](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm).

>[!NOTE]
>To learn more about a version without opening a file, you can also [compare](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-compare.htm) a historical version to the Latest version of a file.

## Editing File Properties

Only the [file properties](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm) of the [*Latest*](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions.htm) version of a file can be edited since all others in the file history are read-only. Furthermore, only an owner can edit a file.

To edit file properties:

1. [Open](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-open.htm) the *Latest* version of a file you own.
2. Make any desired changes to the following [file properties](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm). Changes are automatically saved. Note that you cannot change a file's name or version information.
	-  Edit the keywords or file description.
	-  Update the list of [owners](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties-owners.htm) collaborating on this file.
	-  Change the [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) on the file to control how the file is deployed.

## Sharing Files with Others

By default, an R Script or any other repository-managed file is visible on the API only to the owners, referred to as authors in API calls, of that file. However, you can make files visible to other authenticated and/or anonymous users on the API and in the Repository Manager in the following ways.

1.  **Grant ownership to others**. You can [grant (or revoke) ownership](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties-owners.htm) to other authenticated users so that they can edit and manage that file. 

	>[!NOTE]
	>An R script is always available for execution on the API to all of its owners.

2.  **Specify access rights on a file**. You can set the [access level](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) (**Private**, **Restricted**, **Shared**, or **Public**) on a file to control who can access it or, in the case of a script, execute the script.
 
>[!NOTE]
>To learn more about other file properties, [read here](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm).
 
## Working with Historical Versions

The DeployR repository supports a versioned file system, thereby offering you a full version history for each of your repository files and access to any version upon request pending proper authentication. The Repository Manager simplifies certain interactions with each version of the file you have had over time.

>[!NOTE]
>You can only see the history for the files you own.

The current working version of a file is referred to as the ***Latest* version**. You can edit the Latest version of a file you own, but all historical version are read-only.

For any file you own, you can access the file's version history. From this version history, you can:

- [Open](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-open.htm) a version
- [Compare](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-compare.htm) one past version to the Latest version of the file
- [Revert](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-revert.htm) a past version to the *Latest* version of the file

>[!NOTE]
>You cannot delete an individual version from the version history. If you [delete a file](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/c-file-delete.htm), all of its version history is also deleted.

To see the version history of a file you own:

1. In the **Files** tab of the Repository Manager, click the name of the user directory under **My Files** containing the file in which you are interested.
2. Identify the file whose version history you want to see.
3. Click the icon in the **History** column for that file. The **Version History** table appears in the **Files** tab.

### Comparing Version Differences

When a file has two or more versions, you can compare the differences between one previous version and the Latest version of that file.

- **For scripts**, you can compare the R code itself. The differences are highlighted in color to help you identify what is in the previous version and what is from the Latest version of that script.
- **For other files**, you can compare the version dates, comments, and file sizes.
 
To compare two file versions:

1. In the **Files** tab of the Repository Manager, click the name of the directory under **My Files** that contains the file that interests you.
2. Identify the file whose version history you want to see.
3. Click the icon in the **History** column for that file. The **Version History** table appears in the Files tab.
4. In the **Version History**, click the **Compare** icon in the row of the previous version you want to compare to the Latest version. The **Compare Differences** dialog opens.
5. Review the differences between the past version, which is referenced by its ID, and the Latest version.
6. If desired, you can click **Revert** to [revert](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-revert.htm) the past version to *Latest*.

### Reverting a Past Version to the Latest Version

If you want to restore a historical version to Latest version of a file, you can [revert that version](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/d-file-versions-compare.htm). Reverting will copy the contents of the selected past version and make that copy the Latest version of the file. What was previously the Latest version is now preserved in the file's version history with a version ID.

![](media/deployr-repository-manager-files/workflowrestorefile.png)

## Deleting Files

Only the sole owner of a file can delete the file (and all of its previous versions) permanently.

When you attempt to delete a file you own, DeployR checks to see if there are other owners. If there are other owners, then you are removed as an owner and the file is no longer editable by you; however the file and its version history remain available in the repository to the other owners. But, if you are the sole owner at the time you attempt to delete this file, the file and all its versions will be permanently deleted from the repository.

After you delete a file, you may see that file under **Other Files** in the Files tab. If so, this is due to the fact that the file had multiple owners and the file's [access rights](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/e-file-properties.htm#AccessRights) were set to **Restricted**, **Shared**, or **Public**. In this case, you might be able to see the file in your **Files** tab, but you will not be able to edit that file since you are no longer its owner.

![](media/deployr-repository-manager-files/workflowdeletefile.png)  
*OpenWorkflow: Deleting Files*

>[!NOTE]
>You cannot delete a previous version of a file without deleting the entire file history.

To delete a file or yourself as its owner:

1. From the **Files** tab, select the file in the table. The **Manage** dropdown menu appears.
2. In the **Manage** dropdown menu, choose **Delete**.
3. Confirm that you want to delete the file(s). If you are not the sole owner, then you will be stripped of your ownership rights and the file(s) will no longer appear in any directory under **My Files**.

## Closing Files

You can close one file at any time.

To close a file:

1. Go to the tab of the file you would like to close.
2. From the **Manage** menu on the right, choose **Close**. If there are any unsaved code changes, you will be prompted to [save](https://deployr.revolutionanalytics.com/documents/help/repo-man/Content/j-script-test-save.htm) or discard those changes. The tab disappears and the **Files** tab appears onscreen.

>[!NOTE]
>Alternately, hover over the filename in the tab at the top of the screen and click the **x**.
 