# Open Redirect vulnerability (CVE-2024-39694) 

It is possible for an attacker to craft malicious Urls that certain functions in IdentityServer will incorrectly treat as local and trusted. If such a Url is returned as a redirect, some browsers will follow it to a third-party, untrusted site. 

Note: by itself, this vulnerability does not allow an attacker to obtain user credentials, authorization codes, access tokens, refresh tokens, or identity tokens. An attacker could however exploit this vulnerability as part of a phishing attack designed to steal user credentials. 

Duende.IdentityServer 5.1 and earlier and all versions of IdentityServer4 are no longer supported and will not be receiving updates. 

If upgrading is not possible, use IUrlHelper.IsLocalUrl from ASP.NET Core 5.0 or later to validate return Urls in user interface code in the IdentityServer host.

## What is OpenID Connect

OpenID Connect is an interoperable authentication protocol based on the OAuth 2.0 framework of specifications (IETF RFC 6749 and 6750). It simplifies the way to verify the identity of users based on the authentication performed by an Authorization Server and to obtain user profile information in an interoperable and REST-like manner.

OpenID Connect enables application and website developers to launch sign-in flows and receive verifiable assertions about users across Web-based, mobile, and JavaScript clients. And the specification suite is extensible to support a range of optional features such as encryption of identity data, discovery of OpenID Providers, and session logout.


How OpenID Connect Works

1. End user navigates to a website or web application via a browser.
2. End user clicks sign-in and types their username and password.
3. The RP (Relying Party / Client Side) sends a request to the OpenID Provider (OP).
4. The OP authenticates the User and obtains authorization.
5. The OP responds with an Identity Token and usually an Access Token.
6. The RP can send a request with the Access Token to the User device.
7. The UserInfo Endpoint returns Claims about the End-User.

An OpenID Provider (OP) is an entity that has implemented the OpenID Connect and OAuth 2.0 protocols, OP’s can sometimes be referred to by the role it plays, such as: a security token service, an identity provider (IDP), or an authorization server.

RP stands for Relying Party, an application or website that outsources its user authentication function to an IDP.

OAuth 2.0, is a framework, specified by the IETF in RFCs 6749 and 6750 (published in 2012) designed to support the development of authentication and authorization protocols. It provides a variety of standardized message flows based on JSON and HTTP; OpenID Connect uses these to provide Identity services.

OpenID Connect has many architectural similarities to OpenID 2.0, and in fact the protocols solve a very similar set of problems. However, OpenID 2.0 used XML and a custom message signature scheme that in practice sometimes proved difficult for developers to get right, with the effect that OpenID 2.0 implementations would sometimes mysteriously refuse to interoperate. OAuth 2.0, the substrate for OpenID Connect, outsources the necessary encryption to the Web’s built-in TLS (also called HTTPS or SSL) infrastructure, which is universally implemented on both client and server platforms. OpenID Connect uses standard JSON Web Token (JWT) data structures when signatures are required. This makes OpenID Connect dramatically easier for developers to implement, and in practice has resulted in much better interoperability.

The FIDO Alliance is one organization in which non-password authentication technologies are being explored. Some OpenID Foundation members are also members of the FIDO Alliance, working on authentication technologies there that can be used by OpenID Providers.

The Security Assertion Markup Language (SAML) is an XML-based federation technology used in some enterprise and academic use cases. OpenID Connect can satisfy these same use cases but with a simpler, JSON/REST based protocol. OpenID Connect was designed to also support native apps and mobile applications, whereas SAML was designed only for Web-based applications. SAML and OpenID Connect will likely coexist for quite some time, with each being deployed in situations where they make sense.

OpenID Connect uses the JSON Web Token (JWT) and JSON Object Signing and Encryption (JOSE) specifications.

# References

* https://openid.net/developers/how-connect-works/
* https://github.com/onelogin/openid-connect-dotnet-core-sample
* https://github.com/microsoft/WSL/releases 
* https://github.com/IdentityServer/IdentityServer4 
* https://github.com/IdentityModel 
* https://github.com/openiddict/openiddict-core 
* https://github.com/onelogin/openid-connect-dotnet-core-sample 
* https://github.com/aspnet/AspNetKatana 
* https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/pushd 