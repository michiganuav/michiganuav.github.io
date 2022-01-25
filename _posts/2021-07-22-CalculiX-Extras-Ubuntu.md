---
title: Calculix Extras for Ubuntu
author: Peter A. Gustafson
tags: CalculiX Ubuntu
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

# Archived!

-   This post is outdated and is kept only for archival purposes.
-   See [CalculiX Extras for Ubuntu](/calculix_extras_ubuntu.html) for the latest build

# Install calculix software dependencies

This was most recently tested to work on ubuntu 20.04.2 LTS.

1.  pull the latest list of packages from ubuntu
```console
sudo apt-get update
```
1.  Upgrade to the latest available packages of those that are
	currently installed. This will take some time (the first
	time especially).
```console
sudo apt-get upgrade
```
	1.  Install the compilers and libraries that are required:
```console
sudo apt-get install libexodusii-dev libspooles-dev libblas3 libblas-dev \
                liblapack3 liblapack-dev libarpack2 libarpack2-dev \
				libparpack2 gfortran gcc unzip make
```
	1.  Install Paraview and optionally emacs (or your preferred
		text editor):
```console
sudo apt-get install paraview emacs
```
	1.  Optionally install the Intel Math Kernel Library (MKL)
	    -   This would make blas, lapack, and arpack redundant (installed above) and may provide faster solutions
		-   Further, it is closed source non-free and may violate some code licensing restrictions
		-   It does, however, have the [the Pardiso library](https://www.pardiso-project.org/) (which can optionally be used by ccx for improved solver speed)
		-   The Makefile (see below) assumes this has been installed
```console
sudo apt-get install intel-mkl
```
	1.  Optionally install [the Pardiso library](https://www.pardiso-project.org/) without the Intel Math Kernel Library (MKL) 
	    -   (There is no need if you have installed the Intel MKL library)
	    -   This may provide faster solutions
		-   Further, it is closed source non-free and may violate some code licensing restrictions
		-   You will need to edit the Makefile to address this path

# Setup a CalculiX environment

1.  Follow the instructions 2-6 at [here](calculiX_extras.html)
1.  Alter step 7 (where the Makefile is updated): 
```console
cd src
wget https://openaircraft.com/assets/code/calculix/ubuntu-ccx-makefile-2.17.zip
unzip -o ubuntu-ccx-makefile.zip
```
1.  Edit the makefile as necessary (this may not be necessary depending on your choice of patches)
    -   It is assumed that the pardiso solver is available
		-   i.e., the Intel MKL library OR the Pardiso library is installed.
	-   The additional solvers (CUDA-CUSP, etc) are not compiled in by default but can be enabled within the Makefile
	-   ExodusII is compiled by default but can be disabled within the Makefile
```console
emacs Makefile
```
1.  Then make the binary:
```console
make -j8
```
1.  For convenience, add this working directory to your PATH
    variable. This needs only be done once:
```console
echo PATH=$PATH:~/CalculiX/ccx_2.17/src >> ~/.bashrc
source ~/.bashrc
```

# Run a test job

```console
cd
cd CalculiX
mkdir test
cd test
wget https://openaircraft.com/assets/examples/beamlin.zip
unzip beamlin.zip
ccx_2.17 beamlin -o exo
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

# Support the maintenance of this software

Please donate to support the maintenance of this code!

{% include donate.html %}
