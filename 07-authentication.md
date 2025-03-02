# Evolution of Authentication Protocols: From Cleartext Passwords to Phishing-Resistant Cryptography  

Protocols are the rules that must be followed by participants or entities in some particular interactions. Thus, network protocols are the rules followed in networked communication systems. Security protocols are the communication rules followed in security applications. Cryptographic protocols are the rules that apply cryptographic techniques so that participants or entities can communicate securely. Authentication protocols fall into the security and cryptographic protocol categories and can be defined as a set of rules that must be followed for entities to prove or confirm their identity. That is, Alice must prove to Bob that she is really Alice, or Bob must prove to Alice that he is really Bob.

The landscape of digital authentication has undergone radical transformation since the dawn of computing, driven by escalating cybersecurity threats and technological innovation. This paper chronicles the progression from rudimentary cleartext password systems to modern cryptographic frameworks like AWS Signature Version 4 (SigV4), FIDO passkeys, and multi-factor authentication (MFA). Each evolutionary stage addressed critical vulnerabilities of its predecessors while introducing new challenges, ultimately shaping today’s emphasis on phishing-resistant, passwordless architectures. Key milestones include the shift from plaintext transmission to cryptographic hashing, the rise of public key infrastructure (PKI) enabling HTTPS, and the advent of asymmetric protocols like FIDO that eliminate shared secrets. However, persistent limitations—such as certificate authority vulnerabilities in PKI, brute-force risks in improperly implemented MFA, and device dependency in passkeys—underscore the need for continuous innovation. Emerging trends point toward decentralized identity models, quantum-resistant algorithms, and biometric-integrated authentication ecosystems.  

---

## Cleartext Passwords: The Fragile Foundations of Digital Trust  

### The Era of Unencrypted Credentials  
In the early days of networked computing, authentication relied overwhelmingly on cleartext passwords transmitted without encryption. Protocols like FTP, Telnet, and HTTP sent credentials as plaintext, exposing users to interception via packet sniffing or man-in-the-middle (MitM) attacks. A striking example persists in legacy systems: a 2016 case study revealed a French educational web app storing passwords in a cookie named `mdp` (French: *mot de passe*) as an unsalted hexadecimal string, despite lacking HTTPS encryption[1]. This implementation—where credentials were hashed client-side but transmitted over unsecured channels—highlighted the naivety of early security practices. Attackers could trivially harvest session cookies via ARP spoofing on school networks, bypassing server-side protections entirely[1].  

### Limitations and Legacy Risks  
Cleartext systems suffered three fatal flaws:  
1. **Interception Vulnerabilities**: Network eavesdroppers could capture credentials in transit.  
2. **Storage Exposure**: Databases storing plaintext passwords became high-value targets, exemplified by the 2013 Adobe breach exposing 153 million user credentials.  
3. **Reuse and Weak Entropy**: Users repeated simple passwords across services, amplifying breach impacts.  

While hashing mechanisms like SHA-1 emerged to obscure stored passwords, unsalted hashes remained susceptible to rainbow table attacks. The transition to HTTPS marked a partial solution, but legacy systems without transport layer security (TLS) perpetuated risks well into the 2010s[1].  

---

## Cryptographic Hashing and Salted Storage: Mitigating Post-Breach Risks  

### From Plaintext to One-Way Functions  
The introduction of cryptographic hashing represented a paradigm shift, ensuring passwords couldn’t be easily reversed even if databases were compromised. Algorithms like MD5 and SHA-1 converted plaintext into fixed-length digests, while salting—prepending unique random values to each password—thwarted precomputed hash attacks. For instance, the `mdp` cookie’s hexadecimal string in the French school system likely derived from a salted hash, though its static nature and lack of TLS negated these benefits[1].  

