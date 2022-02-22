---
layout: single
title: VM Driver File Format
author_profile: true
---

<!--- This section covers material documented in great detail in the code. Expect that the code is the primary documentation and will reflect changes that may not yet be documented here. -->
### VM Prescribed Event Driver File:

The Vegetation Management Prescribed Event Driver File uses a custom text format designed to be human readable and easily written by hand (and not be that hard to automate). Since the language of FATES is Fortran we borrow a few conventions from that language.

<!--- Basics? -->

#### General File Structure at a Glance:

There should be at least two lines.  The first (non-comment) line should be a *header line* that specifies the field names (see example below).  The lines that follow are *event lines*.

Minimal Generic Example:

	Date        Lat  Lon  EventSpec
	YYYY-MM-DD  X.X  Y.Y  do_something(x = 3.0, y = 2, z = 13.1)

<!--- Details? -->

#### File Format:

The file should be UTF-8 text with the extension `.txt` and UNIX line endings. Other text variants may work but have not been tested.

#### Blank lines:

Blank lines are ignored and can occur anywhere in the file.

#### Comments:

Any content following a '!' on a line is ignored.

#### Header Line:

Ideally the header will occur as the first line of *content*.  Comment lines describing the file (e.g. creator, date, project, etc) may precede it (a good idea) but all events should follow it.

The header line is primarily for readability and we do minimal checking of it, so most typos will not cause problems. The field order is fixed and changing the order of headings will not overide that.

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
	Easier to read:  do_something(dbh_min = 3.0, dbh_max = 55.5, z = 13.1)

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
