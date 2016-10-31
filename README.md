
# Trellis

Tyson Malchow 10.2016

**Abstract**: A web-of-trust protocol used for fair sharing of digital resources between socially recognized
individuals.

## Motivation
Tools such as Captcha codes allow us, importantly, to distinguish people from bots. This is crucial first and foremost 
because bots are central to information-based attacks used against any shared digital service - from Facebook to DNS.

For all of the digital systems that we build between each other to remain useful and highly available, 
these information channels must be controlled crucially through a vehement appeal to true fair use. For fair, democratic
consumption, the free speech rights of socially-recognized individuals is utmost.  Those with more hardware are not entitled 
better or higher access (though they may acceptably achieve it); access must instead be fundamentally controlled through
sharing, which requires the establishment of trust in individuality and identity with any remote party. 

The high rate of AI proliferation foreshadows the obsolescence of tools like Captcha - and with it, our ability to strongly
protect ourselves from such attacks. When social systems constructed atop the Internet lose their ability to establish 
this trust with their participants, they quickly break down. Without being provably rooted in physical social structure, 
exploits of many forms to the those systems are always conceivable - from bots just spamming comments sections with advertisements
to massive DDoS network traffic.

At first glance, existing system like Facebook seem like likely solution candidates. They are however susceptible to these 
types of attacks themselves, particularly with the progressing field of AI. 

A a centralized/governance approach also seems at first a likely solution. It has the ability to do better than the (existing)
system of Facebook or Twitter because it may mandate physical registration. However, it's subject to the much more grotesque, 
long-term exploits of corruption and social cancer.

A sustainable system must instead afford dualism: it must accept and foster the fracturing and reconnection of social groups,
and give nobody keys to the collective kingdom.

In this way, when two remote groups can find no social ties between their members, it becomes easy for the resources owned 
by each group to isolate and protect themselves against the other. The resources re-allocate to assist with those connected
to the social graph of their owner. Every group must maintain enough resources to support all its members, and no member 
is bound to use any resources but their own (though, some of the data they choose share with their connections will be 
present on the resources entrusted as agents to those connections).

The solution is a distributed web-of-trust which enables people to (provably) cross connect systems they each trust along
with an "immune system" capable of coping with the nature of persisted fair resource allocation amidst network attack. This
paper describes such a protocol.

 *(Interesting, solution-filled implications if AI ever become self-aware enough to merit their own identities; the problem
 remains a social one instead of a technological one)*

## Authentication
To protect the network against unwanted actors, the system provides several layers of conceptual security and
penetration mitigation.

When a client connects to a host, it will be given a *bandwidth grant* which includes both a burst size and measurement interval. 
It must not exceed this limit or it may be blocked for some period of time. Host at any point may decide to throttle the target 
up or down and notify it by sending it an updated bandwidth grant. The grant may be zero, in which case the client should 
immediately disconnect and wait the specified allotment of time before attempting a reconnection. Hosts should recognize
that variable network conditions may result in burstier traffic than explicitly allowed and provide sensible allowances.

The client, after connecting, and before or after receiving the bandwidth grant, must present a *crypto start request*. This
request will indicate the specific set of cryptographic parameters that will be used, along with two optional parameters:
a preshared key and a network ID(HEX-encoded SHA256 of the root public key). 

The host may use either of these parameters, if present, to verify whether or not the crypto protocol should be engaged and
the connection permitted to continue. A PSK is preferrable since no network ID is leaked, but it requires a pre-existing
relationship between the host and the client.    
 
If the host decides the client is allowed to proceeed, then it responds with a *crypto start response*, immediately after 
which key-exchange begins and the two sides validate identities. Hosts will generate a new keypair unique to the 
remote agent, ensuring that cryptographic communication is possible even if a client wishes to remain anonymous (??). 
 
After key exchange, the API is symmetric and either side may invoke RPC on the other. Both sides are permitted, at any time, 
to send updated bandwidth grants.
 
### Crypto scheme "trellis1"

 - Key exchange: ECDH
 - Cipher: AES in CBC mode with PKCS5 Padding
 - Message Signatures: SHA256withECDSA
 - Public key: X509 encoded key using ECDH key algorithm
 - Secret key: AES key with first 16 bytes of the SHA256 digest of the secret
 
### Crypto scheme "trellis2"

 - *Open to suggestions*
 
 