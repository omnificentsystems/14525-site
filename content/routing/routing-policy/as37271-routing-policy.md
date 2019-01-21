---
title: "Routing Policy"
date: 2019-01-21
draft: true
linktitle: Routing Policy
tags: ['policy', 'routing']
categories: ['routing']
layout: subsection
---

# Workonline Communications External Routing Policy
- - - - - -

This document describes the policy implemented at the borders of AS37271
to control the import and export of routing information with other
autonomous systems.

Workonline is a strong supporter and active member of the MANRS[^manrs]
operator community, and considers compliance with the MANRS actions fundamental
to our responsibilities to our customers and the Internet community at
large.

The policies defined herein are designed to provide the highest feasible
levels of security, stability and performance for our inter-connectivity
with other network operators.

Readers are encouraged to contact noc@workonline.co.za if they have
queries related to any of the information contained herein.

## Finding Information

Workonline maintains up to date information in multiple publicly
available datasources, including:

- Peering DB: https://peeringdb.com
- AFRINIC whois: http://afrinic.net/en/services/whois-query
- Merit RADb IRR Database: http://radb.net
- Workonline Network Information Portal: https://as37271.fyi

If this document, or the above locations do not contain the information
that you are looking for, or if any information contained therein
appears to be inaccurate, please email noc@workonline.co.za.

## Peering Relationships

The topological relationship of AS37271 to the external peers of AS37271
may be generally categorised into:

- Customers: networks for which AS37271 announces transit to other
  Internet networks
- Peers: networks with whom AS37271 exchanges routing information
  relating only to our respective customers
- Transits: networks that announce transit to AS37271 and our customers
  to other Internet networks

These terms are used herein to describe the topological relationship
between AS37271 and an adjacent network, and do not necessarily imply
the existence of a particular commercial relationship.

The term "Non-customer" may be used to describe an interconnection
relationship other than those with customers of AS37271.

AS37271 may have multiple distinct relationships with a given AS at
different points of interconnection. Thus, polices described here as
applying to particular types of interconnection relationship should be
construed as applying only to those interconnections or peerings of that
type, rather than to all adjacencies with a given AS.

## Import and Export Policies

The policies described in this section apply to all Internet
interconnections at the border of AS37271.

Some policies are implemented uniformly across all adjacencies, whilst
others will vary according to the topological relationship between
AS37271 and the adjacent networks.

### Autonomous System

Workonline Communications (Pty) Ltd operates a single global autonomous
system across our network, using ASN 37271.

### BGP Protocol

AS37271 will exchange routing information for Internet networks with
external peers using version 4 of the Border Gateway Protocol[^rfc4271]
only.

### Bogon Address Space

AS37271 will not accept routing information for, and will not deliver
IP packets addressed to or from, "Bogon" address space, including:
- Non-global IPv4 address space per IANA Special-Purpose IPv4 Address
  Registry [^iana-ipv4]
- Non-global IPv6 address space per IANA Special-Purpose IPv6 Address
  Registry [^iana-ipv6]
- IPv4 or IPv6 address space present in the Team Cymru Fullbogons list[^cymru]

### Prefix Length

AS37271 will accept routing information for IP prefixes conforming to
minimum and maximum prefix-lengths as follows:

| Address Family | Min Length | Max Length |
|----------------|------------|------------|
| IPv4           | 8 bits     | 24 bits    |
| IPv6           | 16 bits    | 48 bits    |

### Special Purpose ASNs

ASNs appearing in the IANA Special Purpose AS Numbers Registry[^iana-asn]
should not appear in BGP attributes of routes in the Internet routing
table.

As such AS37271 will:
- Filter received BGP updates with such ASNs appearing in the AS_PATH
  attribute
- Remove any occurrences of such ASNs from the AS_PATH attribute of sent
  BGP updates
- Remove any BGP communities on import and on export carrying such ASNs
  in the first 16 bits

Single homed customers of AS37271 that do not qualify for the allocation
of an ASN under the relevant RIR policies in force in their region will
will be allocated an ASN by Workonline from the range reserved for
private use (per RFC6996[^rfc6996]) for the purposes of establishing
eBGP peering with AS37271 only.

Prefixes learnt from such customers and advertised to other external
peers will appear to originate at AS37271.

Customers making use of a private use ASN as above will not be permitted
to advertise transit for any network originating at any other AS.

### Ingress BGP Filtering

#### Prefix Filter Maintenance

Workonline makes use of RPSL data contained in the distributed IRR
system to maintain per-neighbor AS filters for all customer and peer
networks of AS37271.

Data is queried from RADb[^radb] and data from all databases mirrored by
RADb are included.

Workonline maintains an `as-set` object of the form
`AS37271:AS-CUSTOMERS:PeerAS` or `AS37271:AS-PEERS:PeerAS` for each
neighbor ASN in RADb. Usually, such objects will have `member`
attributes listing one or more object maintained by the operator of the
neighboring network.

