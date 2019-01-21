---
title: "BGP Community Structure"
date: 2019-01-21
draft: true
linktitle: BGP Community Structure
tags: ['bgp', 'routing', 'communities']
categories: ['routing']
layout: subsection
---

# Omnificent Systems BGP Community Definitions
- - - - - -

Omnificent Systems leverages BGP Standard Communities to provide granular control of internal and external traffic engineering. This document describes the BGP Communities implemented in AS14525. The information contained herein should be considered authoritative for this purpose.

## Origin IDs

Many of the below community definitions make use of 4-digit origin identifiers (IDs). These IDs are used to identify routes with common properties, such as geographic location or connectivity at a particular IXP.

### Group Code Definitions

In the below community definitions, instances of the string `aaaa` should be replaced with the applicable origin ID:
✖️✔️

| Group Code   | Description                                                             | Exported to Peers
| :---------   | :---------------------------------------------------------------------- | :---------------: |
| `14525:0000` | Global - used to identify any and all routes that transit AS14525       | ✔️ |
| `14525:0001` | Global - used to identify routes that originate from AS14525            | ✔️ |
| `14525:0002` | Global - used to identify default routes that originate from AS14525 for downstream customers | ✖️ |
| `14525:1xxx` | Location identifier for routes within a region, where `xxx` is the **UN M.49** code of the region |
| `14525:2xxx` | Location identifier for routes within a country, where `xxx` is the **ISO 3166-1**[^ISO3166] numeric country code |
| `14525:3xxx` | Location identifier for routes within a metro area, where `xxx` means:  |
|              | `001` - Phoenix, AZ, USA / phx                                          |
|              | `002` - Las Vegas, NV, USA / lsv                                        |
|              | `003` - Honolulu, HI, USA / hnl                                         |
|              | `004` - Dayton, OH, USA / dtn                                           |
|              | `005` - New York City, NY, USA / nyc                                    |
| `14525:4xxx` | Location identifier for routes within a particular Point of Presence (POP), where `xxx` means:  |
|              | `001` - phx01 / IMDC AZP1                                               |
|              | `002` - lsv01 / Switch Las Vegas                                        |
|              | `003` - hnl01 / DRFortress                                              |
|              | `004` - day01 / IMDC OHS1                                               |
|              | `005` - nyc01 / IMDC NJE1                                               |
| `14525:5001` | Customer routes                                                         |
| `14525:5002` | Peer routes                                                             |
| `14525:5100` | All transit provider routes                                             |
| `14525:51xx` | All routes with a specific global transit provider, where `xx` means:   |
|              | `01` AS174 - Cogent                                                     |
|              | `02` AS6939 - Hurricane Electric                                        |
|              | `03` AS1299 - Telia                                                     |
|              | `04` AS3257 - GTT                                                       |
|              | `05` AS209 - CenturyLink                                                |
|              | `06` AS701 - Verizon                                                    |
| `14525:5200` | All IXP peerings                                                        |
| `14525:52xx` | All peerings at a specific IXP, where `xx` means:                       |
|              | `01` DRF-IX                                                             |

## Informational Communities

The following communities are used to provide additional information about the respective route. These communities are added at ingress by the responsible peering router, and any communities received from the peer are deleted.

### Informational Community Definitions

Informational communities implemented in AS14525 are defined as:

| Community Value | Description                         | Exported to peers |
|-----------------|---------------------------------------------------|-----|
| `14525:x`       | Imported from peer type `x`:                      | No  |
|                 | `1` Transit provider                              |     |
|                 | `2` Peering partner                               |     |
|                 | `3` Transit customer                              |     |
|                 | `4` AS37271 originated                            |     |
| `14525:1x`      | Imported into BGP via method `x`:                 | No  |
|                 | `0` AS37271 originated                            |     |
|                 | `1` Learnt from eBGP peer                         |     |
|                 | `2` Protocol redistribution                       |     |
|                 | `3` Imported from VRF RIB                         |     |
|                 | `4` Generated default route                       |     |
| `14525:8xx`     | Error status `xx` during import:                  | Yes |
|                 | `01` Illegal community received from customer     |     |
|                 | `02` Illegal community received from non-customer |     |
| `14525:GGGG`    | Imported via peering in group `GGGG`              | Yes |

