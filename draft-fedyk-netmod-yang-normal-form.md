---
title: "A Normal Form for YANG Modeled Data"
abbrev: "YANG Normal Form"
category: std

docname: draft-fedyk-netmod-yang-normal-form-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Network Modeling"
keyword:
 - YANG
 - normal form
 - canonical form
 - data model

author:
 -
    fullname: "Don Fedyk"
    organization: LabN Consulting, L.L.C.
    email: "dfedyk@labn.net"
 -
    fullname: "Scott Mansfield"
    organization: Ericsson
    email: "scott.mansfield@ericsson.com"

normative:
  RFC7950:
  RFC8342:
  RFC8525:
  RFC7951:
  RFC6241:

informative:
  RFC8340:
  RFC8791:


--- abstract

This document defines a normal form for data modeled with YANG.  A normal
form provides a predictable, canonical serialization of instance data so
that equivalent data produces identical representations.  This facilitates
comparison, hashing, signing, and testing of YANG modeled data across
implementations.


--- middle

# Introduction

YANG {{RFC7950}} is a data modeling language used to model configuration
and state data manipulated by network management protocols such as NETCONF
{{RFC6241}}.  Instance data encoded in XML {{RFC7950}} or JSON
{{RFC7951}} may be serialized in many equivalent ways: element and leaf
ordering, whitespace, namespace prefixes, and numeric or string
canonicalization can all differ while the underlying data remains
semantically identical.

The absence of a single canonical serialization makes it difficult to
compare two instances of data, to compute stable digests, or to apply
digital signatures.  This document defines a *normal form* for YANG
modeled data: a set of rules that, when applied, produce a single
deterministic serialization for any semantically equivalent set of
instance data.

## Terminology

{::boilerplate bcp14-tagged}

The following terms are used as defined in {{RFC7950}} and {{RFC8342}}:

data node, container, list, leaf, leaf-list, anydata, anyxml.

Normal form:
: A canonical, deterministic serialization of YANG modeled data such that
  any two semantically equivalent data trees produce byte-identical
  output under a given encoding.


# Motivation

Several use cases benefit from a normal form:

* Comparison: determining whether two datastores or subtrees are
  equivalent without a semantic diff.
* Integrity and signing: computing a stable digest or signature over
  instance data.
* Testing: comparing expected and actual output in interoperability and
  regression tests.
* Caching and deduplication: using a stable key derived from the data.


# Normal Form Rules

This section defines the rules that produce the normal form.  The rules
apply to data modeled by a set of YANG modules, using the schema to
resolve ordering and canonicalization.

## Node Ordering

Data nodes are ordered according to their definition order within the
governing YANG schema, following the schema node identifier ordering.
List entries whose "ordered-by" is "system" are ordered by their key
values; list entries whose "ordered-by" is "user" retain their
user-defined order.

## Value Canonicalization

Leaf and leaf-list values are represented using the canonical form of
their type as defined in {{RFC7950}}, Section 9, and the JSON encoding
rules of {{RFC7951}} where applicable.

## Namespaces and Prefixes

Namespace identification follows the encoding-specific rules
({{RFC7950}} for XML, {{RFC7951}} for JSON).  Redundant prefix
declarations are removed and a deterministic prefix assignment is used.

## Whitespace and Insignificant Formatting

Insignificant whitespace is removed.  A single deterministic indentation
and line-ending convention is applied per encoding.


# Encodings

## XML

TBD.

## JSON

TBD.


# Security Considerations

Normalization does not change the semantics of the data and therefore
does not by itself introduce new security concerns.  When the normal form
is used as input to digest or signature computation, implementations MUST
ensure the full set of governing schema and the selected encoding are
agreed upon by all parties, since differing schemas may yield differing
normal forms.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
