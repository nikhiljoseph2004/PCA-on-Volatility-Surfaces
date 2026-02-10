# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Complete project documentation and structure
- Comprehensive README.md with project overview, installation, and usage
- requirements.txt with all Python dependencies
- MIT License
- .gitignore for Python projects
- CONTRIBUTING.md with contribution guidelines
- Detailed methodology documentation
- CITATION.md for academic references
- Directory structure (data/, results/, docs/)
- README files in data/ and results/ directories
- LaTeX report moved to docs/ directory

### Changed
- Reorganized repository for professional presentation
- Moved "Final Report" to "docs/Final_Report.tex"

## [1.0.0] - 2025-12-07

### Added
- Initial implementation of PCA analysis on volatility surfaces
- Jupyter notebook with complete analysis pipeline
- Nadaraya-Watson kernel regression for surface construction
- PCA decomposition using scikit-learn
- Interactive 3D visualizations with Plotly
- Analysis of S&P 500 options data (2015-2024)

### Features
- Data loading and preprocessing
- Out-of-the-money (OTM) option filtering
- 2D Gaussian kernel smoothing
- Daily surface change computation
- Principal component extraction
- Factor score time series
- Eigenmode visualization

---

## Version History

### Version Numbering

- **Major version** (X.0.0): Significant changes to methodology or API
- **Minor version** (0.X.0): New features or enhancements
- **Patch version** (0.0.X): Bug fixes or documentation updates

---

## Future Releases

### Planned for v1.1.0
- [ ] Example dataset or data generator
- [ ] Pre-computed results for demonstration
- [ ] Additional visualization options
- [ ] Command-line interface (CLI)

### Planned for v1.2.0
- [ ] Time-varying factor analysis
- [ ] Forecasting capabilities
- [ ] Comparison with alternative methods (ICA, NMF)
- [ ] Performance optimization

### Planned for v2.0.0
- [ ] Support for multiple asset classes
- [ ] Real-time data integration
- [ ] Interactive web dashboard
- [ ] API for programmatic access

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to contribute to this project.
