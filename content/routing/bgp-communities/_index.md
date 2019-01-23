---
title: "BGP Communities"
date: 2019-01-21
draft: false
linktitle: BGP Communities
tags: ['bgp', 'routing', 'communities']
categories: ['routing']
layout: subsection
---

# Omnificent Systems BGP Community Definitions

Omnificent Systems leverages BGP Standard Communities to provide granular control of internal and external traffic engineering. This document describes the BGP Communities implemented in AS14525. The information contained herein should be considered authoritative for this purpose.

## Origin IDs

Many of the below community definitions make use of 4-digit origin identifiers (IDs). These IDs are used to identify routes with common properties, such as geographic location or connectivity at a particular IXP.

| Group Code   | Description                                                             | Exported to Peers
| :---------   | :---------------------------------------------------------------------- | :---------------: |
| `14525:0000` | Global - used to identify any and all routes that transit AS14525       | ✅ |
| `14525:0001` | Global - used to identify routes that originate from AS14525            | ✅ |
| `14525:0002` | Global - used to identify default routes that originate from AS14525 for downstream customers | ❌ |
| `14525:1xxx` | Location identifier for routes within a region, where `xxx` is the **UN M.49** code of the region | ✅ |
| `14525:2xxx` | Location identifier for routes within a country, where `xxx` is the **ISO 3166-1** numeric country code | ✅ |
| `14525:3xxx` | Location identifier for routes within a metro area, where `xxx` means:  | ✅ |
|              | `001` - Phoenix, AZ, USA / phx                                          | |
|              | `002` - Las Vegas, NV, USA / lsv                                        | |
|              | `003` - Honolulu, HI, USA / hnl                                         | |
|              | `004` - Dayton, OH, USA / dtn                                           | |
|              | `005` - New York City, NY, USA / nyc                                    | |
| `14525:4xxx` | Location identifier for routes within a particular Point of Presence (POP), where `xxx` means:  | ✅ |
|              | `001` - phx01 / IMDC AZP1                                               | |
|              | `002` - lsv01 / Switch Las Vegas                                        | |
|              | `003` - hnl01 / DRFortress                                              | |
|              | `004` - day01 / IMDC OHS1                                               | |
|              | `005` - nyc01 / IMDC NJE1                                               | |
| `14525:5001` | Customer routes                                                         | ✅ |
| `14525:5002` | Peer routes                                                             | ✅ |
| `14525:5100` | All transit provider routes                                             | ✅ |
| `14525:51xx` | All routes with a specific global transit provider, where `xx` means:   | ✅ |
|              | `01` AS174 - Cogent                                                     | |
|              | `02` AS6939 - Hurricane Electric                                        | |
|              | `03` AS1299 - Telia                                                     | |
|              | `04` AS3257 - GTT                                                       | |
|              | `05` AS209 - CenturyLink                                                | |
|              | `06` AS701 - Verizon                                                    | |
| `14525:5200` | All IXP peerings                                                        | ✅ |
| `14525:52xx` | All routes from a specific IXP **route server**, where `xx` means:      | ✅ |
|              | `01` DRF-IX                                                             | |

## Routing Control

The following communities are used to influence routing decisions within AS14525.

Many routing control communities are accepted from customer peers. However, only specific routing control communities are accepted from transit, or direct peers. These are indicated in the table below.

Routing control communities are never exported to external peers of AS14525.

### Routing Control Community Definitions

✅ = Community will be accepted and applied for valid customer prefixes.

❎ = Community will be accepted for routes originating from the peer's AS. Community logic will not apply to the peer's customer or peer routes.

❌ = Community will not be accepted.

| Community Value | Description                              | Accepted from customer peers | Accepted from direct peers | Accepted from transit peers |
|-----------------|-------------------------------------------------------------------|-----|-----|-----|
| `14525:xxx`     | Set LOCAL_PREF in AS14525, where `xxx` equals:                    |     |     |     |
|                 | `150` Set LOCAL_PREF to 150                                       | ✅ | ❎ | ❌ |
|                 | `200` Set LOCAL_PREF to 200                                       | ❌ | ❎ | ❌ |
|                 | `250` Set LOCAL_PREF to 250 (default for peers)                   | ❌ | ❎ | ❌ |
|                 | `300` Set LOCAL_PREF to 300                                       | ✅ | ❌ | ❌ |
|                 | `350` Set LOCAL_PREF to 350 (default for customers)               | ✅ | ❌ | ❌ |
|                 | `400` Set LOCAL_PREF to 400                                       | ✅ | ❌ | ❌ |
| `14525:7xx`     | AS14525 internal controls                                         | ❌ | ❌ | ❌ |
| `14525:9xx`     | NEXT_HOP modification controls, where `xx` means:                 |     |     |    |
|                 | `99` Blackhole traffic                                            | ✅ | ❎ | ❎ |
| `14525:1xxxx`   | Prepend `14525` 1x to non-customer peers with origin ID `xxxx`    | ✅ | ❌ | ❌ |
| `14525:2xxxx`   | Prepend `14525` 2x to non-customer peers with origin ID `xxxx`    | ✅ | ❌ | ❌ |
| `14525:3xxxx`   | Prepend `14525` 3x to non-customer peers with origin ID `xxxx`    | ✅ | ❌ | ❌ |
| `14525:4xxxx`   | Suppress export to non-customer peers with origin ID `xxxx`       | ✅ | ❌ | ❌ |
| `14525:5xxxx`   | Don't suppress export to non-customer peers with origin ID `xxxx` | ✅ | ❌ | ❌ |

## Changes and Version Control

Where notice of an impending change is deemed necessary, Omnificent Systems will make every attempt to notify affected customers or peers if it is determined that any impact will occur.

Workonline accepts no liability whatsoever for damages or losses of any nature whatsoever suffered by third parties as a result of their reliance on the information contained herein. By making use of any of information contained herein, the user acknowledges and agrees to these conditions.
