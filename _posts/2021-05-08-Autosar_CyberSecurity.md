---
layout: post
title: Autosar Cyber Security
excerpt: "Overall summary of the cencepts of cyber security on Autosar"
modified: 5/07/2021, 9:00:24
tags: [Autosar, CyberSecurity, C]
comments: true
category: blog
---

# Introduction

Cybersecurity is any collection of mechanisms and processes to protect a system from malicious attacks. Notice that this concept is different from Functional Safety which  the syste has mechanisms to protects itself and the user from own systematic failures.

The principal goals for Autosar Cybersecurity are:

* **Authenticity**, the determination than the entity sending data is the one expected.
* **Confidentiality** ensures that communication between 2 entities cannot be intercepted by a third one.
* **Integrity** ensures that data received by one entity was not modified by another entity during the transmission.
* **Availability** ensures that one entity keeps its stable state during malicious attacks.

The main use case of Cyber security in the Automotive industry are:

* **End 2 End (E2E) communication**. The secured communication between ECUs within a vehicle network.
* **Vehicle connectivity with outsiders**. Secured Car2X communication for wireless software updates (OTA), and hotspots for in-car Infotainment.


Along with the main work product process, any system development that includes cybersecurity has to comply with the following work products (as the client required) from the beginning of the project:

* **Asset Definition** determines the mechanisms, behaviors, and system attributes that require cybersecurity.
* **Thread and Risk Assessment** is the analysis of different conditions within a system to see how easy it is to receive a malicious attack and how much damage might cause.
* **Security Goals Derivation** is the high-level identification of mechanisms to use to mitigate the analyzed risks from Thread and Risk Assessment.
* **Security Architecture Design and Analysis** is the adaptation of the original architecture to add cybersecurity mechanisms.
* **Security Mechanisms Design and Analysis** is the complete design of cybersecurity mechanisms and how they are adapted for each SWC.
* **Functional Security Testing** are functional tests from the system without defining any dedicated cybersecurity condition.
* **Fuzz Testing** is functional tests that search failures from no defined behavior (sending incorrect data or searching unexpected paths of execution).
* **Penetration Testing** is a black-box test performed by expert third parties with already known mechanisms to hack a system.
* **Security Validation** is a white-box test performed by expert third parties with preconditioned mechanisms to hack a system.

# Cryptography Basics

Cryptography is the set of mechanism and algorithm to perform cybersecurity on an SW system. Cryptography is divided into 2 types of cryptography:

* **Symmetric cryptography** uses the same steps to encrypt and decrypt (uses only public keys). Decrypt performs the encrypt steps but in reverse order. Symmetrical cryptography can be defined as either block cryptography or data flow cryptography.
* **Asymmetric cryptography** uses different steps to encrypt and decrypt (uses public key and private key). Some examples of asymmetric cryptography are integrating factor, discrete logarithmic function, an elliptical curve.

Some other *complementary* cryptography mechanisms are:

* **Hashes** are interface functions that hide the real values of data sets.
* **Protocols** are complex mechanisms to use different types of cipher with hashes.

There is a principle called *Kerckhoffs Principle* which indicates that the security of a system relies almost entirely on the **secrecy of the key** and not on the secrecy of the algorithm.

A hash function takes one input value with variable length, then the hash maps the output with a predefined length to represent the fingerprint of data. A secure hash function is infeasible to the inverse computation of the output and it is infeasible when two inputs cannot yield to the same mapped output. Hash functions perform huge changes to relative small input differences. Hash functions are not defined as cybersecurity mechanisms but are very important as a complement to some real security mechanisms. Usually hash are the transport entity of a private key signature that later is verified against the public key signature that is locally addressed.

## Attack Threads

Threads are defined attack types that can be documented as follows:

* **Spoofing** is the act of disguising a communication by acting as the trusted source, Spoofing is mitigated by protecting secret data by no storing the data in no secured sources, and use appropriate techniques of authentification.
* **Tampering** is the act of deliberately modifying the data by using non-authorized channels. Tampering is mitigated by using appropriate authorization techniques and complement the use of Hashes, MACs, and digital signatures.
* **Repudiation** is the act of a system of not having methods to track and log an entity and its actions leaving room for attacks from this entity. Repudiation is mitigated by using digital signatures, use of timestamps during communication between trusted sources, and audit trails.
* **Information Disclosure** is the failure of a system of not protecting sensitive and confidential information from entities that are not supposed to have access to this kind of information. Information Disclosure is mitigated by authorization requests, encryption of sensitive data, privacy-enhanced protocols, and protection of secret data by not storing this data in unsaved places.
* **Denial of Service** is the act of shutting down the system, network, or making inducible any source to its intended users. The way to mitigate denial of service attack is by ensuring appropriate authorization and authentification, filtering entities, and ensuring the quality of service of the system.

