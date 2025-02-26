# The Evolution of Single Sign-On Technologies

## Early Centralized User Management

The journey of authentication begins with simple centralized user management systems of the 1970s and 1980s. Organizations maintained their own user databases with username/password pairs stored in various formats:

- Early Unix systems stored hashed passwords in the `/etc/passwd` file
- Mainframe systems used proprietary authentication databases
- Network operating systems like Novell NetWare introduced directory services

These systems were isolated, requiring users to maintain separate credentials for each system they accessed.

## The Kerberos Revolution

Kerberos, developed at MIT in the late 1980s as part of Project Athena, was the first major SSO protocol. It introduced several revolutionary concepts:

- Ticket-based authentication
- Symmetric key cryptography for secure ticket exchange
- Authentication servers separate from application servers
- Time-limited session tickets

Kerberos used a "ticket-granting ticket" (TGT) approach where users authenticate once to the Key Distribution Center (KDC) and receive tickets for subsequent service access without re-authenticating.

# Detailed Examples of SSO Protocol Workflows

## Kerberos Authentication Example

Let's walk through a detailed Kerberos authentication flow:

1. **User Login & TGT Request**:
   ```
   Client → Authentication Server (AS):
   "User jsmith requests access, here's a timestamp encrypted with a key derived from jsmith's password"
   ```

2. **TGT Issuance**:
   ```
   AS → Client:
   "Here's a Ticket Granting Ticket (TGT) encrypted with the KDC's secret key, and a session key encrypted with the key derived from jsmith's password"
   ```
   
   The TGT contains:
   - Client ID: jsmith
   - TGS ID: krbtgt/EXAMPLE.COM
   - Timestamp: 2025-02-26T09:15:00Z
   - Lifetime: 10 hours
   - Session Key: 0x83F4A72B1C5D9E0A...
   - Client network address: 192.168.1.5

3. **Service Ticket Request**:
   ```
   Client → Ticket Granting Server (TGS):
   "Here's my TGT plus an authenticator (client ID and timestamp) encrypted with the session key, requesting access to the file server"
   ```

4. **Service Ticket Issuance**:
   ```
   TGS → Client:
   "Here's a service ticket for the file server encrypted with the file server's key, and a new session key encrypted with your TGT session key"
   ```

5. **Service Authentication**:
   ```
   Client → File Server:
   "Here's my service ticket and a new authenticator encrypted with the service session key"
   ```

6. **Service Verification** (optional):
   ```
   File Server → Client:
   "Here's the timestamp from your authenticator + 1, encrypted with the service session key"
   ```

Kerberos achieves SSO by maintaining the TGT in the client's credential cache, allowing it to obtain additional service tickets without re-entering credentials.

## SAML 2.0 Federation Example

Here's a detailed SAML 2.0 web browser SSO flow:

1. **Access Attempt**:
   User navigates to `https://app.example.com/dashboard`

2. **SAML Request Generation**:
   ```xml
   <samlp:AuthnRequest 
     xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
     ID="_809707f0030a5d00620c9d9df97f627afe9dcc24"
     Version="2.0"
     IssueInstant="2025-02-26T09:30:00Z"
     Destination="https://idp.company.org/SAML2/SSO/Redirect"
     AssertionConsumerServiceURL="https://app.example.com/saml/acs"
     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST">
     <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
       https://app.example.com
     </saml:Issuer>
   </samlp:AuthnRequest>
   ```

3. **Redirect to IdP**:
   Service Provider redirects browser to Identity Provider with AuthnRequest encoded and signed:
   ```
   https://idp.company.org/SAML2/SSO/Redirect?SAMLRequest=fZJNT%2BMwEIbvSPwHy%2Fd8tMEJqmqFwB YKSOlGhQWEqELIyS5OXbINzgTK3zONQskCx5nxvO%2BjD9%2F...&SigAlg=http%3A%2F%2Fwww.w3.org%2F2001%2F04%2Fxmldsig-more%23rsa-sha256&Signature=HWY%2FrN...
   ```

4. **User Authentication**:
   User authenticates at Identity Provider (if not already authenticated)

