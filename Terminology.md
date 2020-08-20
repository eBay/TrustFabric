# Terminology

* **Application** : Application represents a computer program that interact over network and perform a set of designed functions.

    * <u>Applications vs Services</u> : Applications mostly allow interactive access with a human actor, but may also provide non-interactive actions and interactions with other services.  Jacob Jenkov provided a good guideline for differentiating applications and services [here](http://tutorials.jenkov.com/soa/services-applications.html).

    * <u>Note</u>: Although the term application and service are used interchangeably in this document, TrustFabric uses *Application* as a standard term.  

* **Operators** : Operators are applications build to automate actions of a human operator. Typical example includes a mysql operator, that may perform lifecycle management aspects of a mysql database, just like a DBA.


* **Jobs** : Jobs are applications that are invoked by a pre-defined schedule or events.

* **Resource Owner**: The end user for whom we are requesting identity information. It is the user who performs authentication and provides the consent to client/relying-party to access resources in resource-server. One of the resources they own is their own identity.

* **User** : Same as **Resource Owner** above.

* **Relying Party** :  The client application that supports OAuth 2.0 and relies on the OpenID provider to authenticate the end user and request claims about that user. Client (a.k.a relying party) obtains the authorization from the *resource owner*, which is identified using the `authorized presenter` claim in OIDC.

* **Client** : Same as **Relying Party**.

* **Resource Server** : Resource server is API server that can be used to access the user's information or user's resources. It has the capability of accepting and responding to protected resource requests with the help of access tokens. In TrustFabric implementation, *resource server* may also act as *relying party* in a multi-step transaction where resource-server may obtain the delegation grants based on `authorized forwarder` claim.

* **Authorization Server** : The OAuth2/OIDC authorization server is what the user interacts with when an application is requesting access to their account/resources. In OIDC specification, authorization server also serves as Authentication server. Authorization server provides the access token required by the **client** to connect to the **resource server**. Authorization server implements `/token` and `/authorize` end-points. For OIDC authorization server also implements `/userinfo` endpoint.

* **OIDC Provider (OP)** : Same as **Authorization Server** specifically in context of OIDC.

* **Scope** : Scopes are identifiers for resources that a client wants to access. Scopes are identifiers for 'limiting access' to the resources. This identifier is sent to the OIDC Provider (OP) during an authentication or token request.

* **Authorized Presenter** : Authorized Presenter claims `azp` comes from OIDC spec and is actively used in both identity and access tokens by multiple OIDC providers. The `azp` claim contains the *relying-party* identity and it adds authenticity to the end-user *access token* presented by the *relying-party* to the resource server.

* **Authorized Forwarder** : TrustFabric introduces the authorized forwarder claim `azf`. Purpose of the claim is to allow *resource servers* to present resource owner's *access token* to upstream dependencies, these dependencies could be *resource-servers* and need user context to allow access. This is more prevailing use-case in micro-services as monolith *resource service* may be broken into multiple *resource services* with dependencies and requiring resource-owner context. The `azp` claim has to be initiated based on a consent provided by resource-owner during authentication or by any offline mechanism (e.g. Policy allowing delegation).


* **Application Fabric Layer** : Virtual representation of a trust established between applications for interactions based on respective identities and audience claims (`aud`) for access.

* **Session Layer** : Represents sessions established by the resource-owner with the applications. Session layer requires application fabric as building block for trusting applications. Session layer is implemented by adding additional claims (e.g. `azf`) and flows to OIDC spec.

* **Authorization Code** : OAuth2 and OIDC present the concept of authorization code grant for *relying-party* to obtain *access tokens* using a code provided by *resource owner* for authorization. Authorization code represents resource-owner's authenticated consent to authorize relying party. TrustFabric distinguishes the code grant for generation of application identities (a.k.a. *Bootstrap code*) as different from resource-owner initiated code-grant (a.k.a. *Authorization Code*). 
    *   *Authorization code* : Represents *resource-owner* identity and follows OIDC semantics. These are used in session layer of TrustFabric.
    *   *Bootstrap code* : Represents application identity (including relying-party, resource-servers, jobs, etc) in a bootstrap token (JWT), which can be provided by a authorized application (typically infrastructure systems). This is used in the application fabric layer of TrustFabric.

* **Access Token** : Access tokens are bearer token presented by application or relying-party to gain access to resource-server.
    *   *Application Access Tokens* : Represents the self identity (application). The `sub` claim represents self identity and `aud` claim represents target applications this subject can interact with.
    *   *User Access Tokens* : These are access token given to relying party by the user as an authorization. The `sub` claim represents the `resource owner`, while `azp` claim represents the relying-party/client identity. Similarly `azf` claim represents the applications that can present the access token on behalf of the user.

* **Refresh Token** : Token that can be used by client or application to refresh the access token. Just like access token, there are two kinds of refresh tokens:
    *   *Application Refresh Token* : Refresh token for application identity
    *   *User refresh token* : Refresh token for user access token
