OVERVIEW

This project contains eight Jupyter notebooks that cover the complete introductory
workflow for data analysis in Python:

importing data sets

data wrangling

exploratory data analysis (EDA)

model development

Two classic data sets are used:

Used Car Pricing / Automobile

Laptop Pricing

All notebooks have been executed end to end. Their outputs, including figures, are
saved in the notebooks, so the results can be read without running anything.

PROJECT STRUCTURE

<project folder>/
|
+-- CISC7204 Assgn01 Module 01 - Lab01 Importing Data Sets - Used Car Pricing.ipynb
+-- CISC7204 Assgn01 Module 01 - Lab02 Importing Data Sets - Laptop Pricing.ipynb
+-- CISC7204 Assgn01 Module 02 - Lab01 Data Wrangling - Used Car Pricing.ipynb
+-- CISC7204 Assgn01 Module 02 - Lab02 Data Wrangling - Laptop Pricing.ipynb
+-- CISC7204 Assgn01 Module 03 - Lab01 EDA - Used Car Pricing.ipynb
+-- CISC7204 Assgn01 Module 03 - Lab02 EDA - Laptop Pricing.ipynb
+-- CISC7204 Assgn01 Module 04 - Lab01 Model Development - Used Car Pricing.ipynb
+-- CISC7204 Assgn01 Module 04 - Lab02 Model Development - Laptop Pricing.ipynb
|
+-- data/
| +-- auto.csv
| +-- laptop_pricing_dataset_base.csv
| +-- laptop_pricing_dataset_mod1.csv
| +-- laptop_pricing_dataset_mod2.csv
| +-- automobileEDA.csv
|
+-- usedcars.csv
+-- laptops.csv
+-- clean_df.csv
+-- automobile.csv
|
+-- README.md

DATA SETS

Used Cars / Automobile Data Set

Source: UCI Machine Learning Repository - "Automobile" (imports-85)
File: auto.csv (205 rows x 26 columns, no header row)

Download URL:
https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/auto.csv

Notes:
Missing values are written as "?". The column names are supplied manually by
the notebooks.

Laptop Pricing Data Set

Source: IBM Skills Network
Base file: laptop_pricing_dataset_base.csv (238 rows x 12 columns, no header row)

Download URL:
https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_base.csv

Derived files:

laptop_pricing_dataset_mod1.csv (after data wrangling, with header row)

laptop_pricing_dataset_mod2.csv (used for EDA)

Notes:
Missing values are written as "?".

IMPORTANT:
The notebooks read the data directly from the URLs above, not from the data/
folder. The files in data/ are offline copies. Running the notebooks as shipped
requires network access, or you must change the read paths to the local copies.

REQUIREMENTS

Python version:
Python 3.13 or newer

Required libraries:
python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyterlab

Optional libraries:
python -m pip install statsmodels ipywidgets tabulate openpyxl xlrd requests

Development environment used:
pandas 3.0.5
numpy 2.3.3
matplotlib 3.10.7
seaborn 0.13.2
scipy 1.16.2
scikit-learn 1.9.1
jupyterlab 4.6.3
Python 3.13.7

Mirror for slow PyPI access:
python -m pip install pandas numpy jupyterlab -i https://pypi.tuna.tsinghua.edu.cn/simple

MODULE CONTENTS

Module 01 - Importing Data Sets

Lab 01 - Used Car Pricing

Load the headerless CSV with pd.read_csv(filepath, header=None)
(205 rows x 26 columns).

Preview the data with df.head(5) and df.tail(10).

Create and assign the 26 UCI attribute names manually.

Replace "?" with np.nan and drop rows where price is missing
(201 rows remain).

Export the cleaned data to automobile.csv.

Inspect the data with df.dtypes, df.describe(),
df.describe(include="all"), and df.info().

Lab 02 - Laptop Pricing

Load the data with pd.read_csv(filepath, header=None)
(238 rows x 12 columns).

Create and assign the 12 column names.

Replace "?" with np.nan.

Inspect data types, descriptive statistics, and summary information.

Module 02 - Data Wrangling

Lab 01 - Used Car Pricing

Read the data and assign column names.

Replace "?" with NaN.

Identify and handle missing values:

Replace by mean: normalized-losses, bore, stroke, horsepower, peak-rpm

Replace by mode: num-of-doors

Drop rows: rows with missing price

Correct data types with astype().

Standardize units:

mpg to L/100km

kg to pounds

cm to inches

Normalize data (min-max scaling to [0, 1]).

Bin horsepower into Low / Medium / High.

Create dummy variables for fuel-type and aspiration.

Export the cleaned data to clean_df.csv.

Lab 02 - Laptop Pricing

Evaluate missing data.

Replace missing Weight_kg values with the column mean and missing
Screen_Size_cm values with the most frequent value.

Fix data types to float.

Standardize units:

kg to pounds

cm to inches

Normalize CPU_frequency.

Bin Price into Low / Medium / High and plot a bar chart.

Convert Screen into indicator variables Screen-IPS_panel and
Screen-Full_HD.

Module 03 - Exploratory Data Analysis (EDA)

Lab 01 - Used Car Pricing

