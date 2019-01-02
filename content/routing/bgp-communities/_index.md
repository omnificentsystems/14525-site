---
title: "BGP Communities"
date: 2019-01-01
draft: false
linktitle: BGP Communities
tags: ['bgp', 'routing', 'communities']
categories: ['routing']
layout: subsection
---

# Structure

{{% message %}}
    This page is a work in progress and should be effectively ignored until this message no longer exists.
{{% /message %}}

Please contact [peering@oscloud.io](mailto:peering@oscloud.io) for any questions regarding BGP policy.

---

## Large BGP Communities

Omnificent Systems leverages [Large BGP Communities](http://largebgpcommunities.net) to provide extremely granular control capabilities to transit customers, peers, and internally maintained customers.

Unlike the Extended Community format introduced in [RFC 4360](https://tools.ietf.org/html/rfc4360), which is  comprised of one 32 bit field broken up into two 16 bit fields, large communities are comprised of one 96 bit field, delineated into three 32 bit fields.

### Common Community Type Differences

Standard Communities ([RFC 1997](https://tools.ietf.org/html/rfc1997)):

`NO-EXPORT`

`NO-ADVERTISE`

`NO-ADVERTISE-SUBCONFED`

---

Extended Communities ([RFC 4360](https://tools.ietf.org/html/rfc4360)):

`0-65535 : 0-65535`

---

Large Communities ([RFC 8092](https://tools.ietf.org/html/rfc8092)):

`0-4294967295 : 0-4294967295 : 0-4294967295`

Typically, the three fields are structured as `Operator ASN : Function : Parameter`.

For example:

`ASN : Received from POP : osCloud AZ01` logic would be written as:

`395077:100:11`

`ASN : Prepend 3x : LEVEL 3` Logic would be written as:

`395077:2001:3356`

---

Recommendations for deployment can be found in [RFC 8195](https://tools.ietf.org/html/rfc8195).

### Community Allocations

The Omnificent Systems community structure is allocated into the following buckets:

| ASN      | Function          | Parameter  | Description      |
| -------- | ----------------- | ---------- | ---------------- |
| `395077` | `0-999`           | `*`        | Informational    |
| `395077` | `1000-1999`       | `*`        | Internal Actions |
| `395077` | `2000-2999`       | `*`        | External Actions |
| `395077` | `3000-3999`       | `*`        | Security Actions |
| `395077` | `4000-4294967295` | `*`        | Unassigned       |

## Community Assignments

### Informational Communities

#### Origin Tag Types

|  Tag  |  Info         | Example              | Result                                                            |
|:------:|:--------|:-----------------|:-----------------------------------------|
| `100` | By POP ID | `395077:100:21` | Route learned/originated from **OH01** |
| `200` | By PE Router   | `395077:200:1301` | Route learned/originated from **HI01 PE01** |
| `300` | By Peer AS   | `395077:300:174` | Route learned from `AS174` |
| `400` | By Route Type   | `395077:400:2001` | Transit Route |

##### POPs

| POP Location  | POP Name | POP ID |
|:--            |   :--:   |  :--:  |
| Phoenix, AZ   | AZ01     | **11** |
| Las Vegas, NV | NV01     | **12** |
| Honolulu, HI  | HI01     | **13** |
| Dayton, OH    | OH01     | **21** |
| Edison, NJ    | NJ01     | **22** |
| Atlanta, GA   | GA01     | **23** |

##### Peers

#### Upstream Peers

| Peer Name          | Peer ASN | AZ01 | NV01 | HI01 | OH01 | NJ01 | GA01 |
|:------------------ |:-------- | :--: | :--: | :--: | :--: | :--: | :--: |
| Cogent             | `174`    |  ✅  |  ✅  |     |  ✅  |  ✅  |      |
| CenturyLink/Qwest  | `209`    |  ✅  |  ✅  |     |      |      |      |
| CenturyLink/Level3 | `3356`   |      |      |      |      |      |  ✅  |
| Hawaiian Telcom    | `36149`  |      |      |  ✅  |      |      |      |
| Hurricane Electric | `6939`   |  ✅  |      |  ✅  |      |      |      |

##### Route Types

| Type | ID             |
|:-------:|:-------:|
| Peering | `1001` |
| IX           | `1002` |
| Transit   | `2001` |
| Customer | `3001` |

### Action Communities

## Local Preference

### By Route Type

| Route Type | `AS395077` LocalPref |
|:------------|:-----------------------|
| Customer    | `140` |
| Peer             | `80` |
| IX                  | `60` |
| Transit         | `40` |

### By Location
