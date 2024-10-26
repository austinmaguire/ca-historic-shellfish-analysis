# California Historical Shellfish Analysis

Analysis of commercial shellfish landings in California from 1928-2002 using data from NOAA's ERDDAP server and the California Department of Fish and Game.

## Overview

This project analyzes historical trends in California's shellfish fisheries using commercial landing data. The data comes from the California Department of Fish and Game's fish-tickets system via NOAA's ERDDAP (Environmental Research Division's Data Access Program) server.

## Data Source

- **Dataset**: California Fish Market Catch Landings, Long List, 1928-2002, Monthly
- **Provider**: NOAA ERDDAP/CA DFG
- **Dataset ID**: erdCAMarCatLM
- **Access Method**: ERDDAP tabledap API via PyDAP

The dataset contains monthly records of commercial landings for fish and invertebrates caught off the California coast, organized by species and port location.

## Project Structure

```
.
├── data/
│   ├── processed/
│   ├── raw/
│   └── reference/
├── docs/
│   ├── dataset_info/
│   └── pydap_info/
├── notebooks/
│   ├── initial_data_import.ipynb
│   ├── subset_creation.ipynb
│   └── data_EDA.ipynb
└── environment.yml
```

## Setup and Installation

1. Create the conda environment:

```bash
conda env create -f environment.yml
conda activate pydap
```

2. Key dependencies:

- Python 3.12
- PyDAP for ERDDAP access
- pandas, numpy for data manipulation
- matplotlib, seaborn for visualization
- xarray for multidimensional data

## Data Pipeline

1. **Data Import** (`initial_data_import.ipynb`):

   - Connects to ERDDAP server via PyDAP
   - Downloads complete California fisheries dataset
   - Performs initial data validation

2. **Shellfish Subsetting** (`subset_creation.ipynb`):

   - Identifies shellfish species from master list
   - Creates subsets for:
     - Abalone (9 species)
     - Clams (8 species)
     - Crabs (8 species)
     - Oysters (5 species)
     - Shrimp/Prawns (8 species)

3. **Analysis** (`data_EDA.ipynb`):
   - Time series analysis of landings
   - Port-specific trends
   - Species-specific analysis

## Geographic Coverage

Data covers six main port regions in California:

- Eureka (Del Norte, Humboldt, Mendocino Counties)
- San Francisco (Sonoma, Marin, San Mateo, San Francisco Counties)
- Monterey (Santa Cruz, Monterey Counties)
- Santa Barbara (San Luis Obispo, Santa Barbara, Ventura Counties)
- Los Angeles (Los Angeles, Orange Counties)
- San Diego (San Diego County)

## Data Limitations

- Excludes offshore foreign fisheries
- Excludes tropical tuna fisheries
- Post-1971 commercial freshwater catches not included
- Limited spatial resolution (only to port region)
- Sacramento region data merged with San Francisco (1928-1971)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- NOAA ERDDAP for data access
- California Department of Fish and Game for historical records
- Environmental Research Division for data maintenance

## Contact

For questions about this analysis, please open an issue in this repository.
