# Trusted Cloud Native Application Fabric

TrustFabric implements layered approach for implementing secure fabric of application interactions. 

Trusted Underlay
================
Application identities are established first using the bootstrapping and token negotiation. Once identity is established, these identities are used to establish connectivity to allowed services over TLS based on policies. This establishes a secure underlay interaction pattern for applications. As new services interactions are added or removed access patterns will change.



Trusted Overlay
===============
Users interact with applications and these applications may have to interact with other services on-behalf-of the user. These interactions help avoid a classic security concern with microservices i.e. "Confused deputy". TrustFabric allows `obo` (On Behalf Of) claims and `azp` (authorized presenter) claims to allow applications to interact with other applications with user's identity.

![](./media/TrustFabric-Fabric-Overlay.png)