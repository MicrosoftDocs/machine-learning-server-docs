#Microsoft R Release Notes

##Microsoft R Client 2016

The following release notes apply to Microsoft R Client.

**New Features in this Release**

This guide is an introduction to using the new features of Microsoft R Services. The current release is Microsoft R Services 2016, which includes the following new features:

+ It is now Microsoft R Services! With the acquisition of Revolution Analytics by Microsoft, we are quickly moving to integrate the product line. With this release you will see Microsoft R Server 8.0 on Linux and Revolution R Enterprise 8.0 on Windows, both bringing you the full feature set of the Revolution R Enterprise 7, and more!
+ Revolution R Open has become Microsoft R Open.
+ RevoScaleR now includes two new functions (as experimental) for cleaning and analyzing text data: rxGetFuzzyKeys and rxGetFuzzyDist.

**Bug Fixes**

+ When using rxDataStep, new variables created in a transformation no longer inherit the rxLowHigh attribute of the variable used to create them.