## Symmetric cryptography

Symmetric cryptography uses the same key to encrypt and decrypt. This type of encryption is normally used for data confidentiality, integrity, and authenticity that requires low-profile security mechanism. The distribution of keys shall be careful because anyone with the key can access all security mechanisms. 

The most used in the automotive industry symmetric cryptography is the **Advanced Encryption Standard (AES)**, AES is based on block cryptography algorithms and needs the definition of the key and the data length (128 bits, 192 bits, or 256 bits). AES performs cycles of encryption rounds, these rounds operate combinations of byte substitution, shifting of bytes, or key addition to produce a CipherText.

In general, symmetric cryptography can create **messages of authentication of code (MACs)** during their procedure using the secret key. MAC is a fixed-side digital code that can be only created by the key creator. The MAC is transmitted along with the encrypted data, if the receiver can decipher the MAC, then it is assumed that the encrypted data can be decrypted too. There are two types of MACs, the one defined by hash functions **(HMAC)** or the one defined by block encryption **(CMAC)**.

## Asymmetric cryptography

Asymmetric cryptography uses two keys (one public and another private), These keys have relatively large lengths. The public key can be shared with all the entities that can collaborate with the system; whereas the private key cannot be shared with all the entities of the system and shall be dedicated to a specific entity.

Asymmetric cryptography algorithms are based on one key encrypting the data and the other one decrypts the data. For example, if the public key is used to encrypt data, then the private key is used to decrypt the data. The most used algorithms of asymmetric cryptography are **RSA**(Rivest, Shamir, and Adleman) and **ECC**(Elliptic Curve Cryptography).

### Digital Signature

The **digital signature** is based on asymmetric cryptography and offers data authenticity and data integrity. The digital signature creator generates the signature by the hash generation of complex data, to later encrypt this data before transmitting it to the user. The user decrypts the received data to  compare the hash data with the decrypted hash data and if they are the same, then the data integrity and data authenticity are guaranteed. Notice that the encryption can be done by the private key or public key, or otherwise, depending of the mechanism architecture (decryption is going alway to be the opposite key than the encryption key).

Digital signatures are mainly used by **certifications**. Certifications are mechanisms to ensure that one data-sending entity is allowed to send those data. Certifications might contain personal data from the sender (for example IDs, sensitive information) to be identified, this personal data has an expiration time to use.

The automotive industry permits secure interaction of the ECU with diagnostic tools or external b*back-ends* using certifications. This ECU access is supported by different certifications for each access level (Root, Tester, Checker, …).

# Cybersecurity use cases on the Automotive industry
## Flash Programming

The ECU encryption can be performed using two methods:

* **HMACs use**. The manufacturer creates an HMAC by a secret key and a hash. Then the manufacturer transfers the file to reflash along with the HMAC to the ECU. The ECU takes the HMAC and compares it to the decrypted HMAC by the secret key. If the HMAC verification is correct, then the ECU reflash is permitted.
* **Digital Signature**. Same process than the HMAC but using public keys and private keys. The public key is generated by the manufacturer delivering a digital signature, then the ECU takes this signature and processes it with the private key. If the signature verification is correct, then the ECU reflash is permitted.

