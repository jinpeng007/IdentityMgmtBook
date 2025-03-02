# Evolution of Authentication Protocols: From Cleartext Passwords to Phishing-Resistant Cryptography  

Protocols are the rules that must be followed by participants or entities in some particular interactions. Thus, network protocols are the rules followed in networked communication systems. Security protocols are the communication rules followed in security applications. Cryptographic protocols are the rules that apply cryptographic techniques so that participants or entities can communicate securely. Authentication protocols fall into the security and cryptographic protocol categories and can be defined as a set of rules that must be followed for entities to prove or confirm their identity. That is, Alice must prove to Bob that she is really Alice, or Bob must prove to Alice that he is really Bob.

The landscape of digital authentication has undergone radical transformation since the dawn of computing, driven by escalating cybersecurity threats and technological innovation. This paper chronicles the progression from rudimentary cleartext password systems to modern cryptographic frameworks like AWS Signature Version 4 (SigV4), FIDO passkeys, and multi-factor authentication (MFA). Each evolutionary stage addressed critical vulnerabilities of its predecessors while introducing new challenges, ultimately shaping today’s emphasis on phishing-resistant, passwordless architectures. Key milestones include the shift from plaintext transmission to cryptographic hashing, the rise of public key infrastructure (PKI) enabling HTTPS, and the advent of asymmetric protocols like FIDO that eliminate shared secrets. However, persistent limitations—such as certificate authority vulnerabilities in PKI, brute-force risks in improperly implemented MFA, and device dependency in passkeys—underscore the need for continuous innovation. Emerging trends point toward decentralized identity models, quantum-resistant algorithms, and biometric-integrated authentication ecosystems.  

# The Evolution of Authentication Mechanisms: From Passwords to Modern Authentication

In the ever-evolving landscape of digital security, authentication mechanisms stand as the primary gatekeepers protecting our most sensitive information. These systems have undergone remarkable transformation over the decades, driven by the relentless advancement of technology, escalating security threats, and changing user expectations. What began as simple text-based passwords has blossomed into a sophisticated ecosystem of biometric recognition, hardware tokens, and artificial intelligence-driven security frameworks. This paper traces this fascinating journey, exploring how authentication has adapted to meet the challenges of an increasingly complex digital world.

## Early Password-Based Authentication (1960s-1980s)

The dawn of digital authentication was remarkably straightforward by today's standards. In the 1960s, when multi-user computing systems first emerged, authentication mechanisms were rudimentary at best. Early systems stored passwords as plain text in system files, with little consideration for what we now recognize as fundamental security principles. These pioneering systems relied on simple username and password pairs, providing basic access control without encryption or hashing of credentials. Password complexity requirements were virtually non-existent; users could select any combination of characters, including notoriously weak choices like "password" or their own names.

As security threats began to emerge, password storage methods evolved in response. Systems transitioned from plain text storage to basic encryption techniques, offering minimal protection against unauthorized access. However, the true breakthrough came with the implementation of one-way hashing algorithms. Rather than storing the actual password, systems began storing a cryptographic "fingerprint" of the password, making it computationally infeasible to reverse-engineer the original credential. Early hashing algorithms like MD5 and SHA-1 represented significant advancements, though they would later prove vulnerable to various attacks.

The introduction of "salt" — random data added to passwords before hashing — marked another critical development. This addition prevented attackers from using precomputed tables (rainbow tables) to crack multiple passwords simultaneously. As computing power increased, security experts implemented multiple hash iterations to increase the computational cost of password cracking attempts. Modern password hashing algorithms like bcrypt, PBKDF2, and Argon2 incorporate adaptive work factors and memory-hard functions, making them resistant to brute force attacks even as hardware capabilities continue to advance.

In parallel with these technical developments, organizations began implementing increasingly sophisticated password policies. Minimum length requirements forced users to create longer passwords with greater entropy. Character complexity rules mandated combinations of uppercase letters, lowercase letters, numbers, and special characters. Password expiration policies required regular credential updates, while history enforcement prevented users from recycling previous passwords. Account lockout mechanisms emerged as a defense against brute force attacks, temporarily disabling accounts after multiple failed authentication attempts.