Ingress prefix-lists are built by recursively resolving the appropriate
`as-set` down to a list of member ASNs, and then searching for `route`
or `route6` objects with the `origin` attribute set to one of these
ASNs. This process is repeated daily, and updated prefix-lists are
installed on border routers at approximately 03:00 in the timezone local
to each router.

Prefixes received from customers are accepted only if a `route` or
`route6` object exactly matching the advertised prefix is found.

Prefixes received from peers are accepted as long as a `route` or
`route6` object is found for a covering prefix.

In each case, prefixes received are subjected to the other conditions
described herein, including minimum and maximum prefix length.

#### Maximum Prefixes Limits

Workonline will usually set limits on the maximum number of prefixes
accepted from a given eBGP neighbor. Where a reasonable suggested value
is published in the Peering DB, that value will be used. Otherwise
Workonline use its discretion to arrive at an appropriate value.

#### Other Filters

Workonline does not currently apply AS_PATH or any other BGP attribute
based filtering on prefixes received from neighbors.

### Egress BGP Filtering

#### Prefix Filtering Towards Customers

AS37271 will advertise one or more of the following sets of prefixes
towards its customer peers:

1. **Full DFZ Routes:**
   All routes learnt by Workonline from customer and non-customer peers
   according to the policies described here.
2. **Default Route:**
   A single route of last resort, i.e. `0.0.0.0/0` or `::/0`, generated
   locally on the border router upon which the eBGP session with AS37271
   is configured.
3. **Partial DFZ Routes:**
   In the case of "partial transit" customers, a subset of the full DFZ
   routes, being those imported from non-customer peers within the
   defined scope of the relevant customer's service. In all cases,
   partial transit customers will be advertised all prefixes imported
   from other customers, regardless of their scope of service.

#### Prefix Filtering Towards Non-customers

AS37271 will advertise towards its non-customer peers only those routes
that are originated in AS37271 or imported from customer peers.

Workonline maintains an `as-set` object `AS-WOLCOMM` in the RADb
database that includes all such prefixes, and may be used by neighboring
networks to create and maintain the appropriate ingress session filters.

Note that `AS-WOLCOMM` includes a superset of the prefixes of networks
for which AS37271 advertises transit. Its size should not be used as an
indicator of the number of routes that a non-customer peer should expect
to receive.

Peers wishing to set a sensible maximum-prefixes limit on sessions with
AS37271 should refer to the 'IPv4 Prefixes' and 'IPv6 Prefixes' field in
our record on Peering DB[^pdb].

AS37271 will always maintain consistent announcements of prefixes
towards peers, barring only:

- Prefixes learnt from "partial transit" customers will be advertised to
  non-customers only within that customers defined scope of service, and
- Advertisement of prefixes learnt from customers may be suppressed at
  certain peering locations, or towards specific neighbor ASNs, as the
  result of traffic engineering communities received from customers, as
  described below

To prevent the inadvertent advertisement of transit between non-customer
networks, a BGP community will be appended on import to indicate that a
prefix was imported from a customer peer, and only prefixes carrying
such community will be exported to non-customer neighbors.

## Route Selection and BGP Attribute Handling

Workonline maintains a [publicly available looking-glass](/looking-glass)
to inspect route selection towards external networks from the
perspective of AS37271.

The following BGP attribute handling policies (along with the policies
of other Internet networks) define the expected route selection, both
for traffic routed from and towards AS37271.

### Local Preference

Workonline sets the BGP LOCAL_PREF attribute on import according to the
relationship of AS37271 to the adjacent neighbor or the type of route,
as per the below table.

Where a range of values is indicated, other factors such as whether a
peering crosses an IXP or PNI are used to select the eventual value. By
default, the mid-range value will be used.

|                      | LOCAL_PREF     |
|----------------------|----------------|
| Transit              | `101` to `199` |
| Peer                 | `201` to `299` |
| Customer[^lp]        | `301` to `399` |
| Locally Originated   | `450`          |
| Blackhole Routes     | `999`          |

[^lp]: Transit customers may send BGP communities to influence the
       LOCAL_PREF set on their routes as described in
       [BGP Communities](/bgp-communities)

### AS Path

Workonline will not modify the AS_PATH attribute of any route on import
into AS37271. Workonline will, by default, prepend a single instance of
`37271` on export to all external neighbors.

Customers of AS37271 may send BGP communities to influence the export of
their routes to non-customer peers. Such communities may be used to
suppress advertisement, or to prepend `37271` up to 3 times to selected
groups of non-customer peers. Customers are not able to perform such
engineering on advertisement to other customers of AS37271.

The above functionality is documented fully in
[BGP Communities](/bgp-communities).

### Multi-exit Discriminators and Hot-potato Routing

