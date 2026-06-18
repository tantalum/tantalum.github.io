---
title: "An Architecture for Passworless Authentication"
layout: post
---

## Background and Scope: Who is This For

This post describes an architecture passwordless user authentication for consumer-facing web applications. To break that down a little more:

*Passwordless:* The goal is to allow the user to authenticate them selves without using or having to remember a password.

*User:* We are discussion human users as apposed to [service accounts](https://unix.stackexchange.com/questions/314725/what-is-the-difference-between-user-and-service-account).

*Authentication:* This architecture is focused on [Authenticating](https://en.wikipedia.org/wiki/Authentication), i.e. verifying, a user's identity.

*Consumer-facing:* This is your run of the mill website where, today, users register with a username, email, and password. Users login by providing a username and password and are able to reset their password by requesting a magic link sent to their inbox. MFA might or might not be a requirement.

*Web application:* The user is authenticating to a web application hosted on the internet, as opposed to a locally running app or logging into a personal device like your phone or computer.

<!--more-->

This architecture is indented to describe a framework for authenticating users to a web application. This architecture doesn't describe other facets of user management such as [authorization](https://en.wikipedia.org/wiki/Authorization) or user roles. Neither does this architecture describe a full security architecture such as rate limiting, credential expiration or access and refresh token management. Although this architecture has all the components for [MFA](https://en.wikipedia.org/wiki/Multi-factor_authentication), we will focus on single factor authentication and will consider MFA out of scope. 

## Current State: Passwords Have Eaten the Web

Logging in with a username and password is a universally well understood and familiar UX pattern for users. Additionally, passwords are portable: If user gets a new device and needs to log into a service, all they have to do is type in their username and password.

### The Problems With Passwords

**Password resets:** Users regularly forget their passwords. The most common password reset mechanism is by sending a Magic Link to the email associated with the account. In effect, this pattern makes access to the email account a user credential. This is an Out-Of-Band Authenticator in NIST parlance. See: [NIST SP 800-63 B Section 5.1.3.1](https://pages.nist.gov/800-63-3/sp800-63b.html)

**The password is sent to the server:** Sending the password to server means the server, and anyone who can control the server, has access to your raw password. 

If I can try a bunch of guess with the server letting me know if the password is correct or not, I might be able to guess your password.

If I can control the login page, I can get your password because you have to type it into the login page. 

If I control the login action on the server, I can get your password.

If I can get on the path between you and the server, I can get your password because it is sent to the server. Less of an issue today thanks to TLS, but still feasible in some cases.

The server stores the hashed password, along with the passwords for all the other users. This means if I can get a copy of that data, I have a fair chance at cracking the password of at least a few users.

<!-- TODO: Add reference to how fast passwords can be cracked. -->

**Passwords are hard to remember:** As a result people tend to reuse their passwords, or an easy to recognize password pattern. This means for a given user, if their password on service `A` is compromised, there is a good chance you will be able guess their password on service `B`.

The two issues of services having direct access to a users password, and users reusing their passwords, combine to make passwords particularly easy to compromise. As a result the Information Security world has been looking for an alternative to passwords for over a decade, or at the very least augment password with things like Multi-Factor Authentication (MFA) and [WebAuthN](https://www.w3.org/TR/webauthn-2/)

## WebAuthN, Passkeys and Why They Suck

First for some definitions:

- *Public Key Credential:* A public key that is associated with an account. A user authenticates by proving access to the private key associated with the public key credential. 
- *[WebAuthN](https://www.w3.org/TR/webauthn-2/):* Is a browser API for creating a utilizing a public key credential.
- *[Passkey](https://www.passkeys.com/what-are-passkeys.html):* A public key credential that is compatible with WebAuthN.

### The Problems with Passkeys

- Passkeys are tied to a device, if you lose your device you lose your credentials
- Passkeys can't be transferred across devices. If you need to login with a new device, you might need to access to your old device.
- Passkeys registration and authentication is a little more complex, from a technical perspective, than password based authentication. 
- Users are used to passwords, and people like what is familiar.
- Lets face it: Passkeys are a new technology and we are still figuring some of this stuff out.
<!-- TODO: Is this a comprehensive list -->

## The Architecture

### One User Multiple Credentials

When designing an authentication system focused on single user password based logins, the most straight forward approach is to have a `users` table. This `users` table will generally have one column for the password hash and another column for the user's email address. The main purpose of the email field is to sometimes serve as the username, and mainly to store the email used when the user forgets their password.  This is a relatively simplistic and naive implementation, but is in fact how many web authentication systems are designed.

<!-- TODO: Reference real world examples --> 

A more nimble, but more complex, approach is to have multiple credentials of different types associated with a user. Password, MFA tokens and passkeys all become credentials associated with a user. Once we have different credentials of different types associated to a user, we can ditch the password and let the user authenticate with any of the valid credentials. 

### The Entities

#### A User

This is the typical "user" entity found in most web applications. In this context a user is an entity that can be authenticated. 

<pre class="mermaid">
classDiagram
    class User {
        +id ID
    }
</pre>

#### A Credential

A credential is an authentication mechanism and associated data. For example a passkey credential and the associated public key, or a password credential and the hashed password. A user's credential's should be unique, as in a single user should not have two credentials of the same type and authentication data. For example a user shouldn't have two passkey credentials with the same public key.

For our use case we are going to define two types of credentials:

1. A Passkey Credential: A passkey credential contains an ID and a public key
2. An Email Credential: An email credentials contains and ID and a verified email address.

<!-- NOTE: Should valid be a more generic "state"? -->

<pre class="mermaid">
classDiagram
    Credential --> User
    User : +id ID
    class Credential {
        +id ID
        +User user
        +boolean valid
    }
    class PasskeyCredential {
        +PublicKey publicKey
    }
    class EmailCredential {
        +String emailAddress
    }
    Credential <|-- EmailCredential
    Credential <|-- PasskeyCredential

</pre>

#### An Authetication Challenge

Whenever a user wants to authenticate to the app, the user needs to provide the correct response to a given challenge. For example when authenticating using a Passkey the user needs to respond to the challenge by siging the challenge with the private key associated with the passkey proving access to the private key. When attempting to authenticate via email, the user needs to provide the secret code sent to the email proving access to the email address. In this second case the secret code is part of the challenge.

<pre class="mermaid">
classDiagram
    class AuthenticationChallenge {
        +id ID
        +Timestamp expiration
        +Credential credential
    }
    class MagicLinkChallenge {
        +String nonce
    }
    class PasskeyChallenge {
        +String nonce
    }
    AuthenticationChallenge <|-- MagicLinkChallenge
    AuthenticationChallenge <|-- PasskeyChallenge

</pre>

<!-- TODO: Email Verification Request -->

### The Flows

#### Registration

The registration flow is very similar to the typical user registration flow for any password-based authentication system, except instead of submitting an email and password the user submits and email and a passkey public key credential.

During registration the submits the information defining the user, including the initial credential data. For this design we will assume two credentials are provided on registration: 

* An email credential
* A passkey credential

<pre class="mermaid">
sequenceDiagram
    actor User
    participant Server
    participant DB@{ "type": "database", "alias": "User Database"}
    participant EmailServer@{ "alias": "Email Server"}
    User->>Server: Registration data including public key and email address
    activate Server
    Server->>DB: Store user data and credentials
    Server->>EmailServer: Send email address verification
    Server->>User: Registration confirmation
    Note right of Server: All credentials are invalid since registration is not yet complete
    deactivate Server
    User->>Server: Confirms email address
    activate Server
    Server->>User: Registration complete
    Note right of Server: Credentials are valid and user can login
    deactivate Server
</pre>

#### Email Verification

The email verification flow is used to validate that the user has access to the email address provided. This flow us used during registration to establish the initial Email Credential and any time the user wants to modify, or add, an Email Credential.

<pre class="mermaid">
sequenceDiagram
    actor User
    participant Server
    participant EmailServer@{ "alias": "Email Server"}
    User->>Server: Registration data including public key and email address
    activate Server
    Server->>EmailServer: Send email address verification
    Server->>User: Success message
    deactivate Server
    User->>Server: Confirms email address
    activate Server
    Server->>User: Email verified
    deactivate Server
</pre>

#### Passkey Authentication

The standard WebAuthN login flow.

#### Magic Link Authentication

Allows the user to login using to the registered email as a credential. When sending the link to the user the link should contain the nonce, and the server will lookup the challenge using the nonce. If the challenge is found and valid, the user can be authenticated. If the nonce is not found, or the challenge associated with the nonce is not longer valid, authentication is denined and the user is provided an error message. 

[User enumeration](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account) is an important consideration here, especially when responding to the user after the Magic Link is requested. Most applications will respond to that request letting the requester know to either check their email address, or that the email address was invalid. This type of message easily allows an attacker to determine if a given email address has an account on the app or not. Instead apps should respond with a generic message letting the user know that if their email address was valid a magic link will be sent, and providing the same response when requesting a magic link regardless of whether the address was valid or not.

<pre class="mermaid">
sequenceDiagram
    actor User
    participant Server
    participant DB@{ "type": "database", "alias": "User Database"}
    participant EmailServer@{ "alias": "Email Server"}
    User->>Server: Requests login with Magic Link
    activate Server
    Server->>DB: Create Magic Link Challenge
    DB->>Server: Challenge created
    Server->>EmailServer: Send magic link with nonce
    Server->>User: Notify user to check email
    deactivate Server
    User->>Server: Clicks Link
    activate Server
    Server->>DB: Retrieve Magic Link Challenge
    DB->>Server: Provide Magic Link Challenge
    alt Link valid
        Server->>DB: Set challenge as invalid since it was already used
    	Server->>User: Login Successful
    else Link invalid
    	Server->>User: Login Unsuccessful
    end
    deactivate Server
</pre>

<script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
    mermaid.initialize({ startOnLoad: true });
</script>
