---
title: RoQ Candidates with Interactive Connectivity Establishment (ICE)
abbrev: RoQ Candidates for ICE
category: exp

docname: draft-dawkins-avtcore-roq-ice-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Web and Internet Transport"
workgroup: "Audio/Video Transport Core Maintenance"
keyword:
 - RTP over QUIC
 - RoQ
 - SDP
 - ICE
venue:
  group: "Audio/Video Transport Core Maintenance"
  type: "Working Group"
  mail: "avt@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/avt/"
  github: "SpencerDawkins/roq-ice"
  latest: "https://SpencerDawkins.github.io/roq-ice/draft-dawkins-avtcore-roq-ice.html"

author:
 -
    fullname: "Spencer Dawkins"
    organization: Wonder Hamster Internetworking LLC
    country: United States of America
    email: "spencerdawkins.ietf@gmail.com"

normative:

  ICE-protos:
    target: https://www.iana.org/assignments/ice/ice.xhtml#transport-extensions
    title: "ICE Transport Protocols"
    date: September 2021

informative:

--- abstract

This document describes the use of Interactive Connectivity Establishment (ICE) procedures to select candidate paths for use with RTP over QUIC (RoQ).

--- middle

# Introduction

This document describes the use of Interactive Connectivity Establishment (ICE) procedures ({{!RFC8445}}) to select candidate paths for use with RTP over QUIC (RoQ) ({{!I-D.ietf-avtcore-rtp-over-quic}}).

# Conventions and Definitions

{::boilerplate bcp14-tagged}

## Notes for Readers {#readernotes}

(Note to RFC Editor - if this document ever reaches you, please remove this section)

SDP for RoQ is defined in a separate document {{!I-D.draft-ietf-avtcore-sdp-roq}}, which mentions use of ICE with RoQ. This document describes how RoQ implementations would use ICE candidates as part of RoQ RTP session setup. That description might reasonably be folded back into {{!I-D.draft-ietf-avtcore-sdp-roq}} at some point, but is currently separated to allow QUIC experts and ICE experts to review the description without having to excavate it as part of a larger specification.

This document has not yet been adopted by any IETF working group, so does not carry any special status within the IETF.

# Background {#background}

The QUIC transport protocol ({{!RFC9000}}) is encapsulated in UDP ({{!RFC768}} and {{!RFC2460}}), so in theory, a RoQ endpoint could use ICE procedures to identify candidate endpoint addresses and choose among them.

While that would work in theory, in practice, network operators expressed concern that STUN ({{?RFC5389}}) and TURN ({{?RFC5766}}) servers configured for characteristic UDP traffic (for example, query-response protocols such as DNS ({{?RFC1034}})) would need to know that RoQ endpoints expected to use ICE candidate addresses for realtime media flows, in order to configure their servers appropriately.

This specification defines a new ICE Transport Protocol ({{iana}}), so that these servers can identify RoQ flows, and provides additional application considerations ({{appcons}}) when ICE is used for RoQ flows.

# Additional Application Considerations for RoQ ICE Usage {#appcons}

The intention for ICE usage for RoQ Candidates is that it should be as similar as possible to ICE usage for UDP candidates ({{!RFC8445}}).

The biggest operational difference is that network operators might handle RoQ flows differently than other UDP flows, because the ICE Transport Token allows them to identify a connection-oriented media flow, but there are other relevant considerations.

The description for gathering candidates in {{Section 4.1 of !RFC6544}} includes a number of helpful considerations that would also apply to RoQ endpoints, including, for instance, deciding whether it is preferable to have no media at all, rather than to have media over TCP. Wise implementors will study those considerations carefully.

The recommended formula for candidate type preferences, referenced in {{Section 4.2 of !RFC6544}} and described in {{Section 5.1.2.1 of !RFC8445}} SHOULD also be used for RoQ candidates.

> **Editor's Note:** {{Section 4.2 of !RFC6544}} suggests recommended candidate type preferences for NAT-assisted candidates (105) and UDP-tunneled candidates (75). Are RECOMMENDED candidate type preferences useful for RoQ candidates?

The procedure described in {{Section 6 of !RFC6544}} for receiving the initial offer and answer also apply for RoQ, except that RoQ candidates are encoded as described in {{Section 3 and Section 4 of !I-D.draft-ietf-avtcore-sdp-roq}}.

Because RoQ is also connection-oriented, the possibility described in {{Section 6.1 of !RFC6544}} for an ICE offerer to initiate a QUIC connection to carry RoQ media sessions on a selected candidate pair in parallel with ICE-lite operation also applies, if the offerer is using the a=setup:active attribute. When this is the case, the requirements that the answerer accept that offer and repspond to it while keeping the already-created connectionin that section also apply to RoQ.

> **Editor's Note:** Harald Alvestrand notes that QUIC doesn't hae peer-to-peer extensions specified, so the client end of the connection will have to specifu port 9 (discard), meaning that it can do outgoing QUIC but not incoming QUIC over this UDP port pair. What other implications follow from this limitation?

# Security Considerations

The Security Considerations contained in {{!RFC8445}}, {{!RFC6445}}, {{!RFC9000}}, and ({{!I-D.ietf-avtcore-rtp-over-quic}}) are included here by reference.

# IANA Considerations {#iana}

IANA has created a sub-registry "ICE Transport Protocols" in the "Interactive Connectivity Establishment (ICE)" registry for ICE candidate-attribute transport extensions.  This specification adds the "RoQ" token to that registry.

|---
|Token  | Reference |
|---
| RoQ | (This Specification {{iana}}) |
|===

--- back

# Acknowledgments
{:numbered="false"}

This draft builds on considerable previous work that resulted in the base "Interactive Connectivity Establishment (ICE)" specification ({{!RFC8445}}) and the related "TCP Candidates with Interactive Connectivity Establishment (ICE)" specification {{!RFC6544}}. The author of this specification owes those authors many thanks.
