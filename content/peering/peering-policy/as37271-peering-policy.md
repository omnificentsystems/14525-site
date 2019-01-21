---
title: "Peering Policy"
date: 2019-01-21
draft: true
linktitle: Peering Policy
tags: ['policy', 'peering']
categories: ['peering']
layout: subsection
---

# Workonline Communications IP Interconnection Policy
- - - - - -

This document describes the policy followed by Workonline when
evaluating requests for the establishment of settlement-free IP
interconnections between AS37271 and other autonomous systems.

Everything contained herein applies equally to IPv4 and IPv6, as
appropriate.

This document is provided for informational purposes only. Nothing
herein should be construed as a binding offer to enter into a peering
agreement. Workonline will at all times evaluate each request for
peering on its individual merits, and reserves its rights to deviate in
any way from this policy in its sole and absolute discretion.

## Requesting Peering

Operators wishing to establish peering with AS37271 on a settlement free
basis should email peering@workonline.co.za, and include the details of
the peering being requested, including:
- Peer's ASN
- A brief description of the peer's network and its business
- PNI location or IXP
- In the case of PNI, the interface speed required
- Contact details for provisioning and operations
- IP addressing details and other BGP session attributes
- Information necessary for the construction of Workonline's ingress BGP
  filters

In all cases, prospective peers are encouraged to familiarise themselves
with the content of this document, and to ensure that any requirements
are met as fully as possible prior to initiating a peering request. If
it is not possible, for whatever reason, to comply with an individual
requirement, Workonline's attention should be drawn to that fact, and
the reasons for which compliance is not possible clearly explained.

## Operational & Routing Information

In addition to the specific evaluation requirements listed below,
prospective peers are strongly encouraged to familiarise themselves with
the [Workonline Communications External Routing Policy](/routing-policy)
which provides detailed information on the operational policies that
determine the behaviour of all IP interconnections with AS37271.

## Evaluation Criteria

Workonline will evaluate all peering requests against the below
criteria. Some criteria are marked *required*: requests to peer are highly
unlikely to be accepted without **all** such criteria being met.

### Autonomous System Number
*required*

Peers must operate a properly allocated, globally unique autonomous
system number. AS37271 will not peer with reserved or
special-purpose[^iana-asn] ASNs.

### BGP Only Routing
*required*

AS37271 will exchange routing information for Internet networks with
external peers using version 4 of the Border Gateway Protocol[^rfc4271]
only.

Peers must only send traffic towards AS37271 destined to a prefix for
which AS37271 has advertised reachability. Setting static routes towards
AS37271 is expressly prohibited.

### Non-customer
*required*

Workonline does not peer with its IP transit customers, nor with its
customers' customers.

### Global Reachability
*required*

Peers must demonstrate that all announced prefixes (or their aggregates)
are visible in the Internet DFZ.

### Operational Contacts
*required*

Peers are required to operate a NOC (or equivalent) that is contactable
on a 24x7x365 basis, staffed with suitably skilled network engineers,
and available on a reasonable time-frame to jointly troubleshoot
operational problems involving the peering interconnection.

Operational contact details should be published on Peering DB[^pdb],
along with other relevant peering details.

### Multiple Locations
*required*

Peers should be willing and able to interconnect in at least 3
geographically diverse locations. Peers should furthermore be willing to
interconnect within any location, or at any IXP, where they and
Workonline are both present.

### Congestion Free Interconnections
*required*

Peers must maintain congestion free interconnections between themselves
and AS37271 (or, in the case of IXP peering, between themselves and the
IXP fabric). Aggregate interconnection capacity must be sufficient to
operate congestion-free in the face of at least one failure.

Peers must agree to upgrade interconnection capacity to meet growth in
peak demand as and when required.

### Traffic Volume
*required*

Workonline does not require a minimum set threshold to consider peering
at an IXP, although there should be sufficient traffic exchanged to
warrant the administration of the peering relationship. For peering on
PNI, Workonline requires a minimum 600 Mbps traffic exchange at the 95th
percentile on a monthly basis.

Workonline does not require traffic ingress/egress ratios in most cases.

### Backbone Transport Capacity
*required*

Peers operating an Internet backbone must ensure they maintain
sufficient on-net capacity to provide congestion free transport of
traffic, including in the face of the failure of at least one
interconnection with AS37271.

### Documented Routing Policy
*required*

Peers should maintain a publicly accessible description of their
external routing policy, in a form usable by Workonline engineers to
determine the correctness of the routing information received in BGP.

At minimum, this should consist of the following RPSL objects,
registered in one of the IRR databases mirrored by RADB[^radb]:
- `autnum`: An object describing the policy and contact details for the
  peer's ASN
- `as-set`: An object describing the set of ASNs for which the peer
  announces transit
- `route`/`route6`: An object, for each prefix announced by the peer,
  specifying the ASN originating that prefix in BGP

Workonline will use the above information to build inbound prefix
filters automatically. Peers must ensure that this information is kept
up to date by themselves and their customers at all times.

### Anti-spoofing
*required*

Peers must take reasonable measures to prevent IP packets with a spoofed
source address being sent to Workonline via any peering interconnection.
Workonline will have regard to the nature of the network operated by its
peers when determining whether a particular measure is reasonable in
this context.

### Anti-leak Filtering

Peers should have mechanisms in place to ensure that only prefixes for
which they are properly authorised to announce transit are exported to
AS37271.

### Announcement Consistency

Peers operating an Internet backbone or with a dominantly inbound
traffic profile should ensure that a consistent set of prefixes are
announced to AS37271 at all points of interconnection, unless otherwise
agreed on a case-by-case basis.

### MANRS Participation

Workonline is a committed participant in the MANRS[^manrs] community and
prefers to peer with network operators that share the same commitment to
global routing security.

### Peering Contracts

Workonline prefers not to sign contracts for peering. We will evaluate
requests on a case-by-case basis, balancing the additional
administration burden with the benefit from peering.

### RPKI ROA Signing

Whilst not currently used for routing validation, Workonline prefers to
peer with operators who maintain validly signed ROAs for their own
prefixes, and who encourage their customers to do the same.

### Locally Transit-Free

For peering in markets where Workonline offers IP transit services to
its customers, we prefer that our peers are not IP transit customers of
any other network operator within the same market.

### Operational Tools

Workonline prefers to peer with network operators that maintain a set of
publicly available troubleshooting tools (e.g. looking glass, route
server, etc) that Workonline engineers can use to diagnose routing
issues.

## Review

Where Workonline decides to enter into a new peering relationship, we do
so, in part, on the basis of the peer's eligibility under the above
criteria. As such, existing peering arrangements will be subject to
periodic review, to ensure that conditions continue to be met.

Where an existing peers circumstances change, Workonline encourages them
to contact peering@workonline.co.za to discuss any potential impact on
our continued peering relationship.

[^iana-asn]:  IANA Special-Purpose Autonomous System (AS) Numbers Registry -
              https://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml
[^pdb]:       Peering DB -
              https://peeringdb.com/
[^radb]:      Merit RADb -
              http://radb.net
[^manrs]:     Mutually Agreed Norms for Routing Security (MANRS) -
              https://www.routingmanifesto.org/
[^rfc4271]:   A Border Gateway Protocol 4 (BGP-4) -
              https://tools.ietf.org/html/rfc4271
