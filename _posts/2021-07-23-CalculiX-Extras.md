---
title: Calculix Extras
tags: CalculiX Paraview
author: Peter A. Gustafson
permalink: calculix_extras.html
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

**Thank you for your interest in CalculiX Extras.

([Last update 7/23/2021](https://ccx.openaircraft.com/ccx_extras_changelog.txt))** 

**Extensions written by Peter A. Gustafson.**

**Current patch level - 2.17**

# Capabilities

## CalculiX Extras builds on the [CalculiX](https://www.calculix.de/) v2.17 code base

-   It adds CUDA based solvers. Please note this is research level code.
    It is considered a stable code but must be used with due diligence.
    -   Currently, these solvers can be called for static analysis in
        mechanical models in CalculiX.
        -   [Cuda-Cusp](https://github.com/cusplibrary/cusplibrary)
            -   (Outstanding performance for appropriate models.)
        -   [SuiteSparse Cholmod](https://people.engr.tamu.edu/davis/suitesparse.html)
            -   (Modest performance at last testing in ccx version
                2.11. However, since then CUDA has been added to
                Cholmod and it has not been subsequently testing with
                CalculiX Extras)
-   It extends CalculiX to write results into
    [ExodusII](https://gsjaardema.github.io/seacas/) format.
    Postprocessing can be accomplished with several readers. A
    recommended postprocessor is [Paraview](https://www.paraview.org/).

![ccx_exodusII](/assets/images/2016/01/ccx_exodusII.png)

# Build Instructions

These instructions are limited. They assume you are able to build
CalculiX from source. *Warning: building all these libraries from source
can be a challenge for seasoned developers. Ubuntu specific instructions
can be found at [CalculiX Extras for Ubuntu](CalculiX_Extras_Ubuntu.html)*

1.  Choose your extra: exo, solver, or both
    1.  For exo files: download, build, and install
        [ExodusII](https://sourceforge.net/projects/exodusii/)
    2.  For additional solvers: download and install:
        1.  [Cuda-Cusp](https://cusplibrary.github.com) (The cuda based
            solver requires an install of Nvidia's [CUDA SDK and
            libs](https://developer.nvidia.com/cuda-downloads)
        2.  SuiteSparse (which includes Cholmod )
2.  Download [CalculiX ccx version 2.17](/assets/code/calculix/ccx_2.17.src.tar.bz2)
```console
wget https://openaircraft.com/assets/code/calculix/ccx_2.17.src.tar.bz2
```
3.  Download [CalculiX Extras patches](#Download)
```console
wget https://openaircraft.com/assets/code/calculix/ccx_extras.2.17.Makefile.patch
wget https://openaircraft.com/assets/code/calculix/ccx_extras.2.17.exo.patch
wget https://openaircraft.com/assets/code/calculix/ccx_extras.2.17.solver.patch
```
4.  Unpack ccx_2.17.src.tar.bz2
```console
tar xavf ccx_2.17.src.tar.bz2
```
5.  Patch the Makefile
```console
cd CalculiX/ccx_2.17
patch -p2 < ../../ccx_extras.2.17.Makefile.patch
```
6.  Apply patches your extra: exo, solver, or both (order of patch
    application doesn't matter)
```console
patch -p2 < ../../ccx_extras.2.17.exo.patch
patch -p2 < ../../ccx_extras.2.17.solver.patch
```
7.  Modify the Makefile based on the locations of your libraries
```console
cd src
emacs Makefile
```
8.  Make
```console
make
```
9.  Send bug reports to peter.gustafson@wmich.edu

# FAQ

-   If you've made an attempt... Yes, I'd be glad to help you!
	-   But you must be able to compile ccx (without my patches)
		first.
	-   See the first line of the build instructions... this can be
		tough and this is the minimum starting point for support from
		me.
	-   (PS The ccx mailing list is great.)
-   If you've not yet made an attempt, please try before you contact
    me. [See the FAQ to understand my reasoning...](/FAQ.html)

# Usage

-   The solvers can be called using the *static keyword:
```console
*static, solver=cudacusp
*static, solver=cholmod
```
-   ExodusII output format is called on job execution using the command
    line.
```console
ccx -i jobname -o exo
```

# Benchmarking

A rigorous round of
[benchmarking](https://arc.aiaa.org/doi/pdf/10.2514/6.2014-0346) has
been completed. If you are unable to download the paper, please
contact me using the submission form below.

-   -   I'm happy to collaborate with others who are interested in
        further benchmarking.
    -   Benchmarking will be highly dependent on hardware and software
        configuration.
    -   I'm seeking additional "real world" models for use in
        benchmarking.
    -   An NSF grant to WMU funded the hardware used for benchmarking.

# Known Bugs and Functional Deficits

-   The solvers have only been applied to static solid mechanics models.
-   The solvers have have not been actively tested since version 2.11 so
    your mileage may vary.
-   There are known limitations for output requests with ExodusII
    -   Several output requests to ExodusII format are untested.
    -   It is not currently possible to change output requests *between
        steps*
    -   Node sets are partially implemented but element sets are not
        implemented
-   **Developing for CalculiX**
    -   Doxygen was used to generate documentation which may help with
        development for CalculiX. The [documentation is
        here](https://doxygen.openaircraft.com/ccx-doxygen/index.html).
        Note also the user documentation for
        [ccx](https://ccx.openaircraft.com/ccx-doc/ccx_2.17/doc/ccx/index.html)
        and
        [cgx](https://ccx.openaircraft.com/ccx-doc/cgx_2.17/doc/cgx/index.html).

# Download

-   In order to quantify interest and justify ongoing development, I
	ask that you submit your name and organizational info via email to
	peter.gustafson@wmich.edu. *This info will not be used for any
	marketing. It will not be sold.* Without any expressed interest,
	my time on this project is not justified.
	-   Your participation is voluntary. Please resubmit with each version
		you download.
	-   Thank you for your consideration.

## Download the latest patches.

-   [ccx_extras.2.17.Makefile.patch](/assets/code/calculix/ccx_extras.2.17.Makefile.patch)
-   [ccx_extras.2.17.exo.patch](/assets/code/calculix/ccx_extras.2.17.exo.patch)
-   [ccx_extras.2.17.solver.patch](/assets/code/calculix/ccx_extras.2.17.solver.patch)

# Support maintenance of this code

Please donate to support maintenance of this code!

{% include donate.html %}
