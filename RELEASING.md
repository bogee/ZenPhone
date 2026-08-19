# Releasing ZenPhone

Git tags matching `v*` trigger `.github/workflows/release.yml`. The workflow builds and verifies the release APK, signs it with the permanent release identity, and publishes the APK, SHA-256 checksum, and public certificate to GitHub Releases.

## Release identity

The release signing key must remain unchanged for the lifetime of the app. Android will reject an update signed by a different identity.

Certificate SHA-256 fingerprint:

```text
6F:F2:F6:FC:91:C7:3F:D8:4B:56:33:B6:98:6E:A9:93:3F:CA:F0:BA:F0:56:32:6D:EC:2B:E0:87:13:83:4D:AB
```

The private keystore is not tracked by Git. The maintainer copy is stored at `~/.android/zenphone-release.jks`, its password is stored in macOS Keychain under `com.bogee.ZenPhone.release-keystore`, and encrypted copies are configured in these GitHub Actions secrets:

- `ZENPHONE_RELEASE_KEYSTORE_BASE64`
- `ZENPHONE_RELEASE_STORE_PASSWORD`
- `ZENPHONE_RELEASE_KEY_PASSWORD`

Back up the local keystore and Keychain password securely. GitHub secrets cannot be downloaded after they are saved.

## Publishing an update

1. Increase both `versionCode` and `versionName` in `app/build.gradle`.
2. Merge the tested change into `master`.
3. Create and push a tag equal to `v` plus `versionName`, for example `v1.1.1`.
4. Wait for the **Release APK** workflow and verify the published checksum and certificate.

Obtainium users should add `https://github.com/bogee/ZenPhone`. Future releases must use a higher `versionCode` and the same signing key.
