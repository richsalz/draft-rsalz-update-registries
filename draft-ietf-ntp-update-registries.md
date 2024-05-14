---
docname: draft-ietf-ntp-update-registries-latest
updates: 5905, 5906, 8573, 7822, 7821
title: Updating the NTP Registries
category: std
stream: IETF
workgroup: ntp
ipr: trust200902

stand_alone: yes
pi:
  toc: yes
  sortrefs: yes
  symrefs: yes
  compact: yes
  subcompact: no

keyword: [NTP, extensions, registries, IANA]

author:
  -
    name: Rich Salz
    org: Akamai Technologies
    email: rsalz@akamai.com
normative:
  RFC5905: RFC5905
  RFC5906: RFC5906
  RFC7821: RFC7821
  RFC7822: RFC7822
  RFC8126: RFC8126
  RFC8573: RFC8573
  RFC8915: RFC8915

--- abstract

The Network Time Protocol (NTP) and Network Time Security (NTS) documents
define a number of assigned number registries, collectively called the NTP
registries.

Some registries have wrong values, some registries
do not follow current common practice, and some are just right.
For the sake of completeness, this document reviews all NTP and NTS registries,
and makes updates where necessary.

This document updates RFC 5905, RFC 5906, RFC 8573, RFC 7822, and
RFC 7821.

--- middle

# Introduction

The Network Time Protocol (NTP) and Network Time Security (NTS) documents
define a number of assigned number registries, collectively called the NTP
registries.
The NTP registries can all be found at
<https://www.iana.org/assignments/ntp-parameters/ntp-parameters.xhtml>
and the NTS registries can all be found at
<https://www.iana.org/assignments/nts/nts.xhtml>.

Some registries have wrong values, some registries
do not follow current common practice, and some are just right.
For the sake of completeness, this document reviews all NTP and NTS registries,
and makes updates where necessary.

The bulk of this document can be divided into two parts:

- First, each registry, its defining document, and a summary of its
syntax is defined.
- Second, the revised format and entries for each registry that is
being modified is specified.

# Existing Registries

This section describes the registries and the rules for them.
It is intended to be a short summary of the syntax and registration
requirements for each registry.
The semantics and protocol processing rules for each registry -- that is,
how an implementation acts when sending or receiving any of the fields --
are not described here.

## Reference ID, Kiss-o'-Death

{{RFC5905}} defined two registries; the Reference ID in Section 7.3, and the
Kiss-o'-Death in Section 7.4.  Both of these are allowed to be four ASCII
characters; padded on the right with all-bits-zero if necessary.
Entries that start with 0x58, the ASCII
letter uppercase X, are reserved for Private or Experimental Use.
Both registries are first-come first-served. The formal request to define
the registries is in {{RFC5905, Section 16}}.

## Extension Field Types

{{RFC5905, Section 7.5}} defined the on-the-wire format of extension
fields but did not create a registry for them.

{{RFC5906, Section 13}} mentioned the Extension Field Types registry, and defined it
indirectly by defining 30 extensions (10 each for request, response, and
error response).
It did not provide a formal definition of the columns in the registry.
{{RFC5906, Section 10}} splits the Field Type into four subfields,
only for use within the Autokey extensions.

{{RFC7821}} added a new entry, Checksum Complement, to the Extension
Field Types registry.

{{RFC7822}} clarified the processing rules for Extension Field Types,
particularly around the interaction with the Message Authentication
Code (MAC) field. NTPv4 packets may contain a MAC that appears where
one would expect the next extension field header.

{{RFC8573}} changed the cryptography used in the MAC field.

{{RFC8915}} added four new entries to the Extension Field Types registry.

The following problems exist with the current registry:

- Many of the entries in the Extension Field Types registry have
swapped some of the nibbles; 0x1234 is listed as 0x1432 for example.
This was due to documentation errors with the original implementation
of Autokey.
This document marks the erroneous values as reserved, in case there
is an implementation that used the registered values
instead of what the original implementation used.
- Some values were mistakenly re-used.

## Network Time Security Registries

{{RFC8915}} defines the NTS protocol.
Its registries are listed here for completeness, but no changes
to them are specified in this document.

