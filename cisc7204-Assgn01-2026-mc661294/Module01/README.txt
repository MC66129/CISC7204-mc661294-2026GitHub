================================================================================
IMPORTING DATA SETS - USED CARS AND LAPTOP PRICING
Two Jupyter notebooks for loading, inspecting and cleaning CSV data with pandas
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This project contains two self-contained Jupyter notebooks that walk through the
standard first stage of a data analysis workflow in Python:

  * load a raw, headerless CSV file into a pandas DataFrame
  * inspect the shape, structure and content of the frame
  * attach meaningful column names
  * handle missing values
  * produce summary statistics for numeric and categorical columns
  * export the cleaned frame back to CSV

  Lab 01 works on the UCI "Automobile" (Used Cars Pricing) data set.
  Lab 02 works on the Laptop Pricing data set.

Both notebooks are executed end to end and ship with their outputs saved, so the
results can be read without running anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, jupyterlab (or notebook)
  Optional ....... matplotlib, seaborn, scipy, scikit-learn, statsmodels,
                   ipywidgets, tabulate, openpyxl, xlrd

  Developed and tested against:
      pandas 3.0.5      numpy 2.3.3        matplotlib 3.10.7
      seaborn 0.13.2    scipy 1.16.2       scikit-learn 1.9.1
      statsmodels 0.15.0                   ipywidgets 8.1.9
      jupyterlab 4.6.3                     python 3.13.7