5. **SAML Assertion Generation**:
   ```xml
   <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
     ID="_93af655219464fb403b34436cfb0c5cb1d9a5502"
     IssueInstant="2025-02-26T09:33:00Z"
     Version="2.0">
     <saml:Issuer>https://idp.company.org</saml:Issuer>
     <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
       <ds:SignedInfo>
         <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
         <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
         <ds:Reference URI="#_93af655219464fb403b34436cfb0c5cb1d9a5502">...</ds:Reference>
       </ds:SignedInfo>
       <ds:SignatureValue>j5Kl6YbJfN...</ds:SignatureValue>
       <ds:KeyInfo>...</ds:KeyInfo>
     </ds:Signature>
     <saml:Subject>
       <saml:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress">
         janet.smith@company.org
       </saml:NameID>
       <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
         <saml:SubjectConfirmationData
           InResponseTo="_809707f0030a5d00620c9d9df97f627afe9dcc24"
           NotOnOrAfter="2025-02-26T09:38:00Z"
           Recipient="https://app.example.com/saml/acs"/>
       </saml:SubjectConfirmation>
     </saml:Subject>
     <saml:Conditions NotBefore="2025-02-26T09:30:00Z" NotOnOrAfter="2025-02-26T09:38:00Z">
       <saml:AudienceRestriction>
         <saml:Audience>https://app.example.com</saml:Audience>
       </saml:AudienceRestriction>
     </saml:Conditions>
     <saml:AuthnStatement AuthnInstant="2025-02-26T09:32:00Z" 
       SessionIndex="_a75adf55-01d7-40cc-929f-dbd8372ebdfc">
       <saml:AuthnContext>
         <saml:AuthnContextClassRef>
           urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
         </saml:AuthnContextClassRef>
       </saml:AuthnContext>
     </saml:AuthnStatement>
     <saml:AttributeStatement>
       <saml:Attribute Name="FirstName" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue>Janet</saml:AttributeValue>
       </saml:Attribute>
       <saml:Attribute Name="LastName" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue>Smith</saml:AttributeValue>
       </saml:Attribute>
       <saml:Attribute Name="Role" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue>Manager</saml:AttributeValue>
       </saml:Attribute>
     </saml:AttributeStatement>
   </saml:Assertion>
   ```

6. **SAML Response Creation**:
   The IdP wraps the assertion in a SAML Response:
   ```xml
   <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
     Destination="https://app.example.com/saml/acs"
     ID="_6c3a4f8b9c2d1e0f"
     InResponseTo="_809707f0030a5d00620c9d9df97f627afe9dcc24"
     IssueInstant="2025-02-26T09:33:00Z" 
     Version="2.0">
     <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
       https://idp.company.org
     </saml:Issuer>
     <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
     </samlp:Status>
     <!-- Assertion from above goes here -->
   </samlp:Response>
   ```

7. **Browser POST to Service Provider**:
   The Identity Provider returns an HTML form that automatically POSTs the SAML Response:
   ```html
   <form method="post" action="https://app.example.com/saml/acs">
     <input type="hidden" name="SAMLResponse" value="PHNhbWxwOlJlc3BvbnNlI..."/>
     <input type="hidden" name="RelayState" value="/dashboard"/>
     <input type="submit" value="Continue"/>
   </form>
   <script>document.forms[0].submit();</script>
   ```

8. **Service Provider Verification**:
   The SP validates the digital signature, checks conditions, and creates a local session.

9. **Access Granted**:
   User is redirected to the originally requested resource `/dashboard`.

## OAuth 2.0 & OpenID Connect Example

Here's a detailed OAuth 2.0 Authorization Code flow with OpenID Connect:

1. **Authorization Request**:
   ```
   GET https://auth.provider.com/authorize?
     response_type=code
     &client_id=s6BhdRkqt3
     &redirect_uri=https%3A%2F%2Fclient.example.org%2Fcallback
     &scope=openid%20profile%20email
     &state=af0ifjsldkj
     &nonce=n-0S6_WzA2Mj
   ```

2. **User Authentication & Consent**:
   User logs in at auth.provider.com and approves requested scopes

3. **Authorization Code Return**:
   ```
   HTTP/1.1 302 Found
   Location: https://client.example.org/callback?
     code=SplxlOBeZQQYbYS6WxSbIA
     &state=af0ifjsldkj
   ```

4. **Token Request**:
   ```
   POST /token HTTP/1.1
   Host: auth.provider.com
   Content-Type: application/x-www-form-urlencoded
   Authorization: Basic czZCaGRSa3F0Mzo3RmpmcDBaQnIxS3REUmJuZlZkbUl3

   grant_type=authorization_code
   &code=SplxlOBeZQQYbYS6WxSbIA
   &redirect_uri=https%3A%2F%2Fclient.example.org%2Fcallback
   ```