Sections 7.1 through 7.5 (inclusive) added entries to existing registries.

Section 7.6 created a new registry, NTS Key Establishment Record Types,
that partitions the assigned numbers into three different registration
policies: IETF Review, Specification Required, and Private or Experimental Use.

Section 7.7 created a new registry, NTS Next Protocols,
that similarly partitions the assigned numbers.

Section 7.8 created two new registries, NTS Error Codes and NTS Warning Codes.
Both registries are also partitioned the same way.

# Updated Registries

The following general guidelines apply to all registries updated here:

- Every registry reserves a partition for Private or Experimental Use.

- Entries with ASCII fields are now limited to uppercase letters or digits; fields
starting with 0x58, the uppercase letter "X", are reserved for Private or
Experimental Use.

- The policy for every registry is now Specification Required, as defined
in {{RFC8126, Section 4.6}}.

The IESG is requested to choose three designated experts, with two being
required to approve a registry change. Guidance for such experts is
given below.

Each entry described in the sub-sections below is intended to completely
replace the existing entry with the same name.

## Guidance to Designated Experts

The designated experts (DE) should be familiar with {{RFC8126}}, particularly
Section 5. As that reference suggests, the DE should ascertain the existence
of a suitable specification, and verify that it is publicly available. The DE
is also expected to check the clarity of purpose and use of the requested
code points.

In addition, the DE is expected to be familiar with this document,
specifically the history documented here.

# IANA Considerations

## NTP Reference Identifier Codes

The registration procedure is changed to Specification Required.

The Note is changed to read as follows:

- Codes beginning with the character "X" are reserved for experimentation
and development. IANA cannot assign them.

The columns are defined as follows:

- ID (required): a four-byte value padded on the right with all-bits-zero.
Each byte other than padding must be an ASCII uppercase letter or digits.

- Clock source (required): A brief text description of the ID.

- Reference (required): the publication defining the ID.

The existing entries are left unchanged.

## NTP Kiss-o'-Death Codes

The registration procedure is changed to Specification Required.

The Note is changed to read as follows:

- Codes beginning with the character "X" are reserved for experimentation
and development. IANA cannot assign them.

The columns are defined as follows:

- ID (required): a four-byte value padded on the right with all-bits-zero.
Each byte other than padding must be an ASCII uppercase letter or digits.

- Meaning source (required): A brief text description of the ID.

- Reference (required): the publication defining the ID.

The existing entries are left unchanged.
## NTP Extension Field Types

The registration procedure is changed to Specification Required.

The reference {{RFC5906}} should be added, if possible.

The following two Notes are added:

- Field Types in the range 0xF000 through 0xFFFF, inclusive, are reserved
for experimentation and development. IANA cannot assign them.
Both NTS Cookie and Autokey Message Request have the same Field Type;
in practice this is not a problem as the field semantics will be
determined by other parts of the message.

- The "Reserved for historic reasons" is for differences between the
original documentation and implementation of Autokey and marks
the erroneous values as reserved, in case there is an implementation
that used the registered values instead of what the original
implementation used.

The columns are defined as follows:

- Field Type (required): A two-byte value in hexadecimal.

- Meaning (required): A brief text description of the field type.

- Reference (required): the publication defining the field type.

The table is replaced with the following entries.
IANA is requested to replace "This RFC" with the actual RFC number once
assigned.

