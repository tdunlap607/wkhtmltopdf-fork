# CI/CD Status

This document describes the CI/CD workflows for this repository.

## Active Workflows

### Unpatched Build (Qt 5.15)
[![Unpatched](https://github.com/tdunlap607/wkhtmltopdf-fork/workflows/Unpatched/badge.svg)](https://github.com/tdunlap607/wkhtmltopdf-fork/actions/workflows/unpatched.yml)

**Purpose**: Build wkhtmltopdf and wkhtmltoimage using system Qt libraries (Qt 5.15).

**Triggers**:
- Push to `master` branch
- Pull requests to `master` branch
- Manual workflow dispatch

**Platform**: Ubuntu 22.04

**Build Steps**:
1. Install Qt 5.15 dependencies (WebKit, XMLPatterns, SVG)
2. Configure with qmake
3. Build with parallel make
4. Verify binaries are created and functional

**Notes**:
- Qt 4.8 builds removed (EOL 2015, unavailable on modern Ubuntu)
- Uses apt package caching for faster builds
- Validates binary versions after build

### CodeQL Security Scan
[![CodeQL](https://github.com/tdunlap607/wkhtmltopdf-fork/workflows/CodeQL%20Security%20Scan/badge.svg)](https://github.com/tdunlap607/wkhtmltopdf-fork/actions/workflows/codeql.yml)

**Purpose**: Automated security vulnerability detection for C++ code.

**Triggers**:
- Push to `master` branch
- Pull requests to `master` branch
- Weekly schedule (Mondays at 00:00 UTC)
- Manual workflow dispatch

**Platform**: Ubuntu 22.04

**Analysis**:
- C++ code scanning
- Results available in GitHub Security tab
- No external uploads

### Official Builds
[![Official](https://github.com/tdunlap607/wkhtmltopdf-fork/workflows/Official/badge.svg)](https://github.com/tdunlap607/wkhtmltopdf-fork/actions/workflows/official.yml)

**Purpose**: Build official packages using external wkhtmltopdf/packaging repository.

**Status**: ✅ Active

**Triggers**:
- Push to `master` branch
- Pull requests to `master` branch
- Manual workflow dispatch

**Platforms**:
- Linux (Docker-based on Ubuntu 22.04)
- macOS 13
- Windows 2022

**Build Process**:
- Uses external `wkhtmltopdf/packaging` repository
- Builds with Qt 4.8 via custom build scripts
- Creates platform-specific packages

**Notes**:
- No publishing steps - builds only
- Artifacts remain local to workflow run
- Requires complex multi-platform build environment

## Automation

### Dependabot
- **Enabled**: Yes
- **Scope**: GitHub Actions only
- **Schedule**: Weekly
- **Max PRs**: 5 concurrent
- **Labels**: `dependencies`, `github-actions`

## Publishing Status

⚠️ **IMPORTANT**: No publishing, releasing, or deployment workflows are enabled.

All workflows are configured for testing and validation only:
- ✅ No Docker image pushes
- ✅ No package registry uploads
- ✅ No GitHub Releases creation
- ✅ No artifact uploads to external services
- ✅ Security scan results stay in GitHub Security tab

## Maintenance

### Runner Images
All workflows use currently supported GitHub-hosted runner images:
- **Linux**: ubuntu-22.04 (LTS)
- **macOS**: macos-13 (when official builds re-enabled)
- **Windows**: windows-2022 (when official builds re-enabled)

### GitHub Actions Versions
All actions pinned to latest major versions:
- `actions/checkout@v4`
- `actions/cache@v4`
- `github/codeql-action/*@v3`

Dependabot will automatically create PRs to keep these updated.

## Local Development

To build locally with Qt 5:

```bash
# Install dependencies (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install -y \
  libqt5webkit5-dev \
  libqt5xmlpatterns5-dev \
  libqt5svg5-dev \
  qtbase5-dev

# Configure and build
qmake CONFIG+=silent
make -j$(nproc)

# Test binaries
LD_LIBRARY_PATH=bin bin/wkhtmltopdf --version
LD_LIBRARY_PATH=bin bin/wkhtmltoimage --version
```

## Future Improvements

Potential enhancements (to be done in separate PRs):
- Add unit tests
- Add integration tests
- Re-enable official package builds after validation
- Add code coverage reporting (internal only)
- Add performance benchmarking
