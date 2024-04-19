# Thales
Thales solves customer problems. We design and deliver [authenticators](https://cpl.thalesgroup.com/access-management/authenticators) that support your workflow, keep you in compliance, and keep your employees and customers safe.

Our solution portfolio includes FIDO Authenticators, hardware and software OTP tokens, X.509 certificate-based USB tokens and smart cards, OOB, hybrid tokens, and phone tokens for all mobile platforms. 

## Stop Phishing Attacks with Strong Passwordless Multi-Factor Authentication

[Thales FIDO2 security keys](https://cpl.thalesgroup.com/access-management/authenticators/fido-devices)
are offering organizations passwordless, phishing-resistant authentication, allowing them to stop account takeover and remove risk of unauthorized access to sensitive resources like SaaS applications and Windows endpoints.

## Enable multiple user authentication journeys
Thales supports numerous passwordless authentication journeys with a wide range of FIDO authenticators 
Facilitate users’ MFA adoption with biometric authentication 

Combining biometrics and NFC, the innovative [SafeNet IDPrime FIDO Bio Smart Card](https://https://cpl.thalesgroup.com/access-management/safenet-idprime-fido-biometric-smart-card)
allows end users to authenticate from multiple types of devices securely and easily, with just a fingerprint instead of a password


![image](https://github.com/ForgeRock/Thales-Auth-Tree-Node/assets/88729940/222911cf-7ab2-4ff3-a6f7-882ad37123d0)



The **SafeNet IDPrime 3940 FIDO** Smart Card is FIDO2.0 and U2F certified. It is designed for PKI-based applications and comes with a SafeNet Minidriver that offers perfect integration--native support for Microsoft® environments without any additional middleware. This dual-interface smart card, allows communication either via a contact interface or via a contactless ISO14443 interface, and is compatible with some NFC readers.

## USB Token with Touch Sense Options

The **SafeNet eToken FIDO USB token** is FIDO2.0 and U2F certified. It is an ideal solution for enterprises looking to deploy passwordless authentication for employees. This authenticator is a compact, tamper-evident USB with presence detection, which creates a third factor of authentication: Something you have (physical token), something you know (PIN), something you do (touching the token).

# Thales FIDO Authentication Tree 
The tree below shows the flow to register and authenticate Thales FIDO authenticator

![image](https://user-images.githubusercontent.com/88729940/131348378-e76840b0-388c-46a9-a139-b433b834b8ff.png)

 
1.	In this tree, the user will provide his user name and password (‘[Username Collector](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-username-collector)’ node and ‘[Password Collector](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-password-collector)’ node) and then be directed to the FIDO authentication flow (‘[WebAuthn Authentication Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-webauthn-auth)’).
2.	If the user doesn’t have a FIDO Authenticator, he will be asked if he wishes to register (‘Register Thales Authenticator’ - [Message Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-message)). If his reply is positive (True), he will be directed to the registration node (‘[WebAuthn Registration Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-webauthn-registration)’ Node). If his reply is negative (False), he will be directed to provide his user name and password.

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
Reference [WebAuthn Authentication Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-webauthn-auth)

The WebAuthn Authentication node allows users of supported clients to use a registered FIDO2 device during authentication.

![image](https://user-images.githubusercontent.com/88729940/131348413-bc62b100-d3ef-41e6-b8dc-44123cc79811.png)


Input to the **WebAuthn Authentication Node:**
- Data Store Decision (True)

Output from the **WebAuthn Authentication Node:**
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
Reference [Message Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-message)

![image](https://user-images.githubusercontent.com/88729940/129359089-96a5ea06-264e-4c4b-ac1e-fa1e475f790f.png)

The Message node allows you to present a custom message to the user, such as “Have you registered your FIDO device?” else go back to collect the username.

Possible Input to the **Message Node:**
  - Have you registered your FIDO device?

Possible Output from the **Message Node:**
  - True --> WebAuthn Registration Node
  - False --> Username Collector

Configuration of **Message Node:**
- Message
  - Value: “Have you registered your FIDO device?”
- Positive answer
  - Value: “Yes”
- Negative Answer
  - Value: “Not yet”

## WebAuthn Registration Node

Reference [WebAuthn Registration Node](https://backstage.forgerock.com/docs/am/7.1/authentication-guide/auth-node-configuration-hints.html#auth-node-webauthn-registration)

The WebAuthn Registration authentication node allows users of supported clients to register FIDO2 devices for authentication.

![image](https://user-images.githubusercontent.com/88729940/131348452-4f3610af-d376-4be7-ab87-9033fff55434.png)

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

