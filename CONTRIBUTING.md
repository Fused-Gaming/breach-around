# Contributing to Breach Around

Thank you for your interest in contributing to Breach Around! We welcome contributions from the community to help improve this security tool.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Contribution Workflow](#contribution-workflow)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Security Considerations](#security-considerations)

---

## 🤝 Code of Conduct

### Our Standards

- **Respectful**: Treat all contributors with respect and professionalism
- **Collaborative**: Work together to solve problems
- **Constructive**: Provide helpful feedback and suggestions
- **Inclusive**: Welcome contributors of all experience levels
- **Ethical**: Maintain the integrity of security research

### Unacceptable Behavior

- Harassment or discrimination of any kind
- Sharing of exploits or vulnerabilities publicly before responsible disclosure
- Using the tool or its development for illegal activities
- Violating the privacy or security of others

---

## 🛠️ How Can I Contribute?

### Reporting Bugs

Found a bug? Help us fix it!

1. **Check existing issues** to avoid duplicates
2. **Use the bug report template** when creating an issue
3. **Include details**:
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment (OS, Python version, etc.)
   - Error messages or logs
   - Screenshots if applicable

### Suggesting Features

Have an idea for improvement?

1. **Search existing feature requests** first
2. **Open a discussion** to gauge interest
3. **Provide context**:
   - Use case and problem it solves
   - Proposed solution
   - Alternatives considered
   - Impact on existing functionality

### Improving Documentation

Documentation improvements are always welcome!

- Fix typos or unclear explanations
- Add examples and use cases
- Improve installation instructions
- Create tutorials or guides
- Translate documentation (coming soon)

### Contributing Code

Ready to write code?

1. **Start small**: First-time contributors should look for issues labeled `good-first-issue`
2. **Discuss first**: For major changes, open an issue or discussion first
3. **Follow the workflow**: See [Contribution Workflow](#contribution-workflow) below

---

## 💻 Development Setup

### Prerequisites

- Python 3.8 or higher
- Git
- Virtual environment tool (venv, virtualenv, or conda)

### Setup Steps

```bash
# 1. Fork the repository on GitHub
# 2. Clone your fork
git clone https://github.com/YOUR_USERNAME/breach-around.git
cd breach-around

# 3. Add upstream remote
git remote add upstream https://github.com/Fused-Gaming/breach-around.git

# 4. Create virtual environment
python -m venv venv

# 5. Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Linux/Mac:
source venv/bin/activate

# 6. Install development dependencies
pip install -e ".[dev]"

# 7. Install pre-commit hooks
pre-commit install

# 8. Verify setup
python -m pytest
```

### Development Tools

We use these tools to maintain code quality:

- **pytest** - Unit testing
- **black** - Code formatting
- **flake8** - Code linting
- **mypy** - Type checking
- **pre-commit** - Git hooks for automated checks

---

## 🔄 Contribution Workflow

### 1. Create a Branch

```bash
# Sync with upstream
git fetch upstream
git checkout main
git merge upstream/main

# Create feature branch
git checkout -b feature/your-feature-name
# Or for bug fixes:
git checkout -b fix/bug-description
```

### 2. Make Changes

- Write clean, readable code
- Follow [Coding Standards](#coding-standards)
- Add tests for new functionality
- Update documentation as needed

### 3. Test Your Changes

```bash
# Run all tests
python -m pytest

# Run specific test file
python -m pytest tests/test_feature.py

# Run with coverage
python -m pytest --cov=breach_around

# Run linters
black breach_around/ tests/
flake8 breach_around/ tests/
mypy breach_around/
```

### 4. Commit Your Changes

```bash
# Stage your changes
git add .

# Commit with a descriptive message
git commit -m "Add feature: description of what you did"
```

#### Commit Message Guidelines

Follow this format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(api): add support for DeHashed API

Implemented DeHashed API integration with rate limiting
and error handling. Added tests for all endpoints.

Closes #123
```

```
fix(parser): handle malformed CSV entries

Fixed bug where parser would crash on entries with
missing fields. Now gracefully handles and logs errors.

Fixes #456
```

### 5. Push and Create Pull Request

```bash
# Push to your fork
git push origin feature/your-feature-name
```

Then on GitHub:
1. Navigate to your fork
2. Click "New Pull Request"
3. Fill out the PR template
4. Link related issues
5. Request review

### 6. Address Review Feedback

- Respond to comments professionally
- Make requested changes
- Push updates to the same branch
- Mark conversations as resolved

---

## 📏 Coding Standards

### Python Style Guide

- Follow [PEP 8](https://pep8.org/)
- Use [Black](https://github.com/psf/black) for formatting
- Maximum line length: 100 characters
- Use type hints for function signatures

### Code Organization

```python
"""Module docstring explaining purpose."""

import standard_library
import third_party_packages
import local_modules


class MyClass:
    """Class docstring."""

    def __init__(self):
        """Initialize the class."""
        pass

    def public_method(self, param: str) -> bool:
        """
        Public method with type hints and docstring.

        Args:
            param: Description of parameter

        Returns:
            Description of return value
        """
        return self._private_method(param)

    def _private_method(self, param: str) -> bool:
        """Private method prefixed with underscore."""
        return True
```

### Best Practices

1. **DRY (Don't Repeat Yourself)**
   - Extract common logic into functions
   - Use inheritance and composition appropriately

2. **SOLID Principles**
   - Single Responsibility: One class/function, one purpose
   - Open/Closed: Open for extension, closed for modification
   - Liskov Substitution: Subtypes must be substitutable
   - Interface Segregation: Many specific interfaces > one general
   - Dependency Inversion: Depend on abstractions, not concretions

3. **Error Handling**
   - Use specific exception types
   - Provide helpful error messages
   - Log errors appropriately
   - Don't suppress exceptions silently

4. **Security**
   - Never log sensitive data (passwords, API keys)
   - Validate and sanitize inputs
   - Use secure random for cryptographic operations
   - Follow principle of least privilege

---

## 🧪 Testing Guidelines

### Test Structure

```python
import pytest
from breach_around.module import function


class TestFeature:
    """Test suite for specific feature."""

    def test_normal_case(self):
        """Test normal operation."""
        result = function("valid_input")
        assert result == expected_output

    def test_edge_case(self):
        """Test edge cases."""
        result = function("")
        assert result is None

    def test_error_handling(self):
        """Test error conditions."""
        with pytest.raises(ValueError):
            function("invalid_input")
```

### Testing Requirements

- **Unit Tests**: Test individual functions and classes
- **Integration Tests**: Test component interactions
- **Edge Cases**: Test boundary conditions
- **Error Cases**: Test error handling
- **Coverage**: Aim for >80% code coverage

### Mocking External Services

```python
from unittest.mock import Mock, patch

@patch('breach_around.api.requests.get')
def test_api_call(mock_get):
    """Test API call with mocked response."""
    mock_get.return_value.json.return_value = {"status": "ok"}
    result = api_function()
    assert result["status"] == "ok"
```

---

## 📚 Documentation

### Code Documentation

- **Docstrings**: All public functions, classes, and modules
- **Inline Comments**: Explain complex logic
- **Type Hints**: Use for better IDE support and clarity

### User Documentation

When adding features, update:
- README.md - If it affects usage
- QUICKSTART.md - If it's a common operation
- API documentation - For programmatic usage
- Examples - Add practical examples

### Documentation Style

```python
def check_breach(email: str, services: List[str]) -> BreachResult:
    """
    Check if an email appears in breach databases.

    This function queries multiple breach databases and aggregates
    the results into a unified report.

    Args:
        email: Email address to check
        services: List of service names to query
            (e.g., ['proxynova', 'hibp'])

    Returns:
        BreachResult object containing aggregated results

    Raises:
        ValueError: If email format is invalid
        APIError: If all services fail to respond

    Examples:
        >>> result = check_breach("user@example.com", ["hibp"])
        >>> print(result.status)
        'breached'
    """
    pass
```

---

## 🔒 Security Considerations

### Handling Sensitive Data

- **Never log passwords or API keys**
- **Sanitize output before logging**
- **Use secure random for tokens**
- **Clear sensitive data from memory when done**

### API Security

- **Rate limit API calls**
- **Implement exponential backoff**
- **Validate responses before processing**
- **Handle timeouts gracefully**

### Vulnerability Disclosure

Found a security issue? **Do NOT open a public issue!**

1. Review [SECURITY.md](/.github/SECURITY.md)
2. Report via private channel
3. Allow time for fix before disclosure
4. Receive credit in release notes

---

## 🏷️ Issue and PR Labels

### Type Labels
- `bug` - Something isn't working
- `feature` - New feature request
- `enhancement` - Improve existing feature
- `documentation` - Documentation improvements

### Priority Labels
- `critical` - Security issues, data loss
- `high` - Major bugs, important features
- `medium` - Standard bugs and features
- `low` - Nice-to-have improvements

### Status Labels
- `good-first-issue` - Good for newcomers
- `help-wanted` - Community help needed
- `blocked` - Waiting on dependencies
- `wontfix` - Not planned to be fixed

---

## 🎉 Recognition

Contributors will be recognized in:
- Release notes
- README acknowledgments
- Git commit history

Significant contributors may be invited to join the maintainer team!

---

## 📞 Getting Help

Need assistance?

- **Questions**: Open a [Discussion](https://github.com/Fused-Gaming/breach-around/discussions)
- **Bugs**: Open an [Issue](https://github.com/Fused-Gaming/breach-around/issues)
- **Security**: See [SECURITY.md](/.github/SECURITY.md)

---

## 📜 License

By contributing to Breach Around, you agree that your contributions will be licensed under the same license as the project (MIT License).

---

Thank you for contributing to Breach Around! Your efforts help make security tools more accessible and effective for everyone. 🛡️