5. **Token Response**:
   ```json
   {
     "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2F1dGgucHJvdmlkZXIuY29tIiwic3ViIjoiMjQ4Mjg5NzYxMDAxIiwiYXVkIjoiczZCaGRSa3F0MyIsImV4cCI6MTU3Mjk2MDI3MSwiaWF0IjoxNTcyOTU2NjcxLCJzY29wZSI6InByb2ZpbGUgZW1haWwifQ.signature",
     "token_type": "Bearer",
     "expires_in": 3600,
     "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA",
     "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2F1dGgucHJvdmlkZXIuY29tIiwic3ViIjoiMjQ4Mjg5NzYxMDAxIiwiYXVkIjoiczZCaGRSa3F0MyIsImV4cCI6MTU3Mjk2MDI3MSwiaWF0IjoxNTcyOTU2NjcxLCJub25jZSI6Im4tMFM2X1d6QTJNaiIsIm5hbWUiOiJKYW5ldCBTbWl0aCIsImVtYWlsIjoiamFuZXQuc21pdGhAZXhhbXBsZS5jb20ifQ.signature"
   }
   ```

6. **ID Token Decoded**:
   Header:
   ```json
   {
     "alg": "RS256",
     "typ": "JWT",
     "kid": "1e9gdk7"
   }
   ```
   
   Payload:
   ```json
   {
     "iss": "https://auth.provider.com",
     "sub": "248289761001",
     "aud": "s6BhdRkqt3",
     "exp": 1572960271,
     "iat": 1572956671,
     "nonce": "n-0S6_WzA2Mj",
     "name": "Janet Smith",
     "email": "janet.smith@example.com"
   }
   ```

7. **Client Verification**:
   - Validates `iss` matches expected provider
   - Checks `aud` matches client ID
   - Verifies signature using provider's public key
   - Confirms `nonce` matches sent value to prevent replay attacks
   - Checks `exp` to ensure token is not expired

8. **Userinfo Request** (optional):
   ```
   GET /userinfo HTTP/1.1
   Host: auth.provider.com
   Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVC...
   ```

9. **Userinfo Response**:
   ```json
   {
     "sub": "248289761001",
     "name": "Janet Smith",
     "given_name": "Janet",
     "family_name": "Smith",
     "email": "janet.smith@example.com",
     "email_verified": true,
     "picture": "https://provider.com/photos/janet.jpg"
   }
   ```

## Cryptographic Implementation Details

Let's examine the cryptographic details in each protocol:

### Kerberos Cryptography

In Kerberos, multiple encryption operations occur:

1. **Key Derivation**: The user's key is derived from their password using a one-way function:
   ```
   User_Key = PBKDF2(password, salt, iterations)
   ```

2. **TGT Encryption**: The TGT is encrypted with the KDC's secret key:
   ```
   Encrypted_TGT = AES_Encrypt(TGT_Contents, KDC_Key)
   ```

3. **Session Key Protection**: The session key is encrypted with the user's key:
   ```
   Encrypted_Session_Key = AES_Encrypt(Session_Key, User_Key)
   ```

4. **Authenticator Creation**: The authenticator contains a timestamp and is encrypted with the session key:
   ```
   Authenticator = AES_Encrypt({ client_ID, timestamp }, Session_Key)
   ```

### SAML Signature Verification

SAML relies on XML Digital Signatures:

1. **Canonicalization**: Transform XML to canonical form:
   ```
   c14n_xml = Canonicalize(saml_assertion, "http://www.w3.org/2001/10/xml-exc-c14n#")
   ```

2. **Digest Calculation**: Generate a digest of the canonicalized content:
   ```
   digest_value = SHA256(c14n_xml)
   ```

3. **

## Microsoft Active Directory

Microsoft's Active Directory, introduced with Windows 2000, brought Kerberos to enterprise environments, combining it with LDAP directory services:

- Domain Controllers maintained centralized authentication
- Group Policy provided centralized authorization management
- Kerberos tickets enabled seamless access to network resources
- Trust relationships allowed cross-domain authentication

This marked the first widespread enterprise SSO implementation, but remained limited to Microsoft ecosystems.

## SAML: Federation Emerges

Security Assertion Markup Language (SAML), introduced in 2002, revolutionized SSO by enabling cross-domain authentication through XML-based assertions:

