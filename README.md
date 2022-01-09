# The Vegetation Management Module

The Vegetation Management Module (VM) is a research tool that allows the simulation of realistic vegetation management activities within the [Functionally Assembled Terrestrial Ecosystem Simulator (FATES)](https://github.com/NGEET/fates).

*This documentation is a work in progress. Not everything may be here and not everything may be completely accurate yet. Bear with us.*

## Motivation:

The Selective Logging Module developed by Maoyi Huang et al. (Huang, Xu, et al. 2019) already allows for the simulation of logging within FATES, and has been a significant boon the FATES community.  However there are several important limitations to what the logging module can do:

- The only management simulated is harvest and the only form of harvest is a bole harvest.
- Planting and other intermediate operations are not implemented.
- Wood is harvested from all woody PFTs (trees and shrubs) in the canopy layer.  Harvesting from specific PFTs is not an option.
- Only woody PFTs can be harvested. Understory species can not be harvested.
- Only one type of logging events can be scheduled per simulation. This event may occur on a periodic basis (monthly, daily, etc.) or on one specific date.  Events cannot occur on an arbitrary sequence of dates or when certain conditions are met.
- Wood is harvested from all grid cells and all patches within them.  Specific locations or patches cannot be targeted.
- Logging occurs as a fraction of the number of plants present.  Removals by biomass are not possible.
- Harvest can be limited to plants over a given DBH but no other size criteria are provided for harvest.  This prevents the simulation of mangement that targets small or mid-sized trees.

The VM Module is a new module that was designed to address these issues while allowing compatibility with the logging module.  

## Details:

### Conceptual Model:

We chose to develop VM around a hierarchical conceptual model that bridges (abstracts) real-world management activities with the model-world in a logical way ((Diagram)). At the lowest level the module provides routines that handle fundamental processes of mortality and generation at the model level.  These routines are used to build representations of specific real-world management activities, which we call *operations*, at the next higher level of abstraction.  These operations can be combined in a sequence to represent a full *management cycle*.  Different management modalities we term *Management Regimes*.  You can build new management regimes for your system of interest without any code if the operations already exist.  If you need a new management operation you can code them without needing to know how to handle tricky things like mass conservation, fluxes, and reporting in the model.
...without needing to know all the details of how the FATES model deals with tricky...

While the initial development has focused on forest management the VM Module can be used to modify plant or remove any type of vegetation.

Core Concepts:
Operations
Flux Profiles

### Modularity:

The VM Module refactors some of the code in FATES to better isolate code pertaining to management, including the logging module, from the rest of code base. ((DIAGRRAM))  The goal was to limit the need for other FATES modules to be aware of management processes and assumptions.  This reduces dependancies and improves expansibility.

### Compatibility:

It you have used the logging module in the past you can continue to use it with VM installed and you will get identical results.  However, VM can do most of what the Selective Logging Module can do but with greater flexibility.  So while you can mix Logging Module and VM Events this is really intended to allow existing cases to work while you try out VM.


## Features:

### Version 1.0:
The initial version of the Vegetation Management Module used in my upcoming paper includes the following...
Operation events for:
- Planting
- Understory / competition control
- Three +? forms of stand thinning.
- Multiple forms of harvest
- ...

### Version 1.1:

Version 1.1 is currently in testing and adds several features that will be documented in an upcoming paper.

1. Rectangular region specification in events.
1. Wildcards in dates for repeating events.

	Dates can now include the wildcard character \*. Any position containing \* will match any value in that position, allowing a repeating event with a single line.

	Examples:
	- \*\*\*\*-01-01 = event every year on the first of January
	- 2018-\*\*-15 = repeat event on the 15th of every month in 2018
	- Even more complicated stuff like 20\*5-1\*-3\* = Nov 30 and Dec 30 & 31 in 2005, 2015, 2025 etc.
	
	This does not allow for every kind of repetion (i.e odd years) can be pretty useful.

1. Conditional events

	Heuristics / automatic triggering of events. [Conditional ]
	Many / some events now take additional arguments that specify conditions that a patch must match for the event to execute.

1. Was harvest\_trees() added here?????

1. Biomass harvest flux

	You can now harvest all the aboveground biomass of plants, not just the trunk, using the harvest\_biomass() event code (or in the code with the flux\_profile = agb\_harvest argument ?????).


## Getting Started

### Installation:

The Vegetation Management Module is not currently part of the official distribution of FATES.  You install VM similar to FATES, but pointing to our forks of the appropriate repositories as follows:



### Setting up a Simulation:

Running a simulation with the VM module requires:
- Two namelist settings.
- Preparing a VM Event Driver File. (See below for more on the file format and how to specify events.)



In `user_nl_clm` add the following lines:


Operations:

Event Driver File:



Management Beyond the VM Module:
There are aspects of management that aren't directly addressed in the VM Module code. For example, managed species may require new or revised PFTs.  ...

Beyond Management
While designed with management in mind we also recognize that the VM Module may be useful for a wider range of experiments. It provides a way to do initial experiments into ecological processes that are not yet represented in FATES.  

The *meaning* of a VM Event is up to you.  If you say a mortality event is a beetle outbreak, it is!  So if you have some information on the timing and demographics of an outbreak event you can easily set up a VM driver file to simulate it.  Most interesting ecological processes interact with and respond to climate so you are probably going to have to actually write some code to fully capture your process but VM may be a good way to perform some sensitivity studies before committing to the project.


Copyright?
