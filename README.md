# Thales
Thales solves customer problems. We design and deliver authenticators that support your workflow, keep you in compliance, and keep your employees and customers safe.

Our solution portfolio includes FIDO authenticators, hardware and software OTP tokens, X.509 certificate-based USB tokens and smart cards, OOB, hybrid tokens, and phone tokens for all mobile platforms. 

Thales FIDO2 authenticators are customizable, and our FIDO smart card is additionally:
- Common Criteria certified
- ANSSI qualified
- NFC reader compatible to enable contactless authorization

## Prerequisites - Supported Authenticators
FIDO
- SafeNet IDPrime 3940 FIDO (Smart Card)
- SafeNet eToken FIDO

## FIDO2 and PKI Support, All in One Device
The **SafeNet IDPrime 3940 FIDO** Smart Card is FIDO2.0 and U2F certified. It is designed for PKI-based applications and comes with a SafeNet Minidriver that offers perfect integration--native support for Microsoft® environments without any additional middleware. This dual-interface smart card, allows communication either via a contact interface or via a contactless ISO14443 interface, and is compatible with some NFC readers.

## USB Token with Touch Sense Options

The **SafeNet eToken FIDO USB token** is FIDO2.0 and U2F certified. It is an ideal solution for enterprises looking to deploy passwordless authentication for employees. This authenticator is a compact, tamper-evident USB with presence detection, which creates a third factor of authentication: Something you have (physical token), something you know (PIN), something you do (touching the token).

# Thales FIDO Authentication Tree 
The tree below shows the flow to register a Thales FIDO authenticator.

 
1.	In this tree, the user will provide his user name and password (‘Username Collector’ node and ‘Password Collector’ node) and then be directed to the FIDO authentication flow (‘WebAuthn Authentication Node’).
2.	If the user doesn’t have a FIDO Authenticator, he will be asked if he wishes to register (‘Register Thales Authenticator’ - Message Node). If his reply is positive (True), he will be directed to the registration node (‘WebAuthn Registration Node’ Node). If his reply is negative (False), he will be directed to provide his user name and password.

This listing focuses on the settings used to support Thales authenticators with ForgeRock access management.

## ForgeRock WebAuthn Integration

For strong authentication, ForgeRock Access Management supports WebAuthn, enabling FIDO2 hardware devices, such as the Thales FIDO authenticators. 

By using Thales FIDO authenticators and ForgeRock Access Management, your end users can replace their username-password with a much stronger authentication method, either 2FA (Two Factor Authentication) or MFA (Multi Factor Authentication), yet still keep the registration and authentication procedure easy and friendly.

Using ForgeRock for access management, you can create authentication flows such as:
- FIDO authentication with a PIN
- PIN and password 
- PIN and Touch sense 

With Thales FIDO authenticators, you can create either 2FA (username/password + Thales FIDO authenticator) or MFA (username/password + Thales FIDO authenticator + PIN).

Combining ForgeRock access management with Thales authenticators, you have the flexibility to configure the environment and conditions. 

In the following flows, we will demonstrate how we build different flows (for either 2FA or MFA) starting with a simple flow that defines registration for a Thales FIDO authenticator. 

# WebAuthn Authentication Node
Reference WebAuthn Authentication Node

The WebAuthn Authentication node allows users of supported clients to use a registered FIDO2 device during authentication.
 
Input to the WebAuthn Authentication Node:
- Data Store Decision (True)

Output from the WebAuthn Authentication Node:
 - Unsupported --> Failure
 - No Device Registered --> Message Node
 - Success --> Success
 - Failure --> Failure
 - Client Error --> Failure

Configuration of WebAuthn Authentication Node:
- User verification requirement:
  - DISCOURAGED – disable the PIN prompt for WebAuthn
  - PREFERRED – for MFA, the user will be prompted to enter a PIN
  - REQUIRED – for MFA, the user will be prompted to enter a PIN

## Message Node
Reference Message Node
 
The Message node allows you to present a custom message to the user, such as “Have you registered your FIDO device?” else go back to collect the username.

Possible Input to the Message Node:
  - Have you registered your FIDO device?

Possible Output from the Message Node:
  - True --> WebAuthn Registration Node
  - False --> Username Collector
Configuration of Message Node:
- Message
  - Value: “Have you registered your FIDO device?”
- Positive answer
  - Value: “Yes”
- Negative Answer
  - Value: “Not yet”

## WebAuthn Registration Node

Reference [WebAuthn Registration Node](https://backstage.forgerock.com/docs/am/6.5/authentication-guide/#auth-node-webauthn-registration)

The WebAuthn Registration authentication node allows users of supported clients to register FIDO2 devices for authentication.
 
Input to the **WebAuthn Registration Node:**
  - Message Node (True)

Output from the **WebAuthn Registration Node:**
  - Unsupported --> Failure
  - Success --> Success
  - Failure --> Failure
  - Client Error --> Failure

Configuration of **WebAuthn Registration Node:**
- User verification requirement:
  - DISCOURAGED – disable the PIN prompt for WebAuthn
  - PREFERRED – for MFA, the user will be prompted to enter a PIN
  - REQUIRED – for MFA, the user will be prompted to enter a PIN
- Authentication attachment:
  - CROSS_PLATFORM – instruct the browser to prompt for a Thales Authenticator
  - UNSPECIFIED – the user may be prompted for a platform authenticator such as Windows Hello on a Windows 10 device
- Limit registration: 
  - Enabled - Specify whether the same authenticator can be registered multiple times. If enabled, the client should not activate an authenticator that is already registered for registration

