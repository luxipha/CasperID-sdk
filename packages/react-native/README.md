# @casperid/react-native

React Native SDK for CasperID hosted authorization.

## Installation

```bash
npm install @casperid/react-native expo-crypto expo-web-browser expo-linking @react-native-async-storage/async-storage
```

## Setup

The SDK requires Expo modules. Make sure your project is configured with Expo or has the required native modules installed.

### Bare React Native

If you're not using Expo managed workflow, install the native peer dependencies:

```bash
npx expo install expo-crypto expo-web-browser expo-linking @react-native-async-storage/async-storage
```

### Expo

Expo managed workflow includes these modules by default. Just add the SDK:

```bash
npx expo install @casperid/react-native
```

## Usage

```tsx
import {
    CasperIDButton,
    useHostedAuthorizationCallback,
    authorizeWithCasperID,
    buildHostedAuthorizationUrl,
    consumeHostedAuthorizationCallback,
} from '@casperid/react-native';

// Option 1: Button component
function App() {
    return (
        <CasperIDButton
            clientId="your-client-id"
            redirectUri="casperid://callback"
            onSuccess={(result) => {
                console.log('code:', result.code);
                // Exchange code for tokens on your backend
            }}
            onError={(error) => {
                console.error(error);
            }}
        />
    );
}

// Option 2: Hook for redirect callback
function App() {
    useHostedAuthorizationCallback(
        (result) => {
            console.log('code:', result.code);
        },
        (error) => {
            console.error(error);
        }
    );

    // ... rest of your app
}

// Option 3: Programmatic
async function startAuthorization() {
    const result = await authorizeWithCasperID({
        clientId: 'your-client-id',
        redirectUri: 'casperid://callback',
    });
    console.log(result.code);
}
```

## Deep Linking

Configure a custom URL scheme in your `app.json` (Expo) or native project:

```json
{
    "expo": {
        "scheme": "casperid"
    }
}
```

## API

### `authorizeWithCasperID(options: HostedAuthorizationOptions): Promise<HostedAuthorizationResult>`

Opens the CasperID authorization page and returns the result.

### `buildHostedAuthorizationUrl(options: HostedAuthorizationOptions): Promise<{ url: string; state: string }>`

Builds the authorization URL with PKCE and stores the transaction.

### `consumeHostedAuthorizationCallback(url: string): Promise<HostedAuthorizationResult | null>`

Parses and validates a redirect callback URL.

### `CasperIDButton`

A `Pressable` component that triggers authorization on press.

### `useHostedAuthorizationCallback(onSuccess, onError?)`

A hook that listens for deep link callbacks and consumes them.