Despite these advancements, the fundamental limitations of password-based authentication became increasingly apparent. Users struggled to memorize complex passwords, leading to dangerous workarounds like writing credentials on sticky notes or reusing passwords across multiple systems. Organizations needed more robust authentication methods, setting the stage for the next phase in authentication evolution.

## Challenge-Response Protocols (1980s-1990s)

The 1980s saw the emergence of challenge-response protocols, addressing a critical vulnerability in traditional password systems: the transmission of authentication credentials across networks. These protocols introduced a more sophisticated approach where the server would issue a random challenge, and the client would prove knowledge of a secret without actually transmitting it. This methodology effectively protected against replay attacks, where an attacker might capture and retransmit authentication traffic to gain unauthorized access.

Challenge Handshake Authentication Protocol (CHAP) exemplified this approach with its three-way handshake process. Using MD5-based challenge responses, CHAP represented a significant improvement over earlier Password Authentication Protocol (PAP), which transmitted credentials in plaintext. CHAP found widespread adoption in point-to-point protocols, providing a more secure foundation for network authentication.

Perhaps the most significant development of this era was the Kerberos authentication protocol, developed at MIT as part of Project Athena. Kerberos introduced ticket-based authentication, allowing users to authenticate once with a Key Distribution Center (KDC) and receive time-limited credentials (tickets) for accessing various network services. This ticket-based approach enabled single sign-on capabilities and mutual authentication between clients and servers. Kerberos remains relevant today, forming the backbone of authentication in many enterprise environments, including Microsoft Active Directory.

These challenge-response protocols addressed many limitations of simple password systems, but as the internet expanded and e-commerce emerged, even more robust authentication methods were needed to secure online transactions and communications.

## Certificate-Based Authentication (1990s)

The expansion of the internet in the 1990s created unprecedented security challenges. Users needed to securely communicate with organizations they had never previously encountered, without established trust relationships. Certificate-based authentication emerged as a solution to this problem, built upon Public Key Infrastructure (PKI).

PKI introduced digital certificates — electronic documents that bind a public key to an identity, verified by trusted third parties known as Certificate Authorities (CAs). This system utilized asymmetric cryptography with public/private key pairs, allowing secure authentication without shared secrets. Certificate Revocation Lists (CRLs) and later the Online Certificate Status Protocol (OCSP) provided mechanisms to invalidate compromised certificates, adding another layer of security to the PKI ecosystem.

Smart card authentication represented a physical implementation of certificate-based security. These tamper-resistant cards contained digital certificates and private keys, protected by Personal Identification Numbers (PINs). By combining possession (the physical card) with knowledge (the PIN), smart cards introduced multi-factor authentication to enterprise environments, significantly raising the bar for attackers.

Client certificate authentication extended these concepts to web environments through Secure Sockets Layer/Transport Layer Security (SSL/TLS) protocols. Mutual TLS (mTLS) enabled both servers and clients to authenticate each other using certificates, creating highly secure communication channels. While browser-based certificate management proved somewhat cumbersome for typical users, enterprise PKI deployments successfully implemented these technologies for high-security applications.

Certificate-based authentication addressed many limitations of password systems, particularly for securing communications across untrusted networks. However, managing certificates across multiple systems and applications remained challenging, driving demand for more integrated authentication approaches.

## Single Sign-On Evolution (2000s)

The proliferation of web applications in the early 2000s created a new authentication challenge: users needed to maintain separate credentials for dozens or even hundreds of services. Single Sign-On (SSO) emerged as a solution to this problem, allowing users to authenticate once and access multiple applications without re-entering credentials.

Early web-based SSO implementations relied on cookie-based systems. Domain cookies stored authentication information that could be accessed by applications within the same domain. Various techniques emerged to enable cross-domain authentication, though these often required complex session management to maintain security.

