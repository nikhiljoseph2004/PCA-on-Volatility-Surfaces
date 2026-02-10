# Contributing to PCA on Volatility Surfaces

Thank you for your interest in contributing to this project! This document provides guidelines for contributing to the repository.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [How Can I Contribute?](#how-can-i-contribute)
3. [Development Setup](#development-setup)
4. [Contribution Workflow](#contribution-workflow)
5. [Style Guidelines](#style-guidelines)
6. [Testing](#testing)
7. [Documentation](#documentation)

## Code of Conduct

This project adheres to a standard code of conduct. By participating, you are expected to:

- Be respectful and inclusive
- Welcome newcomers and help them get started
- Accept constructive criticism gracefully
- Focus on what is best for the community

## How Can I Contribute?

### Reporting Bugs

If you find a bug, please open an issue with:

- **Clear title**: Briefly describe the problem
- **Description**: Detailed explanation of the issue
- **Steps to reproduce**: How to trigger the bug
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Environment**: Python version, OS, package versions
- **Code snippets**: If applicable, minimal reproducible example

### Suggesting Enhancements

We welcome suggestions for improvements! Please open an issue with:

- **Clear title**: Brief description of the enhancement
- **Motivation**: Why is this enhancement needed?
- **Proposed solution**: How would you implement it?
- **Alternatives**: Other approaches you considered

### Contributing Code

Areas where contributions are particularly welcome:

1. **Additional analysis techniques**:
   - Alternative dimensionality reduction methods (ICA, NMF)
   - Time-varying factor loadings
   - Forecasting models

2. **Visualization improvements**:
   - Additional plot types
   - Enhanced interactivity
   - Better default styling

3. **Performance optimization**:
   - Faster surface construction
   - Parallel processing
   - Memory efficiency

4. **Documentation**:
   - Expanded examples
   - Tutorial notebooks
   - API documentation

5. **Data handling**:
   - Support for different data formats
   - Additional data sources
   - Data validation utilities

## Development Setup

### Prerequisites

- Python 3.8 or higher
- Git
- Virtual environment tool (venv or conda)

### Setup Steps

1. **Fork the repository** on GitHub

2. **Clone your fork**:
```bash
git clone https://github.com/YOUR-USERNAME/PCA-on-Volatility-Surfaces.git
cd PCA-on-Volatility-Surfaces
```

3. **Add upstream remote**:
```bash
git remote add upstream https://github.com/nikhiljoseph2004/PCA-on-Volatility-Surfaces.git
```

4. **Create a virtual environment**:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

5. **Install dependencies**:
```bash
pip install -r requirements.txt
```

6. **Create a branch for your feature**:
```bash
git checkout -b feature/your-feature-name
```

## Contribution Workflow

1. **Sync with upstream**:
```bash
git fetch upstream
git checkout main
git merge upstream/main
```

2. **Create a feature branch**:
```bash
git checkout -b feature/descriptive-name
```

3. **Make your changes**:
   - Write clear, well-commented code
   - Follow the style guidelines below
   - Add tests if applicable

4. **Test your changes**:
   - Run the notebook to ensure it works
   - Check that visualizations render correctly
   - Verify no regressions

5. **Commit your changes**:
```bash
git add .
git commit -m "Brief description of changes"
```

Use clear commit messages:
- Start with a verb (Add, Fix, Update, Remove)
- Be concise but descriptive
- Reference issues if applicable (#123)

6. **Push to your fork**:
```bash
git push origin feature/your-feature-name
```

7. **Open a Pull Request**:
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Select your branch
   - Fill out the PR template
   - Link related issues

## Style Guidelines

### Python Code Style

Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) conventions:

- **Indentation**: 4 spaces (no tabs)
- **Line length**: Max 88 characters (Black formatter standard)
- **Naming**:
  - Functions and variables: `snake_case`
  - Classes: `PascalCase`
  - Constants: `UPPER_CASE`
- **Imports**: 
  - Standard library first
  - Third-party packages second
  - Local modules last
  - Alphabetical within each group

### Example:

```python
import numpy as np
import pandas as pd
from sklearn.decomposition import PCA

def compute_surface_changes(surfaces: np.ndarray) -> np.ndarray:
    """
    Compute daily changes in log-implied volatility surfaces.
    
    Parameters
    ----------
    surfaces : np.ndarray
        Array of daily volatility surfaces, shape (n_days, n_features)
    
    Returns
    -------
    np.ndarray
        Array of daily changes, shape (n_days-1, n_features)
    """
    log_surfaces = np.log(surfaces)
    changes = np.diff(log_surfaces, axis=0)
    return changes
```

### Jupyter Notebook Guidelines

- **Clear structure**: Use markdown cells to organize sections
- **Documentation**: Explain the purpose of each code cell
- **Comments**: Add inline comments for complex operations
- **Outputs**: Clear visualizations with labels and titles
- **Cell order**: Ensure cells run sequentially from top to bottom
- **Restart and run all**: Before committing, restart kernel and run all cells

### Documentation Style

- Use **Markdown** for all documentation files
- Include code examples where applicable
- Add links to relevant resources
- Use proper formatting:
  - Headers: `#`, `##`, `###`
  - Code blocks: ` ```python `
  - Inline code: \`code\`
  - Lists: `-` or `1.`
  - Bold: `**text**`
  - Italic: `*text*`

## Testing

Currently, this project uses manual testing via the Jupyter notebook. When adding new features:

1. **Test thoroughly**: Try various inputs and edge cases
2. **Document test cases**: Describe what you tested
3. **Visual verification**: Check that plots render correctly
4. **Reproducibility**: Ensure results are consistent across runs

Future enhancement: Add unit tests with pytest.

## Documentation

When adding features, update:

1. **README.md**: If it affects usage or installation
2. **docs/methodology.md**: If you change the analysis approach
3. **Inline comments**: For complex code sections
4. **Docstrings**: For all new functions and classes
5. **Example notebooks**: If adding new functionality

## Review Process

All contributions go through code review:

1. **Automated checks**: (Future) Linting, formatting, tests
2. **Maintainer review**: Code quality, design, documentation
3. **Discussion**: Feedback and requested changes
4. **Approval**: Once all concerns are addressed
5. **Merge**: Integrated into main branch

## Questions?

If you have questions about contributing:

- Open an issue with the "question" label
- Contact the maintainers directly
- Check existing issues and pull requests for similar discussions

## Recognition

All contributors will be acknowledged in:
- The project README
- Release notes (if applicable)
- Academic citations (for significant contributions)

Thank you for contributing to this project! Your efforts help advance quantitative finance research.
