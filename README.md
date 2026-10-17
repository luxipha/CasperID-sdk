# CasperID SDKs

React lives in `packages/react`; React Native lives in `packages/react-native`. Each package has its own lockfile and release version.
Run `npm ci` and `npm run build` inside either package. React tests run with `npm run test:run`.
CI builds both packages and checks npm package contents. Generated dist files are not source-controlled.
The manual Publish SDK workflow uses npm trusted publishing and the npm-release environment. Configure the trusted publisher on npm for this repository and `.github/workflows/publish.yml` before use. It requires a new unpublished version and publishes only on explicit dispatch.
No npm packages are published as part of repository extraction. The corrected React 2.0.2 package includes declarations and is distributed as the immutable `react-v2.0.2` GitHub release asset until an npm trusted publisher is configured. CasperDrop pins that tarball with a lockfile integrity hash.
