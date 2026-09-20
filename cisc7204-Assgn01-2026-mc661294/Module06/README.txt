================================================================================
PROJECT CHALLENGE - PRACTICE AND FINAL PROJECTS
Insurance cost and house pricing, end-to-end data analytics with Python
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This folder contains the two end-to-end projects that close the course. Each
applies the full workflow covered in Modules 01 to 05:

  * import a data set into a pandas dataframe
  * wrangle the data (headers, missing values, data types)
  * exploratory data analysis (regression plots, box plots, correlation)
  * model development (single and multiple linear regression, pipelines)
  * model evaluation and refinement (train/test split, Ridge, polynomials)

  Practice Project - medical insurance charges (5 tasks).
  Final Project    - King County house prices (10 questions).

Both notebooks are executed end to end and ship with their outputs saved, so the
results - including all 4 figures - can be read without running anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, matplotlib, seaborn, scikit-learn, jupyterlab
  Optional ....... scipy, statsmodels, ipywidgets

  Environment used for these two notebooks:

      python    3.x
      pandas    2.2.x  (NOT 3.x - see the note below)
      numpy     2.1.x
      scikit-learn 1.6.x

  NOTE: the embedded outputs were produced with pandas 2.x. Re-running the
  Practice Project under pandas 3.x CRASHES. See VERSION NOTES.


INSTALLATION
--------------------------------------------------------------------------------

  Minimum:

      python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyterlab

  Full set:

      python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn ^
          statsmodels ipywidgets tabulate openpyxl xlrd requests

  If PyPI is slow or unreachable, use a mirror:

      python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyterlab ^
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  (The caret "^" is the Windows command-line line-continuation character. On
  macOS or Linux use "\" instead, or simply put everything on one line.)


PROJECT STRUCTURE
--------------------------------------------------------------------------------

  <project folder>/
  |
  +-- ... Module 06 - PracticeProj ....ipynb
  |       Practice Project - insurance cost analysis (18 code cells, 2 figures).
  |
  +-- ... Module 06 - FinalProj ....ipynb
  |       Final Project - house pricing analysis (32 code cells, 2 figures,
  |       10 questions).
  |
  +-- medical_insurance_dataset.csv
  |       Read by the Practice Project (kept in the same folder).
  |
  +-- housing.csv
  |       Read by the Final Project (2.5 MB, King County house sales).
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  File                              Rows    Notes
  --------------------------------  ------  --------------------------------
  medical_insurance_dataset.csv      (small) Insurance charges; 7 fields:
                                                age, gender, bmi, no_of_children,
                                                smoker, region, charges.
  housing.csv                        21613   King County (Seattle) house sales
                                                May 2014 - May 2015; 21 fields.

  Source URLs:

    medical_insurance_dataset.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/medical_insurance_dataset.csv

    housing.csv (the notebook's commented download refers to kc_house_data_NaN.csv)
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/FinalModule_Coursera/data/kc_house_data_NaN.csv

  Both notebooks read the local copies in this folder, so they run offline.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel with scikit-learn installed. Use a pandas 2.x
      kernel - the Practice Project fails under pandas 3.x (see below).

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Practice Project - Insurance Cost (5 tasks)

    Task 1  Import the data
                pd.read_csv(path, header=None) + the 7 column names
    Task 2  Data wrangling
                replace '?' with NaN; fill age with the mean and smoker with
                the most frequent value; convert dtypes; round charges to 2 dp
    Task 3  Exploratory data analysis
                regplot of bmi vs charges; boxplot of smoker vs charges;
                the correlation matrix - smoker is the strongest single factor
    Task 4  Model development
                single-variable linear regression on smoker
                multiple linear regression on all six predictors
                a StandardScaler + PolynomialFeatures + LinearRegression pipeline
    Task 5  Model refinement
                train/test split (20% test); Ridge(alpha=0.1); a degree-2
                polynomial transform of the training data

  Final Project - House Pricing (10 questions)

    Q1   dtypes of every column
    Q2   drop the id and Unnamed: 0 columns
    Q3   value_counts of floors
    Q4   boxplot of price by waterfront
    Q5   regplot of price against sqft_above
    Q6   simple linear regression on sqft_living
    Q7   multiple linear regression on 11 features
    Q8   a pipeline (scale + polynomial + linear regression)
    Q9   Ridge regression (alpha 0.1) on the train/test split
    Q10  degree-2 polynomial transform of train and test, then Ridge


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both are derived from the IBM Skills Network project templates. The
      JupyterLite-only cells (piplite / pyodide downloads) are commented out.

  2.  Kernel metadata.
      The notebooks declare the kernel name "python3", matching the pandas 2.x
      environment in which they were executed.

  3.  Completed tasks.
      All task / question cells are filled with working code.


VERSION NOTES (pandas 2.x vs 3.x) - READ BEFORE RE-RUNNING
--------------------------------------------------------------------------------

  The embedded outputs were produced with pandas 2.x. Under pandas 3.x:

  a)  The Practice Project CRASHES in Task 2. The cell that fills the missing
      age and smoker values uses

          df["age"].replace(np.nan, mean_age, inplace=True)

      In pandas 3 this is a chained assignment: the write is silently discarded
      (Copy-on-Write), the NaN values stay in place, and the following
      astype("int") call raises

          ValueError: cannot convert float NaN to integer

      Fix, if you must re-run under pandas 3:
          df["age"]    = df["age"].replace(np.nan, mean_age)
          df["smoker"] = df["smoker"].replace(np.nan, is_smoker)

  b)  The Final Project runs under pandas 3, but its bedrooms/bathrooms
      replace(..., inplace=True) calls are also chained assignments; they are
      harmless here because those columns have no missing values.

  c)  dtype of text columns prints as "str" instead of "object".

  Recommendation: keep the saved outputs (produced under pandas 2.x) and do not
  re-run under pandas 3.x unless you apply the fix above.


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ValueError: cannot convert float NaN to integer
      You re-ran the Practice Project under pandas 3.x. Apply the assignment-form
      replace shown above, or re-run under a pandas 2.x kernel.

  ModuleNotFoundError: No module named 'seaborn' / 'sklearn'
      pip install seaborn scikit-learn  (or use the mirror above)

  FileNotFoundError: medical_insurance_dataset.csv / housing.csv
      The CSV must sit in the same folder as the notebook. Both are included
      in this folder.


================================================================================