As web applications became more sophisticated, token-based systems gained popularity. JSON Web Tokens (JWT), SAML assertions, OAuth tokens, and OpenID Connect ID tokens provided standardized formats for sharing authentication information across application boundaries. These tokens contained digitally signed claims about user identity and permissions, enabling secure authentication across distributed systems.

Enterprise environments adopted formal SSO protocols to address their specific security and compliance requirements. Security Assertion Markup Language (SAML) emerged as a dominant standard, using XML-based assertions to communicate authentication information between identity providers and service providers. SAML's rich attribute support enabled fine-grained access control based on user properties beyond simple identity. Meanwhile, WS-Federation provided similar functionality for Windows-centric environments, with tight integration into Microsoft identity systems and claims-based authentication.

As SSO systems matured, they incorporated increasingly sophisticated features. Step-up authentication allowed systems to request additional verification when users attempted to access sensitive resources. Adaptive authentication adjusted security requirements based on context, while risk-based authentication incorporated real-time threat analysis. Device fingerprinting and behavioral analytics added invisible layers of security, enhancing protection without burdening users with additional authentication steps.

The evolution of SSO technologies dramatically improved both security and user experience, but even these sophisticated systems remained vulnerable to certain attack vectors. The next phase of authentication development would address these remaining vulnerabilities through multi-factor approaches.

## Multi-Factor Authentication (2010s)

The 2010s witnessed the mainstream adoption of multi-factor authentication (MFA), fundamentally changing the authentication landscape by requiring users to verify their identity through multiple independent methods. Security experts categorized these methods into three factor categories: something you know, something you have, and something you are.

"Something you know" encompasses traditional knowledge-based factors like passwords, PINs, and security questions. While these methods remained foundational, security professionals increasingly recognized their limitations when used in isolation.

"Something you have" refers to physical possession factors: hardware tokens, smart cards, mobile devices, and security keys. These tangible authenticators provided significantly stronger security than knowledge factors alone, as attackers would need physical access to the device to compromise authentication.

"Something you are" incorporates biometric factors like fingerprints, facial recognition, voice recognition, and iris scanning. These inherent physical characteristics offered convenience alongside security, though implementation challenges and privacy concerns initially limited their adoption in certain contexts.

Time-Based One-Time Passwords (TOTP), standardized in RFC 6238, became one of the most widely deployed second factors. This approach generated synchronized, time-based codes typically displayed through mobile authenticator applications. TOTP represented a "soft token" implementation that balanced security and convenience, though it remained vulnerable to sophisticated phishing attacks.

Hardware security keys emerged as a particularly robust authentication factor, with the FIDO Universal Second Factor (U2F) protocol and later FIDO2/WebAuthn standards providing phishing-resistant authentication through NFC and USB interfaces. These devices cryptographically verified both the user and the service, preventing credential theft even if users were tricked into accessing fraudulent websites.

Multi-factor authentication dramatically raised the security bar, addressing many vulnerabilities of single-factor systems. However, the additional authentication steps sometimes created friction in the user experience, driving innovation toward more seamless approaches.

## Modern Authentication Methods (2020s)

The 2020s have been characterized by a push toward passwordless authentication — eliminating passwords entirely in favor of stronger, more user-friendly alternatives. Email magic links emerged as an early passwordless approach, sending one-time login links with limited time validity to users' email addresses. Upon clicking these links, users could register their devices for future authentication, streamlining subsequent logins.

Push notifications extended this concept further, allowing users to approve authentication requests through secured mobile applications. These systems provided rich contextual information about login attempts, enabling users to identify and reject suspicious authentication requests. By communicating through secure, dedicated channels, push notifications significantly reduced vulnerability to phishing attacks.

Biometric authentication has become increasingly mainstream, with implementations like TouchID/FaceID (Apple), Windows Hello (Microsoft), and various Android biometric APIs making facial and fingerprint recognition commonplace. Secure enclaves and trusted execution environments protect biometric data, addressing many privacy concerns associated with these technologies.

