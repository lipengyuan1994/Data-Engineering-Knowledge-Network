_This is the third whitepaper on how Google uses encryption to protect your data. In this whitepaper, you will find more detail on encryption in transit for Google Cloud and Google Workspace._

_For all Google products, we strive to keep customer data highly protected and to be as transparent as possible about how we secure it._

_This content was last updated in September 2022, and represents the status quo as of the time it was written. Google's security policies and systems may change going forward, as we continually improve protection for our customers._

## CIO-level summary

-   Google employs several security measures to help ensure the authenticity, integrity, and privacy of data in transit.
-   For the use cases discussed in this whitepaper, Google encrypts and authenticates data in transit at one or more network layers when data moves outside physical boundaries not controlled by Google or on behalf of Google. All VM-to-VM traffic within a VPC network and peered VPC networks is encrypted.
-   Depending on the connection that is being made, Google applies default protections to data in transit. For example, we secure communications between the user and the Google Front End (GFE) using TLS.
-   Google Cloud customers with additional requirements for encryption of data over WAN can choose to implement further protections for data as it moves from a user to an application, or virtual machine to virtual machine. These protections include IPSec tunnels, Gmail S/MIME, managed SSL certificates, and Istio.
-   Google works actively with the industry to help bring encryption in transit to everyone, everywhere. We have several open-source projects that encourage the use of encryption in transit and data security on the Internet at large including Certificate Transparency, Chrome APIs, and secure SMTP.
-   Google plans to remain the industry leader in encryption in transit. To this end, we dedicate resources toward the development and improvement of encryption technology. Our work in this area includes innovations in the areas of Key Transparency and post-quantum cryptography.

## Introduction

Security is often a deciding factor when choosing a public cloud provider. At Google, security is of the utmost importance. We work tirelessly to protect your data—whether it is traveling over the Internet, moving within Google's infrastructure, or stored on our servers.

Central to Google's security strategy are authentication, integrity, and encryption, for both data at rest and in transit. This paper describes our approach to encryption in transit for Google Cloud.

