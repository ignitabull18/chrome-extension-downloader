# AGENTS.md - Agent Collaboration Guide

This file serves as a communication hub for AI agents working on this project. It contains project-specific rules, patterns, and important information for maintaining consistency across sessions.

## Byterover MCP Integration

You are given two tools from Byterover MCP server, including:

### 1. `byterover-store-knowledge`
You `MUST` always use this tool when:

+ Learning new patterns, APIs, or architectural decisions from the codebase
+ Encountering error solutions or debugging techniques
+ Finding reusable code patterns or utility functions
+ Completing any significant task or plan implementation

### 2. `byterover-retrieve-knowledge`
You `MUST` always use this tool when:

+ Starting any new task or implementation to gather relevant context
+ Before making architectural decisions to understand existing patterns
+ When debugging issues to check for previous solutions
+ Working with unfamiliar parts of the codebase

## Project-Specific Rules

### File Organization

1. **Keep files under 200 lines** when possible
   - If a file exceeds 200 lines, consider splitting into logical modules
   - Current files:
     - `chrome_extension_downloader.py`: ~670 lines (main script - acceptable for entry point)
     - `crx_utils.py`: ~422 lines (consider splitting if adding more features)

2. **Maintain clean codebase structure**
   - All Python files in root directory (acceptable for small project)
   - Configuration in `config.json`
   - Documentation files in root

### Documentation Requirements

1. **CHANGELOG.md** - MUST be updated when:
   - Adding new features
   - Fixing bugs
   - Making breaking changes
   - Updating dependencies

2. **CLAUDE.md** - Contains project context for AI agents
   - Update when architectural decisions change
   - Update when adding major features

3. **AGENTS.md** (this file) - Agent collaboration guide
   - Update when adding new patterns or rules
   - Document any agent-specific workflows

### Code Quality Standards

1. **Type Hints**: Use type hints throughout (Python 3.7+)
2. **Error Handling**: Follow existing patterns with try/except and logging
3. **Logging**: Use appropriate log levels (DEBUG, INFO, WARNING, ERROR)
4. **Configuration**: Centralize in `Config` class, use `config.json`
5. **File Operations**: Use `pathlib.Path` for file paths

### Package Management

**ALWAYS use `uv` instead of `pip`** for all package management operations:
- Install dependencies: `uv pip install -r requirements.txt`
- Install new packages: `uv pip install <package>`
- Update packages: `uv pip install --upgrade <package>`
- Never use `pip` directly - always use `uv pip` or `uv` commands
- This ensures faster, more reliable package installation

### Virtual Environment

**IMPORTANT: A virtual environment already exists at `.venv/`**
- The project has a pre-configured virtual environment created with `uv venv`
- **DO NOT create a new virtual environment** - use the existing one
- To activate: `source .venv/bin/activate` (on macOS/Linux) or `.venv\Scripts\activate` (on Windows)
- When running Python commands, either:
  - Activate the environment first: `source .venv/bin/activate && python chrome_extension_downloader.py`
  - Or use the venv Python directly: `.venv/bin/python chrome_extension_downloader.py`
- The `.venv/` directory is already in `.gitignore` - do not commit it
- If you need to recreate the environment (not recommended), use: `uv venv`

### Testing Considerations

- Test with valid extension IDs (32 chars, a-p only)
- Test CRX2 and CRX3 formats
- Test batch downloads
- Test error scenarios (invalid IDs, network failures)
- Test on different platforms if possible

### Common Patterns

#### Adding New Features

1. Check `CLAUDE.md` for project context
2. Use `byterover-retrieve-knowledge` to check for existing patterns
3. Follow existing code style and structure
4. **Use `uv` for any package management** (never use `pip` directly)
5. Update `CHANGELOG.md` with changes
6. Update `CLAUDE.md` if architecture changes
7. Use `byterover-store-knowledge` to save new patterns

#### Error Handling Pattern

```python
try:
    # Operation
    result = some_operation()
    logger.info(f"Success: {result}")
    return result
except SpecificException as e:
    logger.error(f"Error: {e}")
    raise ValueError(f"User-friendly message: {e}")
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    raise
```

#### Configuration Access Pattern

```python
# Access config values
value = self.config.config["section"]["key"]

# Override with command-line args if needed
if args.custom_value:
    self.config.config["section"]["key"] = args.custom_value
```

### Important Notes

1. **Virtual Environment**: A `.venv/` directory already exists - use it, don't create a new one
2. **SSL Verification**: Disabled by default for corporate compatibility
3. **Extension ID Validation**: Always validate before processing
4. **File Size Limits**: Enforced via config (default 100MB)
5. **Retry Logic**: 3 attempts with exponential backoff
6. **Platform Detection**: Auto-detects OS and architecture

### Project Status

- ✅ Core functionality complete
- ✅ Batch downloads working
- ✅ Configuration system in place
- ✅ Error handling implemented
- ✅ Documentation complete

### Known Limitations

1. Metadata extraction is simplified (not fully implemented)
2. No support for Chrome Apps (only extensions)
3. Caching is in-memory only (not persistent)
4. No extension version selection

### Future Enhancements

- Better metadata extraction from Chrome Web Store
- Support for Chrome Apps
- File-based caching
- Progress bars with tqdm
- Support for unpacked extensions
- Extension version selection

---

**Last Updated**: Initial setup
**Maintained By**: AI Agents working on this project
