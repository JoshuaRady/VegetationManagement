---
layout: single
title: Using the Vegetation Management Module
author_profile: true
---

*This page is incomplete.  Please check back later.*

The [Quick Start Section](/Installation/) gives an example of how to run a simulation with the Vegetation Management Module.  Here we explain the process in more detail.  

<!-- Copied to installation:
You will need to know how to configure, build, and run a CTSM-FATES simulation. If you haven't learned that yet consult some of the excellent resources that can get you going with CTSM. [ADD REFS] -->

Start as you would with any simulation.  Figure out what you want to simulate...

If you are comparing to data...

As you design your simulation think about what management activities you want to simulate, where, when, and under what conditions.  Break down the management into its component steps.  For example if you want to simulate a 

Set up your simulation as you normally would.



### Operations and Events:

VM abstracts management processes in much the way a manager would.
VM represents management as specific activities ...

Operations represent specific real-world human interventions into the behavior of an ecosystem, such as planting or timber harvest. Events are individual occurrences of an operation, e.g. planting tulips in Copenhagen in September 1991. You tell FATES what management to simulate by specifying one or more events in a driver file.

Core Concepts:
Regimes
Management cycles
Operations
Flux Profiles

Building new operations...

Logging events...
				While you can mix Logging Module and VM Events this is mainly intended to allow existing cases to continue to work [while you try out or transition to VM].