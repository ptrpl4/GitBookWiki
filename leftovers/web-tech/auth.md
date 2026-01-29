# Identification Authentication Authorization

#### links

- https://www.youtube.com/playlist?list=PLkZYeFmDuaN2pZOuMWjIfvZ6v2ZFp2jyK
- https://podlodka.io/388

## Basics

* Identification
  * Claiming an identity (get user data)
* Authentication
  * Process of verifying that identity (verify user data)
* Multi-Factor Authentication (MFA)
  * Two-Factor Authentication (2FA) - specific case of MFA
* Authorization
  * Process of checking permissions (provide access to service)

## Authentication strategies

- Basic auth - part of http spec. (RFC 7617)
	- base64 `username:password` in Authorization header
- Session-Based
	- Server creates and stores session after successful login
	- Client receives session ID (typically in cookie)
	- Session ID sent with subsequent requests
- Token-Based
	- Server issues a token after successful login 
	- Client includes token in subsequent requests (typically in Authorization header)
	- JWT [jwt](jwt.md) (JSON Web Token - Self-contained, encoded JSON with signature)

## Protocol Layer (Standards & Frameworks)

### OAuth 2.0

- **Type**: Authorization framework (NOT authentication)
- **Purpose**: Delegated access - allows applications to access user resources without sharing passwords
- **What it provides**: Access tokens for API authorization
- **What it doesn't provide**: User identity information
- **Key roles**:
  - Resource Owner (user)
  - Client (application requesting access)
  - Authorization Server (issues tokens)
  - Resource Server (API being accessed)
- **Common flows**:
  - Authorization Code Flow (most secure, for server-side apps)
  - Client Credentials Flow (machine-to-machine)
  - PKCE (Proof Key for Code Exchange) - security extension for public clients
- **Token formats**: Access tokens can be JWT or opaque (implementation choice)
- **Use case**: "Allow app X to access your Google Drive", third-party API access, delegated permissions
- **Note**: Often confused with authentication, but OAuth 2.0 alone doesn't tell you WHO the user is

#### OpenID Connect (OIDC)

- **Type**: Authentication protocol
- **Purpose**: User identity verification and authentication
- **Built on**: OAuth 2.0 (adds authentication layer on top)
- **What it provides**: 
  - ID Token (always JWT format) containing user identity claims
  - UserInfo endpoint for additional user information
  - Standardized authentication flows
- **Key difference from OAuth 2.0**: Returns ID Token with user identity, not just access token
- **Tokens**:
  - **ID Token**: Always JWT, contains claims about authentication event and user (sub, iss, aud, exp, etc.)
  - **Access Token**: Can be JWT or opaque, used for API access (inherited from OAuth 2.0)
- **Common flows**:
  - Authorization Code Flow (recommended)
  - Implicit Flow (deprecated, insecure)
  - Hybrid Flow
- **Pros**: Standardized identity layer, interoperable, includes user information, widely adopted
- **Cons**: More complex than simple token auth, requires proper implementation and validation
- **Use case**: "Sign in with Google/Facebook/etc", SSO implementations, user authentication, federated identity
- **Note**: OIDC = OAuth 2.0 + identity layer

### FedCM (Federated Credential Management)

- **Type**: Browser API for federated identity
- **Purpose**: Privacy-preserving federated authentication
- **Specification**: W3C standard
- **Why it exists**: Third-party cookies are being deprecated by browsers, breaking traditional OAuth/OIDC flows
- **How it works**: 
  - Browser acts as intermediary between website and Identity Provider
  - Native browser UI for IdP selection and consent
  - Token exchange happens through browser, not popups/redirects
  - No third-party cookies needed
- **Implementation**: JavaScript API (`navigator.credentials.get()`)
- **Current status**: 
  - Implemented in Chrome 108+ (stable)
  - Experimental/in progress in other browsers
  - Active W3C specification
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
