---
layout: single
title: Versions and Features
author_profile: true
---

<!-- # Temporary -->

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


