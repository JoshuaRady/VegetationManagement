---
layout: single
title: Installation
author_profile: true
---

<!-- # Temporary -->

## Getting Started

### Installation

The Vegetation Management Module is not currently part of the official distribution of FATES.  You install VM similar to FATES, but pointing to our forks of the appropriate repositories as follows:

```
#Go to the directory where you install your repositories:
cd ~/myrepos/

#Clone the Vegetation Management fork of CTSM:
git clone --branch Vegetation_Management https://github.com/JoshuaRady/ctsm.git ctsm_fates_Vegetation_Management

cd ctsm_fates_Vegetation_Management

#I like to confirm what I have before I move on:
git describe
#Should be ctsm1.0.dev113_fates_api14.2.0.n01-2-g132dae57
#Or?
#Check the version section for the correct version...

git rev-parse --short HEAD
#Should be 132dae57 ?????

git branch
#Should be *Vegetation_Management.
	
#Now we install the Vegetation Management enabled version of FATES inside CTSM:

#Open Externals_CLM.cfg with your favorite editor and change the repo_url line to:
#repo_url = https://github.com/JoshuaRady/fates

#Run manage_externals:
./manage_externals/checkout_externals

#A bunch of stuff happens here...

#Switch to the appropriate FATES branch:
cd src/fates/
git checkout VegetationManagement

#Confrim you have what you want:
git branch
#Should be *VegetationManagement

git rev-parse --short HEAD
#Should be the hash shown in the version section?????

```

### Setting up a Simulation / Quick Start

Running a simulation with the VM module requires:
- Two namelist settings.
- Preparing a VM Event Driver File. (See below for more on the file format and how to specify events.)



In `user_nl_clm` add the following lines:


