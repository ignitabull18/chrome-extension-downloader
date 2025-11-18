# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **Automatic ZIP extraction**: Extensions are now automatically extracted to a configurable directory after download
  - New configuration options: `extract_directory` and `auto_extract` in `output` section
  - Default extraction directory: `/Users/ignitabull/Desktop/Development/Browser Extensions`
  - Extracted files are organized in folders named `{extension_name}_{extension_id}`
  - Existing extraction directories are automatically removed before re-extraction
- Initial project setup with CHANGELOG.md, CLAUDE.md, and updated AGENTS.md
- Project documentation and rules files
- Package management rule: Always use `uv` instead of `pip` for faster, more reliable installations

## [1.0.0] - Initial Release

### Features
- Download Chrome extensions from Chrome Web Store by extension ID
- Convert CRX files to ZIP format (supports CRX2 and CRX3)
- Batch download support for multiple extensions
- Interactive mode for user-friendly interface
- Configuration management via JSON config file
- Concurrent downloads with configurable worker limits
- Download caching system
- Progress tracking with file size information
- Automatic SSL bypass for corporate environments
- Comprehensive logging system
- File integrity validation
- Retry logic with exponential backoff
- Platform auto-detection (OS, architecture)
- Support for downloading from file lists

### Technical Details
- Python 3.7+ support
- Uses Chrome Web Store internal API
- Handles both CRX2 and CRX3 file formats
- Supports nested CRX files (Opera addons)
- Type hints throughout codebase
- Error handling and validation

---

## Notes for AI Agents

This changelog should be updated whenever:
- New features are added
- Bug fixes are implemented
- Breaking changes are made
- Dependencies are updated
- Configuration options change

Please maintain this file to help other AI agents understand the project's evolution.

