# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Data Science Guided Capstone project that builds a predictive pricing model for ski resort tickets. The project focuses on Big Mountain Resort, aiming to provide guidance for their pricing strategy and facility investment decisions based on market analysis.

**Business Goal**: Build a model to predict ski resort ticket prices based on resort features (vertical drop, terrain, lifts, etc.) to help Big Mountain optimize pricing relative to competitors.

**Target Variable**: `AdultWeekend` - the price of an adult weekend ticket

## Environment Setup

This project uses **Pipenv** for dependency management with Python 3.8.

### Setup Commands

```bash
# Install dependencies
pipenv install

# Activate virtual environment
pipenv shell

# Launch Jupyter Lab
jupyter lab
```

### Key Dependencies

- pandas==1.1.0
- jupyter==1.0.0
- jupyterlab==2.2.5
- scikit-learn==0.23.2
- matplotlib==3.3.1
- seaborn==0.10.1
- lxml==4.5.2

## Project Structure

```
DataScienceGuidedCapstone/
├── raw_data/               # Original, immutable data
│   └── ski_resort_data.csv
├── Notebooks/              # Analysis notebooks (sequential workflow)
│   ├── library/            # Utility modules
│   │   └── sb_utils.py     # File saving utilities
│   ├── 02_data_wrangling.ipynb
│   ├── 03_exploratory_data_analysis.ipynb
│   ├── 04_preprocessing_and_training.ipynb
│   └── 05_modeling.ipynb
├── data/                   # Intermediate processed data (created by notebooks)
├── models/                 # Saved model files (created by notebooks)
└── images/                 # Supporting images/diagrams
```

## Workflow

### Sequential Notebook Execution

The notebooks follow a **sequential workflow** representing the Data Science Method. Each notebook depends on outputs from the previous one:

1. **02_data_wrangling.ipynb**: Load raw data, clean, handle missing values, derive state-level statistics, save to `data/`
2. **03_exploratory_data_analysis.ipynb**: Load cleaned data, perform EDA, feature engineering, PCA analysis
3. **04_preprocessing_and_training.ipynb**: Train/test split, model training (Linear Regression, Random Forest), hyperparameter tuning, save best model to `models/`
4. **05_modeling.ipynb**: Load trained model, generate pricing scenarios for Big Mountain Resort

### Running Notebooks

From the project root with `pipenv shell` active:

```bash
# Launch Jupyter Lab
jupyter lab

# Navigate to Notebooks/ directory in the UI
# Open and run notebooks sequentially (02 → 03 → 04 → 05)
```

**Important**: Notebooks must be run in order as later notebooks depend on files created by earlier ones.

## Key Architecture Patterns

### Data Flow

1. **Raw data** (`raw_data/ski_resort_data.csv`) → never modified
2. **Cleaned data** → saved to `data/` directory (created by notebook 02)
3. **Feature-engineered data** → saved to `data/` (created by notebook 03)
4. **Trained models** → saved to `models/` as pickle files (created by notebook 04)

### File Saving Utility

The project uses a custom utility (`library/sb_utils.py`) for safe file operations:

```python
from library.sb_utils import save_file

# Saves CSV or PKL files with overwrite protection
save_file(data, filename='output.csv', dname='../data')
```

This utility:
- Creates directories if they don't exist
- Prompts for confirmation before overwriting existing files
- Supports CSV and PKL formats

### Model Versioning

Models are saved with version and sklearn version metadata:

```python
# In notebook 04: Save model
model.version = '1.0'
model.sklearn_version = sklearn_version
pickle.dump(model, file)

# In notebook 05: Load and validate
with open(model_path, 'rb') as f:
    model = pickle.load(f)
# Check version compatibility
```

## Coding Tasks Pattern

Notebooks contain guided coding exercises marked as:

```python
#Code task n#
# Instruction describing what to implement
some_function(___)  # ___ indicates where to insert code
```

When assisting with these tasks:
- Fill in the `___` placeholders appropriately
- Follow the existing code style and patterns
- Refer to earlier completed sections for context

## Data Science Method Steps

Each notebook corresponds to a specific step:

- **Step 2 (Data Wrangling)**: Collect, organize, clean data; identify target and features
- **Step 3 (EDA)**: Explore relationships, visualize distributions, engineer features
- **Step 4 (Preprocessing & Training)**: Split data, handle missing values, train models, select best model
- **Step 5 (Modeling)**: Apply model to business scenarios, generate insights

## Best Practices

### Notebook Hygiene

- **Version control**: Commit notebooks after significant changes or before major edits
- **Structure**: Use clear markdown headings (already provided in templates)
- **Documentation**: Add brief notes after results to highlight key takeaways
- **Imports**: Keep all imports at the top of the notebook

### Data Management

- Never modify files in `raw_data/`
- Intermediate data goes in `data/` directory
- Use `save_file()` utility to prevent accidental overwrites
- Document data transformations in notebook markdown cells

### Model Development

- Always establish baseline performance (e.g., mean predictor) before ML models
- Use scikit-learn pipelines for reproducibility
- Perform train/test splits before any data transformation
- Validate cross-platform compatibility (check sklearn versions)
- Save models with metadata (version, sklearn_version)

## Common Commands

```bash
# Environment management
pipenv install              # Install dependencies
pipenv shell               # Activate environment
pipenv --rm                # Remove virtual environment

# Jupyter
jupyter lab                # Launch Jupyter Lab
jupyter notebook           # Launch classic Jupyter

# Check Python/package versions (from within pipenv shell)
python --version           # Should be 3.8.x
pip list                   # List installed packages
```

## Testing

This is an educational project without formal test suite. Validation is performed through:
- Manual verification in notebooks
- Cross-validation in model training (notebook 04)
- Comparison of pandas vs sklearn implementations for equivalence

## Notes for AI Assistants

- This is a **guided learning project** with structured exercises
- Code tasks are intentionally left incomplete for educational purposes
- When helping, explain the reasoning behind solutions, not just the code
- The project assumes Big Mountain Resort data is included in the dataset
- State-level population data is scraped/augmented during data wrangling
- The model assumes pricing is set by a free market based on facility value
