# HWI Builder

Automated release builds for [HWI (Hardware Wallet Interface)](https://github.com/bitcoin-core/HWI).

## Purpose

This repository provides automated GitHub Actions builds of HWI from the official [bitcoin-core/HWI](https://github.com/bitcoin-core/HWI) repository. It does **not** contain HWI source code—it only contains the build workflow that checks out and builds from the official upstream repository.

## Rationale

The Swan Bitcoin team strongly prefers using official releases from bitcoin-core/HWI. Unfortunately, the current official release process is manually performed by a single maintainer and we have been unable to coordinate with them to produce a new release in a timely manner. As of this writing, the most recent official release is over a year old and does not include critical device support features that have since been contributed to the project.

This repository is a temporary measure until an official release is published.

We have reviewed the changes contributed to bitcoin-core/HWI since the last official release and have not identified any significant risks in building and publishing binaries from the current codebase. However, we believe in the principle: **don't trust; verify.**

This is why we have chosen to create a build-only repository rather than a full fork. By isolating the build infrastructure from the source code, we make it straightforward to audit exactly what is being built. The workflow checks out a specific, documented commit from the official repository—nothing more, nothing less.

## Current Build Configuration

| Setting             | Value                                                                                            |
| ------------------- | ------------------------------------------------------------------------------------------------ |
| Upstream Repository | [bitcoin-core/HWI](https://github.com/bitcoin-core/HWI)                                          |
| Upstream Commit     | [`4e342db`](https://github.com/bitcoin-core/HWI/commit/4e342db25d58354cf838734173d2bf195cda32f9) |

## Build Artifacts

Each release includes binaries for:

| Platform | Architecture          | GUI |
| -------- | --------------------- | --- |
| Linux    | x86_64                | Yes |
| Linux    | aarch64               | No  |
| macOS    | x86_64 (Intel)        | Yes |
| macOS    | arm64 (Apple Silicon) | No  |
| Windows  | x86_64                | Yes |
| Python   | any                   | Yes |

## Verification

All releases include [artifact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds) providing cryptographic proof that:

1. The artifacts were built by GitHub Actions in this repository
2. The source was checked out from bitcoin-core/HWI at the specified commit
3. The artifacts have not been modified since the build

Verify any artifact with:

```bash
gh attestation verify <artifact> --repo <this-repo>
```

## Running a Build

Builds are triggered manually via GitHub Actions:

1. Go to Actions → "Build and Release HWI"
2. Click "Run workflow"
3. Enter:
   - **Version**: The version number (e.g., `3.1.1`)
   - **Upstream commit**: The commit hash from bitcoin-core/HWI to build
   - **Create release**: Whether to publish a GitHub release
