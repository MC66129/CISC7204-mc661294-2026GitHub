================================================================================
EXPLORATORY DATA ANALYSIS - USED CARS AND LAPTOP PRICING
Two Jupyter notebooks for descriptive statistics, correlation and visualization
================================================================================


OVERVIEW
--------------------------------------------------------------------------------

This project contains two self-contained Jupyter notebooks that explore a data
set before any model is fitted:

  * inspect the structure and data types of every column
  * compute and interpret correlation coefficients (Pearson)
  * test whether a correlation is statistically significant with a p-value
  * plot individual feature patterns: regression plots for continuous features,
    box plots for categorical ones
  * produce descriptive statistics with describe()
  * count the units of each category with value_counts()
  * aggregate with groupby() and reshape the result into a pivot table
  * visualize the grouped result as an annotated heat map

  Lab 01 explores the Used Cars Pricing data set (201 rows x 29 columns).
  Lab 02 explores the Laptop Pricing data set (the "mod2" version).

Both notebooks are executed end to end and ship with their outputs saved, so the
results - including all 19 matplotlib/seaborn figures - can be read without
running anything.


REQUIREMENTS
--------------------------------------------------------------------------------

  Python ......... 3.13 or newer
  Required ....... pandas, numpy, matplotlib, seaborn, scipy, jupyterlab
  Optional ....... scikit-learn, statsmodels, ipywidgets, tabulate, openpyxl, xlrd

  Developed and tested against:
      pandas 3.0.5      numpy 2.3.3        matplotlib 3.10.7
      seaborn 0.13.2    scipy 1.16.2       scikit-learn 1.9.1
      statsmodels 0.15.0                   ipywidgets 8.1.9
      jupyterlab 4.6.3                     python 3.13.7


