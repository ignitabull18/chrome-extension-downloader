# CLAUDE.md - Project Context for AI Agents

This file provides essential context about the Chrome Extension Downloader project for AI agents working on this codebase.

## Project Overview

**Chrome Extension Downloader** is a Python 3.7+ tool that automatically downloads Chrome extensions from the Chrome Web Store, converts them from CRX to ZIP format, with automatic SSL bypass for better compatibility.

## Core Functionality

### Main Components

1. **`chrome_extension_downloader.py`** - Main entry point
   - `Config` class: Manages JSON-based configuration
   - `AutoExtensionDownloader` class: Handles downloads, validation, and conversion
   - Command-line interface with multiple modes
   - Interactive mode for non-technical users

2. **`crx_utils.py`** - Core utilities
   - `ChromeWebStoreURLBuilder` class: Constructs Chrome Web Store download URLs
   - Platform detection (OS, architecture)
   - CRX to ZIP conversion (CRX2 and CRX3 formats)
   - URL parsing from Chrome Web Store links

### Key Features

- **Single & Batch Downloads**: Download one or multiple extensions simultaneously
- **Configuration Management**: JSON-based configuration with sensible defaults
- **Concurrent Processing**: Multi-threaded downloads with configurable limits
- **Caching System**: Avoid re-downloading the same extensions
- **Progress Tracking**: Real-time download progress with file size information
- **Automatic SSL Bypass**: Works in corporate environments with SSL issues
- **Comprehensive Logging**: Detailed logging with multiple levels
- **Interactive Mode**: User-friendly interface for non-technical users

## Architecture

### Download Flow

1. **URL Construction**: Builds Chrome Web Store download URL using platform info
2. **Download**: Downloads CRX file with retry logic and progress tracking
3. **Conversion**: Converts CRX format to ZIP (handles CRX2 and CRX3)
4. **Validation**: Validates ZIP file integrity
5. **Cleanup**: Optionally removes temporary CRX files

### Configuration System

Configuration is managed via `config.json` with sections:
- `download`: Timeouts, retries, SSL settings, user agent
- `output`: Default directory, auto-cleanup, subdirectories
- `performance`: Concurrent downloads, chunk size, caching
- `security`: Validation, integrity checks, rate limiting

### Error Handling

- Extension ID validation (32 chars, a-p only)
- HTTP status code checking (204, 200)
- Retry logic with exponential backoff
- File size validation
- ZIP integrity verification
- Graceful error messages

## Technical Details

### Package Management

**IMPORTANT: Always use `uv` instead of `pip`**
- Install dependencies: `uv pip install -r requirements.txt`
- Install new packages: `uv pip install <package>`
- Update packages: `uv pip install --upgrade <package>`
- Never use `pip` directly - always use `uv pip` or `uv` commands
- `uv` provides faster, more reliable package installation

### Dependencies

- `requests>=2.25.0` - HTTP requests
- `urllib3>=1.26.0` - SSL/TLS handling
- `colorama>=0.4.4` - Terminal output
- `pyyaml>=6.0` - Configuration (optional)
- `typing-extensions>=4.0.0` - Type hints support

### Platform Detection

Auto-detects:
- OS: Windows, macOS, Linux
- Architecture: x86-32, x86-64, ARM
- Chrome version: Uses high version (9999.0.9999.0) to avoid 204 responses

### CRX Format Support

- **CRX2**: Older format with public key and signature
- **CRX3**: Newer format with header length
- **Nested CRX**: Handles Opera addons with nested CRX files
- **ZIP Detection**: Automatically detects if file is already ZIP

## Usage Patterns

### Command Line

```bash
# Single extension
python chrome_extension_downloader.py <extension_id>

# Batch download
python chrome_extension_downloader.py --batch <id1> <id2> ...

# From file
python chrome_extension_downloader.py --from-file extensions.txt

# Interactive mode
python chrome_extension_downloader.py --interactive
```

### Programmatic Usage

```python
from chrome_extension_downloader import AutoExtensionDownloader, Config

config = Config()
downloader = AutoExtensionDownloader(config)
result = downloader.download_and_convert("gppongmhjkpfnbhagpmjfkannfbllamg")
```

## File Structure

```
chrome-extension-downloader/
├── chrome_extension_downloader.py    # Main script
├── crx_utils.py                      # Core utilities
├── config.json                       # Configuration file
├── requirements.txt                  # Python dependencies
├── README.md                         # User documentation
├── CHANGELOG.md                      # Version history
├── CLAUDE.md                         # This file
├── AGENTS.md                         # Agent collaboration guide
└── LICENSE                           # MIT License
```

## Important Notes for AI Agents

### When Making Changes

1. **Always update CHANGELOG.md** when adding features or fixing bugs
2. **Keep files under 200 lines** when possible - split large files
3. **Update AGENTS.md** if architectural decisions change
4. **Maintain type hints** - this project uses Python 3.7+ typing
5. **Always use `uv` for package management** - never use `pip` directly
6. **Test error handling** - the app handles many edge cases

### Code Style

- Use type hints throughout
- Follow existing error handling patterns
- Maintain logging at appropriate levels
- Keep configuration centralized in Config class
- Use pathlib.Path for file operations

### Security Considerations

- SSL verification is disabled by default for corporate compatibility
- Extension IDs are validated before processing
- File size limits prevent abuse
- Rate limiting is configurable

### Testing Considerations

- Test with various extension IDs
- Test CRX2 and CRX3 formats
- Test batch downloads
- Test error scenarios (invalid IDs, network failures)
- Test on different platforms if possible

## Common Issues

### HTTP 204 Responses
- Some extensions may not be available for direct download
- Solution: Uses high Chrome version number to minimize this

### SSL Certificate Errors
- SSL verification is automatically disabled
- Works in corporate environments with SSL restrictions

### Invalid Extension IDs
- Must be exactly 32 characters (a-p only)
- Validated before download attempts

## Future Enhancements

Potential areas for improvement:
- Better metadata extraction from Chrome Web Store
- Support for Chrome Apps (not just extensions)
- Enhanced caching with file-based storage
- Progress bars with tqdm or similar
- Support for unpacked extensions
- Extension version selection

---

**Last Updated**: Initial setup
**Maintained By**: AI Agents working on this project

