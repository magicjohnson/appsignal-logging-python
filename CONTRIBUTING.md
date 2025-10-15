# Contributing to AppSignal Logging for Python

Thank you for your interest in contributing! 🎉

## Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/appsignal-logging-python.git
   cd appsignal-logging-python
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install development dependencies**
   ```bash
   pip install -e ".[dev]"
   ```

## Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=appsignal_logging --cov-report=html

# Run specific test
pytest tests/test_handlers.py::TestAppSignalHTTPHandler::test_when_initialized_with_api_key_should_create_handler_with_defaults
```

## Code Quality

Before submitting a PR, ensure your code passes all checks:

```bash
# Format code
black src tests

# Lint code
ruff check src tests

# Type check
mypy src

# Run all checks
black src tests && ruff check src tests && mypy src && pytest
```

## Commit Messages

Follow conventional commits format:

- `feat:` new feature
- `fix:` bug fix
- `docs:` documentation changes
- `test:` test additions or changes
- `refactor:` code refactoring
- `chore:` maintenance tasks

Example:
```
feat: add support for custom flush intervals in NDJSON handler
```

## Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Update documentation if needed
7. Commit your changes (`git commit -m 'feat: add amazing feature'`)
8. Push to your fork (`git push origin feature/amazing-feature`)
9. Open a Pull Request

## Code Style

- Follow PEP 8
- Use type hints
- Write docstrings for public APIs
- Keep line length to 100 characters
- Use meaningful variable names

## Testing Guidelines

- Write tests for all new features
- Maintain test coverage above 90%
- Use descriptive test names: `test_when_<condition>_should_<outcome>`
- Mock external dependencies (httpx, network calls)
- Test edge cases and error scenarios

## Questions?

Feel free to open an issue for any questions or concerns!