INSTALLATION
--------------------------------------------------------------------------------

  Minimum:

      python -m pip install pandas numpy matplotlib seaborn scipy jupyterlab

  Full set used during development:

      python -m pip install pandas numpy matplotlib seaborn scipy scikit-learn ^
          statsmodels ipywidgets tabulate openpyxl xlrd requests

  If PyPI is slow or unreachable, use a mirror:

      python -m pip install pandas numpy matplotlib seaborn scipy jupyterlab ^
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  (The caret "^" is the Windows command-line line-continuation character. On
  macOS or Linux use "\" instead, or simply put everything on one line.)


PROJECT STRUCTURE
--------------------------------------------------------------------------------

  <project folder>/
  |
  +-- ... Module 03 - Lab01 ... Used Car Pricing.ipynb
  |       Notebook 1 - EDA on the Used Cars data set (47 code cells,
  |       includes 5 "Question" tasks and 9 figures).
  |
  +-- ... Module 03 - Lab02 ... Laptop Pricing.ipynb
  |       Notebook 2 - EDA on the Laptop Pricing data set (18 code cells,
  |       includes 4 numbered "Task" blocks and 10 figures).
  |
  +-- data/
  |   +-- automobileEDA.csv
  |   |       Used Cars data - 201 rows x 29 columns, header row present.
  |   +-- laptop_pricing_dataset_mod2.csv
  |           Laptop Pricing data - 238 rows, header row present.
  |
  +-- screenshots/
  |       Captured output images (optional).
  |
  +-- README.txt
          This file.


DATA
--------------------------------------------------------------------------------

  File                                Rows    Notes
  ----------------------------------  ------  --------------------------------
  automobileEDA.csv                   201     header row present; 29 columns,
                                                a mix of numeric and text. This
                                                is the cleaned/encoded data set
                                                produced in the data-wrangling
                                                stage.
  laptop_pricing_dataset_mod2.csv     238     header row present; contains the
                                                columns Weight_pounds and
                                                Screen_Size_inch produced in the
                                                data-wrangling stage.

  Download URLs:

    automobileEDA.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-SkillsNetwork/labs/Data%20files/automobileEDA.csv

    laptop_pricing_dataset_mod2.csv
      https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/
      IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_mod2.csv

  IMPORTANT: the notebooks read the data DIRECTLY FROM THE URLS ABOVE, not from
  the data/ folder. The files in data/ are offline copies of exactly the same
  content; to use them, point the read call at the local path instead. Running
  the notebooks as shipped therefore requires network access.


RUNNING
--------------------------------------------------------------------------------

  1.  Start Jupyter from this folder:

          jupyter lab

  2.  Open either notebook.

  3.  Select a Python kernel that has pandas, seaborn and scipy installed.

  4.  Run everything:

          Kernel -> Restart Kernel and Run All Cells

  The notebooks contain no randomness and no interactive input, so a full run is
  deterministic. Every figure is produced with the inline matplotlib backend and
  is stored inside the notebook.


WHAT THE CODE DOES
--------------------------------------------------------------------------------

  Lab 01 - Used Cars Pricing (EDA)

    1.  Load the data
            path = ".../automobileEDA.csv"
            df = pd.read_csv(path) ; df.head()

    2.  Look at the column types
            print(df.dtypes)
        Question 1: the data type of "peak-rpm" is float64.

    3.  Correlation matrix
            df.corr(numeric_only=True)
        Question 2: correlation of bore / stroke / compression-ratio / horsepower
            df[['bore','stroke','compression-ratio','horsepower']].corr()

    4.  Continuous features vs price (regression plots + correlation)
            engine-size  vs price   -> +0.87   strong positive
            highway-mpg  vs price   -> -0.70   strong negative
            peak-rpm     vs price   -> -0.10   weak
        Question 3a: df[["stroke","price"]].corr()   -> 0.082 (weak)
        Question 3b: sns.regplot(x="stroke", y="price", data=df)
                     a weak correlation means the regression line fits poorly

    5.  Categorical features vs price (box plots)
            body-style, engine-location, drive-wheels

    6.  Descriptive statistics
            df.describe()                       numeric columns
            df.describe(include=['object'])     categorical columns

    7.  Value counts
            df['drive-wheels'].value_counts().to_frame()
            df['engine-location'].value_counts().to_frame()

    8.  Grouping
            df_group_one.groupby(['drive-wheels'], as_index=False)
                         .mean(numeric_only=True)
            df_gptest.groupby(['drive-wheels','body-style'], as_index=False)
                      .mean(numeric_only=True)
        Question 4: average price per body-style
            df[['body-style','price']].groupby(['body-style'], as_index=False)
                                      .mean(numeric_only=True)

    9.  Pivot table and heat map
            grouped_pivot = grouped_test1.pivot(index='drive-wheels',
                                                columns='body-style')
            grouped_pivot = grouped_pivot.fillna(0)
            then plt.pcolor(...) twice - once plain, once with axis labels

   10.  Pearson correlation with p-values (scipy.stats.pearsonr)
            wheel-base, horsepower, length, width, curb-weight,
            engine-size, bore, city-mpg, highway-mpg
        A p-value below 0.001 means the correlation is statistically
        significant; engine-size vs price has the strongest linear relationship.

  Lab 02 - Laptop Pricing (EDA)

    Task 1  regression plots for the continuous features
                CPU_frequency, Screen_Size_inch, Weight_pounds   vs Price
            plus the correlation of each of them with Price
            (CPU_frequency ~0.36 - the strongest of the three)
    --      box plots for the categorical features
                Category, GPU, OS, CPU_core, RAM_GB, Storage_GB_SSD vs Price
    Task 2  descriptive statistics
                print(df.describe())
                print(df.describe(include=['object']))
    Task 3  groupby + pivot table + heat map
                df[['GPU','CPU_core','Price']].groupby(['GPU','CPU_core'],
                                                       as_index=False).mean()
                .pivot(index='GPU', columns='CPU_core')
    Task 4  Pearson coefficient and p-value for
                RAM_GB, CPU_frequency, Storage_GB_SSD, Screen_Size_inch,
                Weight_pounds, CPU_core, OS, GPU, Category


IMPLEMENTATION NOTES
--------------------------------------------------------------------------------

  1.  Origin of the notebooks.
      Both notebooks are derived from the IBM Skills Network "Exploratory Data
      Analysis" lab templates. Lab 02 is shipped as a JupyterLite version; Lab 01
      already reads its CSV straight from a URL.

  2.  JupyterLite cells removed (Lab 02).
      The template installed seaborn with "import piplite; await
      piplite.install('seaborn')" and downloaded the data with
      "from pyodide.http import pyfetch" plus an async download() helper. Neither
      piplite nor pyodide exists in a regular kernel. Those cells were removed and
      the data is read straight from its source URL, which is the fallback the
      template itself describes in a note cell.

  3.  pandas 3 fix: df.corr() on a frame that contains text columns.
      The templates call

          df.corr()

      on a frame that still holds text columns such as "make" and "body-style".
      pandas 1.x silently ignored non-numeric columns; pandas 3 raises
      "ValueError: could not convert string to float". Both calls were changed to

          df.corr(numeric_only=True)

      (Lab 01, two occurrences).

  4.  pandas 3 fix: groupby(...).mean() on a frame that contains text columns.
      The templates compute

          df_gptest.groupby(['drive-wheels','body-style'], as_index=False).mean()

      pandas 1.x dropped the text columns; pandas 3 raises
      "TypeError: dtype 'str' does not support operation 'mean'". The three
      aggregation calls were changed to .mean(numeric_only=True)
      (Lab 01 cells for drive-wheels, drive-wheels + body-style, and the
      Question 4 answer).

  5.  Question / Task answer cells filled in.
      The templates leave the "Write your code below" cells empty. They now
      contain working code for Lab 01 Questions 1 to 4 (peak-rpm dtype, subset
      correlation, stroke vs price correlation and its regplot, average price per
      body-style) and for Lab 02 Tasks 1 to 4.

  6.  Kernel metadata.
      The notebooks declare the kernel name "csp". If that kernel does not exist
      on your machine, pick any Python kernel - the metadata only sets the default
      and does not affect the code.


VERSION NOTES (pandas 3.x)
--------------------------------------------------------------------------------

  The original lab material was written for pandas 1.x / 2.x. The following
  differences appear under pandas 3.x:

  a)  df.corr() and groupby(...).mean() no longer silently skip text columns -
      they raise. Both were fixed explicitly (see implementation notes 3 and 4).
      This one changes behaviour, which is why the code was rewritten.

  b)  dtype of text columns
          pandas 1.x / 2.x : object
          pandas 3.x       : str     (new default string dtype)

  c)  class name printed by df.info()
          pandas 1.x / 2.x : <class 'pandas.core.frame.DataFrame'>
          pandas 3.x       : <class 'pandas.DataFrame'>

  d)  value_counts() prints a header line ("Name: count, dtype: int64") that the
      older output did not have.

  e)  Figures are embedded in the notebook as PNG output; make sure the plotting
      cells keep their "%matplotlib inline" line.


TROUBLESHOOTING
--------------------------------------------------------------------------------

  ValueError: could not convert string to float: 'alfa-romero'
      df.corr() was called on a frame that contains text columns. Use
          df.corr(numeric_only=True)

  TypeError: dtype 'str' does not support operation 'mean'
      groupby(...).mean() was called on a frame that contains text columns. Use
          df.groupby([...], as_index=False).mean(numeric_only=True)

  ModuleNotFoundError: No module named 'piplite' / 'pyodide'
      You are running an unmodified JupyterLite template. Remove those cells and
      read the CSV from its URL instead.

  Figures do not appear in the notebook
      Keep the "%matplotlib inline" lines, and make sure the kernel has
      matplotlib and seaborn installed.

  pip install is very slow or times out
      Add a mirror:
          -i https://pypi.tuna.tsinghua.edu.cn/simple

  URLError / connection error when running a notebook
      The notebooks read their CSV files over the network. Restore connectivity,
      or point the read calls at the local copies in data/.


================================================================================