Load the cleaned automobileEDA.csv (201 rows x 29 columns).

Inspect data types and the correlation matrix with
df.corr(numeric_only=True).

Continuous variables vs price: regression plots and correlation
coefficients.

engine-size vs price: +0.87 (strong positive)

highway-mpg vs price: -0.70 (strong negative)

peak-rpm vs price: -0.10 (weak)

Categorical variables vs price: box plots.

Descriptive statistics and value counts.

Grouping and pivot tables: groupby() + pivot().

Heat map visualization of grouped results.

Pearson correlation coefficients and p-values using
scipy.stats.pearsonr.

Important variables:
length, width, curb-weight, engine-size, horsepower, city-mpg,
highway-mpg, wheel-base, bore, drive-wheels

Lab 02 - Laptop Pricing

Regression plots and correlations for continuous variables.

Box plots for categorical variables:
Category, GPU, OS, CPU_core, RAM_GB, Storage_GB_SSD.

Descriptive statistics.

Grouping, pivot table, and heat map:
GPU x CPU_core vs Price.

Pearson correlation coefficients and p-values.

Strongest correlations:
RAM_GB (0.55), CPU_core (0.46), CPU_frequency (0.37)

Module 04 - Model Development

Lab 01 - Used Car Pricing

Simple linear regression: highway-mpg -> price (R-squared ~ 0.497).

Multiple linear regression:
horsepower, curb-weight, engine-size, highway-mpg -> price
(R-squared ~ 0.809).

Model evaluation visualization: regression plots, residual plots, and
distribution plots.

Polynomial regression: np.polyfit / PolynomialFeatures, degrees 2 to 11.

Pipeline: StandardScaler + PolynomialFeatures + LinearRegression.

In-sample evaluation metrics: R-squared and MSE.

Lab 02 - Laptop Pricing

Simple linear regression: CPU_frequency -> Price.

Multiple linear regression: multiple features -> Price.

Polynomial regression with an interactive slider.

Pipeline construction and prediction.

IMPLEMENTATION NOTES

Origin of the notebooks
All notebooks are adapted from IBM Skills Network lab templates. The original
templates were JupyterLite versions.

JupyterLite download cells removed
The original templates used pyodide.http.pyfetch and an async download()
helper to download the data. These are not available in a regular Jupyter
kernel. They were removed, and the data is read directly from the source URL.

pandas 3.x compatibility fixes

df.corr() -> df.corr(numeric_only=True)
to avoid errors caused by text columns.

groupby().mean() -> groupby().mean(numeric_only=True).

Single-column replace(inplace=True) ->
df["col"] = df["col"].replace(...)
to avoid ChainedAssignmentError.

Blank code cells filled in
The "Write your code below" cells in the templates now contain working code.

Kernel metadata
The notebooks declare the kernel as csp or python3. If that kernel does not
exist on your machine, select any Python kernel. The metadata only sets the
default and does not affect the code.

Randomness
The notebooks contain no random processes, so a full run is deterministic.

VERSION NOTES (pandas 3.x)

Item pandas 1.x / 2.x pandas 3.x
Text column dtype object str
df.info() class name <class 'pandas.core.frame.DataFrame'> <class 'pandas.DataFrame'>
corr() / groupby().mean() silently skip text columns raise errors (fixed)
pd.get_dummies() output 0/1 integers True/False booleans
value_counts() output no header line includes "Name: count, dtype: int64"

TROUBLESHOOTING

Problem: ModuleNotFoundError: No module named 'pyodide'
Solution: Remove the JupyterLite download cells and read the data from the URL.

Problem: KeyError: 'price', or the first row becomes the column names
Solution: pd.read_csv() was called without header=None.

Problem: ValueError: could not convert string to float
Solution: df.corr() needs numeric_only=True.

Problem: TypeError: dtype 'str' does not support operation 'mean'
Solution: groupby().mean() needs numeric_only=True.

Problem: ChainedAssignmentError
Solution: Avoid single-column replace(inplace=True). Use assignment instead.

Problem: Figures do not appear
Solution: Keep %matplotlib inline and install matplotlib and seaborn.

Problem: pip install times out
Solution: Add a mirror:
-i https://pypi.tuna.tsinghua.edu.cn/simple

Problem: URLError / connection error
Solution: The notebooks read CSV files from the network. Restore connectivity,
or point the read paths to the local files in data/.

HOW TO RUN

Start Jupyter from the project folder:

jupyter lab

Open any notebook.

Select a Python kernel with pandas, matplotlib, seaborn, scipy, and
scikit-learn installed.

Run all cells:

Kernel -> Restart Kernel and Run All Cells

To reproduce the saved outputs exactly, use a pandas 2.x kernel if possible.

AUTHORS AND COPYRIGHT

Original lab authors:
Joseph Santarcangelo, Abhishek Gagneja, Vicky Kuo, and others.

Contributors:
Mahdi Noorian PhD, Bahare Talayian, Eric Xiao, Steven Dong, Parizad,
Hima Vasudevan, Fiorella Wenver, Yi Yao.

Copyright:
(c) IBM Corporation 2023. All rights reserved.