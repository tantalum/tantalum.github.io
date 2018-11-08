---
title: "DNS Security: Comparing DNS-over-TLS, DNSSEC and DNS-over-HTTPS"
layout: post
date: 2018-11-08
---

DNS security has been getting a lot of attention these past couple of
years. This has lead to a number of DNS security-enhancing standards to be
proposed, with the three big ones being DNS-over-TLS, DNSSEC and
DNS-over-HTTPS.  In this article we will discuss all three of those
standards, the threat model they assume and what protection the provide.

<!--more-->

## DNS-over-TLS

[RFC7858](DoT) is a very easy read and is explicit about DNS-over-TLS's
threat model.

> Encryption provided by TLS eliminates opportunities for eavesdropping and
> on-path tampering with DNS queries in the network, such as discussed in
> RFC 7626.
>
> ...
>
> The protocol described here works for queries and responses between stub
> clients and recursive servers.  It might work equally between recursive
> clients and authoritative servers, but this application of the protocol is
> out of scope

The *stub client* is usually the machine originating the DNS request, the
*recursive server* is the server providing the answer directly back to the
client. In simple terms, DoT's purpose is to secure the
connection between the machine making the DNS query and the DNS server the
machine is configured to use. Establishing TLS connection between the
client and the DNS servers, means that other on-path or on-network devices
cannot see what DNS records are being looked up and cannot alter the
response from the DNS server.

The way DoT accomplishes this is by defining a new well-know port (853/TCP)
exclusively for clients to connect to when making a DNS query over TLS.
Once the client establishes a TCP connection to the server, the client will
immediately start a TLS handshake. [RFC7858](DoT) does have one very
important caveat in it with regards to establishing the TLS connection:

> The client will then authenticate the server, *if required.*

DoT does _NOT_ require the client to authenticate that the server it thinks
it's connected to really is the server it is connected to. This opens up
the door for very easy MitM attacks if the client is not configured to
authenticate the server. DoT defines two "Usage Profiles" to handle two
distinct use cases

1. Opportunistic Privacy Profile

    For this profile, the user (aka. client) does not require privacy but
    does desire it when possible. This is used when the client learns of
    the DNS servers from an untrusted sources (e.g. DHCP server). In this
    case the client might or might not validate the resolver and this
    privacy profile is in place to maximize compatability with common
    network setups (e.g. The Hotel WiFi Hotspot). This profile *leaves
    clients vulnerable to on-path active attackers.*

2. Out-of-Band Key-Pinned Privacy Profile

    With the Out-of-Band Key-Pinned Privacy Profile the client and DNS
    server have an already established trust relationship, and Subject
    Public Key Info (SPKI) for the DNS server is available to the client.
    In this case the client will use the key information to authenticate
    the client using the methods described in
    [RFC7469](https://tools.ietf.org/html/rfc7469) 

As such only the Out-of-Band Key-Pinned privacy profile provides strong
privacy and security guarantees when performing DNS lookups. 

In short, DNS-over-TLS is designed to provide confidentiality and
integrity between a DNS client and server, only when using the Out of
Band Key Pinned Privacy Profile. DoT also provide the Opportunistic privacy
profile for compatibility with current network setups, although this
privacy profile does not guarantee confidentiality and integrity.

## DNSSEC

Per [RFC4033](DNSSEC):

> The Domain Name System Security Extensions (DNSSEC) add data origin
> authentication and data integrity to the Domain Name System.
>
> ...
>
> The Domain Name System (DNS) security extensions provide origin
> authentication and integrity assurance services for DNS data, including
> mechanisms for authenticated denial of existence of DNS data. 

In simpler terms, DNSSEC enables the requester of a DNS record to validate
that the response they received was from the authoritative DNS server
controlling the record. Or as stated later in the document:

> but DNSSEC itself is concerned with object security of DNS data

With DNSSEC and DNS server will sign it's resource records (`RRsets` in the
RFC) and publish that signature, and the public key to validate the
signature. A DNSSEC aware resolver can than create a chain of trust (aka.
an authentication chain) from the root of the DNS hierarchy down to key
signing the requested records. The implication here is that each DNSSEC
resolver has to be configured with one-or-more keys associated with the
root resolver that function as the root of trust.

DNSSEC defines 4 states that indicate different levels for trust that a
record originated from a trusted source:

1. *Secure*: The resolver is able to validate the full chain of trust, and
verify all the signatures, all the way up to a trust anchor.

2. *Insecure*: The resolver has a valid chain of trust that terminates at a
non-existent DS record. This means that there are branches/leafs in the DNS
tree that cannot be validated, because there is no signed key by which to
validate them.

3. *Bogus*: The chain of trust is invalid. This could range from signatures
with unsupported algorithms, to missing data, to attempted tampering.

4. *Indeterminate*: There is no trust anchor for a specific portion of the
tree. This is the default operation mode.

If DoT's goal is to provide confidentiality and integrity of the
transmission on the wire, DNSSEC's goal is provide authenticity of the data
and prove that the data originated from a trusted source.

## DNS-over-HTTPS

[DNS-over-HTTPS](DoH) shares one of it's primary goals with DNS-over-TLS,
namely providing privacy for DNS queries and responses and preventing
on-path tampering. [RFC8484](DoH) explicitly states DoH's two goals in the
introduction:

> Two primary use cases were considered during this protocol's development.
> These use cases are preventing on-path devices from interfering with DNS
> operations, and also allowing web applications to access DNS information
> via existing browser APIs in a safe way consistent with Cross Origin
> Resource Sharing (CORS) .

DNS-over-HTTPS is not it self and extension of the DNS system or protocol,
but instead defines a standard way in which to request DNS data over HTTPS.
I won't focus on the specifics of DoH request and response formats, but
from a security perspective [RFC8484](DoH) does state:

> DoH encrypts DNS traffic and requires authentication of the server.

In essence DoH depends on the security of the HTTP client to provide it's
security guarantees, thus handing off all the complexities of establishing
a secure TLS connection to the HTTP client making the connection. One thing
to note is that while DoT provides a method for server authentication, DoH
*requires* the server to be authenticated.

# Securing DNS

DoT and DoH both aim at providing confidentiality and integrity of DNS data
and securing the communication channel between the DNS client and server.
DNSSEC however aims and insuring the authenticity of DNS data and ensuring
the DNS record data originated from an authenticated source. DNSSEC is both
compatible with, and complementary to, DoT and DoH. 


[DoT]: https://tools.ietf.org/html/rfc7858
[DoH]: https://tools.ietf.org/html/rfc8484
[DNSSEC]: https://tools.ietf.org/html/rfc4033