### Persistent Vulnerabilities  
Hashing alone couldn’t address systemic weaknesses:  
- **Offline Brute-Force Attacks**: High-performance GPUs could crack weakly salted SHA-1 hashes at rates exceeding 10 billion guesses per second by 2015.  
- **Protocol-Level Flaws**: Services hashing passwords client-side before transmission, as seen in the `mdp` example, created false security if the hash itself became a reusable credential[1].  
- **Algorithmic Obsolescence**: SHA-1’s collision vulnerabilities, demonstrated practically in 2017, necessitated migration to SHA-256 and bcrypt.  

These gaps set the stage for asymmetric cryptography and PKI, which decoupled authentication from shared secrets.  

---

## Public Key Infrastructure (PKI): Enabling Encrypted Channels  

### The PKI Revolution  
PKI, rooted in Diffie-Hellman key exchange (1976) and RSA encryption (1977), introduced a hierarchical trust model using digital certificates[2]. By binding public keys to identities via certificate authorities (CAs), PKI enabled TLS—the backbone of HTTPS. Modern implementations like AWS SigV4 leverage PKI-derived HMAC-SHA256 for API request signing, where credentials include access keys, regions, and services within a scoped hash[3]. For example, AWS SigV4 constructs a signing key through iterative HMAC operations involving secret keys, dates, and service identifiers, ensuring request integrity[3].  

### Limitations of Centralized Trust  
Despite its ubiquity, PKI faces critical challenges:  
- **CA Compromise**: Breaches like the 2011 DigiNotar hack enabled widespread TLS interception.  
- **Complex Key Management**: Enterprises struggle with certificate lifecycle management, leading to expiration-related outages.  
- **Quantum Vulnerabilities**: Shor’s algorithm threatens RSA and ECC, necessitating post-quantum migrations.  

PKI’s reliance on centralized CAs and algorithmic assumptions underscores the need for decentralized alternatives.  

---

## Multi-Factor Authentication (MFA): Layered Defense and Its Pitfalls  

### The Rise of MFA  
MFA emerged to counteract credential stuffing and phishing by requiring additional verification factors—possession (e.g., smartphones), biometrics, or one-time passwords (OTPs). Slack’s 2015 implementation of time-based OTPs (TOTPs) exemplified this shift, though lax rate limits allowed brute-force bypasses[4]. Modern systems combine TOTPs with contextual signals like IP geolocation, reducing false positives.  

### Implementation Flaws and Workarounds  
MFA’s efficacy hinges on rigorous deployment:  
- **OTP Brute-Forcing**: Six-digit codes with insufficient rate limiting can be cracked in under 10,000 attempts[4].  
- **Phishing Resilience**: Push notification fatigue and SIM-swapping attacks undermine SMS-based MFA.  
- **Recovery Loopholes**: Weak account recovery flows often revert to single-factor authentication.  

The 2023 Uber breach, where MFA prompts were spammed until an employee approved one, illustrates these systemic risks.  

---

## AWS SigV4 and Modern API Authentication  

### Signature-Based Request Authorization  
AWS SigV4 exemplifies modern API security, using HMAC-SHA256 to sign requests with access keys, timestamps, and scoped credentials[3]. Each signature incorporates the request payload, headers, and service endpoint, making replay attacks infeasible. For instance, a SigV4 signature for an S3 bucket upload includes the `AWS4-HMAC-SHA256` algorithm, credential scope (e.g., `us-east-1/s3/aws4_request`), and a hex-encoded digest of the canonical request[3].  

### Shortcomings of Secret Key Dependency  
While robust against tampering, SigV4 inherits PKI’s key management burdens:  
- **Key Rotation Complexity**: Frequent rotation to mitigate exposure risks strains DevOps workflows.  
- **Insider Threats**: Compromised IAM credentials grant full API access within defined policies.  

Temporary security tokens and hardware security modules (HSMs) partially alleviate these issues but add operational overhead.  

---

## FIDO Passkeys: Passwordless Authentication and Phishing Resistance  

### The FIDO2 Protocol Stack  
FIDO passkeys, standardized by the FIDO Alliance, replace passwords with asymmetric key pairs. During registration, a device generates a public-private key pair, with the public key stored by the service and the private key secured locally via biometrics or PINs[6]. Authentication involves signing a service-provided challenge, as seen in ePassport protocols where RFID tags perform elliptic curve Diffie-Hellman (ECDH) key agreements[8].  

