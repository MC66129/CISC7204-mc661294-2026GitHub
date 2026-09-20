================================================================================
MODEL EVALUATION AND REFINEMENT - USED CARS AND LAPTOP PRICING
Two Jupyter notebooks for cross-validation, Ridge regression and tuning
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This project contains two self-contained Jupyter notebooks that evaluate and
refine regression models in Python with scikit-learn:

  * train / test splitting to estimate out-of-sample performance
  * cross-validation (cross_val_score and cross_val_predict)
  * diagnosing overfitting and underfitting across polynomial degrees
  * Ridge regression as a regularised alternative to plain linear regression
  * grid search over the Ridge alpha hyper-parameter

  Lab 01 refines models on the Used Cars Pricing data set.
  Lab 02 refines models on the Laptop Pricing data set.

Both notebooks are executed end to end and ship with their outputs saved, so the
results - including all 8 figures - can be read without running anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, matplotlib, scikit-learn, jupyterlab
  Optional ....... seaborn, scipy, statsmodels, ipywidgets

  Environment used for these two notebooks:

      python    3.x
      pandas    2.2.x  (NOT 3.x - see the note below)
      numpy     2.1.x
      scikit-learn 1.6.x

  NOTE: the embedded outputs were produced with pandas 2.x. Re-running Lab 01
  under pandas 3.x changes several numbers. See VERSION NOTES.


INSTALLATION
--------------------------------------------------------------------------------

  Minimum:

      python -m pip install pandas numpy matplotlib scikit-learn jupyterlab

  Full set:

      python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn ^
          statsmodels ipywidgets tabulate openpyxl xlrd requests

  If PyPI is slow or unreachable, use a mirror:

      python -m pip install pandas numpy matplotlib scikit-learn jupyterlab ^
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  (The caret "^" is the Windows command-line line-continuation character. On
  macOS or Linux use "\" instead, or simply put everything on one line.)


PROJECT STRUCTURE
--------------------------------------------------------------------------------

  <project folder>/
  |
  +-- ... Module 05 - Lab01 ... Used Car Pricing.ipynb
  |       Notebook 1 - evaluation and refinement on the Used Cars data set
  |       (64 code cells, 6 figures).
  |
  +-- ... Module 05 - Lab02 ... Laptop Pricing.ipynb
  |       Notebook 2 - evaluation and refinement on the Laptop Pricing data set
  |       (23 code cells, 2 figures).
  |
  +-- module_5_auto.csv
  |       Local copy of the data written by Lab 01 (it is also re-read from its
  |       source URL at the top of the notebook).
  |
  +-- laptops.csv
  |       Local copy read by Lab 02.
  |
  +-- usedcars.csv / laptop_pricing_dataset_mod2.csv
  |       Backup copies of the same inputs.
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  File                              Rows    Notes
  --------------------------------  ------  --------------------------------
  module_5_auto.csv                 201     Used Cars data; Lab 01 reads it
                                              from its source URL and writes a
                                              local copy.
  laptops.csv                       239     Laptop data; read locally by
                                              Lab 02.

  Source URLs:

    module_5_auto.csv (Lab 01)
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/module_5_auto.csv

    laptop_pricing_dataset_mod2.csv (Lab 02)
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_mod2.csv

  Running the notebooks as shipped needs the local laptops.csv plus network
  access for the URL-based reads.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel with scikit-learn installed. Use a pandas 2.x
      kernel if you want outputs identical to the saved ones.

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Lab 01 - Used Cars Pricing (model evaluation and refinement)

    1.  Train / test split
            train_test_split(x_data, y_data, test_size=0.45, random_state=0)

    2.  Cross-validation
            cross_val_score(lre, x_data[['horsepower']], y_data, cv=4)
            cross_val_predict(...)  ->  predictions from the held-out folds
            negative mean-squared-error converted back to a positive MSE

    3.  Overfitting / underfitting
            polynomial features of degree 1..5 fit on 'horsepower'
            training R-squared rises with the degree while the test R-squared
            collapses (degree 5 test R-squared ~ -29.87), a clear overfit

    4.  Ridge regression
            Ridge(alpha=...)  -  a regularisation term shrinks the coefficients
            and stabilises the fit

    5.  Grid search
            GridSearchCV(Ridge(), [{'alpha': [0.001 ... 100000]}], cv=4)
            best alpha ~ 10000 for the four-feature model

  Lab 02 - Laptop Pricing (model evaluation and refinement)

    The same techniques on the laptop data: train/test split, cross_val_score
    with different folds, Ridge regression at several alpha values, and a
    grid search to select the best alpha.


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both are derived from the IBM Skills Network "Model Evaluation and
      Refinement" lab templates. JupyterLite-only cells are commented out.

  2.  Kernel metadata.
      The notebooks declare the kernel name "python3", matching the pandas 2.x
      environment in which they were executed.

  3.  Completed task cells.
      The template "Write your code below" cells are filled with working code.


VERSION NOTES (pandas 2.x vs 3.x)
--------------------------------------------------------------------------------

  The embedded outputs were produced with pandas 2.x. Under pandas 3.x Lab 01
  changes in four cells:

  a)  The degree-5 polynomial predictions and its test R-squared differ,
      because a 5th-degree polynomial on 'horsepower' is ill-conditioned and
      small numeric differences in the numpy / scikit-learn stack change the
      fitted coefficients. The conclusion (severe overfitting) is unchanged.

      Example - test R-squared of the degree-5 fit:
          pandas 2.x (saved output) : -29.8713
          pandas 3.x (csp kernel)   : +0.0881

  b)  dtype of text columns prints as "str" instead of "object".

  To keep the saved outputs exactly reproducible, re-run with a pandas 2.x
  kernel (Anaconda base or the "python3" kernel). Do NOT re-run Lab 01 under
  pandas 3.x if you need the numbers to match the course material.


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ModuleNotFoundError: No module named 'sklearn'
      pip install scikit-learn  (or use the mirror above)

  Predictions differ from the saved output
      You re-ran under pandas 3.x / a different scikit-learn. See VERSION NOTES.

  URLError / connection error
      Lab 01 reads module_5_auto.csv and Lab 02 reads laptop_pricing_dataset_mod2.csv
      over the network. Restore connectivity or point the reads at local copies.


================================================================================
