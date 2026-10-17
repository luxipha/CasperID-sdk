# @casperid/react

The official React SDK for integrating CasperID — the privacy-first, decentralized identity layer for the open web.

## Hosted Authorization

Passkeys registered for `casperid.com` must be used from the CasperID account domain. SDK v2 starts the hosted flow and does not embed identity or KYC screens in the partner application.

```tsx
import { authorizeWithCasperID } from '@casperid/react';

const result = await authorizeWithCasperID({
  clientId: 'app_your_client_id',
  redirectUri: `${window.location.origin}/auth/callback`,
  accountUrl: 'https://account.casperid.com',
  mode: 'auto',
  // Optional. Popup flows default to 30 minutes; set 0 to disable.
  timeoutMs: 30 * 60 * 1000,
});
```

`auto` opens a popup on desktop and redirects the full page on mobile. On the callback route, call `consumeHostedAuthorizationCallback()`. Send its `code`, `codeVerifier`, and `redirectUri` to your backend, which exchanges them at `POST /api/oauth/token` using the app secret. Never expose the app secret in frontend code.

## Why CasperID?

CasperID provides a "Sign in with Google" style experience but with the security and ownership of Web3.

- **Biometric Security:** Native Passkey (WebAuthn) support for passwordless, secure logins.
- **Privacy by Design:** Selective disclosure via Zero-Knowledge Proofs (ZKP). Only share what you need (e.g., "Over 18") without revealing raw data.
- **User Ownership:** Identities (DIDs) are owned by users on the Casper blockchain, not by a central authority.
- **Verification Tiers:** Securely handle everything from simple email verification (L1) to full KYC document verification (L3).

## Installation

```bash
npm install @casperid/react
```

## Quick Start

```tsx
import { CasperIDButton, useHostedAuthorizationCallback } from '@casperid/react';

function App() {
  const handleSuccess = (result) => {
    // Send result.code, result.codeVerifier, and result.redirectUri to your backend.
  };
  useHostedAuthorizationCallback(handleSuccess, console.error);

  return (
    <CasperIDButton
      clientId="app_your_client_id"
      redirectUri={`${window.location.origin}/auth/callback`}
      mode="auto"
      timeoutMs={30 * 60 * 1000}
      onSuccess={handleSuccess}
      onError={console.error}
    >
      Continue with CasperID
    </CasperIDButton>
  );
}
```

## Backend Exchange

Exchange the authorization code server-side at `POST /api/oauth/token` using your app secret and PKCE verifier. Never expose the app secret in browser code.

### Understanding User Identity

The returned business token includes:

- **`humanId`**: This is the unique, user-friendly handle (e.g., `alice.casper`). **This should be prioritized for your UI.**
- **`name`**: The user's verified full name (or `null` if not shared).
- **`wallet`**: The Casper wallet address.
- **`authorization_status`**: `complete` or `provisional`.
- **`required_tier` / `tier`**: The app's required tier and the user's actual tier.
- **`scopes`**: The permissions approved by the user.

## Documentation

For full documentation, visit [casperid.com/business/docs](http://casperid.com/business/docs).

## License

MIT
