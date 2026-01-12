---
title: "An Architecture for Passworless Authentication"
layout: post
---

## Background and Scope: Who is This For

This is a passwordless user authentication architecture for consumer-facing web applications. To break that down a little more:

Passwordless: The goal is to allow the user to authenticate them selves without using or having to remember a password.
User: We are discussion human users as apposed to [service accounts](https://unix.stackexchange.com/questions/314725/what-is-the-difference-between-user-and-service-account).
Authentication: This architecture is focused on [Authenticating](https://en.wikipedia.org/wiki/Authentication), i.e. verifying, a user's identity.
Consumer-facing: This is your run of the mill website where, today, users register with a username, email, and password. Users login by providing a username and password and are able to reset their password by requesting a magic link sent to their inbox. MFA might or might not be a requirement.
Web application: The user is authenticating to a web application hosted on the internet, as opposed to a locally running app or logging into a personal device like your phone or computer.

<!--more-->

## Current State: Passwords Have Eaten the Web

Logging in with a username and password is a universally well understood pattern for users. Users like being able to log into a website using a username and password because is familiar. Additionally, passwords are portable: If user gets a new device and needs to log into a service, all they have to do is type in their username and password.

### The Problems With Passwords

**Password resets:** Users regularly forget their passwords. The most common password reset mechanism is by sending a Magic Link to the email associated with the account. This pattern effectively makes access to the email account a user credential. Essentially this is an Out-Of-Band Authenticator in NIST parlance. See: [NIST SP 800-63 B Section 5.1.3.1](https://pages.nist.gov/800-63-3/sp800-63b.html)

**The password is sent to the server:** Sending the password to server means the server, and anyone who can control the server, has access to your raw password. 

If I can try a bunch of guess with the server letting me know if the password is correct or not, I might be able to guess your password.

If I can control the login page, I can get your password because you have to type it into the login page. 

If I control the login action on the server, I can get your password.

If I can get on the path between you and the server, I can get your password because it is sent to the server. Less of an issue today thanks to TLS, but still feasible in some cases.

The server stores the hashed password, along with the passwords for all the other users. This means if I can get a copy of that data, I have a fair chance at cracking the password of at least a few users.

<!-- TODO: Add reference to how fast passwords can be cracked. -->

**Passwords are hard to remember:** As a result people tend to reuse their passwords, or an easy to recognize password patter. This means for a given user, if their password on service `A` is compromised, there is a good chance you will be able guess their password on service `B`.

The two issues of services having direct access to a users password, and users reusing their passwords, combine to make passwords particularly easy to compromise. As a result the Information Security world has been looking for an alternative to passwords for over a decade, or at the very least augment password with things like Multi-Factor Authentication (MFA) and [WebAuthN](https://www.w3.org/TR/webauthn-2/)

## WebAuthN, Passkeys and Why They Suck

First for some definitions:

- *Public Key Credential:* A public key that is associated with an account. A user authenticates by proving access to the private key associated with the public key credential. 
- *[WebAuthN](https://www.w3.org/TR/webauthn-2/):* Is a browser API for creating a utilizing a public key credential.
- *[Passkey](https://www.passkeys.com/what-are-passkeys.html):* A public key credential that is compatible with WebAuthN.

### The Problems with Passkeys

- Passkeys are tied to a device, if you lose your device you lose your credentials
- Passkeys can't be transferred across devices. If you need to login with a new device, you might need access to your old device.
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

#### A Credential

A credential is an authentication mechanism and associated data. For example a passkey credential and the associated public key, or a password credential and the hashed password. A user's credential's should be unique, as in a single user should not have two credentials of the same type and authentication data. For example a user shouldn't have to passkey credentials with the public key.

### The Flows

#### Registration

The registration flow is very similar to the typical user registration flow for any password-based authentication system, except instead of submitting an email and password the user submits and email and a passkey public key credential.

During registration the submits the information defining the user, including the initial credential data. For this design we will assume two credentials are provided on registration: 

* An email credential
* A passkey credential

<!-- TODO: Sequence Diagram -->
<!-- TODO: Email verification -->

#### Email Verification

Send an email with a code, user must open the link to verify the email address. 

#### Passkey Login

The standard WebAuthN login flow.

#### Magic Link Login

Allows the user to login using to the registered email as a credential. 

