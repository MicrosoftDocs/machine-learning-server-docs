---

# required metadata
title: "Package Dependencies for Microsoft R Server installations on Linux and Hadoop"
description: "Required Linux packages for a Microsoft R Server installation on Linux and Hadoop systems."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/05/2016"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: "r-server"
ms.custom: ""

---
# Package Dependencies for Microsoft R Server installations on Linux and Hadoop
--------------------
This article lists the Linux packages required for running or building Microsoft R packages on Linux and Hadoop platforms.

For computers with an internet connection, setup will download and add any missing dependencies automatically, but if your system is not internet-connected or not configured to use a package manager, a manual install of dependent packages is required.

## Package dependencies for Microsoft R Server 9.0.1

Package  | Architecture  | Version  | Repository  | Size
---------|---------------|----------|-------------|-----
cairo   | x86_64   | 1.8.8-6.el6_6   | base  | 309 k
fontconfig  | x86_64  | 2.8.0-5.el6  | base  | 186 k
freetype  | x86_64  | 2.3.11-17.el6  | base  | 361 k
libICE  | x86_64  | 1.0.6-1.el6  | base  | 53 k
libSM  | x86_64  | 1.2.1-2.el6  | base  | 37 k
libX11  | x86_64  | 1.6.3-2.el6  | base  | 586 k
libX11-common  | not applicable   | 1.6.3-2.el6  | base  | 169 k
libXau  | x86_64  | 1.0.6-4.el6  | base  | 24 k
libXft  | x86_64  | 2.3.2-1.el6  | base  | 55 k
libXrender  | x86_64  | 0.9.8-2.1.el6_8.1  | updates  | 24 k
libXt  | x86_64  | 1.1.4-6.1.el6  | base  | 165 k
libgomp  | x86_64  | 4.4.7-17.el6  | base  | 134 k
libpng  | x86_64  | 2:1.2.49-2.el6_7  | base  | 182 k
libthai  | x86_64  | 0.1.12-3.el6  | base  | 183 k
libxcb  | x86_64  | 1.11-2.el6  | base  | 142 k
pango  | x86_64  | 1.28.1-11.el6  | base  | 351 k
pixman  | x86_64  | 0.32.8-1.el6  | base  | 243 k


## Package dependencies for Microsoft R Server 8.0.5

In release 8.0.5, the installer was significantly revised. Most package dependencies are included in the version of Microsoft R Open that installs with R Server, accounting for the much smaller list of dependencies.

The following list of eighteen packages are Linux packages that `yum` looks for during an R Server installation.

Package  | Architecture  | Version  | Repository  | Size
---------|---------------|----------|-------------|-----
cairo   | x86_64   | 1.8.8-6.el6_6   | base  | 309 k
fontconfig  | x86_64  | 2.8.0-5.el6  | base  | 186 k
freetype  | x86_64  | 2.3.11-17.el6  | base  | 361 k
libICE  | x86_64  | 1.0.6-1.el6  | base  | 53 k
libSM  | x86_64  | 1.2.1-2.el6  | base  | 37 k
libX11  | x86_64  | 1.6.3-2.el6  | base  | 586 k
libX11-common  | not applicable   | 1.6.3-2.el6  | base  | 169 k
libXau  | x86_64  | 1.0.6-4.el6  | base  | 24 k
libXft  | x86_64  | 2.3.2-1.el6  | base  | 55 k
libXrender  | x86_64  | 0.9.8-2.1.el6_8.1  | updates  | 24 k
libXt  | x86_64  | 1.1.4-6.1.el6  | base  | 165 k
libgomp  | x86_64  | 4.4.7-17.el6  | base  | 134 k
libpng  | x86_64  | 2:1.2.49-2.el6_7  | base  | 182 k
libthai  | x86_64  | 0.1.12-3.el6  | base  | 183 k
libxcb  | x86_64  | 1.11-2.el6  | base  | 142 k
pango  | x86_64  | 1.28.1-11.el6  | base  | 351 k
pixman  | x86_64  | 0.32.8-1.el6  | base  | 243 k


## Package dependencies for Microsoft R Server 8.0.0

Most of the packages listed in Table 1 (for RHEL systems) and Table 2 (for SLES 11 systems), are explicitly required by Microsoft R Server, either to build R itself or as a prerequisite for an additional Microsoft package. Table 3 and Table 4 are downstream dependencies (dependencies of packages listed in Table 1 or Table 2), for RHEL systems and Table 4 for SLES 11 systems, respectively. These are automatically installed while the automated script is running. Dependency lists are based on default installs of RHEL 6 and SLES11; if your system has a minimal installation of the operating system, additional packages may be required.

Table 1. Packages Explicitly Required by Microsoft R Server on RHEL Systems

	bzip2-devel
	cairo-devel
	ed
	gcc-objc
	gcc-c++
	gcc-gfortran
	ghostscript-fonts
	libicu
	libicu-devel
	libgfortran
	libjpeg\*-devel \
	libSM-devel
	libtiff-devel
	libXmu-devel
	make
	ncurses-devel
	pango
	pango-devel
	perl
	readline-devel
	texinfo
	tk-devel

Table 2. Packages Explicitly Required by Microsoft R Server on SLES 11 Systems

	cairo-devel
	ed
	gcc-c++
	gcc-fortran
	gcc-objc
	ghostscript-fonts-std
	libgfortran43
	libjpeg-devel
	libpng-devel
	libtiff-devel
	make
	ncurses-devel
	pango-devel
	perl  
	readline-devel
	texinfo
	tk-devel
	xorg-x11-libSM-devel
	xorg-x11-libXmu-devel

Table 3. Secondary Dependencies Installed for Microsoft R Server on RHEL Systems

	cloog-ppl
	cpp
	font-config-devel
	freetype
	freetype-devel
	gcc
	glib2-devel
	libICE-devel
	libobjc
	libpng-devel
	libstdc++-devel
	libX11-devel
	libXau-devel
	libxcb-devel
	libXext-devel
	libXft-devel
	libXmu
	libXrender-devel
	libXt-develmpfr
	pixman-devel
	ppl glibc-headers
	tcl gmp
	tcl-devel kernel-headers
	tk
	xorg-x11-proto-devel
 	zlib-devel

Table 4. Secondary Dependencies Installed for Microsoft R Server on SLES 11 Systems

	cpp43
	fontconfig-devel
	freetype2-devel
	gcc
	gcc43 libX11
	gcc43-c++
	gcc43-fortran
	gcc43-objc
	glib2-devel
	glibc-devel
	libobjc43 tcl
	libpciaccess0
	libpciaccess0-devel
	libpixman-1-0-devel
	libstdc++-devel
	libstdc++43-devel
	libuuid-devel gmp
	linux-kernel-headers
	pkg-config
	tack
	tcl-devel
	xorg-x11-devel
	xorg-x11-fonts-devel
	org-x11-libICE-devel
	xorg-x11-libX11-devel
	xorg-x11-libXau-devel
	xorg-x11-libXdmcp-devel
	xorg-x11-libXext-devel
	xorg-x11-libXfixes-devel
	xorg-x11-libXp-devel
	xorg-x11-libXpm-devel
	xorg-x11-libXprintUtil-devel
	xorg-x11-libXrender-devel
	xorg-x11-libXt-devel
	xorg-x11-libXv-devel
	xorg-x11-libfontenc-devel
	xorg-x11-libxcb-devel
	xorg-x11-libxkbfile-devel
	xorg-x11-proto-devel
	xorg-x11-util-devel
	xorg-x11-xtrans-devel
	zlib-devel
