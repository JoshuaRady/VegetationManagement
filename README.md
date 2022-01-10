# The Vegetation Management Module

The Vegetation Management Module (VM) is a research tool that allows the simulation of realistic vegetation management activities within the [Functionally Assembled Terrestrial Ecosystem Simulator (FATES)](https://github.com/NGEET/fates).

*This documentation is a work in progress. Not everything may be here and not everything may be completely accurate yet. Bear with us.*

## In Brief...

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

## Details / Overview

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

<!--- This was too many layers:
## Versions:

### Host Land Model

#### CTSM -->

## Host Land Model Support

### CTSM:

The Vegetation Management Module requires a special version of CTSM that adds a couple of namelist items. The release version is the Vegetation\_Management branch (CHANGE) of our [CTSM fork](https://github.com/JoshuaRady/ctsm/tree/Vegetation_Management). See the [installation section](#Installation) for more installation help.

The current release version based off CTSM ctsm1.0.dev113_fates_api14.2.0.n01-2-g132dae57 (Confirm has and add tag?)
The current release version for VM 1.0 has the following properties:
<!--- The following table requires an extension not present in BBEdit but renders properly on GitHub. -->
| Property      | Value                                            |
| ------------- | ------------------------------------------------ |
| Base CTSM Tag | ctsm1.0.dev113\_fates\_api14.2.0.n01-2-g132dae57 |
| VM Repository | [https://github.com/JoshuaRady/ctsm.git](https://github.com/JoshuaRady/ctsm/tree/Vegetation_Management) |
| VM Branch     | Vegetation\_Management                           |
| VM Tag        | ToDo?????                                        |
| VM Hash       | 132dae57                                         |

(https://github.com/JoshuaRady/fates/tree/VegetationManagement).

### ELM:

A VM enabled version of ELM is not yet available (because no one has asked). However, it should be pretty easy to get working. If you are interested in using the Vegetation Management Module with ELM please reach out.

## Vegetation Module Versions

### VM Module Version 1.0:
The initial version of the Vegetation Management Module implements activities throughout the forest management life cycle including:

- Planting
- Understory / competition control
- Stand thinning (multiple versions)
- Harvest (selective to clearcut)

This version is documented and used in my upcoming paper XXXXX under development.

| Property       | Value                                          |
| -------------- | ---------------------------------------------- |
| Base FATES Tag | sci.1.43.2\_api.14.2.0 (12/14/2020)            |
| VM Repository  | [https://github.com/JoshuaRady/fates.git](https://github.com/JoshuaRady/fates/tree/VegetationManagement) |
| VM Branch      | VegetationManagement                           |
| VM Tag         | ToDo?????                                      |
| VM Hash        | 1e2ae37b\* (Feature frozen but bug updates are possible) |

### VM Module Version 1.1:

Version 1.1 is currently in testing and adds several features that will be documented in an upcoming paper.

#### New Features:

1. Rectangular region specifications for events.
2. Repeating events using wildcards in dates.

	Dates can now include the wildcard character '\*'. Any position containing \* will match any value in that position, allowing a repeating event with a single event specification.

	Examples:
	- \*\*\*\*-01-01 = repeat event every year on the first of January
	- 2018-\*\*-15 = repeat event on the 15th of every month in 2018
	- Even more complicated stuff like 20\*5-1\*-3\* = Nov 30 and Dec 30 & 31 in 2005, 2015, 2025 etc.
	
	This does not allow for every kind of repetion (i.e odd years) can be pretty useful.

1. Conditional events

	Heuristics / automatic triggering of events. [Conditional ]
	
	In version 1.0 events affected all patches of a site.  The 
	
	Many / some events now take additional arguments that specify conditions that a patch must match for the event to execute.

1. Was harvest\_trees() added here?????

1. Biomass harvest flux

	You can now harvest all the aboveground biomass of plants, not just the trunk, using the harvest\_biomass() event code (or in the code with the flux\_profile = agb\_harvest argument ?????).


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

## Using the Vegetation Management Module

The Quick Start Section gives an example of how to run a simulation with the Vegetation Management Module.  Here we explain the process in more detail.  You will need to know how to configure, build, and run a CTSM-FATES simulation so if you haven't done that consult some of the excellent resources that can get you going with that. [ADD REFS]

Start as you would with any simulation.  Figure out what you want to simulate...

If you are comparing to data...

As you design your simulation think about what management activities you want to simulate, where, when, and under what conditions.  Break down the management into its component steps.  For example if you want to simulate a 

Set up your simulation as you normall would.



### Operations and Events:

VM abstracts management processes in much the way a manager would.
VM represents management as specific activities 

Operations represent specific real-world human interventions into the behavior of an ecosystem, such as planting or timber harvest. Events are individual occurrences of an operation, e.g. planting tulips in Copenhagen in September 1991. You tell FATES what management to simulate by specifying one or more events in a driver file.

<!--- This section covers material documented in great detail in the code. Expect that the code is the primary documentation and will reflect changes that may not yet be documented here. -->
### VM Prescribed Event Driver File:

The Vegetation Prescribed Management Event Driver File uses a custom text format designed to be human readable and easily written by hand.
 Since the language of FATES is Fortran we borrow a few conventions from that language.

#### General File Structure:

There should be at least two lines.  The first (non-comment) line should be a header line that specifies the field names.  The following lines are event lines.

#### File Format [Where does this go?  Drop the header?]
The file should be UTF-8 with the extension `.txt` and UNIX line endings. Other text variants may work but have not been tested.

Minimal Generic Example:

	Date        Lat  Lon  EventSpec
	YYYY-MM-DD  X.X  Y.Y  do_something(x = 3.0, y = 2, z = 13.1)

#### Blank lines:

Blank lines are ignored and can occur anywhere in the file.

#### Comments:

Any content following a '!' on a line is ignored.

#### Header Line:

Ideally the header will occur as the first line of *content*.  Comment lines describing the file (e.g. creator, date, project, etc) may precede it (a good idea) but all events should follow it.

The header line is primarily for readability and we do minimal checking of it, so most typos will not cause problems. The field order is fixed and changing the order in the header will not overide that.

#### Field Delimiters:

The header columns and event fields are delimited by one or more spaces. For flexibility column widths are not specified. While aligning columns for readability is encouraged it is not required. Any number of spaces between fields will be accepted.

Tabs or other whitespace may cause weirdness. Do not use them.

#### Event Lines:

Each event is placed on its own line, preferably in chronological order.

##### Event Field Order:

The field order is the **date** of the event, the **latitude** and **longitude** (in model grid units) where the event should occur, followed by an **event specification**, which is written like a Fortran function call (see generic examples above and specific ones below).

##### Date Field

Dates should follow the following rules

- Field Order:
	- Year first, then month, then day.
- Field Separators:
	- Currently we handle slash, dash, and period (/,-,.) but not space, since it is our parental (data line) field delimiter.
- Digits:
	- We allow 1 or 2 digit month and day strings.  Leading 0s Are allow but not required.
	- We allow years of up to 10 digits (in case you have a huge allocation).
- Thousands separator:
	- No commas or periods to separate 1000s in years are allowed.

Valid Examples:

	YYYY-MM-DD
	YYYY/MM/DD
	00YY.0M.0D
	YY-M-D
	YYYYYYYY/MM/DD
    Even this would work: 0000YYYY/M.DD

Starting in version 1.1 dates may now include the wildcard character '\*'. Any position containing \* will match any value in that position, allowing a repeating event with a single event specification.

Wildcard Examples:

	****-01-01    = repeat event every year on the first of January
	2018-**-15    = repeat event on the 15th of every month in 2018
	20\*5-1\*-3\* = Nov 30 and Dec 30 & 31 in 2005, 2015, 2025 etc. (Weird but would work.)

##### Coordinate Fields

The geographic coordinates where an event should occur should be specified as decimal values in model grid style (latitude -90 - 90, logitude 0 - 360, with no cardinal directions).

<font color="green">
Right:

	Lat   Lon
	38.0  281.5

</font>

<font color="red">
Wrong:

	Lat        Lon
	38°1′48″N  78°28′44″W
</font>

FATES is not privy to the boundaries of grid cells so single coordinates will only match when they are 'close' to the center of a grid cell.

Passing the special value -999 for both latitude and longitude specifies 'everywhere'.  (This is a good option for single point simulations.)

Starting in version 1.1 coordinates may be passed either as single values, specifying a point or grid cell, or a range, specifying a rectangular region, in Fortran 2003 array format, i.e. with square brackets and values separated by a colon.

Format Examples:

	38.0             Proper syntax for a single value.
	[33.5:42.5]      Proper syntax and style for a range.
	[ 33.5 : 42.5 ]  While it is bad style we allow unnecessary whitespace.  Reads as above.

Regions of different events can overlap but date rules still apply.  Only one event can occur in a given location per time-step.

(Combining single values for one coordinate and a range or -999 for the other should work and will produce a stripe. Weird.)

##### Event Specifications

The event specification tells VM which operation to perform.  It is formated like a Fortran function call.

Examples:

	event_name(arg1 = val1, arg2 = [val2.1, val2.2, val2.3], arg3 = val3)

*The full list of events and their interfaces (arguments) are currently only available in the code.*

###### Event Type or Name String:

The event specification starts with an event name (i.e. the "function name").

    !   This event name must match an existing event type.  Event specifications map to site level
    ! subroutines that execute the event.  These subroutines declare their interface: the arguments
    ! and values that they take.

###### Arguments:

Arguments follow the event name, enclosed in parentheses, and separated by commas.

Arguments may be optional, depending on the event, but all arguments that are present must be passed by name.  Name value pairs are separated by '='.
The order of arguments does not matter but it is best to keep it same as the order in the event's interface declaration.

Some arguments can take arrays of values.  These are specified using square brackets and commas, i.e. name = [value1, value2].  Single values will be accepted for array arguments.

###### White Space:

Spaces are allowed between elements in event specifications for readability but are not required.  Other whitespace should be avoided. The style suggestion is to include spaces between arguments and name values pairs but not next to parentheses.

	Harder to read:  do_something ( dbh_min=3.0,dbh_max=55.5,z=13.1 )
	Earlier to read: do_something(dbh_min = 3.0, dbh_max = 55.5, z = 13.1)

###### Case:
Event and argument names are case sensitive.

### Using Events?????

VM harvest events can be specified with height...

#### Conditional Events:

In version 1.0 events affect all patches of a site where they occur.  Starting in version 1.1 there are conditional variants of some (but not all) operations.  For example `replant()` is a conditional version of `plant()` that only plants when a patch is bare.

Some conditional events take additional criteria arguments that a patch must match for the event to execute.  These arguments start with `if_`.  clearcut_if() takes the same arguments as clearcut() but only performs the action for patches of the appropriate age, basal area, or mean DBH (if passed).

	clearcut([pfts], [dbh_min], [ht_min])
	clearcut_if([pfts], [dbh_min], [ht_min], [patch_fraction], [if_age_min], [if_age_max], [if_bai], [if_dbh_mean], [if_dbh_max])

When performing simulations with Pure PPA there is generally only one patch per site.  In this case conditionals control whether an event occurs at all in that grid cell.

Some events even in version 1.0 can exhibit conditional behavior indirectly.  For example the harvest routines all allow you to specify range of trees sizes to remove.  If there are no matching trees the event won't occur.  However, for some more complicated operations (clearcut) the conditionals are required.

#### Detailed Example Driver File:

```
! Example_VM_Driver_File.txt
! Comments can go anywhere.
! The first content line should be the header line:
Date        Latitude  Longitude EventSpec
1980-03-01  ...


! Check once ever year and harvest when the mean stand DBH reaches 26 cm:
19**-01-01  -999      -999      clearcut_if(if_dbh_mean = 26.0)

! Replant with PFT 2 if a harvest has recently occurred:
19**-05-01  -999      -999      replant(pfts = 2)

...

```


## Draft Sections

Management Beyond the VM Module:
There are aspects of management that aren't directly addressed in the VM Module code. For example, managed species may require new or revised PFTs.  ...

Beyond Management
While designed with management in mind we also recognize that the VM Module may be useful for a wider range of experiments. It provides a way to do initial experiments into ecological processes that are not yet represented in FATES.  

The *meaning* of a VM Event is up to you.  If you say a mortality event is a beetle outbreak, it is!  So if you have some information on the timing and demographics of an outbreak event you can easily set up a VM driver file to simulate it.  Most interesting ecological processes interact with and respond to climate so you are probably going to have to actually write some code to fully capture your process but VM may be a good way to perform some sensitivity studies before committing to the project.


Copyright?
