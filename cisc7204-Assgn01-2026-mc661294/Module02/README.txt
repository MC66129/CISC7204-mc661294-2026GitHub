================================================================================
DATA WRANGLING - USED CARS AND LAPTOP PRICING
Two Jupyter notebooks for cleaning, standardizing and encoding tabular data
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This project contains two self-contained Jupyter notebooks that cover the data
pre-processing stage of a data analysis workflow in Python:

  * identify and handle missing values (replace by mean, replace by mode, drop)
  * correct data types that were read as text
  * standardize units (mpg to L/100km, kg to pounds, cm to inches)
  * normalize numeric variables into a common range (min-max scaling)
  * bin a continuous variable into categorical groups
  * convert categorical variables into indicator (dummy) variables
  * write the cleaned data back to CSV

  Lab 01 works on the UCI "Automobile" (Used Cars Pricing) data set.
  Lab 02 works on the Laptop Pricing data set (the updated "mod1" version).

Both notebooks are executed end to end and ship with their outputs saved, so the
results - including the matplotlib figures - can be read without running
anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, matplotlib, jupyterlab (or notebook)
  Optional ....... seaborn, scipy, scikit-learn, statsmodels, ipywidgets,
                   tabulate, openpyxl, xlrd

  Developed and tested against:
      pandas 3.0.5      numpy 2.3.3        matplotlib 3.10.7
      seaborn 0.13.2    scipy 1.16.2       scikit-learn 1.9.1
      statsmodels 0.15.0                   ipywidgets 8.1.9
      jupyterlab 4.6.3                     python 3.13.7


