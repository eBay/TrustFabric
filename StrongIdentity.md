# Strong Identity: Protections and extensibility

## Goals

Important goal of TrustFabric specification is a strong identity that has following characteristics:

1. Non-repudiation
2. Integrity Protection
3. Confidentiality
4. Man-in-the-middle, confused-deputy and replay protection
5. Password-less

## Protections

### Non-repudiation

Non-repudiation is the "no dispute assurance" for the legitimate presenter of the identity. TrustFabric includes following controls for the assurance:

1. Standards compliant JWT signature verification mechanisms based on [RFC 8725](https://tools.ietf.org/html/rfc8725)
2. Key attestation using X.509 certificate
3. Binding `x5u` or `jku` claims based on issues (`iss`) claim
4. Standardization of crypto standards for [signing](./Tokens.md#recommended-digital-signature-algorithms) and [prohibitions](./Tokens.md#prohibited)

### Integrity Protection

Integrity Protection is ensured by using asymmetric encryption scheme for signing the tokens and [prohibiting](./Tokens.md#prohibited) non-compliant algorithms/mechanisms (`alg`).

### Confidentiality

Specification meets the confidentiality requirement by enforcing the TLS 1.2 (or higher) mechanism for encrypting the communication over the network.

### Man-in-the-middle protection

Man-in-the-middle protection differ for session and fabric layer and are covered separately below.

Session (user) layer requires user identity delegation to be tied to authorized party(client/relying-party application) via `azp` claims. Any scenario where resource-server needs user delegation are captured via authorized forwarder (`azf`) claims. With session level `access token` with `azp` and `azf` claims and source service sending the fabric layer `access token` that establishes the identity as referenced in `azp` or `azf` claims, confused-deputy, man-in-the middle (via token stealing or replay) and replay attacks are avoided for session layer.

Session (user) layer relies on the fabric layer for avoiding these types of attacks, hence it is important to build man-in-the-middle, replay and and impersonation protections in the fabric (application) layer tokens.

#### Fabric layer IP based protection

TrustFabric includes instance claims (`inst`) as part of fabric (application) layer tokens. Instance (`inst`) claim is an array representing following:

1. Computes with network routable IP addresses: `inst` claim contains IP address(es) of the instances who can use the token. Typically instances of the application.
2. Non-routable (typically NAT'ed) IP Address: `inst` claim contains IP address of NAT layer (e.g. Node host IP or Cluster IP that is doing NAT). Additional controls may be required to avoid misuse due to NAT (e.g. Hardware root of trust attestations etc)

### Password-less

Session layer requires `client_id` and `client_secret` for client identification. The fabric layer eliminates the passwords by using tokens that are valid for a short duration, eliminating the need for client secret generation and securing them centrally.

In addition, TrustFabric leverages the Vectors Of Trust as specified in [RFC 8485](https://tools.ietf.org/html/rfc8485) and associated claims `vot` and a`vom`. Purpose of these claims is to use Systems of Records and Sources of Truth (e.g. CMDB or API Servers) to verify and validate application deployment and application behaviour before attestation.

#### Local Trusted attestation (e.g. TPM or Hardware root of trust)

Subsequent versions of this specification will provide details for hardware root of trust based attestation.

## Extensibility

Implementations of TrustFabric can extend and adopt additional security controls using custom claims and validation hooks.