On export of any route, AS37271 will set the MULTI_EXIT_DISC (MED)
attribute to a value equal to the IGP cost to the NEXT_HOP address of
the route (before any changes are made to NEXT_HOP on export).

Workonline expects that any neighbor that prefers to perform "Hot
Potato" routing will reset the MED to zero on import.

Similarly, unless agreed otherwise in advance, AS37271 will set the MED
of any routes received from non-customer peers to zero on import.

Routes received from customers with a MED value set will be imported
unmodified by AS37271, and the route with the lowest such value (all
other factors being equal) will be preferred for selection.

### BGP Communities

Workonline uses BGP communities[^rfc1997] extensively in the operation
of the AS37271 network. A large number of communities of the form
`37271:.+` are defined and documented in
[BGP Communities](/bgp-communities).

#### Community Filtering

More generally, AS37271 will send and accept routes with the COMMUNITIES
attribute present to and from all BGP neighbors. Filtering policies
applied to such communities are listed below. Any communities with a
value not listed are passed transitively across AS37271 without
modification.

| Community Value    | Import from Customer                       | Import from Non-customer   | Export                                     |
|--------------------|--------------------------------------------|----------------------------|--------------------------------------------|
| `37271:.+`         | As per [BGP Communities](/bgp-communities) | Delete                     | As per [BGP Communities](/bgp-communities) |
| `0:.+`             | Delete                                     | Delete                     | Delete                                     |
| `23456:.+`         | Delete                                     | Delete                     | Delete                                     |
| `[64496-65534]:.+` | Delete                                     | Delete                     | Delete                                     |
| `65535:.+`         | Unchanged, except as below                 | Unchanged, except as below | Unchanged, except as below                 |

#### Well Known Communities

BGP communities in the range `0xffff0000 - 0xffffffff` are reserved for
use as well known (i.e. global significant and standardised semantics).
Of the communities listed in the IANA registry[^iana-comm], the
following receive special handling in AS37271. Any well-known
communities not listed are handled as described in the preceding
section.

- `NO_EXPORT` (`0xffffff01`)
  AS37271 will suppress the export of routes received with the
  `NO_EXPORT` community attached to any external BGP peers, as described
  in RFC1997[^rfc1997]
- `NO_ADVERTISE` (`0xffffff02`)
  Routers in AS37271 will suppress the advertisement of routes received
  with the `NO_ADVERTISE` community attached to any internal or external
  BGP peers, as described in RFC1997[^rfc1997]

## Changes and Version Control

The version number of this document on the first line, above, is of the
form `x.y.z`. Readers should contact noc@workonline.co.za to obtain the
most recent version.

`z` indicates the sub-minor version, and is incremented when
non-implementation related aspects (e.g. formatting and/or wording
changes) are modified. Such modifications will be made without notice.

`y` indicates the minor version, and is incremented when non-breaking
implementation changes are made. These may include, for example, changes
to policies impacting only internal route selection.
Such modifications will generally be made without notice, other than in
special circumstances.

`x` indicates the major version. The major version is incremented
whenever breaking implementation changes are made, for example
the removal or modification of an existing policy that may be relied
upon by an external peer. Workonline will provide notice of such changes
as it deems necessary and practical in the circumstances.

Where notice of an impending change is deemed necessary, Workonline will
use such methods as it deems convenient and suitable.

Workonline accepts no liability whatsoever for damages or losses of
any nature whatsoever suffered by third parties as a result of their
reliance on the information contained herein. By making use of any of
information contained herein, the user acknowledges and agrees to these
conditions.

[^manrs]:     Mutually Agreed Norms for Routing Security (MANRS) -
              https://www.routingmanifesto.org/
[^rfc4271]:   A Border Gateway Protocol 4 (BGP-4) -
              https://tools.ietf.org/html/rfc4271
[^iana-ipv4]: IANA IPv4 Special-Purpose Address Registry -
              https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml
[^iana-ipv6]: IANA IPv6 Special-Purpose Address Registry -
              https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml
[^cymru]:     Team Cymru Bogon Reference -
              https://www.team-cymru.org/bogon-reference.html
[^iana-asn]:  IANA Special-Purpose Autonomous System (AS) Numbers Registry -
              https://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml
[^rfc6996]:   Autonomous System (AS) Reservation for Private Use -
              https://tools.ietf.org/html/rfc6996
[^radb]:      Merit RADb -
              http://radb.net
[^pdb]:       Workonline Communications on Peering DB -
              https://peeringdb.com/asn/37271
[^iana-comm]: Border Gateway Protocol (BGP) Well-known Communities -
              https://www.iana.org/assignments/bgp-well-known-communities/bgp-well-known-communities.xhtml
[^rfc1997]:   BGP Communities Attribute -
              https://tools.ietf.org/html/rfc1997
