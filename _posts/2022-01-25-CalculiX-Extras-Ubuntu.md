---
title: CalculiX Extras for Ubuntu
author: Peter A. Gustafson
tags: CalculiX Ubuntu
permalink: /calculix_extras_ubuntu.html
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

These instructions compile version 2.19 of [CalculiX](http://www.calculix.de/)

# Clone, install dependencies, and compile

Note this will require sudo privileges

## Initial build
```console
cd
git clone https://github.com/gustafson/CalculiX
cd CalculiX
git checkout ubuntu
./ubuntu-install.sh
```

## Update on new release
```console
cd ~/CalculiX
git pull
./ubuntu-install.sh
```

## Notes

1.   The script above installs the Intel Math Kernel Library (MKL)
	-   This makes blas, lapack, and arpack redundant (also installed above) and may provide faster solutions
	-   It is closed source non-free and may violate some code licensing restrictions
	-   It does, however, have the [the Pardiso library](https://www.pardiso-project.org/) (which can optionally be used by ccx for improved solver speed)
	-   The Makefile (see below) assumes this has been installed
-   You could optionally install [the Pardiso library](https://www.pardiso-project.org/) without the Intel Math Kernel Library (MKL) 
    -   (There is no need if you have installed the Intel MKL library)
    -   This may provide faster solutions
	-   Further, it is closed source non-free and may violate some code licensing restrictions
	-   You will need to edit the Makefile to address this path


# Run a test job

```console
cd ~/CalculiX
mkdir test
cd test
wget https://www.openaircraft.com/assets/examples/beamlin.zip
unzip beamlin.zip
ccx beamlin -o exo
```

# Visualize with Paraview

```console
paraview beamlin.exo &
```
1.  In paraview, in the lower left corner, toggle U, S, and E.
1.  Click "apply" at the top Change the "solid color" (near
    top) to "S".
1.  Change "Surface" to "Surface with Edges"
1.  Spin the model in the window

# Optionally install netcdf-bin

It is sometimes useful to examine the metadata (and other data) from
the exodus file without using paraview.  This can be done with
netcdf-bin:

```console
sudo apt-get install netcdf-bin
ncdump beamlin.exo
```

# Issues

Please report issues/bugs with CalculiX Extras via
[github](https://github.com/gustafson/CalculiX/issues).  Note
issues/bugs with CalculiX (exclusive of CalculiX Extras) should be
discussed on the [CalculiX discourse group](https://calculix.discourse.group).

This was most recently tested using 20.04.3 LTS Focal Fossa and 18.04.6 LTS Bionic Beaver. 
You may check your install using the following:
```console
lsb_release -a
```

For example:
```console
pete@ubuntu:~/CalculiX$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.3 LTS
Release:        20.04
Codename:       focal
```

# Support the maintenance!

Please donate to support the maintenance of this code!

{% include donate.html %}