### Cryptographic Advantages and Adoption Hurdles  
FIDO’s strengths are manifold:  
- **Phishing Resistance**: Cryptographic binding to service domains prevents credential reuse across fake sites[7].  
- **Decentralized Storage**: Passkeys reside on user devices, eliminating central breach vectors.  

However, challenges persist:  
- **Device Dependency**: Losing all registered devices necessitates complex recovery workflows.  
- **Cross-Platform Fragmentation**: Apple, Google, and Microsoft ecosystems implement passkey syncing differently, confusing users.  
- **Enterprise Integration Costs**: Retrofitting legacy systems to FIDO2 requires significant investment[6].  

The 2024 iCloud Keychain update, enabling cross-device passkey syncing, marks progress toward usability without compromising security[6].  

---

## Future Trajectories: Decentralization and Post-Quantum Cryptography  

### Decentralized Identity Frameworks  
Emerging standards like W3C Verifiable Credentials and blockchain-based self-sovereign identity (SSI) aim to dismantle PKI’s centralized trust model. These systems let users control attestations (e.g., age, membership) via distributed ledgers, reducing reliance on CAs.  

### Quantum-Resistant Algorithms  
NIST’s post-quantum cryptography (PQC) project prioritizes lattice-based schemes like CRYSTALS-Kyber, which could replace RSA/ECC in protocols like FIDO2. However, migration complexities—such as hybrid certificate chains and performance overhead—pose formidable barriers.  

### Biometric Fusion and Continuous Authentication  
Behavioral biometrics (keystroke dynamics, gait analysis) and passive liveness detection are being integrated into MFA flows, enabling frictionless yet secure access. Microsoft’s Windows Hello for Business, for instance, combines facial recognition with device-bound TPM modules to achieve phishing-resistant authentication at scale.  

---

## Conclusion  

The evolution from cleartext passwords to FIDO passkeys reflects an arms race between security innovators and adversaries. Each phase—hashing, PKI, MFA—addressed prior vulnerabilities while introducing new attack surfaces. Modern protocols like AWS SigV4 and FIDO2 prioritize cryptographic agility and phishing resistance, yet challenges like quantum threats and usability trade-offs loom large. Future systems must balance decentralization, interoperability, and user-centric design to sustain trust in an increasingly adversarial digital ecosystem. As organizations transition to passwordless architectures, investments in education, recovery mechanisms, and quantum readiness will determine their resilience in the coming decades.