INSTALLATION
--------------------------------------------------------------------------------

  Minimum:

      python -m pip install pandas numpy matplotlib jupyterlab

  Full set used during development:

      python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn ^
          statsmodels ipywidgets tabulate openpyxl xlrd requests

  If PyPI is slow or unreachable, use a mirror:

      python -m pip install pandas numpy matplotlib jupyterlab ^
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  (The caret "^" is the Windows command-line line-continuation character. On
  macOS or Linux use "\" instead, or simply put everything on one line.)


PROJECT STRUCTURE
--------------------------------------------------------------------------------

  <project folder>/
  |
  +-- ... Module 02 - Lab01 ... Used Car Pricing.ipynb
  |       Notebook 1 - data wrangling on the Used Cars data set (46 code cells,
  |       includes 5 "Question" tasks and 3 matplotlib figures).
  |
  +-- ... Module 02 - Lab02 ... Laptop Pricing.ipynb
  |       Notebook 2 - data wrangling on the Laptop Pricing data set (15 code
  |       cells, includes 6 numbered "Task" blocks and 1 matplotlib figure).
  |
  +-- data/
  |   +-- auto.csv
  |   |       Used Cars data - 205 rows x 26 columns, no header row.
  |   +-- laptop_pricing_dataset_mod1.csv
  |           Laptop Pricing data - 238 data rows, header row present.
  |
  +-- screenshots/
  |       Captured output images (optional).
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  File                              Rows    Cols    Notes
  --------------------------------  ------  ------  --------------------------
  auto.csv                          205     26      no header row; the column
                                                    names are supplied by the
                                                    notebook. Missing values are
                                                    the literal character "?"
  laptop_pricing_dataset_mod1.csv   238     13      header row present; the first
                                                    column is a saved index
                                                    ("Unnamed: 0"). Missing values
                                                    are empty cells

  Download URLs:

    auto.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/auto.csv

    laptop_pricing_dataset_mod1.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_mod1.csv

  IMPORTANT: the notebooks read the data DIRECTLY FROM THE URLS ABOVE, not from
  the data/ folder. The files in data/ are offline copies of exactly the same
  content; to use them, point the read call at the local path instead. Running
  the notebooks as shipped therefore requires network access.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel that has pandas and matplotlib installed.

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells

  The notebooks contain no randomness and no interactive input, so a full run is
  deterministic. Lab 01 finishes by writing clean_df.csv into the working
  directory. The three figures in Lab 01 and the one figure in Lab 02 are
  produced with the inline matplotlib backend and are stored inside the notebook.


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Lab 01 - Used Cars Pricing (data wrangling)

    1.  Read the data
            import pandas as pd, matplotlib.pylab as plt
            headers = [...26 names...]
            df = pd.read_csv(filepath, names = headers)
        The file has no header row, so the names are supplied explicitly and the
        missing-value marker "?" is left in place for the next step.

    2.  Convert "?" to NaN
            df.replace("?", np.nan, inplace = True)

    3.  Evaluate missing data
            missing_data = df.isnull()
            loop over the columns and print value_counts()
        Result: seven columns contain missing values, of which
        "normalized-losses" has 41, "num-of-doors" 2, "bore" 4, "stroke" 4,
        "horsepower" 2, "peak-rpm" 2 and "price" 4.

    4.  Handle the missing values
            - "normalized-losses", "bore", "stroke", "horsepower", "peak-rpm":
              replace by the column mean
            - "num-of-doors": replace by the most frequent value ("four")
            - "price": drop the whole row, then reset the index

    5.  Correct the data types
            df[["bore", "stroke"]] = df[["bore", "stroke"]].astype("float")
            df[["normalized-losses"]] = df[["normalized-losses"]].astype("int")
            df[["price"]] = df[["price"]].astype("float")
            df[["peak-rpm"]] = df[["peak-rpm"]].astype("float")

    6.  Standardize units
            df["city-L/100km"]    = 235 / df["city-mpg"]
            df["highway-L/100km"] = 235 / df["highway-mpg"]

    7.  Normalize (min-max) into the range [0, 1]
            df["length"] = df["length"] / df["length"].max()
            df["width"]  = df["width"]  / df["width"].max()
            df["height"] = df["height"] / df["height"].max()

    8.  Bin "horsepower" into 3 equal-width groups
            bins = np.linspace(min(df["horsepower"]), max(df["horsepower"]), 4)
            group_names = ["Low", "Medium", "High"]
            df["horsepower-binned"] = pd.cut(df["horsepower"], bins,
                                             labels=group_names,
                                             include_lowest=True)
        Plus two figures: a histogram of horsepower and a bar chart of the bins.

    9.  Indicator (dummy) variables
            df["fuel-type"] -> fuel-type-gas / fuel-type-diesel
            df["aspiration"] -> aspiration-std / aspiration-turbo
        The original categorical columns are then dropped.

   10.  Export
            df.to_csv("clean_df.csv")

  Lab 02 - Laptop Pricing (data wrangling)

    Task 1  evaluate the data set for missing data
                missing_data = df.isnull() + per-column value_counts()
    Task 2  replace missing "Weight_kg" values with the column mean
    --      replace missing "Screen_Size_cm" values with the most frequent value
    Task 3  fix the data types of "Weight_kg" and "Screen_Size_cm" to float
    Task 4  standardize units and rename the columns
                Weight_kg      x 2.205  -> Weight_pounds
                Screen_Size_cm / 2.54   -> Screen_Size_inch
    --      normalize "CPU_frequency" by its maximum value
    Task 5  bin "Price" into Low / Medium / High ("Price-binned") and plot a bar
            chart of the bins
    Task 6  convert "Screen" into the indicator variables
            "Screen-IPS_panel" and "Screen-Full_HD", then drop "Screen"


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both notebooks are derived from the IBM Skills Network "Data Wrangling" lab
      templates, which are shipped as JupyterLite versions. They were adapted so
      that they run in a normal local Jupyter installation.

  2.  JupyterLite download cells removed.
      The templates opened with "from pyodide.http import pyfetch" plus an async
      "download()" helper. The pyodide module exists only inside JupyterLite and
      raises ModuleNotFoundError in a regular kernel. Those cells were removed and
      the data is read straight from its source URL - the fallback the templates
      themselves describe in a note cell.

  3.  pandas 3 fix: in-place replacement on a single column.
      The templates fill missing values with

          df["normalized-losses"].replace(np.nan, avg_norm_loss, inplace=True)

      Under pandas 3 this is a chained assignment: it raises
      ChainedAssignmentError, the write is discarded (Copy-on-Write), and the NaNs
      stay in the frame. The following "astype('int')" step then fails. Each of
      these calls was therefore rewritten in the equivalent, supported form

          df["normalized-losses"] = df["normalized-losses"].replace(np.nan, avg_norm_loss)

      The same correction was applied to "bore", "stroke", "horsepower",
      "peak-rpm", "num-of-doors" (Lab 01) and to "Weight_kg", "Screen_Size_cm"
      (Lab 02). Frame-level calls such as df.dropna(..., inplace=True),
      df.rename(..., inplace=True) and df.drop(..., inplace=True) are unaffected
      and were left as-is.

  4.  Task / Question answer cells filled in.
      The templates leave the "Write your code below" cells empty. They now
      contain working code for: Lab 01 Questions 1 to 5 (stroke mean, highway mpg
      to L/100km, height normalization, aspiration dummies, merge and drop) and
      Lab 02 Tasks 1 to 6.

  5.  Kernel metadata.
      The notebooks declare the kernel name "csp". If that kernel does not exist
      on your machine, pick any Python kernel - the metadata only sets the default
      and does not affect the code.


VERSION NOTES (pandas 3.x)
--------------------------------------------------------------------------------

  The original lab material was written for pandas 1.x / 2.x. The following
  differences appear under pandas 3.x:

  a)  In-place replacement on a single column silently does nothing (see
      implementation note 3). This one changes results, which is why the code was
      rewritten.

  b)  dtype of text columns
          pandas 1.x / 2.x : object
          pandas 3.x       : str     (new default string dtype)

  c)  class name printed by df.info()
          pandas 1.x / 2.x : <class 'pandas.core.frame.DataFrame'>
          pandas 3.x       : <class 'pandas.DataFrame'>

  d)  Indicator variables are booleans
          pd.get_dummies() produces True / False columns under pandas 3.x
          instead of 0 / 1 integers. Downstream arithmetic still works because
          booleans are treated as 0 and 1.

  e)  value_counts() prints a header line ("Name: count, dtype: int64") that the
      older output did not have.


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ChainedAssignmentError: A value is being set on a copy of a DataFrame
      You are using the in-place form of replace() on a single column. Use
          df["col"] = df["col"].replace(old, new)
      or the frame-level form
          df.replace({"col": {old: new}}, inplace=True)

  ModuleNotFoundError: No module named 'pyodide'
      You are running an unmodified JupyterLite template. Delete the download
      cells, or use the URL-based read shown above.

  ValueError: cannot convert float NaN to integer
      The missing values were never replaced - usually the ChainedAssignmentError
      case above. Check the isnull() counts before converting types.

  Figures do not appear in the notebook
      Keep the "%matplotlib inline" lines at the top of the plotting cells, and
      make sure the kernel has matplotlib installed.

  pip install is very slow or times out
      Add a mirror:
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  URLError / connection error when running a notebook
      The notebooks read their CSV files over the network. Restore connectivity,
      or point the read calls at the local copies in data/.


================================================================================