- Separated authentication (Identity Provider) from authorization (Service Provider)
- Enabled true federation across organizational boundaries
- Used digital signatures with asymmetric cryptography to verify assertions
- Introduced the browser redirect flow common in modern SSO

SAML example flow:
1. User attempts to access a Service Provider (SP)
2. SP redirects to Identity Provider (IdP) with SAML request
3. User authenticates at IdP
4. IdP generates signed SAML assertion
5. Browser posts assertion to SP's Assertion Consumer Service
6. SP verifies signature and grants access

## The OpenID Fragmentation

OpenID (2005) attempted to democratize identity provision but ultimately led to fragmentation:

- Anyone could become an Identity Provider
- Used URL-based identifiers (http://username.myopenid.com)
- Decentralized approach created too many identity options
- User experience suffered with confusing provider selection
- Provider shutdowns orphaned identities

Despite good intentions, the proliferation of identity providers without clear standards created a disjointed ecosystem that failed to gain lasting traction.

## OAuth: Authorization Framework

OAuth (2006, standardized in 2010) shifted focus to authorization rather than authentication:

- Designed for API access delegation without sharing passwords
- Introduced authorization codes, access tokens, and refresh tokens
- Enabled granular permission scopes
- Initially lacked standardized identity layer

Example OAuth 2.0 Authorization Code Flow:
1. Client redirects to authorization server with `client_id`, `redirect_uri`, and requested `scope`
2. User authenticates and approves permissions
3. Authorization server returns authorization code to redirect URI
4. Client exchanges code for access token using client secret
5. Client uses access token to access protected resources

Example OAuth access token (opaque string):
```
MDM0MmY3ZDAtOWRjYi00YjQ3LWIxMGMtN2NmZTRkZjJhMDk5
```

## OpenID Connect: Identity Layer for OAuth

OpenID Connect (OIDC), introduced in 2014, addressed OAuth's authentication limitations:

- Built as an identity layer on top of OAuth 2.0
- Standardized user info endpoints and claims
- Introduced ID tokens using JWT format
- Added support for different authentication flows (implicit, hybrid)

Example ID token (JWT format):
```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiZW1haWwiOiJqb2huQGV4YW1wbGUuY29tIiwiaXNzIjoiaHR0cHM6Ly9pZHAuZXhhbXBsZS5vcmciLCJhdWQiOiJteWNsaWVudGlkIiwiZXhwIjoxNTM2MjMzNDIxLCJpYXQiOjE1MzYyMjk4MjEsIm5vbmNlIjoiNzc3ODg4In0.signature
```

The JWT token consists of three parts:
1. Header (algorithm, token type)
2. Payload (claims about the user)
3. Signature (cryptographically signed content)

## Social Identity Providers

Major platforms like Google, Facebook, and Microsoft became de facto internet identity providers:

- Massive user bases made them natural identity hubs
- Implemented OAuth 2.0 and OIDC standards
- Reduced friction with "Sign in with Google/Facebook" buttons
- Introduced concept of social graphs tied to identity
- Raised privacy concerns about tracking across services

## Cryptography in Modern SSO

Modern SSO systems rely heavily on cryptography:

1. **Digital Signatures**:
   - SAML assertions use XML-DSIG with RSA or ECDSA
   - JWT tokens signed with algorithms like RS256 (RSA + SHA-256)
   - Prevents tampering and provides authenticity verification

2. **Token Encryption**:
   - JWE (JSON Web Encryption) for encrypted tokens
   - Transport layer security (TLS) for data in transit
   - Protects sensitive claims from unauthorized access

3. **Key Management**:
   - Public key infrastructure (PKI) for certificate management
   - JWKS (JSON Web Key Sets) endpoints for key distribution
   - Automated key rotation practices

4. **Cryptographic Challenges**:
   - Nonce values to prevent replay attacks
   - State parameters to prevent CSRF attacks
   - PKCE (Proof Key for Code Exchange) for public clients

## Current State and Future Trends

Today's SSO landscape features:

- **Passwordless Authentication**: FIDO2/WebAuthn standards enabling biometrics and security keys
- **Decentralized Identity**: Blockchain-based self-sovereign identity initiatives
- **Zero Trust Security**: Continuous authentication beyond initial sign-on
- **Adaptive Authentication**: Risk-based approaches that adjust security requirements contextually
- **Mobile Authentication**: Push notifications and app-based authentication methods

The evolution continues as organizations balance security, privacy, and user experience in an increasingly complex digital landscape.​​​​​​​​​​​​​​​​