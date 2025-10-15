# Release Instructions

## Preparing for Release

### 1. Update Version

Update the version in the following files:
- `pyproject.toml` → `version = "0.1.0"`
- `src/appsignal_logging/__init__.py` → `__version__ = "0.1.0"`

### 2. Update CHANGELOG

Create or update `CHANGELOG.md` with a description of changes:

```markdown
# Changelog

## [0.1.0] - 2025-01-15

### Added
- Initial release
- AppSignalHTTPHandler for simple logging
- AppSignalNDJSONHandler for batched logging
- Full test coverage
- Documentation and examples
```

### 3. Run Tests

```bash
# Run all tests
pytest

# Check coverage
pytest --cov=appsignal_logging --cov-report=term-missing

# Ensure coverage > 90%
```

### 4. Run Linters

```bash
black src tests
ruff check src tests
mypy src
```

## Local Build and Testing

### 1. Clean Previous Builds

```bash
rm -rf dist/ build/ *.egg-info
```

### 2. Build the Package

```bash
python -m build
```

This will create files in the `dist/` directory:
- `appsignal_logging-0.1.0.tar.gz`
- `appsignal_logging-0.1.0-py3-none-any.whl`

### 3. Check the Package

```bash
twine check dist/*
```

### 4. Test Installation

```bash
# Create a clean virtual environment
python -m venv test_env
source test_env/bin/activate

# Install from the local file
pip install dist/appsignal_logging-0.1.0-py3-none-any.whl

# Verify installation
python -c "from appsignal_logging import AppSignalHTTPHandler, AppSignalNDJSONHandler; print('OK')"

# Deactivate and remove the virtual environment
deactivate
rm -rf test_env
```

## Publishing to Test PyPI

### 1. Register on Test PyPI

https://test.pypi.org/account/register/

### 2. Create an API Token

https://test.pypi.org/manage/account/token/

### 3. Publish to Test PyPI

```bash
twine upload --repository testpypi dist/*
```

Or with a token:

```bash
twine upload --repository testpypi \
  --username __token__ \
  --password pypi-YOUR-TEST-TOKEN \
  dist/*
```

### 4. Verify on Test PyPI

```bash
pip install --index-url https://test.pypi.org/simple/ \
  --extra-index-url https://pypi.org/simple/ \
  appsignal-logging
```

## Publishing to Production PyPI

### 1. Register on PyPI

https://pypi.org/account/register/

### 2. Create an API Token

https://pypi.org/manage/account/token/

### 3. Set Up GitHub Secret

In the GitHub repository:
1. Go to Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `PYPI_API_TOKEN`
4. Value: Your PyPI token

### 4. Create Git Tag and Release

```bash
# Create a tag
git tag -a v0.1.0 -m "Release version 0.1.0"

# Push the tag
git push origin v0.1.0
```

### 5. Create a GitHub Release

1. Go to https://github.com/yourusername/appsignal-logging-python/releases
2. Click "Draft a new release"
3. Select the tag `v0.1.0`
4. Fill in the release description
5. Click "Publish release"

**GitHub Actions will automatically publish the package to PyPI!**

### 6. Manual Publishing (if needed)

```bash
twine upload dist/*
```

Or with a token:

```bash
twine upload --username __token__ --password pypi-YOUR-PROD-TOKEN dist/*
```

## Verifying the Release

### 1. Check on PyPI

https://pypi.org/project/appsignal-logging/

### 2. Install from PyPI

```bash
pip install appsignal-logging
```

### 3. Verify Functionality

```bash
python -c "
from appsignal_logging import AppSignalHTTPHandler, AppSignalNDJSONHandler
import logging

handler = AppSignalNDJSONHandler('test_key')
logger = logging.getLogger()
logger.addHandler(handler)
logger.info('Test')
print('Success!')
"
```

## Post-Release Tasks

1. Update the version in the `main` branch to the next one (e.g., `0.2.0-dev`)
2. Announce the release (Twitter, Reddit, etc.)
3. Update documentation if needed
4. Close related issues

## Troubleshooting

### Upload Error to PyPI

```
ERROR    HTTPError: 403 Forbidden from https://upload.pypi.org/legacy/
```

**Solution:** Verify the API token is correct.

### Package Already Exists

```
ERROR    HTTPError: 400 Bad Request from https://upload.pypi.org/legacy/
         File already exists.
```

**Solution:** Increment the version in `pyproject.toml` and `__init__.py`.

### Dependency Issues

```
ERROR    InvalidDistribution: Cannot find: httpx>=0.24.0
```

**Solution:** Ensure all dependencies are listed in `pyproject.toml`.

## Useful Commands

```bash
# Show package information
pip show appsignal-logging

# Show dependencies
pip show appsignal-logging --requires

# Show installed files
pip show -f appsignal-logging

# Uninstall package
pip uninstall appsignal-logging -y
```