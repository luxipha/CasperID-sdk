# Changelog

All notable changes to the CasperID React Native SDK will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.1] - 2026-06-16

### Added
- Hosted authorization flow for React Native using `authorizeWithCasperID()`, `consumeHostedAuthorizationCallback()`, `useHostedAuthorizationCallback()`, and `<CasperIDButton>`
- Expo-based browser session support via `expo-web-browser`
- Deep link callback handling via `expo-linking`
- PKCE transaction handling for secure OAuth authorization

### Changed
- SDK is now a lightweight hosted authorization client for React Native
- Package metadata aligned to the hosted-only API surface
- Public entrypoint exports only the hosted authorization helpers and button component

### Fixed
- Expired hosted authorization transactions are now removed from `AsyncStorage` before surfacing the invalid-or-expired state error.

### Removed
- Legacy embedded modal flow is not part of the React Native SDK
- Embedded onboarding, passkey, and verification UI is no longer packaged in the SDK

### Technical
- Async transaction persistence uses `@react-native-async-storage/async-storage`
- Package exports now resolve directly to `dist/index.js`
- Patch release on top of the hosted authorization architecture introduced in `2.0.0`

[2.0.0]: https://github.com/luxipha/CasperID-sdk/releases/tag/v2.0.0
