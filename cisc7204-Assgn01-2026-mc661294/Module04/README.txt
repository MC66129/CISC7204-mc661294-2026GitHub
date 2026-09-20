================================================================================
MODEL DEVELOPMENT - USED CARS AND LAPTOP PRICING
Two Jupyter notebooks for building and visualising linear and polynomial models
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This project contains two self-contained Jupyter notebooks that build predictive
models in Python with scikit-learn:

  * simple linear regression (one predictor)
  * multiple linear regression (several predictors)
  * model evaluation by visualisation (regression, residual and distribution plots)
  * polynomial regression with increasing degrees
  * a reusable training pipeline (scaling + polynomial features + linear model)
  * the R-squared and mean-squared-error measures

  Lab 01 models the Used Cars Pricing data set.
  Lab 02 models the Laptop Pricing data set.

Both notebooks are executed end to end and ship with their outputs saved, so the
results - including all 13 figures - can be read without running anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, matplotlib, seaborn, scikit-learn, jupyterlab
  Optional ....... scipy, statsmodels, ipywidgets

  Environment used for these two notebooks:

      python    3.x
      pandas    2.2.x  (NOT 3.x - see the note below)
      numpy     2.1.x
      matplotlib 3.x
      seaborn    0.13.x
      scikit-learn 1.6.x

  NOTE: the embedded outputs were produced with pandas 2.x. Re-running under
  pandas 3.x does NOT crash these two notebooks, but a few floating-point digits
  change. See VERSION NOTES.


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
  +-- ... Module 04 - Lab01 ... Used Car Pricing.ipynb
  |       Notebook 1 - model development on the Used Cars data set
  |       (65 code cells, 7 figures).
  |
  +-- ... Module 04 - Lab02 ... Laptop Pricing.ipynb
  |       Notebook 2 - model development on the Laptop Pricing data set
  |       (22 code cells, 6 figures, one interactive degree slider).
  |
  +-- usedcars.csv
  |       Local copy read by Lab 01.
  |
  +-- laptops.csv
  |       Local copy read by Lab 02.
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  File                  Rows    Notes
  --------------------  ------  ---------------------------------------------
  usedcars.csv          202     header row present; read locally by Lab 01
  laptops.csv           239     header row present; read locally by Lab 02

  The other inputs are read from their source URLs:

    automobileEDA.csv (Lab 01)
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/automobileEDA.csv

    laptop_pricing_dataset_mod2.csv (Lab 02)
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_mod2.csv

  Running the notebooks as shipped therefore needs the local CSVs in the same
  folder plus network access for the URL-based reads.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel that has scikit-learn and matplotlib installed.
      Use a pandas 2.x kernel if you want outputs identical to the saved ones.

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Lab 01 - Used Cars Pricing (model development)

    1.  Simple linear regression
            X = df[['highway-mpg']];  Y = df['price']
            lm = LinearRegression(); lm.fit(X, Y)
            lm.intercept_, lm.coef_  -> price ~ 38423 - 821.7 x highway-mpg
            R-squared ~ 0.4966  (highway-mpg explains about half the variance)
            engine-size alone gives R-squared ~ 0.7642

    2.  Multiple linear regression
            Z = df[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']]
            R-squared ~ 0.8094  (four predictors beat each single one)

    3.  Model evaluation using visualisation
            regression plots, residual plots and distribution plots compare
            the predicted values against the actual prices

    4.  Polynomial regression
            np.polyfit / PolynomialFeatures with degree 2, 3, ... 11
            R-squared rises with the degree on the training data, which is a
            textbook sign of overfitting

    5.  Pipelines
            Pipeline([('scale', StandardScaler()),
                      ('poly', PolynomialFeatures(include_bias=False)),
                      ('model', LinearRegression())])
            used to compare degrees cleanly and to predict new inputs

  Lab 02 - Laptop Pricing (model development)

    The same workflow applied to laptops:
      - simple linear regression of CPU_frequency on Price
      - multiple linear regression (CPU_frequency, RAM_GB, Storage_GB_SSD,
        GPU, Screen_Size_inch, Weight_pounds)
      - polynomial regression of increasing degree
      - an ipywidgets slider that re-fits the model for a chosen degree


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both are derived from the IBM Skills Network "Model Development" lab
      templates. The JupyterLite-only cells (piplite / pyodide downloads) are
      left in place but commented out, so they do not execute.

  2.  Kernel metadata.
      The notebooks declare the kernel name "python3". This matches the
      pandas 2.x environment in which they were executed. If you re-run them,
      pick any Python kernel with scikit-learn installed.

  3.  Completed task cells.
      The template "Write your code below" cells are filled with working code.


VERSION NOTES (pandas 2.x vs 3.x)
--------------------------------------------------------------------------------

  These two notebooks were executed with pandas 2.x. Under pandas 3.x:

  a)  They still run without errors.

  b)  One cell in Lab 01 (the prediction of a specific car) differs in the last
      significant digits, because the fitted coefficients are computed through
      scikit-learn and the surrounding numeric plumbing differs between numpy /
      scikit-learn versions. The effect is under 0.01% and does not change any
      conclusion.

  c)  dtype of text columns prints as "str" instead of "object".

  To keep the saved outputs exactly reproducible, re-run with a pandas 2.x
  kernel (for example the Anaconda base environment or the "python3" kernel).


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ModuleNotFoundError: No module named 'seaborn'
      pip install seaborn  (or use the mirror above)

  The notebook asks for a kernel named python3 that does not exist
      Pick any Python kernel with scikit-learn installed; the metadata only sets
      the default.

  Predictions differ slightly from the saved output
      You re-ran under a different numpy / scikit-learn version. The differences
      are negligible; see VERSION NOTES.

  URLError / connection error
      The notebooks read automobileEDA.csv and laptop_pricing_dataset_mod2.csv
      over the network. Restore connectivity or point the reads at local copies.


================================================================================
