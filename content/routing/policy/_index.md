---
title: "Routing Policy"
date: 2019-01-21
draft: true
linktitle: Routing Policy
tags: ['policy', 'routing']
categories: ['routing']
layout: subsection
---

# Omnificent Systems External Routing Policy
- - - - - -

This document describes the policy implemented at the borders of AS14525 to control the import and export of routing information with other autonomous systems.

Workonline is a strong supporter and active member of the MANRS[^manrs] operator community, and considers compliance with the MANRS actions fundamental to our responsibilities to our customers and the Internet community at large.

The policies defined herein are designed to provide the highest feasible levels of security, stability and performance for our inter-connectivity with other network operators.

Readers are encouraged to contact peering@as14525.net if they have queries related to any of the information contained herein.

## Finding Information

Workonline maintains up to date information in multiple publicly available datasources, including:

* Peering DB: [as14525.peeringdb.com](https://as14525.peeringdb.com)
* ARIN whois: []()
* ARIN IRR Database: []()
* Omnificent Network Services Informational Site: [as14525.net](https://as14525.net)

If this document, or the above locations do not contain the information you are looking for, or if any information contained therein appears to be inaccurate, please email peering@as14525.net.

## Peering Relationships

The topological relationship of AS14525 to the external peers of AS14525 may be generally categorised into:

* Customers: networks for which AS14525 provides transit to other Internet networks
* Peers: networks with whom AS14525 exchanges routing information relating only to our respective customers
* Transits: networks that provide transit to AS14525 to other Internet networks

These terms are used herein to describe the topological relationship between AS14525 and an adjacent network, and do not necessarily imply the existence of a particular commercial relationship.

The term "Non-customer" may be used to describe an interconnection relationship other than those with customers of AS14525.

AS14525 may have multiple distinct relationships with a given AS at different points of interconnection. Thus, polices described here as applying to particular types of interconnection relationship should be construed as applying only to those interconnections or peerings of that type, rather than to all adjacencies with a given AS.

## Import and Export Policies

Some policies are implemented uniformly across all adjacencies, whilst others will vary according to the topological relationship between AS14525 and the adjacent networks.

### Autonomous Systems

* `14525`
* `395077`

Omnificent Systems Inc. operates a two autonomous systems throughout our network. ASN `395077` is used for legacy transit relationships, and is primarily used by our [osCloud](https://oscloud.io) Business Unit, while ASN `14525` is used for global interconnection. As it relates to routing policy, AS395077 is considered a standard customer of AS14525. All transit, peering, or IX adjacencies will be formed with AS14525, which provides downstream transit for AS395077.

### BGP Protocol

AS14525 will exchange routing information for Internet networks with external peers using Border Gateway Protocol version 4 only.

### Bogon Address Space

AS14525 will not accept routing information for, nor deliver IP packets addressed to or from, "Bogon" address space, including:
* Non-global IPv4 address space per [IANA Special-Purpose IPv4 Address Registry]()
* Non-global IPv6 address space per [IANA Special-Purpose IPv6 Address Registry]()
* IPv4 or IPv6 address space present in the Team Cymru Fullbogons list

### Prefix Length

AS14525 will accept routing information for IP prefixes conforming to minimum and maximum prefix-lengths as follows:

| Address Family | Min Length | Max Length |
|----------------|------------|------------|
| IPv4           | 8 bits     | 24 bits    |
| IPv6           | 16 bits    | 48 bits    |

### Special Purpose ASNs

AS14525 will not accept BGP updates with ASNs belonging to the [IANA Special Purpose AS Numbers Registry]() in the `AS_PATH`. 

Single homed customers of AS14525 that do not qualify for the allocation of an ASN under the relevant RIR policies in force in their region will will be allocated an ASN by Omnificent Systems from the range reserved for private use (per [RFC6996]()) for the purposes of establishing eBGP peering with AS14525 only. Prefixes announced from such customers and advertised to other external peers will appear to originate at AS14525.

### Ingress BGP Filtering

#### Prefix Filter Maintenance

Omnificent Systems makes use of RPSL data contained in the distributed IRR system to maintain per-neighbor AS filters for all customer and peer networks of AS14525. Data is queried from the [NTT]() IRR mirror and data from all databases mirrored by NTT are included.

Omnificent Systems maintains an `as-set` object of the form `AS-AS14525` which may be used to prefix filter adjacencies with AS14525. Please note: its size should not be used as an indicator of the number of routes that a non-customer peer should expect to receive.

Ingress prefix-lists are built by recursively resolving the appropriate `as-set` down to a list of member ASNs, and then searching for `route` or `route6` objects with the `origin` attribute set to one of these ASNs. This process is repeated daily, and updated prefix-lists are installed on border routers at approximately 03:00 in the timezone local to each router.

Prefixes received from customers and peers are accepted only if a `route` or `route6` object matching the advertised prefix and origin AS is found.

In each case, prefixes received are subjected to the other conditions described herein, including minimum and maximum prefix length.

#### Maximum Prefixes Limits

Omnificent Systems will sets limits on the maximum number of prefixes accepted from a given eBGP neighbor. When a reasonable suggested value is published in the PeeringDB, that value will be used. Otherwise, Omnificent Systems use its discretion to arrive at an appropriate value.

#### `AS_PATH` Filters

Omnificent Systems filters all eBGP adjacencies with customers and peers based on `AS_PATH`. All customer or peer routes must originate from their respective ASN, unless otherwise defined in a peer-specific agreement.

### Egress BGP Filtering

#### Prefix Filtering Towards Customers

AS14525 will advertise one or more of the following sets of prefixes towards its customer peers:

> DFZ
> <cite>Default Free Zone</cite>

1. **Full DFZ Routes:**
   All routes learned by Omnificent Systems from customer and non-customer peers
   according to the policies described herein.
2. **Default Route:**
   A single route of last resort, i.e. `0.0.0.0/0` or `::/0`, generated
   locally on the border router upon which the eBGP session with AS14525
   is configured.
3. **Partial DFZ Routes:**
   In the case of "partial transit" customers, a subset of the full DFZ
   routes, being those imported from non-customer peers within the
   defined scope of the relevant customer's service. In all cases,
   partial transit customers will be advertised all prefixes imported
   from other customers, regardless of their scope of service.

#### Prefix Filtering Towards Non-customers

AS14525 will export towards non-customer peers only routes that are originated in AS14525 or imported from customer peers.

Omnificent Systems maintains an `as-set` object `AS-AS14525` in the RADb database that includes all such prefixes, and may be used by neighboring networks to create and maintain the appropriate ingress session filters.

Peers wishing to set a sensible maximum-prefixes limit on sessions with AS14525 should refer to the 'IPv4 Prefixes' and 'IPv6 Prefixes' field in our record on PeeringDB.

AS14525 will make every attempt to maintain consistent announcements of prefixes towards peers. However, advertisement of prefixes learned from customers may be suppressed at certain peering locations, or towards specific neighbor ASNs, as the result of traffic engineering communities received from customers, as described below.

To prevent the inadvertent advertisement of transit between non-customer networks, a BGP community will be appended on import to indicate that a prefix was imported from a customer peer, and only prefixes carrying such community will be exported to non-customer neighbors.

## Route Selection and BGP Attribute Handling

Workonline maintains a [publicly available looking-glass]() to inspect route selection towards external networks from the perspective of AS145251.

The following BGP attribute handling policies define the expected route selection, for traffic routed to and from AS14525.

### Local Preference

Omnificent Systems sets the BGP `LOCAL_PREF` attribute on import according to the relationship of AS14525 to the adjacent neighbor or the type of route, as per the below table.

Where a range of values is indicated, other factors such as whether a peering crosses an IXP or PNI are used to select the eventual value. By default, the mid-range value will be used.

|                      | LOCAL_PREF     |
|----------------------|----------------|
| Transit              | `101` to `199` |
| Peer                 | `201` to `299` |
| Customer.            | `301` to `399` |
| Locally Originated   | `450`          |
| Blackhole Routes     | `999`          |

### AS Path

AS14525 will not modify the `AS_PATH` attribute of any route on import. AS14525 will, by default, prepend a single instance of `14525` on export to all external neighbors.

Customers of AS14525 may send BGP communities to influence the export of their routes to non-customer peers. Such communities may be used to suppress advertisement, prepend `14525` up to 3 times to selected groups of non-customer peers, or influence the `LOCAL_PREF` values of their prefixes relative to the AS14525 routing domain.

The above functionality is documented fully in
[BGP Communities](/routing/bgp-communities).

### Multi-exit Discriminators

On export of any route, AS14525 will set the `MULTI_EXIT_DISC` (MED) attribute to a value equal to the IGP cost to the NEXT_HOP address of the route (before any changes are made to NEXT_HOP on export).

Similarly, unless agreed otherwise in advance, AS14525 will set the MED of any routes received from any single-homed peers (customer and non-customer) to zero on import. Multi-homed peers may be requested to adjust the `MED` values on export to align with AS14525 policy logic.

Routes received from customers with a `MED` value set will be imported unmodified by AS14525, and the route with the lowest such value will be preferred for selection (provided all other path selection factors are equal).

### BGP Communities

Omnificent Systems leverages [BGP communities]() in the operation of the AS14525 network. Publicly relevant communities implemented in the AS14525 network are defined and documented in [BGP Communities](/routing/bgp-communities).

#### Community Filtering

AS14525 will send and accept routes with the `COMMUNITIES` attribute present to and from all BGP neighbors. Filtering policies applied to such communities are listed below.

| Community Value    | Import from Customer                               | Import from Non-customer                           | Export.                                            |
|--------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|
| `14525:.+`         | As per [BGP Communities](/routing/bgp-communities) | As per [BGP Communities](/routing/bgp-communities) | As per [BGP Communities](/routing/bgp-communities) |
| `0:.+`             | Delete                                     | Delete                     | Delete                                                                             |
| `23456:.+`         | Delete                                     | Delete                     | Delete                                                                             |
| `[64496-65534]:.+` | Delete                                     | Delete                     | Delete                                                                             |
| `65535:.+`         | Unchanged, except as below                 | Unchanged, except as below | Unchanged, except as below                                                         |

#### Well Known Communities

BGP communities in the range `65535:0 - 65535:65535` are reserved for use as well known. Of the communities listed in the [IANA registry](), the following are honored in AS14525. Any well-known communities not listed are handled as described in the preceding section.

- `NO_EXPORT` (`65535:1`)
  AS14525 will suppress the export of routes received with the `NO_EXPORT` community attached to any external BGP peers, as described in [RFC1997]().
- `NO_ADVERTISE` (`65535:2`) AS14525 routers will suppress all advertisement of routes received with the `NO_ADVERTISE` community attached to any internal or external BGP peers, as described in [RFC1997]()

Where notice of an impending change is deemed necessary, Omnificent Systems will make every attempt to notify affected customers or peers if it is determined that any impact will occur.

Workonline accepts no liability whatsoever for damages or losses of any nature whatsoever suffered by third parties as a result of their reliance on the information contained herein. By making use of any of information contained herein, the user acknowledges and agrees to these conditions.