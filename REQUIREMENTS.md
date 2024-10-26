# Environment Setup and Dependencies

## System Requirements

- Python 3.12
- Anaconda or Miniconda installed on your system
- Git (for cloning the repository)

## Environment Setup

### Option 1: Using environment.yml (Recommended)

The project uses a Conda environment specified in `environment.yml`. To create and activate the environment:

```zsh
# Create the environment from environment.yml
conda env create -f environment.yml

# Activate the environment
conda activate pydap
```

### Option 2: Manual Installation

If you prefer to install packages manually, the following packages are required:

Core Dependencies:

- Python 3.12
- ipython
- jupyterlab
- matplotlib
- numpy (>= 2.0)
- pandas
- pydap
- scipy
- seaborn
- xarray

Additional Dependencies:

- statsmodels (for statistical analysis)
- openpyxl (for Excel file support)
- notebook (for Jupyter notebook support)
- erddapy (for ERDDAP server interactions)

To install manually:

```zsh
# Create a new environment
conda create -n pydap python=3.12

# Activate the environment
conda activate pydap

# Install conda packages
conda install ipython jupyterlab matplotlib numpy pandas pydap scipy seaborn xarray statsmodels openpyxl notebook

# Install pip packages
pip install erddapy
```

## Verifying Installation

To verify your installation:

```zsh
# Check conda environment
conda list

# Test Python imports
python -c "import numpy; import pandas; import xarray; import pydap; print('All key packages imported successfully!')"
```

## Common Issues and Solutions

1. If you encounter SSL certificate errors with PyDAP:

   - Ensure your system's SSL certificates are up to date
   - Try: `conda install certifi`

2. If Jupyter notebooks don't display properly:

   - Ensure jupyterlab is installed: `conda install jupyterlab`
   - Try running: `jupyter lab build`

3. For ERDDAP connection issues:
   - Check your internet connection
   - Verify the ERDDAP server status
   - Try: `pip install --upgrade erddapy`

## Updating the Environment

To update all packages to their latest compatible versions:

```zsh
conda activate pydap
conda update --all
```

## Project-Specific Notes

This environment is configured for analyzing California historical shellfish fishery data. The key dependencies are:

- `xarray` and `pydap` for working with oceanographic data
- `pandas` and `numpy` for data manipulation
- `matplotlib` and `seaborn` for visualization
- `statsmodels` for time series analysis

The environment is optimized for:

- Processing historical fisheries data
- Creating visualizations
- Statistical analysis
- Working with ERDDAP servers

## Data Files

Note that the raw data files are not included in the environment setup and should be downloaded separately:

- `california_fish_landings_1928_2002.csv` (primary dataset)
- `california_fish_species_list.csv` (reference data)

## Contact

If you encounter any issues with the environment setup, please open an issue in the repository.
