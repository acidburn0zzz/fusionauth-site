---
layout: advice
title: What is OAuth?
description: What is OAuth and when should you use it?
image: advice/what-is-oauth/expert-advice-what-is-oauth-and-why-should-you-use-it.png
author: Ahmed Hashesh and Dan Moore
category: OAuth
date: 2022-01-27
dateModified: 2022-01-27
---

Open Authorization (OAuth) is an open, standardized protocol for internet token-based authorization. The current version, OAuth 2.0, allows services, like Facebook and Google or your own, to manage access to an end user's account information without disclosing the user's credentials.  

First, an authorization flow is used to authenticate and authorize a third-party service. After that, an access token is generated and shared with the third-party service which allows specific information to be accessed. Passwords are never shared because OAuth lets you authorize an application to communicate with another on your behalf.

Instead of passing authentication data between customers and service providers, an OAuth token is provided. Therefore, developers get access to end-user data in a more secure manner. In this article, you’ll learn how OAuth works, how versions 1 and 2 differ, and why you should use OAuth instead of your own authorization solution. 

## Why OAuth

OAuth in general was designed as a reaction to the [direct authentication pattern](http://guides.brucejmack.net/SOA-Patterns/WSSP/1.3DirectvsBrokeredAuthDoc.htm), which features applications requesting usernames and passwords when accessing user-specific data at other services. When utilizing OAuth 2.0, the user still provides this information, but they log into one platform and use tokens generated by that platform to grant access to data and perform actions in one or more other applications.

Before authentication and authorization with OAuth was available, websites would ask users to enter a username and password into a form, for example the credentials for a Gmail account, to gain access to Gmail data. Users realized they didn’t want to grant full access to any old service. They preferred limited access instead. A user might want a third party service to access their email account, but only have rights to read contacts, not send email or add contacts.

This article will focus on OAuth 2.0, the second version of OAuth, though below we do cover differences between the versions.

When using OAuth 2.0, a token, which has a limited lifetime, provides that desired limited access. Connecting multiple applications is easier for users. It is also more secure since user credentials are not shared. Auth is also easier for developers to manage because they only need to integrate OAuth 2.0 in their application instead of having their own database to store users' sensitive information.

Often there is confusion between OAuth and SAML. SAML is primarily an authentication system, while OAuth is an authorization system. Authentication is about confirming the user’s identity. Authorization is about deciding what services, functionality or data they can access. A detailed comparison is [found here](/learn/expert-advice/oauth/saml-vs-oauth) . 


## How OAuth 2.0 Works

Usually, OAuth 2.0 is involved when a user (also called the Resource Owner) is providing a third-party application (called a client application) access to data in a different service (called the Resource Server) without sharing their credentials. Let’s make it more concrete with an example.

If you wanted to print images from your Pinterest account without downloading the images and uploading them again to a printing website, and without sharing your Pinterest username and password with the printing website, you could authorize the printing website to have read-only access to your Pinterest photos.

You can do this using OAuth 2.0. Here's a diagram of the Authorization Code grant. (This is also sometimes called the Authorization Code flow.)

{% plantuml source: _diagrams/learn/expert-advice/oauth/what-is-oauth-authorization-code-grant.plantuml, alt: "Example of the Authorization Code grant" %}

A few bits of jargon:
* The Pinterest Login Service is the Authorization Server (what authenticates the user).
* The Pinterest Photo Service is the Resource Server (what holds the protected data).
* The Printing Website is the Client (what wants access to the protected data).
* The Browser represents the Resource Owner (who can grant access to the protected data).

Here are the steps:

1. The Printing Website (the Client and a third party app, unconnected to Pinterest) redirects the user to the Pinterest Photo Service (which is the Resource Server, as it has access to the images the user is trying to print). This is the authorization request.
2. The user logs in (since Pinterest needs to know who they are) to the Pinterest Login Service (also called an Authorization Server).
3. As part of the login process, the Authorization Server asks the user to give access to a third-party application. 
4. If the user agrees and grants permissions the printing website asks for, the Authorization Server endpoint sends an authorization code to the Printing Website to an endpoint. This endpoint is often called a redirect URI or a callback URL.
5. The Printing Website uses this authorization code, and credentials previously issued by Pinterest, which are tied to the application, to get an access token from the Authorization Server. The Authorization Server is presented with both the authorization code, which represents the choices the user made, and the application credentials. It verifies both of these and generates an access token. This is the token request.
6. The Printing Website receives the access token and stores it safely.
7. The Printing Website sends the access token to the Pinterest Resource Server with a request for the images.
8. The images are returned.

In these steps, the user never shares their credentials with the third-party app. Instead the app gains access based on interactions with the Authorization Server, but that Authorization Server is what confirms the user's identity. 

OAuth 2.0 isn't the first authentication/authorization mechanism to act on behalf of the user. Many authentication systems, including [Kerberos](https://web.mit.edu/kerberos/), operate in the same way. However OAuth 2.0 is unique because its delegated authorization framework is the first to be widely accepted and to function across the web.

### Additional OAuth 2.0 Concepts

The authorization process consists of several parties, including the Resource Owner, the Resource Server, the Authorization Server, and the Client. As mentioned before, the third-party application obtains tokens from the Authorization Server and uses these tokens to access the resources held by the Resource Server. Like any other technical topic, there are common concepts and jargon worth knowing. These include: 

* Scopes
* Grants
* Access Tokens
* Client Ids

Let’s take a look at these concepts. 

#### Scope

A scope is a method of restricting access. Remember in the example above, when the user only wanted to allow access to Gmail contacts and not the ability to send emails? Scopes can help with this.

Instead of giving applications full access to a user's account, it enables apps to request a limited, well, scope of what they can do on the user's behalf. For example, some apps use OAuth2 to identify users and therefore only require a user Id and basic profile information. Other applications may require access to more sensitive data, like the user's birthdate or the ability to post data on the user's behalf. Scopes can represent these different levels of access. They are presented at the initial request of the Client, and then parsed and displayed to the user at the Authorization Server.

Users are more likely to allow an app to gain access to personal data if they understand what exactly the app can and can’t do. Scopes allow them to make informed decisions about what they consent to share with any third-party application.

#### Grants

Grants are authentication flows for obtaining access tokens from the Authorization Server. The grant encapsulates a process, data flow and rules used to generate a token. The core OAuth2 grants (as outlined in [RFC 6749](https://tools.ietf.org/html/rfc6749)) are:

* **Authorization Code Grant:** The fundamental attribute of this grant is the one time authorization code generated from the Authorization Server after authenticating the Resource Owner. This is exchanged for the token using server side code. 
* **Implicit Grant:** A simplified flow used by browser based applications implemented with JavaScript. This is a legacy grant. [Don’t use this grant](/blog/2021/04/29/whats-wrong-with-implicit-grant/).
* **Resource Owner Credentials:** This grant is helpful when the username and password are required for authorization. Also called the Password grant. It should only be used if there is a high level of trust between the Resource Owner and the third-party application or for migrating from legacy systems to OAuth2 based systems.
* **Client Credentials:** This is the right grant when the application is trying to act on behalf of itself without the user’s presence. An example is the Printing Website calling into an Invoice Generation service to create invoices for the prints. This is not done for any user, but instead for the Printing Website itself.

You can [learn more about various grants](/learn/expert-advice/oauth/complete-list-oauth-grants/). 

#### Access Tokens

Programs use access tokens to make requests on a user's behalf. As mentioned above, the access token denotes a particular application's permission to access certain elements of a user's data.

Access tokens are, per the specification, opaque to the Client. While some Authorization Servers generate access tokens that have internal structure, such as JSON Web Tokens (JWTs), others do not.

Access tokens must be kept private, both in transit and at rest. Passing the token through non-encrypted channels makes it easier for replay attacks which is why it’s recommended for OAuth 2.0 flows to always use TLS.

#### Client Ids

OAuth 2.0 typically requires static, out of band initial configuration. For example, before an application can call the GMail API to retrieve contact information on behalf of a user, it must first receive approval from Google. This process is called "Client Registration" and can be done manually or, in certain circumstances, programmatically.

During Client registration, the third party application provides information like the client type, a redirect URL where the authorization code can be sent, and other related information including a website and description. The Authorization Server then generates a client Id and a client secret.

Some clients can safely keep secret values such as the client secret, and are known confidential clients. Others, such as JavaScript Single Page Applications (SPAs) cannot, because their source code can be examined for secrets. These are known as public clients.

### Benefits and Features of OAuth2

As a developer, advantages of integrating OAuth2 into your application include:

* Allowing you to read data about a user from another application or service without handling user credentials.
* Managing the authorization process for online, desktop, and mobile apps in one standardized manner rather than creating a separate process for each type of application.
* Providing consumers with more control over their data by using scopes to authorize access to certain capabilities on a case-by-case basis.
* Isolating all authentication processing, allowing for additional security methods such as MFA to be implemented without affecting any application.

## How OAuth 2.0 Differs from OAuth 1.0

OAuth 2.0 is the second version of OAuth, and is incompatible with the first version. OAuth 2.0 is a complete rewrite of the protocol, which made the two versions suitable for different needs. OAuth1 is rarely seen in the wild, apart from [Twitter's API](https://developer.twitter.com/en/docs/authentication/overview).

OAuth1 was written based on Flickr’s authorization API and Google’s AuthSub. However, challenges arose and paved the way for another version. The developers of the second version wanted to address the following issues present in OAuth1:

* Poor support for the non-browser based applications.
* No separation between the servers performing user authorization and servers that handle the OAuth requests.
* Complicated signatures and the need to have a solid cryptography background and key management skills to handle an OAuth integration.

This led to significant differences between the two versions, including:

* OAuth2 support for six grants compared to three in the first version.
* Signatures are less complicated in OAuth2.
* Access tokens have a short life in OAuth2. This causes some issues mitigated by the concept of [refresh tokens](https://datatracker.ietf.org/doc/html/rfc6749#page-47).
* The terminology differs.

OAuth2 has a different security profile than OAuth1 because it’s more transport dependent, flexible, and less prescriptive. The developers of OAuth2 focused on making it interoperable and versatile across sites and devices. They also included token expiration. Many of the original creators and supporters of OAuth1, regardless of their intentions, refused to accept version 2 because:

* The second version is incompatible. The developers intended for OAuth2 to replace OAuth1 everywhere. 
* OAuth2 doesn’t explicitly specify or enable encryption, signatures, client verification, or channel binding (tying a particular session or transaction to another particular client and server). Instead, it requires implementers to deliver those functionalities via external security protocols like TLS. In OAuth 1.0 each message was individually signed.
* OAuth2 uses bearer tokens, which makes it easier to integrate but less secure. There are efforts to introduce proof of possession/token binding to OAuth2, such as MTLS and DPoP.

### OAuth 2.0 Best Practices

According to the [OAuth2 Security Best Current Practice](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics), the Implicit Grant is not recommended as it is vulnerable to access token leaks. Many individuals have attempted to abuse OAuth2 across applications, so it’s important to follow basic web security rules as well.

For example, to maintain the integrity of any OAuth2 grant:

* Always use a [CSRF (Cross-Site Request Forgery)](https://owasp.org/www-community/attacks/csrf) token in the state parameter.
* To avoid a hijack of the authorization code, always whitelist redirect URLs.
* Always use TLS for all OAuth2 requests.
* Secrets should never be exposed. For example, don’t include a client secret in a mobile application. 
* Store all access tokens securely, since they are bearer tokens and if stolen, can be misused.

## OAuth 2.0 is Not an Authentication Protocol

OAuth2 is an authorization framework, not an authentication protocol. Your authorization policy decisions are decoupled from authentication using OAuth2.

Even though authentication is critical to every grant, the access token itself doesn't necessarily contain information about a specific user, just about what the holder of the token can access.

You can use [OpenID Connect](https://openid.net/connect/) to extend OAuth2 for authentication scenarios, where you need to know who is requesting access, rather than whether an entity has access to a protected resource. Most OAuth2 servers support OIDC, and it supports many of the same grants with additional scopes. 

## Benefits of Using OAuth 2.0 Instead of Your Own Solution

OAuth2 provides you with technical advantages such as a central choke point to manage, limit and control access to services.
With OAuth2, you have the benefits of a standard as well. This includes a team of experts working to push the standard forward in terms of functionality and security, a wide set of libraries that work with the standard, and a population of developers who understand the protocol. Compare that to a homegrown authorization protocol, which has none of these advantages.

In addition, implementing your own solution may be more time-consuming than buying one. You need resources and knowledge to implement and maintain your custom solution.

## Conclusion

OAuth2 is a delegated access authorization system designed for services and users accessing private resources.

In this article, you looked at what OAuth2 is, key concepts, an example grant, what problems OAuth2 solves, how it contrasts with OAuth1, and why you might use it rather than creating your own authentication solution.

## Additional Resources

For practical steps to implement OAuth 2.0 with different Javascript frameworks, see these tutorials: 
* [OAuth and Angular] (https://fusionauth.io/blog/2020/03/31/how-to-securely-implement-oauth-angular )
* [OAuth and React] (https://fusionauth.io/blog/2021/11/11/how-to-authenticate-your-react-app)
* [OAuth and VueJS] (https://fusionauth.io/blog/2020/08/06/securely-implement-oauth-vuejs)