## Routing Control Communities

The following communities are used to influence routing decisions within
AS37271.

Routing control communities are never accepted from non-customer peers.

Some routing control communities are accepted from customer peers, as
indicated below.

If any of the below communities are received from external peers that
are not permitted to send them, they will be deleted on import, and an
error status community will be appended to indicate that this has
occurred.

Routing control communities are never exported to external peers of
AS37271, with the exception of private eBGP controls in some
circumstances.

### Routing Control Community Definitions

Routing control communities defined in the AS37271 namespace are as
follows:

| Community Value | Description                          | Accepted from customer peers |
|-----------------|---------------------------------------------------------------|-----|
| `37271:1xx`     | Set LOCAL_PREF in AS37271, where `xx` means:                  | Yes |
|                 | `20` Set LOCAL_PREF to 200                                    |     |
|                 | `25` Set LOCAL_PREF to 250 (default for peers)                |     |
|                 | `30` Set LOCAL_PREF to 300                                    |     |
|                 | `35` Set LOCAL_PREF to 350 (default for customers)            |     |
|                 | `40` Set LOCAL_PREF to 400                                    |     |
| `37271:6xx`     | Private iBGP controls                                         | No  |
| `37271:7xx`     | Private eBGP controls                                         | No  |
| `37271:9xx`     | NEXT_HOP modification controls, where `xx` means:             | No  |
|                 | `99` Blackhole traffic                                        |     |
| `37271:6xxx`    | Private route-reflection controls                             | No  |
| `37271:1GGGG`   | Prepend 37271 once on non-customer peerings in group `GGGG`   | Yes |
| `37271:2GGGG`   | Prepend 37271 twice on non-customer peerings in group `GGGG`  | Yes |
| `37271:3GGGG`   | Prepend 37271 thrice on non-customer peerings in group `GGGG` | Yes |
| `37271:4GGGG`   | Suppress export on non-customer peerings in group `GGGG`      | Yes |
| `37271:5GGGG`   | Un-suppress export on non-customer peerings in group `GGGG`   | Yes |
| `37271:6GGGG`   | Administrative export scope                                   | No  |

## Changes and Version Control

The version number of this document on the first line, above, is of the
form `x.y.z`. Readers should contact noc@workonline.co.za to obtain the
most recent version.

`z` indicates the sub-minor version, and is incremented when
non-implementation related aspects (e.g. formatting and/or wording
changes) are modified. Such modifications will be made without notice.

`y` indicates the minor version, and is incremented when non-breaking
implementation changes are made. These may include, for example, changes
to definitions of communities not imported or exported to or from
external peers, or the addition of new orthogonal definitions.
Such modifications will generally be made without notice, other than in
special circumstances.

`x` indicates the major version. The major version is incremented
whenever breaking implementation changes are made, for example
the removal or modification of an existing definition that may be in use
by an external peer. Workonline will provide notice of such changes as
it deems necessary and practical in the circumstances.

Where notice of an impending change is deemed necessary, Workonline will
use such methods as it deems convenient and suitable.

Workonline accepts no liability whatsoever for damages or losses of
any nature whatsoever suffered by third parties as a result of their
reliance on the information contained herein. By making use of any of
information contained herein, the user acknowledges and agrees to these
conditions.

[^RFC1997]: RFC1997 BGP Communities Attribute - https://tools.ietf.org/html/rfc1997
[^range]: Community values in the range `0x91970000 - 0x9197ffff`.
[^ISO3166]: ISO 3166-1 Maintenance Agency - http://www.iso.org/iso/country_codes.htm
[^M49]: United Nations M.49 - http://unstats.un.org/unsd/methods/m49/m49regin.htm
[^nt]: Includes transit customers of AS37271, but excludes providers of transit to AS37271
[^any]: Only used to control internal route propagation
[^noinfo]: Not used in the context of informational communities