For data at rest, see [Encryption at Rest in Google Cloud Platform](https://cloud.google.com/security/encryption/default-encryption?authuser=2). For an overview across all of Google Security, see [Google Infrastructure Security Design Overview](https://cloud.google.com/security/security-design?authuser=2).

**Audience:** this document is aimed at CISOs and security operations teams using or considering Google Cloud.

**Prerequisites:** in addition to this introduction, we assume a basic understanding of [encryption](https://en.wikipedia.org/wiki/Encryption) and [cryptographic primitives](https://en.wikipedia.org/wiki/Cryptographic_primitive).

## Authentication, Integrity, and Encryption

Google employs several security measures to help ensure the authenticity, integrity, and privacy of data in transit.

-   **[[Authentication]]:** we verify the data source, either a human or a process, and destination.
-   **Integrity:** we make sure data you send arrives at its destination unaltered.
-   **[[Encryption]]:** we make your data unreadable while in transit to keep it private. Encryption is the process through which legible data (plaintext) is made illegible (ciphertext) with the goal of ensuring the plaintext is only accessible by parties authorized by the owner of the data. The algorithms used in the encryption process are public, but the key required for decrypting the ciphertext is private. Encryption in transit often uses asymmetric key exchange, such as elliptic-curve-based Diffie-Hellman, to establish a shared symmetric key that is used for data encryption. For more information on encryption, see [Introduction to Modern Cryptography](https://www.cs.umd.edu/%7Ejkatz/imc.html).

Encryption can be used to protect data in three states:

-   **Encryption at rest** protects your data from a system compromise or data exfiltration by encrypting data while stored. The Advanced Encryption Standard (AES) is often used to encrypt data at rest.
-   **Encryption in transit:** protects your data if communications are intercepted while data moves between your site and the cloud provider or between two services. This protection is achieved by encrypting the data before transmission; authenticating the endpoints; and, on arrival, decrypting and verifying that the data was not modified. For example, Transport Layer Security (TLS) is often used to encrypt data in transit for transport security, and Secure/Multipurpose Internet Mail Extensions (S/MIME) is used often for email message encryption.
-   **Encryption in use:** protects your data in memory from compromise or data exfiltration by encrypting data while being processed. For more information, see [Confidential Computing](https://cloud.google.com/confidential-computing?authuser=2).

Encryption is one component of a broader security strategy. Encryption in transit defends your data, after a connection is established and authenticated, against potential attackers by:

-   Removing the need to trust the lower layers of the network which are commonly provided by third parties
-   Reducing the potential attack surface
-   Preventing attackers from accessing data if communications are intercepted

With adequate authentication, integrity, and encryption, data that travels between users, devices, or processes can be protected in a hostile environment. The remainder of this paper explains Google's approach to the encryption of data in transit and where it is applied.

## Google's Network Infrastructure

### Physical boundaries

A physical boundary is the barrier to a physical space that is controlled by or on behalf of Google, where we can ensure that rigorous security measures are in place. Physical access to these locations is restricted and heavily monitored. Only a small set of Google employees have access to hardware. Data in transit within these physical boundaries is generally authenticated, but may not be encrypted by default.

Due to the scale of the global Internet, we cannot put the same physical security controls in place for the fiber links in our WAN, or anywhere outside of physical boundaries controlled by or on behalf of Google. For this reason, we automatically enforce additional protections outside of our physical trust boundary. These protections include encryption of data in transit for all traffic outside of our physical boundaries.

### How traffic gets routed

To fully understand how encryption in transit works at Google, it is also necessary to explain how traffic gets routed through the Internet. This section describes how requests get from an end user to the appropriate Google Cloud service or customer application, and how traffic is routed between services.

A **Google Cloud service** is a modular cloud service that we offer to our customers. These services include compute, data storage, data analytics and machine learning. For example, Cloud Storage is a Google Cloud services. A **customer application** is an application hosted on Google Cloud that you, as a Google customer, can build and deploy using Google Cloud services. Customer applications or partner solutions that are hosted on Google Cloud are not considered Google Cloud services[1](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote1). For example, an application you build using Google App Engine, Google Kubernetes Engine, or a VM in Google Compute Engine is a customer application.

_The five kinds of routing requests discussed below are shown in Figure 1. This figure shows the interactions between the various network components and the security in place for each connection._

#### End user (Internet) to a Google Cloud Service

Google Cloud services accept requests from around the world using a globally distributed system called the Google Front End (GFE). GFE terminates traffic for incoming HTTP(S), TCP and [TLS](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#transport_layer_security) proxy traffic, provides DDoS attack countermeasures, and routes and load balances traffic to the Google Cloud services themselves. There are GFE points of presence around the globe with routes advertised via unicast or [Anycast](https://tools.ietf.org/html/rfc1546).

GFEs proxy traffic to Google Cloud services. GFEs route the user's request over our network backbone to a Google Cloud service. This connection is authenticated and encrypted from GFE to the front-end of the Google Cloud service or customer application, when those communications leave a physical boundary controlled by Google or on behalf of Google. Figure 1 shows this interaction (labeled connection A).

#### End user (Internet) to a customer application hosted on Google Cloud

There are several ways traffic from the Internet can be routed to a customer application you host on Google Cloud. The way your traffic is routed depends on your configuration, as explained below. Figure 1 shows this interaction (labeled connection B).

If you are using an HTTP(S) Load Balancing or External SSL Proxy Load Balancing, see [Encryption from the load balancer to the backends](https://cloud.google.com/load-balancing/docs/ssl-certificates/encryption-to-the-backends?authuser=2).

#### Virtual Machine to Virtual Machine

VM-to-VM connections within VPC networks and peered VPC networks inside of Google's production network are authenticated and encrypted. This includes connections between customer VMs and between customer and Google-managed VMs such as Cloud SQL. Figure 1 shows this interaction (labeled connection C).

#### Connectivity to Google APIs and services

Traffic handling differs depending on the location of the Google Cloud service:

-   Most Google APIs and services are hosted on Google Front Ends (GFEs); however, some services are hosted on Google-managed instances. For example, [private services access](https://cloud.google.com/vpc/docs/private-services-access?authuser=2) and [GKE masters for private clusters](https://cloud.google.com/kubernetes-engine/docs/concepts/private-cluster-concept?authuser=2) are hosted on Google-managed instances.
    
    With [Private Google Access](https://cloud.google.com/vpc/docs/private-access-options?authuser=2), VMs that don't have external IP addresses can access [supported Google APIs and services](https://cloud.google.com/vpc/docs/private-google-access?authuser=2), including customer applications hosted on App Engine. For more information about access to Google APIs and services, see [Private access options for services](https://cloud.google.com/vpc/docs/private-access-options?authuser=2).
    
-   If a Compute Engine VM instance connects to the external IP address of another Compute Engine VM instance, traffic remains in Google's production network. Systems that are outside of Google's production network that connect to an external IP address of a Compute Engine VM instance have traffic routed over the internet.
    
    Figure 1 shows an external path (labeled connection D). Typical cases of this kind of routing request are:
    
    -   From a Compute Engine VM to Google Cloud Storage
    -   From a Compute Engine VM to a Machine Learning API

From the VM to the GFE, Google Cloud services support protecting these connections with TLS by default[2](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote2). The connection is authenticated from the GFE to the service and encrypted if the connection leaves a physical boundary. In addition to these default protections, you can apply envelope encryption. For more information, see[Encrypt your data](https://cloud.google.com/architecture/framework/security/data-security?authuser=2#encrypt-data).

#### Google Cloud service to Google Cloud service

Routing from one production service to another takes place on our network backbone and may require routing traffic outside of physical boundaries controlled by or on behalf of Google. Figure 1 shows this interaction (labeled connection E). An example of this kind of traffic is a Google Cloud Storage event triggering Google Cloud Functions. Connections between production services are encrypted if they leave a physical boundary, and authenticated within the physical boundary.

![The user is routed to Google Cloud services across physical boundaries using ALTS encryption.](https://cloud.google.com/static/images/security/whitepaper-transit-01.svg?authuser=2)

_Figure 1: Protection by default and options overlaid on a VPC network_

## Encryption in Transit by Default

Google uses various methods of encryption, both default and user configurable, for data in transit. The type of encryption used depends on the OSI layer, the type of service, and the physical component of the infrastructure. Figures 2 and 3 below illustrate the optional and default protections Google Cloud has in place for layers 3, 4, and 7.

![Encryption options for layers 3 and 4 include data traffic to the VM and across boundaries.](https://cloud.google.com/static/images/security/whitepaper-transit-02.svg?authuser=2)

_Figure 2: Protection by Default and Options at Layers 3 and 4 across Google Cloud_

![Encryption options for layer 7 include those for data in transit between VMs and to the Google Front End.](https://cloud.google.com/static/images/security/whitepaper-transit-03.svg?authuser=2)

_Figure 3: Protection by Default and Options at Layer 7 across Google Cloud[3](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote3)_

The remainder of this section describes the default protections that Google uses to protect data in transit.

### User to Google Front End encryption

Today, many systems use HTTPS to communicate over the Internet. HTTPS provides security by using a TLS connection, which ensures the authenticity, integrity, and privacy of requests and responses. To accept HTTPS requests, the receiver requires a public–private key pair and an X.509 certificate for server authentication from a Certificate Authority (CA). The key pair and certificate help protect a user's requests at the application layer (layer 7) by proving that the receiver owns the domain name for which requests are intended. The following subsections discuss the components of user to GFE encryption, namely: TLS, BoringSSL, and Google's Certificate Authority. Recall that not all customer paths route via the GFE; notably, the GFE is used for traffic from a user to a Google Cloud service, and from a user to a customer application hosted on Google Cloud that uses Google Cloud Load Balancing.

#### Transport Layer Security (TLS)

When a user sends a request to a Google Cloud service, we secure the data in transit; providing authentication, integrity, and encryption, using HTTPS with a certificate from a web (public) certificate authority. Any data the user sends to the GFE is encrypted in transit with Transport Layer Security (TLS) or QUIC. GFE negotiates a particular encryption protocol with the client depending on what the client is able to support. GFE negotiates more modern encryption protocols when possible.

GFE's scaled TLS encryption applies not only to end-user interactions with Google, it also facilitates API interactions with Google over TLS, including Google Cloud. Additionally, our TLS encryption is used in Gmail to exchange email with external mail servers (more detail in [Require TLS in Gmail](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#require_tls_in_gmail)).

Google is an industry leader in both the adoption of TLS and the strengthening of its implementation. To this end, we have enabled, by default, many of the security features of TLS. For example, [since 2011](https://security.googleblog.com/2011/11/protecting-data-for-long-term-with.html) we have been using forward secrecy in our TLS implementation. Forward secrecy makes sure the key that protects a connection is not persisted, so an attacker that intercepts and reads one message cannot read previous messages.

#### BoringSSL

[BoringSSL](https://boringssl.googlesource.com/boringssl) is a Google-maintained, open-source implementation of the TLS protocol, forked from OpenSSL, that is mostly interface-compatible with OpenSSL. [Google forked BoringSSL from OpenSSL](https://www.imperialviolet.org/2015/10/17/boringssl.html) to simplify OpenSSL, both for internal use and to better support the Chromium and Android Open Source Projects. BoringCrypto, the core of BoringSSL, has been validated to FIPS 140-2 level 1.

TLS in the GFE is implemented with BoringSSL. Table 1 shows the encryption protocols that GFE supports when communicating with clients.

Protocols

Authentication

Key exchange

Encryption

Hash Functions

TLS 1.3

RSA 2048

Curve25519

AES-128-GCM

SHA384

TLS 1.2

ECDSA P-256

P-256 (NIST secp256r1)

AES-256-GCM

SHA256

TLS 1.1

 

 

AES-128-CBC

SHA1[7](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote6)

TLS 1.0[4](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote4)

 

 

AES-256-CBC

MD5[8](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote6)

QUIC[5](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote5)

 

 

ChaCha20-Poly1305

 

 

 

 

3DES[6](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote6)

 

_Table 1: Encryption Implemented in the Google Front End for Google Cloud Services and Implemented in the BoringSSL Cryptographic Library_

**Note:** Though TLS 1.1 and TLS 1.0 are supported, we recommend using TLS 1.3 and TLS 1.2 to help protect against known man-in-the-middle attacks. For more information, see [The POODLE Attack and the End of SSL 3.0](https://blog.mozilla.org/security/2014/10/14/the-poodle-attack-and-the-end-of-ssl-3-0).

#### Google's Certificate Authority

As part of TLS, a server must prove its identity to the user when it receives a connection request. This identity verification is achieved in the TLS protocol by having the server present a certificate containing its claimed identity. The certificate contains both the server's DNS hostname and its public key. Once presented, the certificate is signed by an issuing Certificate Authority (CA) that is trusted by the user requesting the connection[9](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote9). As a result, users who request connections to the server only need to trust the root CA. If the server wants to be accessed ubiquitously, the root CA needs to be known to the client devices worldwide. Today, most browsers, and other TLS client implementations, each have their own set of root CAs that are configured as trusted in their “root store”.

Historically, Google operated its own issuing CA, which we used to sign certificates for Google domains. We did not, however, operate our own root CA. Today, our CA certificates are cross-signed by multiple root CAs which are ubiquitously distributed, including DigiCert and roots previously operated by GlobalSign (“GS Root R2” and “GS Root R4”).

In June 2017, [we announced](https://cloudplatform.googleblog.com/2017/06/Google-Cloud-services-are-switching-Certificate-Authority.html) a transition to using Google-owned root CAs. Over time, we plan to operate a ubiquitously distributed root CA which will issue certificates for Google domains and for our customers.

#### Root key migration and key rotation

Root CA keys are not changed often, as migrating to a new root CA requires all browsers and devices to embed trust of that certificate, which takes a long time. As a result, even though Google now operates its own root CAs, we will continue to rely on multiple third-party root CAs for a transitional period to account for legacy devices while we migrate to our own.

Creating a new root CA key requires a key ceremony. At Google, the ceremony mandates that a minimum 3 of the 6 possible authorized individuals physically gather to use hardware keys that are stored in a safe. These individuals meet in a dedicated room, shielded from electromagnetic interference, with an air-gapped Hardware Security Module (HSM), to generate a set of keys and certificates. The dedicated room is in a secure location in Google data centers. Additional controls, such as physical security measures, cameras, and other human observers, ensure that the process goes as planned. If the ceremony is successful the generated certificate is identical to a sample certificate, except for the issuer name, public key and signature. The resulting root CA certificate is then submitted to browser and device root programs for inclusion. This process is designed to ensure that the privacy and security of the associated private keys are well understood so the keys can be relied upon for a decade or more.

As described earlier, CAs use their private keys to sign certificates, and these certificates verify identities when initiating a TLS handshake as part of a user session. Server certificates are signed with intermediate CAs, the creation of which is similar to the creation of a root CA. The intermediate CA's certificates are distributed as part of the TLS session so it's easier to migrate to a new intermediate CA. This method of distribution also enables the CA operator to keep the root CA key material in an offline state.

The security of a TLS session is dependent on how well the server's key is protected. To further mitigate the risk of key compromise, Google's TLS certificate lifetimes are limited to approximately three months and the certificates are rotated approximately every two weeks.

A client that has previously connected to a server can use a private ticket key[10](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote10) to resume a prior session with an abbreviated TLS handshake, making these tickets very valuable to an attacker. Google rotates ticket keys at least once a day and expires the keys across all properties every 3 days. To learn more about session key ticket rotation, see [Measuring the Security Harm of TLS Crypto Shortcuts](https://jhalderm.com/pub/papers/forward-secrecy-imc16.pdf).

### Google Front End to Application Front Ends

In some cases, as discussed in [How traffic gets routed](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#how_traffic_gets_routed), the user connects to a GFE inside of a different physical boundary than the desired service and the associated Application Front End. When this occurs, the user's request and any other layer 7 protocol, such as HTTP, is either protected by TLS, or encapsulated in an RPC which is protected using Application Layer Transport Security (ALTS), discussed in [Service-to-service authentication, integrity, and encryption](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#service_integrity_encryption). These RPCs are authenticated and encrypted.

For Google Cloud services, RPCs are protected using ALTS. For customer applications hosted on Google Cloud, if traffic is routed via the Google Front End, for example if they are using the Google Cloud Load Balancer, traffic to the VM is protected using Google Cloud's virtual network encryption, described in the next section.

### Google Cloud's virtual network encryption and authentication

Encryption of private IP traffic within the same VPC or across peered VPC networks within Google Cloud's virtual network is performed at the network layer.

We use the Advanced Encryption Standard (AES) in Galois/Counter Mode (GCM) with a 128-bit key (AES-128-GCM) to implement encryption at the network layer. Each pair of communicating hosts establishes a session key via a control channel protected by [ALTS](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#service_integrity_encryption) for authenticated and encrypted communications. The session key is used to encrypt all VM-to-VM communication between those hosts, and session keys are rotated periodically.

At the network layer (layer 3), Google Cloud's virtual network authenticates all traffic between VMs. This authentication, achieved via security tokens, protects a compromised host from spoofing packets on the network.

During authentication, security tokens are encapsulated in a tunnel header which contains authentication information about the sender and receiver. The control plane[11](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote11) on the sending side sets the token, and the receiving host validates the token. Security tokens are pre-generated for every flow, and consist of a token key (containing the sender's information) and the host secret. One secret exists for every source-receiver pair of physical boundaries controlled by or on behalf of Google.

Figure 4 shows how token keys, host secrets, and security tokens are created.

![The component parts of a security token can include a token key and a host secret, along with their dependencies.](https://cloud.google.com/static/images/security/security-tokens.svg?authuser=2)

_Figure 4: Security Tokens_

The physical boundary secret is a 128-bit pseudorandom number, from which host secrets are derived by taking an HMAC-SHA1. The physical boundary secret is negotiated by a handshake between the network control planes of a pair of physical boundaries and renegotiated every few hours. The security tokens used for individual VM-to-VM authentication, derived from these and other inputs, are HMACs, negotiated for a given sender and receiver pair.

## Virtual machine to Google Front End encryption

VM to GFE traffic uses external IPs to reach Google services, but you can configure [Private access](https://cloud.google.com/vpc/docs/private-access-options?authuser=2) to use Google-only IP addresses for the requests.

By default, we support TLS traffic from a VM to the GFE. The connection happens in the same way as any other external connection. For more information on TLS, see [Transport Layer Security (TLS)](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#transport_layer_security).

### Service-to-service authentication, integrity, and encryption

Within Google's infrastructure, at the application layer (layer 7), we use our [Application Layer Transport Security (ALTS)](https://cloud.google.com/security/encryption-in-transit/application-layer-transport-security?authuser=2) for the authentication, integrity, and encryption of Google RPC calls from the GFE to a service, and from service to service.

ALTS uses service accounts for authentication. Each service that runs in Google's infrastructure runs as a service account identity with associated cryptographic credentials. When making or receiving RPCs from other services, a service uses its credentials to authenticate. ALTS verifies these credentials using an internal certificate authority.

Within a physical boundary controlled by or on behalf of Google, ALTS provides both authentication and integrity for RPCs in “authentication and integrity” mode. For traffic over the WAN outside of physical boundaries controlled by or on behalf of Google, ALTS enforces encryption for infrastructure RPC traffic automatically in “authentication, integrity, and privacy” mode. Currently, all traffic to Google services, including Google Cloud services, benefits from these same protections.

ALTS is also used to encapsulate other layer 7 protocols, such as HTTP, in infrastructure RPC mechanisms for traffic moving from the Google Front End to the Application Front End. This protection isolates the application layer and removes any dependency on the network path's security.

Security infrastructure services accept and send ALTS communications only in “authentication, integrity and privacy” mode, even within physical boundaries controlled by or on behalf of Google. One example is [Keystore](https://cloud.google.com/security/encryption/default-encryption?authuser=2#key_management), which stores and manages the encryption keys used to protect data stored at rest in Google's infrastructure.

### Network encryption using PSP

The PSP Security Protocol (PSP) is transport-independent, enables per-connection security, and supports offloading of encryption to smart network interface card (SmartNIC) hardware. Whenever SmartNICs are available, we use PSP to encrypt data in transit across our network.

PSP is designed to meet the requirements of large-scale data-center traffic. We use PSP to encrypt traffic in and between our data centers. PSP supports non-TCP protocols such as UDP and uses an encryption key for each Layer 4 connection.

For more information about how we use PSP, see [Announcing PSP's cryptographic hardware offload at scale is now open source](https://cloud.google.com/blog/products/identity-security/announcing-psp-security-protocol-is-now-open-source?authuser=2).

#### ALTS Protocol

ALTS has a secure handshake protocol similar to mutual TLS. Two services wishing to communicate using ALTS employ this handshake protocol to authenticate and negotiate communication parameters before sending any sensitive information. The protocol is a two-step process:

-   **Step 1:Handshake** The client initiates an elliptic curve-Diffie Hellman (ECDH) handshake with the server using Curve25519. The client and server each have certified ECDH public parameters as part of their certificate, which is used during a Diffie Hellman key exchange. The handshake results in a common traffic key that is available on the client and the server. The peer identities from the certificates are surfaced to the application layer to use in authorization decisions.
-   **Step 2: Record encryption** Using the common traffic key from Step 1, data is transmitted from the client to the server securely. Encryption in ALTS is implemented using BoringSSL and other encryption libraries. Encryption is most commonly AES-128-GCM while integrity is provided by AES-GCM's GMAC.

The following diagram shows the ALTS handshake in detail. In newer implementations, a process helper does the handshake; there are still some cases where this is done directly by the applications.

![Client app interacts with a handshake service through a process helper, and with the server app through a key exchange.](https://cloud.google.com/static/images/security/alts-handshake.png?authuser=2)

_Figure 5: ALTS handshake_

As described at the start of section [Service-to-service authentication, integrity, and encryption](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#service_integrity_encryption), ALTS uses service accounts for authentication, with each service that runs on Google's infrastructure running as a service identity with associated cryptographic credentials. During the ALTS handshake, the process helper accesses the private keys and corresponding certificates that each client-server pair uses in their communications. The private key and corresponding certificate (signed [protocol buffer](https://developers.google.com/protocol-buffers/docs/overview?authuser=2)) have been provisioned for the service account identity of the service.

**ALTS Certificates** There are multiple kinds of ALTS certificate:

-   **Machine certificates:** provide an identity to core services on a specific machine. These are rotated approximately every 6 hours.
-   **User certificates:** provide an end user identity for a Google engineer developing code. These are rotated approximately every 20 hours.
-   **[Borg](https://research.google.com/pubs/pub43438.html?authuser=2) job certificates:** provide an identity to jobs running within Google's infrastructure. These are rotated approximately every 48 hours.

The root certification signing key is stored in Google's internal certificate authority (CA), which is unrelated and independent of our [external CA](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#google_certificate_authority).

#### Encryption in ALTS

Encryption in ALTS can be implemented using a variety of algorithms, depending on the machines that are used. For example, most services use AES-128-GCM[12](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote12). More information on ALTS encryption can be found in Table 2.

Machines

Message encryption used

 

Most common

AES-128-GCM

 

Sandy Bridge or older

[AES-128-VCM](https://research.google.com/pubs/pub46483.html?authuser=2)

Uses a VMAC instead of a GMAC and is slightly more efficient on these older machines.

_Table 2: Encryption in ALTS_

Most Google services use ALTS, or RPC encapsulation that uses ALTS. In cases where ALTS is not used, other protections are employed. For example:

-   Some low-level machine management and bootstrapping services use SSH
-   Some low-level infrastructure logging services TLS or Datagram TLS (DTLS)[13](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#footnote13)
-   Some services that use non-TCP transports use other cryptographic protocols or network level protections when inside physical boundaries controlled by or on behalf of Google

Communications between VMs and Google Cloud Platform services use TLS to communicate with the Google Front End, not ALTS. We describe these communications in [Virtual machine to Google Front End encryption](https://cloud.google.com/docs/security/encryption-in-transit?authuser=2#virtual_machine_to_google_front_end_encryption).

## Configuring other encryption in transit options

You can configure protections for your data when it is in transit between Google Cloud and your data centers, or in transit between your applications that are hosted on Google Cloud and user devices.

If you are connecting your data center to Google Cloud, consider the following:

-   Use TLS with [HTTP(S) Load Balancing or External SSL Proxy Load Balancing](https://cloud.google.com/load-balancing?authuser=2) to connect to your cloud service. GFE terminates the TLS connections from your users using SSL certificates that you provision and control. For more information about customizing your certificate, see [SSL certificates overview](https://cloud.google.com/compute/docs/load-balancing/http/ssl-certificates?authuser=2).
-   Create an IPSec tunnel using [Cloud VPN](https://cloud.google.com/network-connectivity/docs/vpn/concepts/overview?authuser=2) or use [Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/how-to/choose-product?authuser=2#cloud-interconnect) to securely connect your on-premises network to your Virtual Private Cloud network. For more information, see [Choosing a Network Connectivity product](https://cloud.google.com/network-connectivity/docs/how-to/choose-product?authuser=2).

If you are connecting your user devices to applications running in Google Cloud, consider the following:

-   Use GFE's support of TLS by configuring the SSL certificate that you use. For example, you can have the TLS session terminate in your application.
-   Consider our free and automated SSL certificates, that are available for both the [Firebase Hosting](https://firebase.google.com/docs/hosting/custom-domain?authuser=2) and [App Engine](https://cloud.google.com/appengine/docs/standard/python/securing-custom-domains-with-ssl?authuser=2) custom domains. With App Engine custom domains, you can also [provide your own SSL certificates](https://cloud.google.com/appengine/docs/standard/python/securing-custom-domains-with-ssl?authuser=2#using_your_own_ssl_certificates)and use an HTTP Strict Transport Security (HSTS) header.
-   For workloads on GKE and Compute Engine, consider [Anthos service mesh](https://cloud.google.com/service-mesh/docs/security/security-overview?authuser=2) so that you can use mTLS for authentication, which ensures that all TCP communications are encrypted in transit.

If you are using Google Workspace, [configure Gmail](https://support.google.com/a/answer/6374496?authuser=2) to enable S/MIME for outgoing emails, [set up policies](https://support.google.com/a/answer/7280976?authuser=2) for content and attachment compliance, and create routing rules for incoming and outgoing emails.

## Research and innovation in encryption in transit

Over the years, we have been involved in several open-source projects and other efforts that encourage the use of encryption in transit on the internet.

These efforts include:

-   [Certificate Transparency (CT)](https://www.certificate-transparency.org/) is an effort that we launched to provide a way for site operators and domain holders to detect if a CA has issued any unauthorized or incorrect certificates.
-   Our annual [HTTPS Transparency Report](https://transparencyreport.google.com/https/overview?authuser=2) tracks our progress towards our goal of 100% encryption in transit for all our properties, including Google Cloud.
-   The development of the [combined elliptic-curve and post-quantum (CECPQ2) algorithm](https://www.chromium.org/cecpq2/), which helps protect TLS connections against quantum computer attacks.

For more information about our recent contributions, see [Collaboration with the security research community](https://cloud.google.com/docs/security/overview/whitepaper?authuser=2#collaboration_with_the_security_research_community).

## What's next

-   For general information on Google Cloud security, including security best practices, see the [Security section of the Google Cloud website](https://cloud.google.com/security?authuser=2).
-   For information on Google Cloud compliance and compliance certifications, see the [Compliance section of the Google Cloud website](https://cloud.google.com/security/compliance?authuser=2), which includes Google's [public SOC3 audit report](https://www.google.com/work/soc3.html?authuser=2).
-   For best practices on how to secure your data in transit, see the [Security Foundations Blueprint](https://cloud.google.com/architecture/security-foundations?authuser=2) and [Google Cloud Architecture Framework: Security, privacy, and compliance](https://cloud.google.com/architecture/framework/security?authuser=2), and [Decide how to meet regulatory requirements for encryption in transit](https://cloud.google.com/architecture/landing-zones/decide-security?authuser=2#encrypt-transit).

## Footnotes

1 Partner solutions include both solutions offered in Cloud Launcher, as well as products built in collaboration with partners, such as Cloud Dataprep.

2 You can still disable this encryption, for example for HTTP access to Google Cloud Storage buckets.

3 VM-to-Service communications not protected at Layer 7 are still protected at layers 3 and 4.

4 Google supports TLS 1.0 for browsers that still use this version of the protocol. Note that any Google site processing credit card information will no longer support TLS 1.0 by July 2018 when Payment Card Industry (PCI) compliance requires its deprecation.

5 For details on QUIC, see [https://www.chromium.org/quic](https://www.chromium.org/quic).

6, 7, 8 For backwards compatibility with some legacy operating systems, we support 3DES, SHA1 and MD5.

9 In the case of chained certificates, the CA is transitively trusted.

10 This could be either a session ticket [RFC 5077](https://tools.ietf.org/html/rfc5077) or a session ID [RFC 5246](https://tools.ietf.org/html/rfc5246).

11 The control plane is the part of the network that carries signalling traffic and is responsible for routing.

12 Previously, other protocols were used but are now deprecated. Less than 1% of jobs use these older protocols.

13 Datagram TLS (DTLS) provides security for datagram-based applications by allowing them to communicate in a way that prevents eavesdropping and tampering.