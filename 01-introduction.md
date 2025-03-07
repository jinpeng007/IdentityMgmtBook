# My Journey Through Security Engineering: Five Key Lessons

I stumbled into security engineering by chance, but it turned out to be a lucky break. The journey wasn’t always easy, but I learned a ton from my mistakes. Looking back, there are five key principles that have guided my approach to building secure systems. 
1. the value of open source in security
2. the importance of simplicity
3. the power of standards
4. the necessity of composable design
5. the critical nature of scalability

Let me briefly share how I discovered these principles through my professional journey.

## The Power of Transparency: OpenSSL and Open Source Security
In 2000, what seemed like a simple request from my boss - "enable SSL for our signaling server" - launched my career in security. As a young engineer working on embedded telecommunication devices, I took on my first major security challenge with nothing but confidence of the youth and access to OpenSSL's source code.

With limited internet resources available, I dove deep into the world of cryptography, learning about RSA, SSL, and encryption by studying OpenSSL's source code. At the end I successfully ported the library to our proprietary operating system and created a proxy service that handled SSL connections. Sure, we made some trade off - like using self-signed certificates since server authentication was not a requirement in the air gapped network - but this experience taught me a crucial lesson: open source security software isn't just convenient; it's fundamental to building trust through transparency and promoting best practices.

## The Art of Simplicity: When Complex Security Becomes No Security
My next role was an entirely different challenge: leading a team to develop authentication and authorization features for a universal device management platform. Another org in the company was using Sun Microsystems' powerful XACML framework for their management platform, so we thought we had found the perfect solution. The framework could handle any access control scenario imaginable with its sophisticated Policy Decision Points and XML-based policy language.

We got caught up in a common mistake - letting the framework dictate the product instead of focusing on customers’ needs. Our customers, who were small groups of enterprise administrators who shared most of the responsibilities, needed something much simpler. We built a Ferrari when a bicycle would have been perfect. The platform was eventually scrapped, but the lesson stuck: security solutions need to be simple to be effective. Complexity isn’t just unnecessary; it can be dangerous.

## The Foundation of Standards: Betting on OpenID Connect
At my next position with a company focusing on Single Sign-On (SSO), I witnessed firsthand how choosing the right standard can make or break a product. While we were developing Cloud-based SSO solutions, the company made a bold bet on OpenID Connect (OIDC) when it was still in its early stages. This decision proved prescient - OIDC became the de facto standard for Cloud-based SSO and identity federation.

However, we also learned this lesson the hard way with our failed attempt to create a Cloud-based LDAP directory service. Microsoft's Active Directory had already cornered this market, leaving no room for newcomers. The experience crystallized another principle: in security, standards matter, and choosing the right one is crucial for success.

## The Beauty of Composability: Amazon's Security Building Blocks
At Amazon Identity, I helped revamp the password reset process by introducing composable validation - a concept that would shape my understanding of security design. Instead of traditional reset links, we created a one-time passcode widget that could verify account ownership through email or mobile phone. The innovation wasn't just in the widget itself, but in its composable nature.
We designed both the widget and its backend logic to be reusable components that could be integrated into any workflow, similar to the MicroFront idea of modern days. This modular approach allowed us to build a sophisticated risk-driven authentication system that could dynamically adjust security levels based on context, using the same widgets. The success of this project taught me that security controls should be designed like Lego blocks - security primitives are hard to build; we should build them like composable pieces that can be combined to create complex, secure workflows. The higher-level functions should be built on top of the solid foundation of the battle-hardened reusable primitives.

## Security at Scale: Lessons from AWS KMS
My last lesson came from scaling AWS Key Management Service (KMS) during a time of crazy growth. Traffic doubled every year, and we had problems with everything. When customers couldn’t get enough of the service, they often had to find workarounds, which might not be as secure. We focused on scaling, from scaling the transactional data store to building a data lake for analytics, and finally optimizing the hardware security modules. Each solution needed careful thinking about scalability. For a while, we almost throttled every caller. But gradually we were able to increase the KMS API quota from 2000 requests per second to 100,000 in large regions to meet customers’ needs - that’s a 50-fold increase! 

This experience drove home a critical principle: security solutions must scale not just technically, but operationally and in terms of security standards. When security services can't scale to meet demand, customers inevitably find workarounds - often at the expense of security itself.

These five lessons - the value of open source, the importance of simplicity, the power of standards, the necessity of composable design, and the critical nature of scalability - continue to guide my approach to security engineering. The rest of the book will show how these fundamental principles can help build more effective, more secure systems.