Sources
[1] How to tell if a webapp transmits my password in cleartext? https://security.stackexchange.com/questions/123058/how-to-tell-if-a-webapp-transmits-my-password-in-cleartext
[2] What is PKI? The Ultimate Guide to Public Key Infrastructure - Venafi https://venafi.com/machine-identity-basics/what-is-pki-and-how-does-it-work/
[3] Elements of an AWS API request signature - AWS Documentation https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv-signing-elements.html
[4] Limitations of MFA and Common techniques to bypass MFA https://www.websecuritylens.org/limitations-of-mfa-and-common-techniques-to-bypass-mfa/
[5] 9 User Authentication Methods to Stay Secure in 2025 - StrongDM https://www.strongdm.com/blog/authentication-methods
[6] What is a FIDO Passkey? FIDO Alliance Passkeys Explained https://www.passkeys.com/fido-passkey
[7] Phish-resistant: FIDO Passkey - Beyond Identity https://www.beyondidentity.com/phishing-101/fido-passkey
[8] [PDF] A Survey on the Evolution of Cryptographic Protocols in ePassports https://eprint.iacr.org/2009/200.pdf
[9] tunnelled clear text passwords in sshd - Super User https://superuser.com/questions/772748/tunnelled-clear-text-passwords-in-sshd
[10] History of the Internet: The Development of PKI - GlobalSign https://www.globalsign.com/en/blog/sg/history-internet-development-pki
[11] AWS Signature Version 4 for API requests https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv.html
[12] [PDF] A Comprehensive Overview of FIDO & Passkeys - rf IDEAS https://www.rfideas.com/sites/default/files/2025-01/rfIDEAS_FIDO%20White%20Paper_WP-704.pdf
[13] How passkeys work, their benefits and downsides - Peter Brumby https://www.pbrumby.com/2023/11/29/how-passkeys-work-benefits-and-downsides/
[14] Authentication Protocols for Internet of Things: A Comprehensive ... https://onlinelibrary.wiley.com/doi/10.1155/2017/6562953
[15] The future of authentication: Predictions and trends for 2023 and ... https://www.linkedin.com/pulse/future-authentication-predictions-trends-2023-beyond-brian-ross
[16] What is Multi-Factor Authentication (MFA)? - CrowdStrike.com https://www.crowdstrike.com/en-us/cybersecurity-101/identity-protection/multifactor-authentication-mfa/
[17] CWE-312: Cleartext Storage of Sensitive Information https://cwe.mitre.org/data/definitions/312.html
[18] Public key infrastructure - Wikipedia https://en.wikipedia.org/wiki/Public_key_infrastructure
[19] Authenticating Requests (AWS Signature Version 4) https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html
[20] The MFA Protection Gaps No One Talks About - iC Consult https://ic-consult.com/en/resource/the-mfa-protection-gaps-no-one-talks-about?scfm-mobile=1
[21] MFA Trends 2025: Future of Multi-Factor Authentication (EN-US) https://emudhra.com/en-us/blog/mfa-solutions-trends-to-watch-out-for-in-2025
[22] Stop clear text credentials exposure - Microsoft Security Blog https://thalpius.com/2024/01/23/microsoft-defender-for-identity-recommended-actions-stop-clear-text-credentials-exposure/
[23] [PDF] A Concise History of Public Key Infrastructure https://cdn.ymaws.com/www.members.issa.org/resource/resmgr/JournalPDFs/History_of_PKI_ISSA0912.pdf
[24] Authentication methods - AWS Identity and Access Management https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv-authentication-methods.html
[25] Addressing the Limitations of Multi-Factor Authentication (MFA) https://www.portnox.com/blog/security-trends/addressing-the-limitations-of-multi-factor-authentication-mfa/
[26] Future Trends in Multi-Factor Authentication: What to Expect | OLOID https://www.oloid.ai/blog/future-trends-in-multi-factor-authentication/
[27] How Passkeys Work https://www.passkeycentral.org/introduction-to-passkeys/how-passkeys-work
[28] Passkeys and Regulatory Compliance: Aligning with PSD2, GDPR ... https://secfense.com/blog/passkeys-compliance-psd2-gdpr-hipaa-ccpa/
[29] A Survey on Key Agreement and Authentication Protocol for Internet ... https://ieeexplore.ieee.org/document/10508354/
[30] Get Started on Your Passwordless Journey - FIDO Alliance https://fidoalliance.org/implement-passkeys-overview/
[31] FIDO Authentication Guide: Passkey, WebAuthn & Best Practices https://doubleoctopus.com/blog/standards-regulations/your-complete-guide-to-fido-fast-identity-online/
[32] [1612.07206] Authentication Protocols for Internet of Things - arXiv https://arxiv.org/abs/1612.07206
[33] Introduction to Passkeys https://www.passkeycentral.org/introduction-to-passkeys/
[34] Are FIDO Passkeys Ready for Enterprise Use? - RSA Security https://www.rsa.com/resources/blog/passwordless/are-fido-passkeys-ready-for-enterprise-use/
[35] (PDF) Authentication Protocols and Techniques A Survey https://www.researchgate.net/publication/326553062_Authentication_Protocols_and_Techniques_A_Survey
[36] The future of passwords: Emerging technologies and trends https://specopssoft.com/blog/future-passwords-emerging-technologies/
