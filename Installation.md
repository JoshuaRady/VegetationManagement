---
layout: single
classes: wide
title: Getting Started with the Vegetation Management Module
author_profile: true
---

For the following it is a good idea to know how to configure, build, and run a CTSM-FATES simulation as well as plotting history file output. If you haven't learned that yet consult some of the excellent resources that can get you going with CTSM.

<!-- ADD REFS -->

## Installation

The Vegetation Management Module is not currently part of the official distribution of FATES.  You install VM similar to FATES, but pointing to our forks of the appropriate repositories as follows:

```bash
#Go to the directory where you install your repositories on your supercomputer of choice:
cd ~/MyRepos/

#===============================================================================
#Clone the Vegetation Management fork of CTSM:
#===============================================================================
git clone --branch VegetationManagement https://github.com/JoshuaRady/ctsm.git ctsm_fates_VegetationManagement

cd ctsm_fates_VegetationManagement

#-----------------------------------------------------------
#I like to confirm what I have before I move on:
#-----------------------------------------------------------
git describe
#Should be ctsm1.0.dev113_fates_api14.2.0.n01-2-g132dae57 for VM version 1.0.
#Check the version page for other versions.

git rev-parse --short HEAD
#Should be 132dae57 for VM version 1.0.

git branch
#Should be *VegetationManagement.

#===============================================================================
#Now we install the Vegetation Management enabled version of FATES inside CTSM:
#===============================================================================
#Open Externals_CLM.cfg with your favorite editor and change the repo_url line to:
#repo_url = https://github.com/JoshuaRady/fates

#Run manage_externals:
./manage_externals/checkout_externals

#A bunch of stuff happens here so a lot of reporting will display.

#Switch to the appropriate FATES branch:
cd src/fates/
git checkout VegetationManagement

#-----------------------------------------------------------
#Confrim you have what you want:
#-----------------------------------------------------------
git branch
#Should be *VegetationManagement

git rev-parse --short HEAD
#Should be 1e2ae37b for version 1.0 or the hash shown in the versions page.

```

Well done, you have completed installation of CTSM-FATES with the Vegetation Management Module!

## Setting up a Simulation: Quick Start

*This section is incomplete.  Please check back later.*

Here we will run a quick example simulation that shows the Vegetation Management Module in work.

Running a simulation with the VM Module requires:
- Creating a new FATES case.
- Preparing a VM Event Driver File.
- Setting two namelist variables.
- Compliling and submitting the case.

### Create a FATES Case

The basic settings for this example case are based on a modernized version of the [FATES tutorial case](https://github.com/NGEET/fates/wiki/Running-FATES:-A-Walk-Through,-February-2019,-Case-1).  It is a single point coldstart simulation in Brazil running from 2001-2005.

```bash
#Go to a directory where you like to set up your cases:
$ cd /path/to/CasesDir/

#Make a case:
#The following settings are for a system set up like NCAR's Cheyenne.  You need to add your own project code:
$ ~/MyRepos/ctsm_fates_VegetationManagement/cime/scripts/create_newcase --case VM_QuickStart --compset 2000_DATM%GSWP3v1_CLM50%FATES_SICE_SOCN_SROF_SGLC_SWAV --mach cheyenne --res 1x1_brazil --project YourPlojCode0001 --walltime 2:00:00 -q economy -v --run-unsupported
$ cd VM_QuickStart

#Set XML settings for the run:
$ ./xmlchange STOP_N=5
$ ./xmlchange RUN_STARTDATE=2001-01-01
$ ./xmlchange STOP_OPTION=nyears
$ ./xmlchange DATM_CLMNCEP_YR_START=1996
$ ./xmlchange DATM_CLMNCEP_YR_END=2001
$ ./xmlchange CLM_FORCE_COLDSTART=on

#Set up to create the namelists:
./case.setup
```

### VM Event Driver File

In a directory you like to store input files for your cases create a file named `VMDF_QuickStart.txt` (the file name doesn't really matter, it just needs to be meaningful to you).  Add the following lines to the file:

*Driver file example to come!*

See [here](/VMDriverFileFormat) for more on the file format and how to specify events.

### VM Namelist Variables

In `user_nl_clm` add the following lines:

```bash
#This tells FATES to output the wood harvested each month:
hist_fincl1 = 'WOOD_PRODUCT'
hist_mfilt = 1

#This tells FATES to load events from the file we created and where to find it:
use_fates_vm_driver_file = .true.
fates_vm_driver_filepath = '/path/to/our/InputFiles/VMDF_QuickStart.txt'
```

### Compile and Submit

Compile the case:

```bash
$ ./case.setup --reset
#We use qcmd as is recommeneded on NCAR's Cheyenne.  Your system may differ:
$ qcmd -- ./case.build
```

Compilation can take several minutes and will output a bunch of progress output.  When the `MODEL BUILD HAS FINISHED SUCCESSFULLY` submit it:

```bash
$ ./case.submit
```

Again, you will get a bunch of reporting.  When the simulation completes examine the history variables XXXXX, YYYYY, and WOOD_PRODUCT with your favorite tool (R, Pyton, ncview, NCL, etc.).  You should get data that looks like...

*Data examples to come!*