INSTALLATION
--------------------------------------------------------------------------------

  Minimum:

      python -m pip install pandas numpy jupyterlab

  Full set used during development:

      python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn ^
          statsmodels ipywidgets tabulate openpyxl xlrd requests

  If PyPI is slow or unreachable, use a mirror:

      python -m pip install pandas numpy jupyterlab ^
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  (The caret "^" is the Windows command-line line-continuation character. On
  macOS or Linux use "\" instead, or simply put everything on one line.)


PROJECT STRUCTURE
--------------------------------------------------------------------------------

  <project folder>/
  |
  +-- ... Module 01 - Lab01 ... Used Car Pricing.ipynb
  |       Notebook 1 - reads and cleans the Used Cars data set.
  |
  +-- ... Module 01 - Lab02 ... Laptop Pricing.ipynb
  |       Notebook 2 - reads and cleans the Laptop Pricing data set.
  |
  +-- data/
  |   +-- auto.csv
  |   |       Used Cars data - 205 rows x 26 columns, no header row.
  |   +-- laptop_pricing_dataset_base.csv
  |           Laptop Pricing data - 238 rows x 12 columns, no header row.
  |
  +-- screenshots/
  |       Captured output images (optional).
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  Both files are comma-separated, UTF-8, and HAVE NO HEADER ROW - the column
  names are supplied by the notebooks themselves. Missing values are written as
  the literal character "?" in the Used Cars file.

  File                              Rows    Cols    Source
  --------------------------------  ------  ------  --------------------------
  auto.csv                          205     26      UCI ML Repository,
                                                    "Automobile" (imports-85)
  laptop_pricing_dataset_base.csv   238     12      IBM Skills Network

  Download URLs:

    auto.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/auto.csv

    laptop_pricing_dataset_base.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_base.csv

  IMPORTANT: the notebooks read the data DIRECTLY FROM THE URLS ABOVE, not from
  the data/ folder. The files in data/ are offline copies of exactly the same
  content; to use them, point the read call at the local path instead. Running
  the notebooks as shipped therefore requires network access.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel that has pandas installed.

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells

  The notebooks contain no randomness and no interactive input, so a full run is
  deterministic. Lab 01 finishes by writing automobile.csv (201 data rows) into
  the working directory.


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Lab 01 - Used Cars Pricing
      1.  import pandas as pd / import numpy as np
      2.  df = pd.read_csv(filepath, header=None)      -> 205 rows x 26 cols
      3.  df.head(5)                                   preview first rows
      4.  df.tail(10)                                  preview last rows
      5.  headers = [...26 names...]                   names from the UCI
                                                       attribute documentation
      6.  df.columns = headers                         attach the names
      7.  df.head(10)                                  re-check
      8.  df1 = df.replace('?', np.nan)                mark missing values
          df = df1.dropna(subset=["price"], axis=0)    drop rows without price
                                                       -> 201 rows remain
      9.  print(df.columns)                            list the column names
      10. df.to_csv("automobile.csv", index=False)     export the clean frame
      11. df.dtypes                                    column data types
      12. df.describe()                                numeric summary
      13. df.describe(include="all")                   full summary
      14. df[['length', 'compression-ratio']].describe()
                                                       summary of a subset
      15. df.info()                                    structure overview

  Lab 02 - Laptop Pricing
      1.  import pandas as pd / import numpy as np
      2.  df = pd.read_csv(filepath, header=None)      -> 238 rows x 12 cols
      3.  print(df.head())                             confirm the load
      4.  headers = [...12 names...]
          df.columns = headers
          print(df.head(10))                           confirm the headers
      5.  df.replace('?', np.nan, inplace=True)        mark missing values
      6.  print(df.dtypes)                             column data types
      7.  print(df.describe(include='all'))            full summary
      8.  print(df.info())                             structure overview


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both notebooks are derived from the IBM Skills Network "Importing Data
      Sets" lab templates, which are shipped as JupyterLite versions. They were
      adapted so that they run in a normal local Jupyter installation.

  2.  JupyterLite download cells removed.
      Four cells used "from pyodide.http import pyfetch" plus an async
      "download()" helper. The pyodide module exists only inside JupyterLite and
      raises ModuleNotFoundError in a regular kernel. Those cells were removed;
      the data is read straight from its source URL, which is exactly the
      fallback the templates themselves describe in a note cell. The error
      outputs produced by the originals were cleared and the notebooks were
      re-executed.

  3.  Redundant read removed (Lab 01).
      A cell calling pd.read_csv(file_name) without header=None would have
      consumed the first data record as a column header. It belonged to the
      JupyterLite branch and was removed.

  4.  "raw" cell converted to "code" (Lab 01).
      The to_csv export was stored as a raw cell, so Jupyter never executed it.
      It is now a normal code cell and the export genuinely runs.

  5.  Kernel metadata.
      The notebooks declare the kernel name "csp". If that kernel does not exist
      on your machine, pick any Python kernel - the metadata only sets the
      default and does not affect the code.


VERSION NOTES (pandas 3.x)
--------------------------------------------------------------------------------

  The original lab material was written for pandas 1.x / 2.x. Three output
  differences appear under pandas 3.x and are expected, not errors:

  a)  dtype of text columns
          pandas 1.x / 2.x : object
          pandas 3.x       : str     (new default string dtype)

  b)  class name printed by df.info()
          pandas 1.x / 2.x : <class 'pandas.core.frame.DataFrame'>
          pandas 3.x       : <class 'pandas.DataFrame'>

  c)  Lab 02, the replace() step
          pandas 1.x / 2.x : no output, because inplace=True returns None
          pandas 3.x       : renders the DataFrame below the cell, because the
                             call now returns the frame


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ModuleNotFoundError: No module named 'pyodide'
      You are running an unmodified JupyterLite template. Either delete the
      download cells or use the URL-based read shown above.

  KeyError: 'price', or the first data row appears as column names
      pd.read_csv() was called without header=None, so the first record was
      consumed as the header. Add header=None.

  Text columns show "str" instead of "object"
      Expected under pandas 3.x. Nothing to fix.

  Charts do not appear
      Add "%matplotlib inline" in the cell that imports matplotlib, or make sure
      the kernel has matplotlib installed.

  pip install is very slow or times out
      Add a mirror:
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  URLError / connection error when running a notebook
      The notebooks read their CSV files over the network. Restore connectivity,
      or point the read call at the local copies in data/.


================================================================================
