---
title: "Peering Policy"
date: 2019-01-21
draft: true
linktitle: Peering Policy
tags: ['policy', 'peering']
categories: ['peering']
layout: subsection
---

# Omnificent Systems Inc IP Interconnection Policy

This document describes the policy followed by Omnificent Systems Inc when evaluating requests for the establishment of settlement-free IP interconnections between AS14525 and other autonomous systems.

Everything contained herein applies equally to IPv4 and IPv6, as appropriate.

This document is provided for informational purposes only. Nothing herein should be construed as a binding offer to enter into a peering agreement. Omnificent Systems Inc will evaluate each peering request on its individual merits, and reserves its rights to deviate in any way from this policy in its sole and absolute discretion.

## Requesting Peering

Operators wishing to establish peering with AS37271 on a settlement free basis should email peering@workonline.co.za, and include the details of the peering being requested, including:
* Potential Peer's ASN
* A brief description of the peer's network and its business
* PNI location or IXP
* In the case of PNI, the interface speed requested
* Contact details for provisioning and operations

## Operational & Routing Information

In addition to the specific evaluation requirements listed below, prospective peers are strongly encouraged to familiarise themselves with the [Omnificent Systems External Routing Policy](/routing/policy) which provides detailed information on the operational policies that determine the behaviour of all IP interconnections with AS14525.

### Autonomous System Number
*required*

Peers must operate a properly allocated, globally unique autonomous system number. AS14525 will not peer with reserved or special-purpose ASNs.

### BGP Only Routing
*required*

AS14525 will exchange routing information for Internet networks with external peers using Border Gateway Protocol version 4 only.

Peers must only send traffic towards AS14525 destined to a prefix for which AS14525 has advertised reachability. Setting static routes towards AS14525 is expressly prohibited and will result in immediate depeering.

### Non-customer
*required*

Omnificent Systems does not peer with its IP transit customers, nor with its customers' customers.

### Operational Contacts
*required*

Peers are required to operate a contactable support center on a 24x7x365 basis, staffed with suitably skilled network engineers, and available on a reasonable time-frame to jointly troubleshoot operational problems involving the peering interconnection.

Operational contact details should be published on PeeringDB, along with other relevant peering details.

### Locations
*required*

Peers should be willing and able to interconnect in any location, or at any Internet Exchange Point, where they and Omnificent Systems share a network point of presence. If additional transport is required to establish a peering relationship, parties agree to share any transport costs under mutually agreeable terms.

### Congestion Free Interconnections
*required*

Peers must maintain congestion free interconnections between themselves and AS14525 (or, in the case of IXP peering, between themselves and the IXP fabric). Peers must agree to upgrade interconnection capacity to meet growth in peak demand as and when required.

### Traffic Volume
*required*

Omnificent Systems does not require a minimum set threshold to consider IXP peering or PNI. The requesting party need only provide sufficient business case as part of the peering request.

### Documented Routing Policy
*required*

Peers should maintain a publicly accessible description of their external routing policy, in a form usable by Workonline engineers to determine the correctness of the routing information received in BGP.

At minimum, this should consist of the following RPSL objects, registered in a publicly available [IRR databases]():

* `autnum`: An object describing the policy and contact details for the peer's ASN
* `as-set`: An object describing the set of ASNs for which the peer announces transit
* `route`/`route6`/`route-set`: An object, for each prefix announced by the peer, specifying the ASN originating that prefix in BGP

Omnificent Systems will use the above information to build inbound prefix filters automatically. Peers must ensure that this information is kept up to date by themselves and their customers at all times.

### Anti-spoofing
*required*

Peers must take reasonable measures to prevent IP packets with a spoofed source address being sent to Workonline via any peering interconnection. Omnificent Systems will have regard to the nature of the network operated by its peers when determining whether a particular measure is reasonable in this context.

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