![Flash Programming Use Case](https://github.com/CharlieGearsTech/CharlieGearsTech.github.io/blob/f7ce567ade5c4187a92a290b9686090720ea272a/images/sc1.png)

## Vehicle-Tester communication

Vehicle Tester communication is the secured way to assign privileges to testers based on the OEM server whitelist back-bone. This is based on platform certifications and root certifications with their respective keys. Here, the tester asks for permission to perform tasks on an ECU by communicating with the OEM server, the OEM server based on the certification request, identifies the tester, and returns with permission to perform the requested tasks.

## OEM Backend communication with the vehicle

The vehicle can have communication with  OEM servers by root certifications and platform certifications with their respective keys. This use case is based on TLS and the revocation of certification if the testers have a compromised key.

# Autosar Crypto Modules

The Autosar cryptography cluster is composed by the following modules:

* **Crypto Service Manager** (CSM): This module communicates with the SWCs to offer services and handles the cryptography operations:
    * Random number and hashing generation.
    * Data encryption/decrypt.
    * MACs
    * Digital Signature
    * Models of operations based on “jobs”
    * Asynchronous/Synchronous process of jobs.
    * Generation, derivation, and exchange of keys.
    * Freshness counting
* **Crypto Interface** (CRYIF): This module enables CSM to access Crypto Driver features based on standardized APIs.
* **Crypto Driver** (CryptoSW and CryptoHw): These modules are the ones performing the cryptographic functions. CryptoSW uses SW libraries whereas CryptoHW uses HW capabilities (SHE, HSM, TPM, …).

![CSM Stack](/CharlieGearsTech.github.io/images/sc2.jpeg)

## Crypto Service Manager

Crypto Service Manager (CSM) is the service module from BSW that manages the crypto services to secure data and processes. A **CSM Job** is the abstraction of CSM keys, CSM queues, and CSM primitive types. CSM has the properties to be able to dispatch jobs based on priorities. The jobs can be synchronous or asynchronous, Synch jobs wait for the returning value of a finished call and do not use callbacks; whereas async jobs are inserted into the queue based on their priority when the respective callback is finished.

Based on the exchange of information on the security stack, each CSM key is mapped to a CRYIF key and a Crypto Driver key.

CSM Queues are mapped based on the Crypto Driver, CRYIF uses one channel for each CSM Queue to a Crypto Driver in order to process one job at a time.

The crypto stack communication is as follows:

* One SWC asks for a CSM service using the *API Csm_Encrypt()*. This API use Job IDs to identify every encryption process.
* CSM adds the required job to one of the available CSM queues, the scheduler processes this job based on its priority.
* Then CSM executes *Csm_MainFunction()* to transmit the selected job to lower modules.
* The job is transmitted to the Crypto Driver, which processes it, and returns a specific value to let CSM know the result.
* CSM returns the result to the SWC while removing the processed job from the queue.

CSM primitives define each job type based on:

* Crypto predefined generic algorithms.
    * MAC generation or verification.
    * A digital signature generation or verification.
    * Encryption/Decryption of symmetric keys.
    * Encryption/Decryption of asymmetric keys.
    * Hashing, random number generation, etc.
* Specific algorithm configuration
    * Algorithm family (AES, RDS, …)
    * Algorithm mode.
    * Job processing mode.
    * Length of input/output data.

CryIf is the implementation of cryptography abstraction made by the crypto drivers and coordinated by CSM access. CryIf offers independent APIs from HW/SW crypto drivers to superior channels to process the crypto operations.

Crypto Drivers are the principal responsibilities of the crypto operations. They specify the crypto capabilities and the composition of the key, as well as, the keys access properties.

The key component is the classification of key types made by the Crypto Drivers. Examples of these key types are Private key, Public key, hash values, random number certification, etc. Autosar defines these key types to perform crypto operations. Each composition key type has one ID, Autosar defines a key type ID below 1000, when two different key types have the same ID, then these key types can be used such the same type (Length, operations, access, etc). Keys ID that is above 1000 is free-definition to e used by the vendor.

## Security on On-board Communication

Security on On-board Communication (**SecOC**) is an additional module at the  Autosar communication stack related to PDU transmission authentication and integrity. SecOC retrieves PDUs from PDUR and adds security information to encrypt based on the PDU payload and the SOME/IP transformer use. SecOC supports various communication buses such as CAN, FlexRay, and Ethernet but are not supporting LIN yet. 

The principal interaction flow of SecOC is:

* PDUR receives one PDU from one specific channel (CAN, FlexRay, Ethernet).
* PDUR transmits the PDU to SecOC, then SecOC converts this PDU to a “secure” PDU.
* SecOC along with CSM verifies the PDU authenticity and integrity.
* SecOC returns the PDU without security wrappers to PDUR to finish the PDU processing. 
    * When SecOC determines that one PDU is insecure, SecOC discards the PDU, and PDUR receives nothing from SecOC.
* SecOC communicates the PDU verification results to ASW.
* Additionally, SecOC can support PDU data freshness.

The transmission of secured data among ECUs is based on the authenticated message that is composed of data + freshness + MAC value. The MAC part of the authenticated message is generated internally by the sender and it is based on the private key shared among ECUs.

Autosar does not specify a freshness definitive calculation. Autosar modules only provide Freshness value callout from a module called Freshness Value Manager (FVM). Freshness is the mechanism to avoid malicious replication of messages that are exchanged between ECUs. These malicious replications are progressive changes without detection. To avoid these replications, some Freshness mechanisms are defined:

* **Message Counter Based Freshness (MCBF)**. Each time a message is transmitted, one internal counter which is independent of the ECU will be incremented. The disadvantage of MCBF is that counters shall be persistent and will add CPU load due to NVM processing. Another disadvantage of MCBF is its relative weakness from out of sync counter conditions.
* **Trip Counter Based Freshness (TCBF)**. TCBF is incremented when a trip is completed (ECU message process). FVM increments the TCBF along with a synch message (TripResetSynchMessage) to both, the sender and receiver. TCBF is based on the hybrid sending of freshness based on 3 counters to support automatic resync:
    * Trip Counter. ECU incremental persistent 4 bytes for each ECU process.
    * Reset Counter. PDU incremental no-persistent value, this counter increases itself when a message counter has suffered an overflow.
    * Message Counter. PDU incremental no- persistent value, this counter increases itself every time a message is sent.
* **Time Stamps.** Verification for each ECU with a real-time value, time-stamps shall be sent by “secure” messages.
Hybrid mechanisms. Combination of Timestamps and Freshness counters.

Conditions of Out of sync counter happens when 2 counter mismatch during E2E communication. The following mechanisms prevent the out of sync counter mismatching:

* **Truncated freshness**. This approach recognizes small differences in the freshness values from two ECUs. For example, sending values with a resolution of 10 permits room for delay messages to one ECU that has freshness between 11 to 19.
* **Challenge-response protocol**. This approach detects large differences in the freshness values from two ECUs. Challenge-response protocol is a “challenge” special message from one ECU to another to perform a sync execution.
* **Periodic counter-message**. This approach injects periodic messages to the schedule to verify the counter from both ECUs and thereby avoiding out of sync conditions.

## Special case Transport Layer Security

Transport Layer Security (**TLS**) ensures the data exchange privacy and integrity on the TCP protocol, which alone cannot ensure these features. TLS is the abstraction of the Session layer at the Ethernet communication stack. This layer is just above the transport layer (TCP in this case).

TLS encapsulates a security mechanism collection:

1. For privacy, symmetric cryptography is used.
2. For data integrity, HMACs are used. HMACs calculations create secret and temporal keys that are linked to the specific session of TLS.
3. The server authenticity is always ensured by Digital Certifications.
4. Optimally, the client can be authenticated if necessary.

Levels of TCP/IP

* Application Layer -> COM/SomeIP
* Session Layer -> TLS
* Transport Layer -> TCP
* Network Layer -> IPv4/IPv6
* Link Layer -> Ethernet

A TLS cryptography session specifies the cryptography mechanisms that one group of clients supports. The server always chooses to communicate with the client with the best robust TLS cryptography session.

# Crypto Instances of use
## Secure Bootloader

Secure Bootloader prevents the execution of altered SW during the trusted execution chain of the ECU. The secure bootloader shall verify the SW integrity for each ECU startup by using checksums, signature validation, and MAC/Keys containment in a secured area. This integrity validation prolongs the startup time, so HW specific solutions can be considered to mitigate high Startup times.

The Secure Bootloader requires truthful SW and HW entities to save the Boot MAC/secret key inside them. For example, HW HSMs compare their Boot MAC against a computation from the bootloader host; if both are equal, then the bootloader is considered to be secure. Thereby, the bootloader performs calculations to determine the Application level MAC and ask HSM to compare with its stored Application-level MAC. If both coincide, then the application is started. If any of these comparisons do not match, the HSM is blocked and triggers an ECU reset.

## Secure Diagnostic

The secure diagnostic is based on the role of esta blishment or whitelist to trusted authorities by certifications. Each role has process permissions or diagnostic data access. The OEM contains back-end devices to establish certifications and roles, while the vehicle verifies these certifications by authentication demands.

> Reference : https://www.autosar.org/
