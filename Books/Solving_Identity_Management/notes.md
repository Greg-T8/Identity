# Solving Identity Management in Modern Applications

<img src='images/1752571728465.png' width="250" />

<details>
<summary>Book Resources</summary>

  
</details>

<!-- omit in toc -->
## Contents

- [1. The Hydra of Modern Identity](#1-the-hydra-of-modern-identity)
  - [Identity Challenges](#identity-challenges)
- [2. The Life of an Identity](#2-the-life-of-an-identity)
  - [Events in the Life of an Identity](#events-in-the-life-of-an-identity)
  - [3. Evolution of Identity](#3-evolution-of-identity)
    - [Identity Management Approaches](#identity-management-approaches)
      - [Per-Application Identity Silo](#per-application-identity-silo)
      - [Centralized User Repository](#centralized-user-repository)
      - [Early SSO Servers](#early-sso-servers)
      - [Federated Identity and SAML 2](#federated-identity-and-saml-2)
      - [WS-Fed](#ws-fed)
      - [OpenID](#openid)
      - [OAuth 2](#oauth-2)
      - [OpenID Connect (OIDC)](#openid-connect-oidc)
      - [OAuth 2.1](#oauth-21)


## 1. The Hydra of Modern Identity

> Wisdom is not a product of schooling but of the lifelong attempt to acquire it. &mdash; Albert Einstein

Over time, teams maintaining an in-house identity service start to feel like they are fighting a hydra &mdash; the mythical beast from Greek mythology with nine heads. When one of her heads was cut off, two more would grow back in its place. The more you try to fix the problem, the worse it gets.

### Identity Challenges

There isn't a master solution to identity management that fits every case. 

Decisions when developing applications:
- Who are your users? And will they authenticate?
- Level of authentication strength?
- How do you provide simple but secure access?  SSO provides convenience, but can also be a single point of failure.
- Will you be migrating from legacy applications?
- Regulatory requirements?
- User experience when onboarding?

This book covers three popular identity protocols: OAuth 2, OIDC (OpenID Connect), and SAML 2.

## 2. The Life of an Identity

**Terminology:**  
- Identifier: refers to a single attribute whose purpose is to uniquely identify a person or entity, within a specific context.
- Identity: a collection of attributes associated with a specific person or entity in a specific context.

An identity includes one or more identifiers, e.g. name, age, address, IP address, model, version number.

A person may have more than one identity, e.g. a person may have a work identity and a personal identity.

An account is a local construct that is used to perform actions on an identity associated with specific context.

An identity management (idM) system is a set of services that support the creation, modification, and removal of identities and associated accounts, including the authentication and authorization required to access resources.

### Events in the Life of an Identity

<img src="images/1752573516381.png" width="350" />

### 3. Evolution of Identity

#### Identity Management Approaches

##### Per-Application Identity Silo

Each application implements its own identity repository, authentication, authorization, and access control. This approach is still used in many scenarios where a user signs up for an application-specific username and password.

##### Centralized User Repository

Includes directory services. Enables applications hosted around the world to leverage the same identity information.

Disadvantages:
- Directory does not maintain user session state.
- Each application still had to collect the username and password, exposing the user's password to each application.
- A compromise at one application might put other applications at risk.

##### Early SSO Servers

Early SSO servers leveraged the directory service but provided a layer on top of it to maintain sessions to remember users that had already authenticated.

An application would redirect the user to an SSO server to have the user authenticated there, and the application would receive the authentication results in a secure predetermined fashion.

If a user accessed a second application shortly after they authenticated for the first application, the SSO server would recognize that the user had already authenticated and would not require them to log in again.

Benefits:
- Users were able to log in once and access multiple applications.
- User's password was only sent to one place, the SSO server.
- Single place to implemnt authentication policy and stronger authentication mechanisms.

Drawbacks:
- The interaction between applications and SSO servers was somewhat proprietary, leading to vendor lock-in.
- SSO products were time-consuming to implement and maintain.
- SSO relied on cookies, which due to browser restrictions, meant the solutions worked within one Internet domain.

##### Federated Identity and SAML 2

The explosion of SaaS applications created challenges for managing identities, as users were able to sign up for an account without the IT department's knowledge. 

The SAML 2 (Security Assertion Markup Language) standard, published in 2005, provided a solution for web single sign-on (SSO) across domains and federated identity. 

With SAML 2, SaaS applications could redirect corporate users back to a corporate authentication service, known as an identity provider (IdP), for authentication.

Identity federation provided a way to link an identity used in an application with an identity at the service provider.

Companies could now have the advantages of single sign-on with both internal and SaaS applications.

However, SAML 2 was no silver bullet. 
- It was complex to configure and implement.
- There was no viable business model to address consumer-facing scenarios, as users were unlikely to pay money for a consumer-facing identity service.
- It only solved the authentication problem, not the authorization problem.

At the time, applications were evolving to architectures based on REST APIs, and SAML 2 was not well suited for this.

##### WS-Fed

The Web Services Federation Language (WS-Fed) was created as part of a larger set of protocols known as WS-* specifications.

The WS-Fed 1.2 specification, published in 2009, provided mechanisms to authorize access to managed resources for identities managed in other realms.

Intially, ADFS only supported WS-Fed. ADFS introduced SAML 2 support in version 2.0, released in 2010.

##### OpenID

With SAML 2 only adopted in employee-facing scenarios, consumers were still forced to register for accounts with each application they used.

A new industry group gave rise to a protocol called OpenID, which included the idea of a user-controlled identity solution for the consumer use case.

The original OpenID protocol was never widely used, but it did highlight the need for user-centric identity solutions and laid the groundwork for OpenID Connect (OIDC).

##### OAuth 2

With the rise of social media, many consumer-facing websites were created that allowed users to upload content such as pictures.

This gave rise to use cases where applications needed to retrieve content on the user's behalf. For example, a person who uploaded photos to a social media site might want to enable another website that printed photos to access their photos at the social media site.

In the absence of a better solution, the user would have to provide their username and password to the photo printing site, which would then use that information to access the social media site.

The OAuth 2 protocol addressed this by allowing a user to authorize a third-party application to access their resources without sharing their credentials.

The OAuth 2 version allows a user to authorize one application, known as a client (the photo printing site), to send a request to an API, known as a resource server (the social media site), on the user's behalf to retrieve data at the resource server owned by the user.

To accommodate this, the application interacts with an authorization server which authenticates a user as part of obtaining their consent for the application to access their resources.

The application receives a token, whcih enables it to call the reosurce server on the user's behalf.

By this time, Google and LinkedIn implemented OAuth 2, enabling consumer-facing applications to use OAuth 2 to access user data at these sites.

There was one problem, though...

OAuth 2 was not designed as a general authentication service and could not securely be used for this purpose, so another solution was needed.

##### OpenID Connect (OIDC)

OpenID Connect (OIDC) was designed to provide a key feature needed for an authentication service: the ability to verify the identity of a user based on the authentication performed by an authorization server.

OIDC was devised as a layer on top of OAuth 2 to provide information in a standard format to applications about the identity of an authenticated user. This provided a solution for applications for user authentication as well as API authorization.

Benefits of OIDC:
- Website developers can delegate the work of implementing authentication and password reset logic to an OIDC provider.
- Users can leverage one account to log in to many sites without exposing their account credentials to other sites.
- Users have fewer usernames and passwords to remember, which reduces the risk of password reuse.
- Providers benefit if OIDC attracts more users to their platform.
- OIDC provides the SSO benefits of SAML 2 and when combined with OAuth 2, it provides a solution for both authentication and authorization.

##### OAuth 2.1

The OAuth 2.0 specification was published in 2012. By 2020, application developers had to read through a lot of OAuth 2-related documents to understand best practices for using OAuth 2 in different scenarios, such as browser-based applications, native mobile applications, and classic web applications.

To address this, the OAuth working group began working on a new specification, OAuth 2.1, which aims to consolidate and simplify the OAuth 2.0 framework by incorporating best practices and removing deprecated features.
