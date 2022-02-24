---
layout: splash
title: The Vegetation Management Module
author_profile: true
header:
  image: /assets/images/Misty_VM_Banner.jpg
  image_description: "A misty morning in a clearcut."
---
The Vegetation Management Module (VM) is a research tool that allows the simulation of realistic vegetation management activities within the [Functionally Assembled Terrestrial Ecosystem Simulator (FATES)](https://github.com/NGEET/fates).

<span class=disclaimer>This documentation is a work in progress.  The Vegetation Management Module is under active development.  Not everything is fully documented yet and not everything may accurately reflect the most recent changes to the code. Please bear with us.</span>

## In Brief

Humans alter ecosystems through management of vegetation in the production of food, fodder, fuel, fiber, and timber.  Management is extensive and has impacts on climate and the provisioning of ecosystem services.

The Vegetation Management Module allows you to simulate many management activities like planting, harvest, species selection, competition control, etc. in two the worlds leading earth system models (CESM / CLM & E3SM / ELM).  It works for single point and regional FATES simulations and allows you to control what happens where and when.  This can be used to reproduce a known series of historical events in order to compare to observations or to simulate an arbitrary management regime under present or future climate.

## Motivation

The Selective Logging Module developed by Maoyi Huang et al. (Huang, Xu, et al. 2019) allows for the simulation of logging within FATES, and has been a significant boon the FATES community.  However there are several important limitations to what the logging module can do:

- The only management simulated is logging and the only form of logging is a bole harvest.
- Planting and other intermediate operations are not implemented.
- Wood is harvested from all woody PFTs (trees and shrubs) in the canopy layer.  Harvesting from specific PFTs is not an option.
- Only woody PFTs can be harvested. Understory species can not be harvested or removed.
- Only one type of logging event can be scheduled per simulation. This event may occur on a periodic basis (monthly, daily, etc.) or on one specific date.  Events cannot occur on an arbitrary sequence of dates or when certain conditions are met.
- Wood is harvested from all grid cells and all patches within them.  Specific locations or patches cannot be targeted.
- Logging occurs as a fraction of the number of plants present.  Removals by basal area or biomass are not possible.
- Harvest can be limited to plants over a given DBH but no other size criteria are available.  This prevents the simulation of mangement that targets small or mid-sized trees.

The VM Module is a new module that was designed to address these issues while allowing compatibility with the Selective Logging Module.

## Conceptual Model

We chose to develop VM around a hierarchical conceptual model that abstracts real-world management activities within the model-world in a logical way. At the lowest level the module provides routines that handle fundamental processes of mortality and recruitment at the model level.  These routines are used to build representations of specific real-world management activities, which we call ***operations***, at the next higher level of abstraction.  These operations can be combined in a sequence to represent full ***management cycles***.  Different management modalities we term ***Management Regimes***.

<!--  Diagram  goes here -->

You can build new management regimes for your system of interest without any code if the components operations already exist.  If you need a new management operation you can code them without needing to know all the details of how the FATES model deals with tricky low level things like mass conservation, fluxes, and reporting in the model.

While the initial development of VM has focused on forest management the VM Module can be used to modify any type of simulated vegetation.

### Modularity

The VM Module refactors some of the code in FATES to better isolate code pertaining to management, including the logging module, from the rest of code base.  The goal was to limit the need for other FATES modules to be aware of management processes and assumptions.  This reduces dependancies and improves expansibility.

<!--  Diagram  goes here -->

### Compatibility

It you have used the logging module in the past you can continue to use it with VM installed and you will get identical results.  While you can mix Logging Module and VM Events this is mainly intended to allow existing cases to continue to work.  VM can do most of what the Selective Logging Module can do but with greater flexibility. See the section on events for more details.
<!-- Add link to events section! -->

<!--- Should we add notes here about the lack of infrastructure mortality. -->
