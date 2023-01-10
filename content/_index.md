---
---

{{< hero title="Get things from one computer to another, safely." >}}
(n.b.: The "safely" is a word randomly chosen from the following list: safely, securely, easily, quickly)

https://xkcd.com/949/
{{< /hero >}}

{{< section-2 >}}
{{< two-columns >}}
{{< tc-content >}}

## Intro

Magic Wormhole is a solution to quickly send files and folders to other devices, no account needed.

When sending a file, you get a short code that may look like this: `42-hurricane-equipment`. You tell that code to the person you want to send the file to, who then enters the code to receive the file. Everything else is handled by the software
{{< /tc-content >}}
{{< tc-terminal >}}
Test-Content
{{< /tc-terminal >}}
{{< /two-columns >}}
{{< /section-2 >}}

{{< section-3 >}}
{{< two-columns >}}
{{< tc-content >}}

## What it does

{{< /tc-content >}}
{{< tc-content >}}

## What it doesn't

{{< /tc-content >}}
{{< /two-columns >}}
{{< /section-3 >}}

{{< section-4 >}}
{{< one-column >}}

## How it works

Both participants of a file transfer connect to a central server, called the rendezvous server. The number at the beginning of the Wormhole code (called "nameplate") is used like a room number so that both sides may find each other on that server. All messages sent to that room will be forwarded to the other client.

The first thing upon an established connection is to create an encrypted channel. For this, the second part of the Wormhole code is used as password. A method called "Password authenticated key exchange" ([PAKE](https://en.wikipedia.org/wiki/Password-authenticated_key_agreement)) is used to establish a strong encryption key from the relatively weak password. This allows the codes to be short and memorizable.

The rendezvous server is not well suited to handle a lot of bandwidth. Therefore, both sides try _really hard_ to connect via a direct connection. This works most of the time when they are in the same network (and often enough across the internet), but if this does not succeed then a relay server which forwards the data between both sides is used as a fallback. The general mechanism is similar to what most internet telephony ([VoIP](https://en.wikipedia.org/wiki/Voice_over_IP)) and conferencing software do as well.

## Security

While establishing the connection, an active attacker ([machine in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)) can hijack the connection when guessing the password correctly. Only one attempt at guessing may be made, and unsuccessfull attempts will be detected. The default password strength is 16 bits of entropy, meaning that the chance of successfully attacking any connection is 1/65536.

Once established, the connection gives the security guarantees of a [secure channel](https://en.wikipedia.org/wiki/Secure_channel):

- Confidentiality: A passive attacker will gain no information besides connection metadata
- Authenticity: An active attacker can not manipulate encrypted messages without being detected
- Secure channel: An active attacker cannot replay past authentic messages, nor change the order in which messages are received

## Privacy

Unless additional measures are taken (see below), the following metadata are inevitably leaked to the rendezvous server:

- Public IP address of both participants
- Time, frequency and size of messages transferred

If a direct connection could not be established, the same data is also leaked to the relay server. Note that sometimes, the size of the file may already identify its contents (for example the size of some specific video).

In order to establish a direct connection, the following data is sent to the other side:

- Public IP address
- Local IP addresses

Especially the latter may leak information about the local network topology, as well as potentially used VPNs. For this reason, there is an option to force the use of a relay server. However keep in mind that the other person might be hosting the relay server you are using and thus still at least get your public IP address. Use [Tor](https://torproject.org/) to defend against this.

Connecting from the Tor network will likely hide your IP address from the involved servers, granting you anonymity. Note that because of the above, simply tunnelling the connection over Tor might not be enough. Instead, use a client that has native Tor support built in. Alternatively, route your entire operating system connection through Tor with [Whonix](https://www.whonix.org/).

## Resilience (?)

## Usage as protocol stack

Magic Wormhole is not defined as a single protocol. Instead, it involves multiple more or less independent protocols to do the work. This means that as an application developer, you may use parts of it to build your own interesting applications. Consider viewing it as a framework, where "file transfer" just happens to be the demonstrating use case. Notable elements:

- The "core" of the protocol allows two sides to bootstrap a small-bandwidth secure connection based on a code.
- The "transit" protocol describes how to establish a direct connection (with relay server fallback) and provides a secure channel on top of a TCP based connection.
  - This is very similar to the usual WebRTC stack (with TURN servers and TLS security), but scoped down to our specific use case:
  - The first difference is that we require a "reliable" transport layer like TCP, unlike WebRTC which uses UDP-based transport for everything except metadata signalling. This pushes a lot of complexity down the stack. If latency is of any importance to you, use WebRTC instead.
  - Wormhole relay servers are really similar to TURN servers, except that they are a lot harder to misuse and thus do not necessarily need access protection. This is because they don't forward connections
  - Relay servers may be reachable using different protocols (TCP, WebSocket, SCCP, …) and automatically bridge them. This guarantees interoperability across different client implementations (most notably: Web clients vs desktop clients).
  - The cryptography is a lot simpler than anything using TLS, notably you don't need any certificates.

See [get it] for a list of available libraries and bindings.
{{< /one-column >}}
{{< /section-4 >}}
