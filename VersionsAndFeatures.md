---
layout: single
title: Versions and Features
author_profile: true
toc: true
toc_sticky: true
---

<span class=disclaimer> The Vegetation Management Module is under active development and this is pre-release documentation.  There may be some changes prior to publication. </span>

## Vegetation Management Module Versions

### VM Module Version 1.0
The initial version of the Vegetation Management Module implements activities throughout the forest management life cycle including:

- Planting
- Understory / competition control
- Stand thinning (multiple versions)
- Harvest (selective to clearcut versions)

Events can be scheduled by date and can be specified to occur in specific grid cells or everywhere at once.

This version is documented and used in an upcoming paper under development.

<!--- The following table requires an extension not present in BBEdit but renders properly on GitHub. -->

| Property       | Value                                                     |
| -------------- | --------------------------------------------------------- |
| Base FATES Tag | sci.1.43.2\_api.14.2.0 (12/14/2020)                       |
| VM Repository  | [https://github.com/JoshuaRady/fates.git](https://github.com/JoshuaRady/fates/tree/VegetationManagement) |
| VM Branch      | VegetationManagement                                      |
| VM Tag         | Pending                                                   |
| VM Hash        | *1e2ae37b* (Feature frozen but bug updates are possible.) |

### VM Module Version 1.1

Version 1.1 is currently in testing and adds several features that will be documented in second upcoming paper.

#### New Features

1. Rectangular region specifications for events:

	Events can now be specified as occurring at a single point, everywhere or within a rectangular region in grid space.  Regions can overlap allowing for complex regional patterns to be built with only a few events.

2. Repeating events using wildcards in dates:

	Dates can now include the wildcard character '\*'. Any position containing \* will match any value in that position, allowing a repeating event with a single event specification.

	Examples:
	- `\*\*\*\*-01-01` = repeat event every year on the first of January
	- `2018-\*\*-15` = repeat event on the 15th of every month in 2018
	- Even more complicated stuff like `20\*5-1\*-3\*` = Nov 30 and Dec 30 & 31 in 2005, 2015, 2025 etc.
	
	This does not allow for every kind of repetion (i.e odd years) can be pretty useful.

1. Conditional events:

	In version 1.0 events will always occur when scheduled and affect all patches of a site.  Now some events take additional arguments that specify conditions that a patch must match for the event to execute (age, composition, structure, etc.).  Combined with repeating events conditionals can be used to implement basic management heuristics.

1. Biomass harvest flux:

	You can now harvest all the aboveground biomass of plants, not just the trunk, using the `harvest\_biomass()` event code (or in the code with the `flux\_profile = agb\_harvest argument`).

## Host Land Model Support

### CTSM

The Vegetation Management Module requires a matched branch of our CTSM fork that adds a couple of namelist items.  See the [installation section](/Installation#Installation) for more installation help.

The current (pre)release version paired to VM 1.0 has the following properties:

| Property      | Value                                            |
| ------------- | ------------------------------------------------ |
| Base CTSM Tag | ctsm1.0.dev113\_fates\_api14.2.0.n01-2-g132dae57 |
| VM Repository | [https://github.com/JoshuaRady/ctsm.git](https://github.com/JoshuaRady/ctsm/tree/VegetationManagement) |
| VM Branch     | Vegetation\_Management                           |
| VM Tag        | Pending                                          |
| VM Hash       | 132dae57                                         |

<!-- Add date for tag? -->

### ELM

A VM enabled version of ELM is not yet available (because no one has asked). However, it should be pretty easy to get working. If you are interested in using the Vegetation Management Module with ELM please reach out.
