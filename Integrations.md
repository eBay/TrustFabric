# Integrations

TrustFabric is based on industry standard specifications like OAuth2, OIDC, JWT etc.. On the Client-side, applications can integrate with TrustFabric seamlessly without much changes to their code. On Server-side (as PEP and PDP), TrustFabric integrations can be done at various levels. It could be as simple as evaluating a standard JWS/JWE tokens, or a more advanced integrations which can provide fine grained authorization decisions including support for direct and delegated authorizations.

TrustFabric is designed to be integrated with a variety of use cases and applications in a cloud environment. The level and type of integration can vary based on the use case. The below sections provide an high level recommendation on some of the following common use cases:

1. Integration for web-applications (HTTPS)
2. Integration for gRPC applications
3. Integration for Generic applications supporting SASL
4. Integration for Service Mesh

**Roadmap** : Specification will cover the specifics in the next iteration.

## HTTPS Integration

### Direct application Integration (SDK)

Applications that have the flexibility to change their code can directly use TrustFabric based SDK to obtain Session layer token or Fabric layer token for connecting to other services. 

TrustFabric based SDK can also be used to authenticate and authorize incoming requests containing TrustFabric identities. 

### Envoy Integration

Applications that cannot be modified may use Envoy as a sidecar proxy to provide TrustFabric based authentication and authorization.

Envoy as a forward proxy can be used to inject TrustFabric tokens to all the outbound requests originating from the applications. 

Envoy as a reverse proxy can be used to authenticate and authorize inbound requests containing TrustFabric tokens.  

Both the approaches can be implemented as Envoy extensions or a combination of extensions along with the Envoy discovery services. 

## gRPC Based Integration

TrustFabric tokens can be used as a Token-based Authentication mechanism in applications using gRPC. 

## SASL Integration

SASL enables schemes defined within the OAuth2 framework for non-HTTP based application protocols. TrustFabric can be used as an authorization server for providing OAuth2 Bearer tokens for SASL based applications.  

Refer [RFC 7628](https://tools.ietf.org/html/rfc7628) for more details.

## Service Mesh Integration

### Istio Integration

TrustFabric tokens can provide a strong identity which can be used as a service identity or end user identity. TrustFabric tokens can be used as a replacement for Istio's certificate based identity or it can coexist.   

**Peer Authentication and Authorization:** 

TrustFabric tokens can provide each service a strong identity which can be used to identify and authorize each service connecting to other services. 

**User Authentication and Authorization:**

TrustFabric tokens obtained via OIDC flow can provide a strong user identity which can be used to identify and authorize users connecting to the services in the service mesh.