Perhaps the most significant recent development has been the widespread adoption of FIDO2 and WebAuthn standards. These technologies support both platform authenticators (built into devices) and roaming authenticators (external security keys), with capabilities for resident credentials and user verification. The attestation mechanisms in these standards ensure the authenticity of authentication devices, preventing sophisticated spoofing attacks.

Building on FIDO2/WebAuthn foundations, passkeys have emerged as a particularly promising authentication method. These synchronized credentials leverage platform integration for seamless cross-device authentication, effectively replacing passwords with cryptographically secure alternatives that resist phishing and credential theft. Major platform providers including Apple, Google, and Microsoft have embraced passkeys, signaling a potential paradigm shift in authentication practices.

Modern authentication increasingly incorporates continuous and risk-based approaches that extend beyond the traditional login event. Risk-based authentication analyzes user behavior, device characteristics, location data, and other contextual factors to construct real-time risk assessments. Machine learning models identify anomalous patterns that might indicate compromise, triggering additional verification when necessary. These adaptive policies maximize security while minimizing user friction during legitimate access attempts.

Zero Trust authentication frameworks have gained prominence, embracing the principle of "never trust, always verify." These architectures implement continuous validation and context-aware access controls, verifying user identity and device security posture for each resource request. Policy-based enforcement ensures consistent security across diverse environments, while real-time risk assessment enables dynamic response to emerging threats.

Behavioral biometrics represent another frontier in continuous authentication. These systems analyze patterns in typing behavior, mouse movements, touchscreen interactions, gait, and voice to create unique user profiles. By continuously comparing current behavior against established patterns, systems can detect potential account compromise even after initial authentication, providing an invisible security layer that doesn't interrupt legitimate users.

## Future Trends

As we look toward the future of authentication, several emerging technologies show particular promise. Quantum-resistant algorithms are being developed to maintain security in a post-quantum computing world, where current cryptographic approaches may become vulnerable. Decentralized identity systems aim to give users greater control over their digital identities, reducing dependence on centralized identity providers. Blockchain-based authentication offers immutable identity verification with transparency and auditability. Artificial intelligence continues to enhance authentication systems through improved anomaly detection and adaptive security policies. Ambient authentication — which verifies identity through environmental signals without explicit user action — represents perhaps the ultimate balance of security and convenience.

Integration trends point toward more cohesive authentication ecosystems. Identity orchestration platforms are streamlining management of complex authentication workflows across diverse applications and environments. Authentication workflow automation reduces operational overhead while ensuring consistent security enforcement. Cross-platform identity solutions are breaking down silos between different technology ecosystems. Internet of Things (IoT) device authentication is addressing the unique challenges of securing billions of connected devices, while edge authentication is bringing verification capabilities closer to users for improved performance and resilience.

Throughout these technological developments, user experience remains a critical focus. Frictionless authentication aims to minimize disruption while maintaining security. Progressive security approaches apply appropriate authentication strength based on risk, rather than enforcing maximum security for all interactions. Context-aware methods intelligently adapt to user circumstances, while self-service capabilities empower users to manage their own identity information. Privacy-preserving design ensures authentication strengthens rather than undermines personal data protection.

## Conclusion

The evolution of authentication mechanisms reflects an ongoing journey to balance security requirements with user experience in an increasingly complex digital landscape. From simple passwords to sophisticated multi-factor and continuous authentication systems, each advancement has introduced new capabilities while addressing the limitations of previous approaches.

This historical progression reveals several enduring principles for effective authentication. Defense in depth — implementing multiple security layers — provides resilience against diverse attack vectors. Continuous adaptation remains essential as threats and technologies evolve. Standards-based approaches enable interoperability across diverse systems, critical for enterprise-scale deployment. User experience considerations directly impact security through compliance and adoption rates. Risk-based decisions optimize the balance between protection and accessibility.

As authentication continues to evolve, organizations face both challenges and opportunities. Those that embrace modern authentication frameworks while maintaining flexibility for future advancements will be best positioned to protect their digital assets while providing seamless experiences for legitimate users. The lessons from authentication's past provide valuable guidance for navigating its future, as we continue seeking the optimal balance between security, privacy, and usability in our increasingly connected world.

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
