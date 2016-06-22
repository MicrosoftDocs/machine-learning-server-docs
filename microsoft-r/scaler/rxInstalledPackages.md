# rxInstalledPackagesXdf       

Lists the packages that are installed in the specified compute context.

## Usage

     rxInstalledPackages(computeContext = NULL, allNodes = FALSE, lib.loc = NULL,
                         priority = NULL, noCache = FALSE, fields = "Package",
                         subarch = NULL)
     

## Arguments

_computeContext_: An ‘RxComputeContext’ or equivalent character string or ‘NULL’.  If set to the default of ‘NULL’, the currently active compute context is used.

_allNodes_: If ‘TRUE’ and an ‘RxInTeradata’ compute context is used, a list of results from each node is returned.

_lib.loc_: A character vector describing the location of R library trees to search through, or ‘NULL’.  The default value of ‘NULL’ corresponds to checking the loaded namespace, then all libraries currently known in ‘.libPaths()’.

_priority_: Character vector or ‘NULL’ (default). If non-null, used to select packages; ‘"high"’ is equivalent to ‘c("base", "recommended")’.  To select all packages without an assigned priority use priority = ‘"NA"’.

_noCache_: If ‘TRUE’, do not use cached information, nor cache it.

_fields_: A character vector giving the fields to extract from each package's DESCRIPTION file, or ‘NULL’. If ‘NULL’, the following fields are used: 
+ Package
+ LibPath
+ Version
+ Priority
+ Depends
+ Imports
+ LinkingTo
+ Suggests
+ Enhances
+ License
+ License_is_FOSS
+ License_restricts_use
+ OS_type
+ MD5sum
+ NeedsCompilation
+ Built
 
  Unavailable fields result in ‘NA’ values.

_subarch_: Character string or ‘NULL’. If non-null and non-empty, selects the packages that are installed for the specified sub-architecture.

## Remarks

This is a wrapper for ‘installed.packages’ that adds specification of a compute context and restriction fo the fields that are returned. See the help file for additional details. 
Note that ‘rxInstalledPackages’ treats the ‘field’ argument differently than does installed.packages, and returns only the ‘fields’ specified in the argument.

Related R functions are `require` and `installed.packages`. 

## Return Value

By default, a character vector of installed packages is returned.
If ‘fields’ is not set to ‘"Package"’, a matrix with one row per package is returned.  
+ The row names are the package names. 
+ + The possible column names are ‘"Package"’, ‘"LibPath"’, ‘"Version"’, ‘"Priority"’, ‘"Depends"’, ‘"Imports"’, ‘"LinkingTo"’, ‘"Suggests"’, ‘"Enhances"’, ‘"License"’, ‘"License_is_FOSS"’, ‘"License_restricts_use"’, ‘"OS_type"’, ‘"MD5sum"’, ‘"NeedsCompilation"’, and ‘"Built"’ (the R version the package was built under).  
+ Additional columns can be specified using the fields argument.  
+ 
If using a distributed compute context with the ‘allNodes’ set to ‘TRUE’, a list of matrices from each node will be returned.



## Examples

The following example finds the packages installed for the current compute context.

~~~~
myPackages <- rxInstalledPackages()
myPackages
~~~~

The following example gets a matrix of information about all the packages installed for the current compute context.

~~~~
myPackageInfo <- rxInstalledPackages(fields = NULL)
myPackageInfo
~~~~

This example demonstrates how to limit the columns returned for each package by using the _fields_ argument.

~~~~
myPackageInfo <- rxInstalledPackages(fields = c("Package", "Version", "Built"))
myPackageInfo
~~~~

The following example shows how to get a list of the packages installed on a SQL Server compute context.

~~~~
sqlServerCompute <- RxInSqlServer(connectionString = 
"Driver=SQL Server;Server=myServer;Database=TestDB;Uid=myID;Pwd=myPwd;")
     sqlPackages <- rxInstalledPackages(computeContext = sqlServerCompute)
     sqlPackages
~~~~

## See Also:
rxFindPackage (link)
