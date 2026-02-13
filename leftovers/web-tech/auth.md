# Identification Authentication Authorisation

#### links

- https://www.youtube.com/playlist?list=PLkZYeFmDuaN2pZOuMWjIfvZ6v2ZFp2jyK
- https://podlodka.io/388

## Basics

* Identification
  * Claiming an identity - "It's me"
* Authentication
  * Process of verifying that identity  - "Prove it"
* Multi-Factor Authentication (MFA)
  * Two-Factor Authentication (2FA) - specific case of MFA
* Authorization
  * Process of checking permissions - "What can you do"

flow:

"Its me" `login / email / username` =>  
=> "Prove it" `password / code / biometrics / key` =>  
=> "What can you do" `roles / scopes / permissions / policies`  

## Authentication strategies

How identity is verified on each request — mechanism and delivery method.

### delivery methods

- Common delivery methods: 
	- Cookie header — carries session ID, JWT, or any token
	- Authorization header — carries Basic credentials, Bearer token, API key
	- Query parameter — carries API key, token (less secure, visible in logs/URLs)

### mechanisms

- Basic auth - part of http spec. (RFC 7617) (stateless)
	- base64 `username:password` in Authorization header (encoding, not encryption!)
- Session-Based (stateful)
	- Server creates and stores session after successful login
	- Client receives session ID (typically in cookie)
	- Session ID sent with ALL subsequent requests
		- stateful - session creates on new login and authentication verifies on every new request
- Token-Based (stateless)
	- Server issues a token after successful login 
	- Client includes token in subsequent requests (typically in Authorization header)
		- stateless - server only checking that token is valid (signature)
	- JWT [jwt](jwt.md) (JSON Web Token - Self-contained, encoded JSON with signature)
- API Keys
	- Static key sent with each request (header or query param)
	- Identifies application/service, not a user
	- Common for server-to-server communication

## Protocol Layer (Standards & Frameworks)

### OAuth 2.0

OAuth 2.0 — authorization framework ([RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)) that standardizes how third-party applications obtain limited access to user resources through token-based delegation, without exposing user credentials.

- **Type**: Authorization framework (NOT authentication)
- **Purpose**: Delegated access - allows applications to access user **resources** without sharing passwords
- **What it provides**: Access tokens for API authorization
- **What it doesn't provide**: User identity information
- **Key roles**:
  - Resource Owner (user)
  - Client (application requesting access)
  - Authorization Server (issues tokens)
  - Resource Server (API being accessed)
- **Common flows**:
  - Authorization Code Flow (most secure, for server-side apps)
	  - PKCE (Proof Key for Code Exchange) - security extension for public clients
  - Client Credentials Flow (machine-to-machine)
- **Token formats**: Access tokens can be JWT or opaque (implementation choice)
- **Use case**: "Allow app X to access your Google Drive", third-party API access, delegated permissions
- **Note**: Often confused with authentication, but OAuth 2.0 alone doesn't tell WHO the user is

**technical markers:**

Required:
- /authorize and /token endpoints
- client_id, redirect_uri, response_type, scope, state in auth request
- grant_type in token request
- access_token + token_type: "Bearer" in response
- Authorization: Bearer `<token>` in API calls

Common but optional:
- refresh_token in response
- expires_in in response
- client_secret in token request (not for public clients)
- PKCE parameters (code_challenge, code_verifier)

#### OpenID Connect (OIDC)

who is exactly IdP?

- **Type**: Authentication protocol **Built on** OAuth 2.0 (adds authentication layer on top)
- **Purpose**: User identity verification and authentication
- **What it provides**: 
  - ID Token (always JWT format) containing user identity claims
  - UserInfo endpoint for additional user information
  - Standardized authentication flows
- **Key difference from OAuth 2.0**: Returns ID Token with user identity, not just access token
- **Tokens**:
  - **ID Token**: Always JWT, contains claims about authentication event and user (sub, iss, aud, exp, etc.)
  - **Access Token**: Can be JWT or opaque, used for API access (inherited from OAuth 2.0)
- **Use case**: "Sign in with Google/Facebook/etc", SSO implementations, user authentication, federated identity
- **Key concept**: Authorization Server (OAuth 2.0) becomes Identity Provider (IdP) 

**technical markers:**

Required:
- scope=openid in authorization request
- id_token in token response (always JWT)
- /.well-known/openid-configuration discovery endpoint
- ID Token claims: iss, sub, aud, exp, iat
- nonce parameter for replay protection

Common but optional:
- /userinfo endpoint
- Additional scopes: profile, email, address, phone
- Additional ID Token claims: name, email, picture, email_verified

##### FedCM (Federated Credential Management)

- [Info](https://developer.mozilla.org/en-US/docs/Web/API/FedCM_API)
- **Type**: Browser API for federated identity
- **Purpose**: Privacy-preserving federated authentication
- **Why it exists**: Third-party cookies are being deprecated by browsers, breaking traditional OAuth/OIDC flows
- **How it works**: 
  - Browser acts as intermediary between website and Identity Provider
  - Native browser UI for IdP selection and consent
  - Token exchange happens through browser, not popups/redirects
  - No third-party cookies needed
- **Implementation**: JavaScript API (`navigator.credentials.get()`)
- **Pros**: Privacy-preserving (prevents tracking), better UX (no popups), works without third-party cookies
- **Cons**: Limited browser support, requires IdP to implement FedCM endpoints, relatively new/evolving spec
- **Use case**: 
  - Modern "Sign in with X" implementations
  - Replacing cookie-based OAuth/OIDC flows
  - Privacy-compliant federated authentication for web apps
- **Migration path**: Fallback from traditional OIDC to FedCM as browsers phase out cookies

## Implementation Patterns

### Single Sign-On (SSO)

- **Concept**: One authentication grants access to multiple applications (approach)
- **Can be implemented using**:
  - OpenID Connect (OIDC) - modern standard
  - FedCM - browser-native approach
  - Custom token-sharing mechanisms
- **How it works**:
  - User authenticates once with central Identity Provider
  - Authentication session/token is shared across applications
  - Applications trust the Identity Provider's authentication
- **Pros**: 
  - Better user experience (single login)
  - Centralized user management
  - Reduced password fatigue
  - Simplified auditing
- **Cons**: 
  - Single point of failure
  - Session management complexity
  - Logout synchronization challenges

## The Third-Party Cookie Problem

**Traditional OAuth/OIDC flow**:
1. User visits website A
2. Website A redirects to Identity Provider (different domain)
3. IdP sets authentication cookie (third-party from website A's perspective)
4. User returns to other sites, IdP cookie enables SSO
5. **Problem**: Browsers are blocking third-party cookies for privacy

**Impact**: Traditional federated auth flows break without third-party cookies

**Solutions**:
- **FedCM**: Browser-native API that doesn't need cookies
- **First-party cookies**: Backend proxy pattern
- **Token-based flows**: Store tokens in localStorage/sessionStorage (with security considerations)

## Access control Types

- ABAC: Attribute-Based Access Control
- PBAC: Permission-Based Access Control
