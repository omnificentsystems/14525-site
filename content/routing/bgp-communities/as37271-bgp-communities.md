---
title: "BGP Community Structure"
date: 2019-01-21
draft: true
linktitle: BGP Community Structure
tags: ['bgp', 'routing', 'communities']
categories: ['routing']
layout: subsection
---

# Workonline Communications BGP Community Definitions
- - - - - -

Workonline Communications AS37271 makes extensive use of BGP Standard
Communities[^RFC1997] to carry supplementary routing information and
provide additional control over route selection to its transit customers.

This document describes the BGP Communities defined in the AS37271
namespace[^range]. The information contained herein should be considered
authoritative for this purpose.

## Group Codes

Many of the below community definitions make use of 4-digit group codes.
Groups are used to identify collections of external peerings with a common
property, such as geographic location or connectivity at a particular IXP.

### Group Code Definitions

In the below community definitions, instances of the string `GGGG` should
be replaced with the applicable group code, as per the following table:

| Group Code | Description                                                             |
| ---------- | ----------------------------------------------------------------------- |
| `0000`     | Global - all internet peerings[^noinfo]                                 |
| `100x`     | Non-transit[^nt] peerings on a continent, where `x` means:              |
|            | `1` - Africa                                                            |
|            | `2` - Asia                                                              |
|            | `3` - North America                                                     |
|            | `4` - South America                                                     |
|            | `5` - Europe                                                            |
|            | `6` - Oceania                                                           |
|            | `7` - Antarctica                                                        |
| `2xxx`     | Non-transit[^nt] peerings within a region, where `xxx` is the **UN M.49**[^M49] code of the region |
| `3xxx`     | Non-transit[^nt] peerings within a country, where `xxx` is the **ISO 3166-1**[^ISO3166] numeric country code |
| `4xxx`     | Non-transit[^nt] peerings within a metro area, where `xxx` means:       |
|            | `000` - Any metro[^any]                                                 |
|            | `001` - Cape Town / CPT                                                 |
|            | `002` - Johannesburg / JHB                                              |
|            | `003` - Durban / DRB                                                    |
|            | `004` - London / LDN                                                    |
|            | `005` - Frankfurt / FRA                                                 |
|            | `006` - Amsterdam / AMS                                                 |
|            | `007` - Nairobi / NBO                                                   |
|            | `008` - Singapore / SIN                                                 |
| `5001`     | Customer peerings                                                       |
| `5002`     | Non-customer peerings                                                   |
| `5003`     | Peerings at any NAP Africa IXP                                          |
| `5100`     | All global transit provider peerings                                    |
| `51xx`     | All peerings with a specific global transit provider, where `xx` means: |
|            | `01` AS3356 - Level3                                                    |
|            | `02` AS174 - Cogent                                                     |
|            | `03` AS2914 - NTT                                                       |
|            | `04` AS1299 - Telia                                                     |
|            | `05` AS4809 - China Telecom                                             |
|            | `06` AS6762 - Telecom Italia Sparkle                                    |
|            | `07` AS3257 - GTT                                                       |
|            | `08` AS6453 - Tata Communications                                       |
| `5200`     | All IXP peerings                                                        |
| `52xx`     | All peerings at a specific IXP, where `xx` means:                       |
|            | `01` NAP Africa CT1                                                     |
|            | `02` NAP Africa JB1                                                     |
|            | `03` NAP Africa DB1                                                     |
|            | `04` CINX                                                               |
|            | `05` JINX                                                               |
|            | `06` DINX                                                               |
|            | `07` LINX LON1 "Juniper"                                                |
|            | `08` LINX LON2 "Extreme"                                                |
|            | `09` LONAP                                                              |
|            | `10` DE-CIX FRA                                                         |
|            | `11` NLix                                                               |
|            | `12` AMS-IX                                                             |
|            | `13` KIXP                                                               |
|            | `14` Equinix SG                                                         |

## Informational Communities

The following communities are used to provide additional information
over and above the information present in other BGP attributes.

Informational communities are never accepted from external peers.

If any of the below communities are received from external peers, they
will be deleted on import, and an error status community will be
appended to indicate that this has occurred.

### Informational Community Definitions

Informational communities defined in the AS37271 namespace are as
follows:

| Community Value | Description                         | Exported to peers |
|-----------------|---------------------------------------------------|-----|
| `37271:x`       | Imported from peer type `x`:                      | No  |
|                 | `1` Transit provider                              |     |
|                 | `2` Peering partner                               |     |
|                 | `3` Transit customer                              |     |
|                 | `4` AS37271 originated                            |     |
| `37271:1x`      | Imported into BGP via method `x`:                 | No  |
|                 | `0` AS37271 originated                            |     |
|                 | `1` Learnt from eBGP peer                         |     |
|                 | `2` Protocol redistribution                       |     |
|                 | `3` Imported from VRF RIB                         |     |
|                 | `4` Generated default route                       |     |
| `37271:8xx`     | Error status `xx` during import:                  | Yes |
|                 | `01` Illegal community received from customer     |     |
|                 | `02` Illegal community received from non-customer |     |
| `37271:GGGG`    | Imported via peering in group `GGGG`              | Yes |

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