| Field Type | Meaning                             | Reference |
|:-----------|:------------------------------------|:----------|
| 0x0000     | Crypto-NAK; authentication failure  | RFC 5905  |
| 0x0002     | Reserved for historic reasons       | This RFC  |
| 0x0102     | Reserved for historic reasons       | This RFC  |
| 0x0104     | Unique Identifier                   | RFC 8915, Section 5.3 |
| 0x0200     | No-Operation Request                | RFC 5906  |
| 0x0201     | Association Message Request         | RFC 5906  |
| 0x0202     | Certificate Message Request         | RFC 5906  |
| 0x0203     | Cookie Message Request              | RFC 5906  |
| 0x0204     | Autokey Message Request             | RFC 5906  |
| 0x0204     | NTS Cookie                          | RFC 8915, Section 5.4 |
| 0x0205     | Leapseconds Message Request         | RFC 5906  |
| 0x0206     | Sign Message Request                | RFC 5906  |
| 0x0207     | IFF Identity Message Request        | RFC 5906  |
| 0x0208     | GQ Identity Message Request         | RFC 5906  |
| 0x0209     | MV Identity Message Request         | RFC 5906  |
| 0x0302     | Reserved for historic reasons       | This RFC  |
| 0x0304     | NTS Cookie Placeholder              | RFC 8915, Section 5.5 |
| 0x0402     | Reserved for historic reasons       | This RFC  |
| 0x0404     | NTS Authenticator and Encrypted Extension Fields | RFC 8915, Section 5.6 |
| 0x0502     | Reserved for historic reasons       | This RFC  |
| 0x0602     | Reserved for historic reasons       | This RFC  |
| 0x0702     | Reserved for historic reasons       | This RFC  |
| 0x0902     | Reserved for historic reasons       | This RFC  |
| 0x2005     | UDP Checksum Complement             | RFC 7821  |
| 0x8002     | Reserved for historic reasons       | This RFC  |
| 0x8102     | Reserved for historic reasons       | This RFC  |
| 0x8200     | No-Operation Response               | RFC 5906  |
| 0x8201     | Association Message Response        | RFC 5906  |
| 0x8202     | Certificate Message Response        | RFC 5906  |
| 0x8203     | Cookie Message Response             | RFC 5906  |
| 0x8204     | Autokey Message Response            | RFC 5906  |
| 0x8205     | Leapseconds Message Response        | RFC 5906  |
| 0x8206     | Sign Message Response               | RFC 5906  |
| 0x8207     | IFF Identity Message Response       | RFC 5906  |
| 0x8208     | GQ Identity Message Response        | RFC 5906  |
| 0x8209     | MV Identity Message Response        | RFC 5906  |
| 0x8302     | Reserved for historic reasons       | This RFC  |
| 0x8402     | Reserved for historic reasons       | This RFC  |
| 0x8502     | Reserved for historic reasons       | This RFC  |
| 0x8602     | Reserved for historic reasons       | This RFC  |
| 0x8702     | Reserved for historic reasons       | This RFC  |
| 0x8802     | Reserved for historic reasons       | This RFC  |
| 0x8902     | Reserved for historic reasons       | This RFC  |
| 0xC002     | Reserved for historic reasons       | This RFC  |
| 0xC102     | Reserved for historic reasons       | This RFC  |
| 0xC200     | No-Operation Error Response         | RFC 5906  |
| 0xC201     | Association Message Error Response  | RFC 5906  |
| 0xC202     | Certificate Message Error Response  | RFC 5906  |
| 0xC203     | Cookie Message Error Response       | RFC 5906  |
| 0xC204     | Autokey Message Error Response      | RFC 5906  |
| 0xC205     | Leapseconds Message Error Response  | RFC 5906  |
| 0xC206     | Sign Message Error Response         | RFC 5906  |
| 0xC207     | IFF Identity Message Error Response | RFC 5906  |
| 0xC208     | GQ Identity Message Error Response  | RFC 5906  |
| 0xC209     | MV Identity Message Error Response  | RFC 5906  |
| 0xC302     | Reserved for historic reasons       | This RFC  |
| 0xC402     | Reserved for historic reasons       | This RFC  |
| 0xC502     | Reserved for historic reasons       | This RFC  |
| 0xC602     | Reserved for historic reasons       | This RFC  |
| 0xC702     | Reserved for historic reasons       | This RFC  |
| 0xC802     | Reserved for historic reasons       | This RFC  |
| 0xC902     | Reserved for historic reasons       | This RFC  |
| 0xF000-<br>0xFFFF    | Reserved for Experimental Use       | This RFC  |

# Security Considerations

This document adds no new security considerations, as they are defined
in the document that defines the extension.  See the References column of the
appropriate table.

# Acknowledgements

The members of the NTP Working Group helped a great deal.
Notable contributors include:

* Miroslav Lichvar, Red Hat
* Daniel Franke, formerly at Akamai Technologies
* Danny Mayer, Network Time Foundation
* Michelle Cotton, formerly at IANA
* Tamme Dittrich, Tweede Golf
