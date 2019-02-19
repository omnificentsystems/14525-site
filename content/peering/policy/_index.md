---
title: "Peering Policy"
date: 2019-01-21
draft: false
linktitle: Peering Policy
tags: ['policy', 'peering']
categories: ['peering']
layout: subsection
---

# Omnificent Systems Inc IP Interconnection Policy

This document describes the policy followed by Omnificent Systems Inc when evaluating requests for the establishment of settlement-free IP interconnections between AS14525 and other autonomous systems.

Omnificent Systems Inc will evaluate each peering request on its individual merits, and reserves its rights to deviate in any way from this policy in its sole and absolute discretion.

## Requesting Peering

Operators wishing to establish peering with AS14525 on a settlement free basis should email peering@as14525.net or complete the [peering request form](https://as14525.net/peering/request/), and include the details of the peering being requested, including:

* Potential Peer's ASN
* A brief description of the peer's network and its business
* PNI location or IXP
* In the case of PNI, the interface speed requested
* Contact details for provisioning and operations

### Autonomous System Number
###### (*required*)

Peers must operate a properly allocated, globally unique autonomous system number. AS14525 will not peer with reserved or special-purpose ASNs.

### BGP Only Routing
###### (*required*)

AS14525 will exchange routing information for Internet networks with external peers using Border Gateway Protocol version 4 only.

Peers must only send traffic towards AS14525 destined to a prefix for which AS14525 has advertised reachability. Setting static routes towards AS14525 is expressly prohibited and will result in immediate depeering.

### Operational Contacts
###### (*required*)

Peers are required to operate a contactable support center on a 24x7x365 basis, staffed with suitably skilled network engineers, and available on a reasonable time-frame to jointly troubleshoot operational problems involving the peering interconnection.

Operational contact details should be published on PeeringDB, along with other relevant peering details.

### Locations
###### (*required*)

Peers should be willing and able to interconnect in any location, or at any Internet Exchange Point, where they and Omnificent Systems share a network point of presence. If additional transport is required to establish a peering relationship, parties agree to share any transport costs under mutually agreeable terms.

### Congestion Free Interconnections
###### (*required*)

Peers must maintain congestion free interconnections between themselves and AS14525 (or, in the case of IXP peering, between themselves and the IXP fabric). Peers must agree to upgrade interconnection capacity to meet growth in peak demand as and when required.

### Traffic Volume
###### (*required*)

Omnificent Systems does not require a minimum set threshold to consider IXP peering or PNI. The requesting party need only provide sufficient business case as part of the peering request.

### Documented Routing Policy
###### (*required*)

Peers should maintain a publicly accessible description of their external routing policy, in a form usable by Workonline engineers to determine the correctness of the routing information received in BGP.

At minimum, this should consist of the following RPSL objects, registered in a publicly available [IRR databases](http://www.irr.net/docs/list.html):

* `autnum`: An object describing the policy and contact details for the peer's ASN
* `as-set`: An object describing the set of ASNs for which the peer announces transit
* `route`/`route6`/`route-set`: An object, for each prefix announced by the peer, specifying the ASN originating that prefix in BGP

Omnificent Systems will use the above information to build inbound prefix filters automatically. Peers must ensure that this information is kept up to date by themselves and their customers at all times.

### Anti-leak Filtering

Peers should have mechanisms in place to ensure that only prefixes for which they are properly authorized to announce are exported to AS14525.

### RPKI ROA Signing

Omnificent Systems prefers to peer with operators who maintain validly signed ROAs for their own prefixes, and who encourage their customers to do the same.

### Operational Tools

Omnificent Systems prefers to peer with network operators that maintain a set of publicly available troubleshooting tools (e.g. looking glass, route server, etc) that Omnificent Systems engineers can use to diagnose routing issues.

## Review

Omnificent Systems enters a peering relationship on the basis of the peer's eligibility under the above criteria. Existing peering arrangements will be subject to periodic review, to ensure that conditions continue to be met.
