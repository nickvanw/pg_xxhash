# GitHub Actions Workflows

This directory contains GitHub Actions workflows for the pg_xxhash project:

## `installcheck.yml`
- **Purpose**: Runs regression tests for the extension
- **Trigger**: Push, pull request, manual dispatch
- **Matrix**: Tests against PostgreSQL versions 11-16
- **Original workflow**: Already existed in the project

## `release.yml`
- **Purpose**: Builds and publishes release binaries for multiple platforms
- **Trigger**: Release creation, git tags (v*), manual dispatch
- **Platforms**: 
  - Linux AMD64 (PostgreSQL 11-16)
  - macOS AMD64 (PostgreSQL 14-16)
- **Artifacts**: Creates tar.gz packages with compiled extension, SQL files, control file, and installation script
- **Release Publishing**: Automatically uploads artifacts to GitHub releases

## `test-build.yml`
- **Purpose**: Test workflow for validating the build process
- **Trigger**: Manual dispatch only
- **Use**: For testing changes to the build process before running the full release workflow

## `arm64-future.yml`
- **Purpose**: Experimental ARM64 cross-compilation workflow
- **Trigger**: Manual dispatch only (disabled by default)
- **Status**: Currently experimental - requires GitHub ARM64 runners or improved cross-compilation setup
- **Use**: Template for future ARM64 support

## Usage

### For Releases
1. Create a git tag: `git tag v0.2.0 && git push origin v0.2.0`
2. Or create a GitHub release via the web interface
3. The `release.yml` workflow will automatically build and publish binaries

### For Testing
- Use `test-build.yml` via manual dispatch to test specific platforms
- Use `arm64-future.yml` with `enable_arm64: true` to test ARM64 cross-compilation

## Release Artifacts

Each release creates packages in the format:
```
pg_xxhash-{os}-{arch}-pg{version}.tar.gz
```

Contents:
- `pg_xxhash.so` (or `.dylib` on macOS) - compiled extension
- `pg_xxhash.control` - extension metadata
- `pg_xxhash--0.1.sql` - extension SQL definitions
- `install.sh` - automated installation script
- `README.md` - installation instructions