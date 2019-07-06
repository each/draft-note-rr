%%%
Title = "A DNS Resource Record for Confidential Comments (NOTE RR)"
abbrev = "note-rr"
docname = "@DOCNAME@"
category = "std"
ipr = "trust200902"
area = "Internet"
workgroup = "DNSOP Working Group"
updates = [6195]
date = 2019-07-06T00:00:00Z

[seriesInfo]
name = "Internet-Draft"
value = "@DOCNAME@"
stream = "IETF"
status = "standard"

[[author]]
initials = "E."
surname = "Hunt"
fullname = "Evan Hunt"
organization = "ISC"
[author.address]
 email = "each@isc.org"
[author.address.postal]
 street = "950 Charter St"
 city = "Redwood City"
 region = "CA"
 code = "94063"
 country = "US"

[[author]]
initials = "D."
surname = "Mahoney"
fullname = "Dan Mahoney"
organization = "ISC"
[author.address]
 email = "dmahoney@isc.org"
[author.address.postal]
 street = "950 Charter St"
 city = "Redwood City"
 region = "CA"
 code = "94063"
 country = "US"
%%%


.# Abstract

While the DNS zone master file format has always allowed comments, there is
no existing mechanism to preserve comments once the zone has been loaded
into memory or converted to a binary representation. This note proposes a
new RR type "NOTE", to be allocated from the Covert-RR type range proposed
in [@!I-D.krecicki-dns-covert], so that confidential comments can be stored
alongside zone data, and included in zone transfers when Covert semantics
are supported by the secondary.

{mainmatter}

# Introduction

DNS zone master files may include comments: any text on a line following
an unquoted semicolon is ignored when parsing the file [@!RFC1034]. These
comments are often used by administrators to keep notes about the zone
data; for example, the purpose of a particular host, or the person
responsible for maintaining it.

When the zone is loaded, however, comments may be lost.  Servers which
dump backup copies of dynamically updated or automatically signed zones
may obliterate comments that were in the original zone files.  Secondary
servers do not receive comment text when transferring zones from primary
servers.

Comments could be stored in the zone itself as TXT RRs; these would be
preserved after zone updates and across zone transfers. However, TXT records
are available to any DNS query. Because zone file comments commonly include
information about internal networks and/or personnel that could be of use
to potential attackers, it is better for distribution of comment data to be
restricted.

A Covert Resource Record, as described in [@!I-D.krecicki-dns-covert],
could be used for the storage of private text information within zone data
itself. This data could be transferred from primary to secondary servers
when Covert semantics are supported, and but would be concealed from normal
DNS queries (except from specific trusted DNS clients) and from secondary
servers that do not signal their support of Covert data transfer.

This document proposes the allocation of a new RR type NOTE from the
Covert-RR type range for this purpose. Comments that the operator wishes
to be stored and transferred with zone data can be encoded as NOTE records.
Traditional zone file comments, indicated by semicolons, would still be
ignored.

## Definitions

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**",
"**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**",
"**RECOMMENDED**", "**NOT RECOMMENDED**", "**MAY**", and
"**OPTIONAL**" in this document are to be interpreted as described in
BCP 14 [@!RFC2119] [@!RFC8174] when, and only when, they appear in all
capitals, as shown here.

# The NOTE RR Type

The NOTE RR is defined for all classes, with mnemonic NOTE and type code
(TBD). The RDATA and presentation formats are identical to those of the TXT
RR defined in [@!RFC1035], e.g:

~~~ ascii-art

    $ORIGIN example.com.
    joesbox   7200  IN  A       192.0.2.42
    7200            IN  AAAA    2001:DB8:3F:B019::17
              0     IN  NOTE    "Desktop system for Joe Smith, x7889"

~~~

The RR type code MUST be allocated from the Covert-RR type range, and
NOTE record data MUST be treated as Covert [@!I-D.krecicki-dns-covert].

# IANA Considerations

IANA is requested to allocate a DNS RR type number from the Covert-RR
type range for the NOTE RR type.

# Security and Privacy Considerations

NOTE data should only be accessible via Covert DNS queries, because zone
file comments commonly include information that could be of use to
potential attackers. Failure to implement the restrictions outlined in
[@!I-D.krecicki-dns-covert] could allow leakage of sensitive information.

{backmatter}
