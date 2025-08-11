---
title: "An Architecture for Passworless Authentication"
layout: post
---

## Background and Scope: Who is This For

This is a passwordless user authentication architecture for consumer-facing web applications. To break that down a little more:

Passwordless: The goal is to allow the user to authenticate them selves without utilizing password.
User: We are discussion human users as apposed to [service accounts](https://unix.stackexchange.com/questions/314725/what-is-the-difference-between-user-and-service-account).
Authentication: This architecture is focused on [Authenticating](https://en.wikipedia.org/wiki/Authentication), i.e. verifying, a user's identity.
Consumer-facing: This is your run of the mill website where, today, users register with a username, email, and password. Users login by providing a username and password and are able to reset their password by requesting a magic link sent to their inbox. MFA might or might not be a requirement.
Web application: The user is authentication to a web application hosted on the internet, as opposed to a locally running app or loggin into their personal device.

## Current State: Passwords Have Eaten the Web

Logging in with a username and password is a universally well understood pattern for users. Users like being able to log into a website using a username and password because is familiar. Additionally, passwords are portable: If user gets a new device and needs to log into a service, all they have to do is type in their username and password.

### The Problems With passwords

**Password Resets:** Users regularly forget their passwords. The most common password reset mechanism is by sending a Magic Link to the email associated with the account. This pattern makes access to the email account a user credential. Essentially this is an Out-Of-Band Authenticator in NIST parlance. See: [NIST SP 800-63 B Section 5.1.3.1](https://pages.nist.gov/800-63-3/sp800-63b.html)

**The password is sent to the server:* Sending the password to server means the server, and anyone who can control the server, has access to your raw password. 

If I can try a bunch of guess with the server letting me know if the password is correct or not, I might be able to guess your password.

If I can control the login page, I can get your password because you have to type it into the login page. 

If I control the login action on the server, I can get your password.

If I can get on the path between you and the server, I can get your password because it is sent to the server. Less of an issue today thanks to TLS, but still feasible in some cases.

The server stores the hashed password, along with the passwords for all the other users. This means if I can get a copy of that data, I have a fair chance at cracking the password of at least a few users.

*Passwords are hard to remember:* As a result people tend to reuse their passwords, or an easy to recognize password patter. This means for a given user, if their password on service `A` is compromised, there is a good chance you will be able guess their password on service `B`.

The two issues of services having direct access to a users password, and users reusing their passwords, combine to make passwords particularly easy to compromise.

