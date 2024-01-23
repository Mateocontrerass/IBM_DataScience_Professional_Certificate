---
jupyter:
  kernelspec:
    display_name: base
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.9.7
  nbformat: 4
  nbformat_minor: 4
---

::: {.cell .markdown}
# **Hands-on lab: Exploratory Data Analysis - Laptops Pricing dataset**

Estimated time needed: **45** minutes

In this lab, you will use the skills acquired throughout the module, to
explore the effect of different features on the price of laptops.
:::

::: {.cell .markdown vscode="{\"languageId\":\"plaintext\"}"}
# Objectives

After completing this lab you will be able to:

-   Visualize individual feature patterns
-   Run descriptive statistical analysis on the dataset
-   Use groups and pivot tables to find the effect of categorical
    variables on price
-   Use Pearson Correlation to measure the interdependence between
    variables
:::

::: {.cell .markdown}
# Setup
:::

::: {.cell .markdown}
For this lab, we will be using the following libraries:

-   `skillsnetwork` for downloading the data
-   [`pandas`](https://pandas.pydata.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01)
    for managing the data.
-   [`numpy`](https://numpy.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01)
    for mathematical operations.
-   [`scipy`](https://docs.scipy.org/doc/scipy/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01)
    for statistical operations.
-   [`seaborn`](https://seaborn.pydata.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01)
    for visualizing the data.
-   [`matplotlib`](https://matplotlib.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01)
    for additional plotting tools.
:::

::: {.cell .markdown}
# Install Required Libraries

You can install the required libraries by simply running the
`pip install` command with a `%` sign before it. For this environment,
`seaborn` library requires installation.
:::

::: {.cell .code}
``` python
import piplite
await piplite.install('seaborn')
```
:::

::: {.cell .markdown}
### Importing Required Libraries

*We recommend you import all required libraries in one place (here):*
:::

::: {.cell .code execution_count="1"}
``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
%matplotlib inline
```
:::

::: {.cell .markdown}
# Import the dataset

You should download the modified version of the data set from the last
module. Run the following code block to download the CSV file to this
environment.

The functions below will download the dataset into your browser:
:::

::: {.cell .code execution_count="10"}
``` python
filepath="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DA0101EN-Coursera/laptop_pricing_dataset_mod2.csv"
```
:::

::: {.cell .markdown}
Import the file to a pandas dataframe.
:::

::: {.cell .code execution_count="28"}
``` python
df = pd.read_csv(filepath, header=0).iloc[:,2:]
df
```

::: {.output .execute_result execution_count="28"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Manufacturer</th>
      <th>Category</th>
      <th>GPU</th>
      <th>OS</th>
      <th>CPU_core</th>
      <th>Screen_Size_inch</th>
      <th>CPU_frequency</th>
      <th>RAM_GB</th>
      <th>Storage_GB_SSD</th>
      <th>Weight_pounds</th>
      <th>Price</th>
      <th>Price-binned</th>
      <th>Screen-Full_HD</th>
      <th>Screen-IPS_panel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Acer</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>14.0</td>
      <td>0.551724</td>
      <td>8</td>
      <td>256</td>
      <td>3.52800</td>
      <td>978</td>
      <td>Low</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dell</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>15.6</td>
      <td>0.689655</td>
      <td>4</td>
      <td>256</td>
      <td>4.85100</td>
      <td>634</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dell</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>15.6</td>
      <td>0.931034</td>
      <td>8</td>
      <td>256</td>
      <td>4.85100</td>
      <td>946</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Dell</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>13.3</td>
      <td>0.551724</td>
      <td>8</td>
      <td>128</td>
      <td>2.69010</td>
      <td>1244</td>
      <td>Low</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HP</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>15.6</td>
      <td>0.620690</td>
      <td>8</td>
      <td>256</td>
      <td>4.21155</td>
      <td>837</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>233</th>
      <td>Lenovo</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>14.0</td>
      <td>0.896552</td>
      <td>8</td>
      <td>256</td>
      <td>3.74850</td>
      <td>1891</td>
      <td>Medium</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>234</th>
      <td>Toshiba</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>13.3</td>
      <td>0.827586</td>
      <td>8</td>
      <td>256</td>
      <td>2.64600</td>
      <td>1950</td>
      <td>Medium</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>235</th>
      <td>Lenovo</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>12.0</td>
      <td>0.896552</td>
      <td>8</td>
      <td>256</td>
      <td>2.99880</td>
      <td>2236</td>
      <td>Medium</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>236</th>
      <td>Lenovo</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>15.6</td>
      <td>0.862069</td>
      <td>6</td>
      <td>256</td>
      <td>5.29200</td>
      <td>883</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>237</th>
      <td>Toshiba</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>14.0</td>
      <td>0.793103</td>
      <td>8</td>
      <td>256</td>
      <td>4.29975</td>
      <td>1499</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>238 rows × 14 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
Print the first 5 entries of the dataset to confirm loading.
:::

::: {.cell .code execution_count="29"}
``` python
df.head(5)
```

::: {.output .execute_result execution_count="29"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Manufacturer</th>
      <th>Category</th>
      <th>GPU</th>
      <th>OS</th>
      <th>CPU_core</th>
      <th>Screen_Size_inch</th>
      <th>CPU_frequency</th>
      <th>RAM_GB</th>
      <th>Storage_GB_SSD</th>
      <th>Weight_pounds</th>
      <th>Price</th>
      <th>Price-binned</th>
      <th>Screen-Full_HD</th>
      <th>Screen-IPS_panel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Acer</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>14.0</td>
      <td>0.551724</td>
      <td>8</td>
      <td>256</td>
      <td>3.52800</td>
      <td>978</td>
      <td>Low</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dell</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>15.6</td>
      <td>0.689655</td>
      <td>4</td>
      <td>256</td>
      <td>4.85100</td>
      <td>634</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dell</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>15.6</td>
      <td>0.931034</td>
      <td>8</td>
      <td>256</td>
      <td>4.85100</td>
      <td>946</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Dell</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>13.3</td>
      <td>0.551724</td>
      <td>8</td>
      <td>128</td>
      <td>2.69010</td>
      <td>1244</td>
      <td>Low</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HP</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>15.6</td>
      <td>0.620690</td>
      <td>8</td>
      <td>256</td>
      <td>4.21155</td>
      <td>837</td>
      <td>Low</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
# Task 1 - Visualize individual feature patterns

### Continuous valued features

Generate regression plots for each of the parameters \"CPU_frequency\",
\"Screen_Size_inch\" and \"Weight_pounds\" against \"Price\". Also,
print the value of correlation of each feature with \"Price\".
:::

::: {.cell .code execution_count="30"}
``` python
# Write your code below and press Shift+Enter to execute
# CPU_frequency plot

sns.regplot(x='CPU_frequency', y='Price', data=df)
plt.ylim(0,)

# Weak
```

::: {.output .execute_result execution_count="30"}
    (0.0, 3974.15)
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/e22d2265561e8bffe8c67cdbddfa01c7d3728ecb.png)
:::
:::

::: {.cell .code execution_count="31"}
``` python
# Write your code below and press Shift+Enter to execute
# Screen_Size_inch plot


sns.regplot(x='Screen_Size_inch', y='Price', data=df)
plt.ylim(0,)



# Weak
```

::: {.output .execute_result execution_count="31"}
    (0.0, 3974.15)
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/b26fc090cac63c9cc64d356cd793520e20570d3a.png)
:::
:::

::: {.cell .code execution_count="32"}
``` python
# Write your code below and press Shift+Enter to execute
# Weight_pounds plot


sns.regplot(x='Weight_pounds', y='Price', data=df)
plt.ylim(0,)

# Weak
```

::: {.output .execute_result execution_count="32"}
    (0.0, 3974.15)
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/7fe217ab3cd6a9d770e72436bcb158f3feb6629f.png)
:::
:::

::: {.cell .code execution_count="34"}
``` python
# Correlation values of the three attributes with Price

df[['Price','Weight_pounds','Screen_Size_inch','CPU_frequency']].corr()
```

::: {.output .execute_result execution_count="34"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Price</th>
      <th>Weight_pounds</th>
      <th>Screen_Size_inch</th>
      <th>CPU_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Price</th>
      <td>1.000000</td>
      <td>-0.050312</td>
      <td>-0.110644</td>
      <td>0.366666</td>
    </tr>
    <tr>
      <th>Weight_pounds</th>
      <td>-0.050312</td>
      <td>1.000000</td>
      <td>0.797534</td>
      <td>0.066522</td>
    </tr>
    <tr>
      <th>Screen_Size_inch</th>
      <td>-0.110644</td>
      <td>0.797534</td>
      <td>1.000000</td>
      <td>-0.000948</td>
    </tr>
    <tr>
      <th>CPU_frequency</th>
      <td>0.366666</td>
      <td>0.066522</td>
      <td>-0.000948</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
Interpretation: \"CPU_frequency\" has a 36% positive correlation with
the price of the laptops. The other two parameters have weak correlation
with price.
:::

::: {.cell .markdown}
### Categorical features

Generate Box plots for the different feature that hold categorical
values. These features would be \"Category\", \"GPU\", \"OS\",
\"CPU_core\", \"RAM_GB\", \"Storage_GB_SSD\"
:::

::: {.cell .code execution_count="36"}
``` python
# Category Box plot

sns.boxplot(x='Category', y='Price', data=df)
```

::: {.output .execute_result execution_count="36"}
    <Axes: xlabel='Category', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/fc85830002a68b694947d075ab56688c44c94c38.png)
:::
:::

::: {.cell .code execution_count="38"}
``` python
# GPU Box plot

sns.boxplot(x='GPU', y='Price', data=df)
```

::: {.output .execute_result execution_count="38"}
    <Axes: xlabel='GPU', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/b458b54afb4644588f3bb74edeea3e59b89d9b90.png)
:::
:::

::: {.cell .markdown}
Interpretation: Computers with GPUs classified as 2 or 3 have higher
prices than those with GPUs classified as 1.
:::

::: {.cell .code execution_count="40"}
``` python
# OS Box plot

sns.boxplot(x='OS', y='Price', data=df)
```

::: {.output .execute_result execution_count="40"}
    <Axes: xlabel='OS', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/28f6f3b4fefe779119d180971da3ac4194040fd3.png)
:::
:::

::: {.cell .markdown}
Interpretation: Computers with OS classified as 1 have higher prices on
average than those with OS number 2.
:::

::: {.cell .code execution_count="41"}
``` python
# CPU_core Box plot

sns.boxplot(x='CPU_core', y='Price', data=df)
```

::: {.output .execute_result execution_count="41"}
    <Axes: xlabel='CPU_core', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/708e527d78af44c4382df731d3cf7465c9c469db.png)
:::
:::

::: {.cell .markdown}
Interpretation: Computers with more CPU cores have higher prices.
:::

::: {.cell .code execution_count="42"}
``` python
# RAM_GB Box plot

sns.boxplot(x='RAM_GB', y='Price', data=df)
```

::: {.output .execute_result execution_count="42"}
    <Axes: xlabel='RAM_GB', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/faf77669f29843db93ef74a4cd0d53a5515192c8.png)
:::
:::

::: {.cell .markdown}
Interpretation: Computers with more RAM GB have higher prices than those
with lower RAM GB.
:::

::: {.cell .code execution_count="43"}
``` python
# Storage_GB_SSD Box plot

sns.boxplot(x='Storage_GB_SSD', y='Price', data=df)
```

::: {.output .execute_result execution_count="43"}
    <Axes: xlabel='Storage_GB_SSD', ylabel='Price'>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/4cb4a28b633f04e459296eaa25b4613f11ebc641.png)
:::
:::

::: {.cell .markdown}
Interpretation: Computers with bigger storage GB are more expensive that
those with less storage GB.
:::

::: {.cell .markdown}
# Task 2 - Descriptive Statistical Analysis
:::

::: {.cell .markdown}
Generate the statistical description of all the features being used in
the data set. Include \"object\" data types as well.
:::

::: {.cell .code execution_count="48"}
``` python
df.describe()
```

::: {.output .execute_result execution_count="48"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Category</th>
      <th>GPU</th>
      <th>OS</th>
      <th>CPU_core</th>
      <th>Screen_Size_inch</th>
      <th>CPU_frequency</th>
      <th>RAM_GB</th>
      <th>Storage_GB_SSD</th>
      <th>Weight_pounds</th>
      <th>Price</th>
      <th>Screen-Full_HD</th>
      <th>Screen-IPS_panel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
      <td>238.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.205882</td>
      <td>2.151261</td>
      <td>1.058824</td>
      <td>5.630252</td>
      <td>14.688655</td>
      <td>0.813822</td>
      <td>7.882353</td>
      <td>245.781513</td>
      <td>4.106221</td>
      <td>1462.344538</td>
      <td>0.676471</td>
      <td>0.323529</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.776533</td>
      <td>0.638282</td>
      <td>0.235790</td>
      <td>1.241787</td>
      <td>1.166045</td>
      <td>0.141860</td>
      <td>2.482603</td>
      <td>34.765316</td>
      <td>1.078442</td>
      <td>574.607699</td>
      <td>0.468809</td>
      <td>0.468809</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>12.000000</td>
      <td>0.413793</td>
      <td>4.000000</td>
      <td>128.000000</td>
      <td>1.786050</td>
      <td>527.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>5.000000</td>
      <td>14.000000</td>
      <td>0.689655</td>
      <td>8.000000</td>
      <td>256.000000</td>
      <td>3.246863</td>
      <td>1066.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>5.000000</td>
      <td>15.000000</td>
      <td>0.862069</td>
      <td>8.000000</td>
      <td>256.000000</td>
      <td>4.106221</td>
      <td>1333.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>7.000000</td>
      <td>15.600000</td>
      <td>0.931034</td>
      <td>8.000000</td>
      <td>256.000000</td>
      <td>4.851000</td>
      <td>1777.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>7.000000</td>
      <td>17.300000</td>
      <td>1.000000</td>
      <td>16.000000</td>
      <td>256.000000</td>
      <td>7.938000</td>
      <td>3810.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .code execution_count="49"}
``` python
df.describe(include=['object'])
```

::: {.output .execute_result execution_count="49"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Manufacturer</th>
      <th>Price-binned</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>238</td>
      <td>238</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>11</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Dell</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>71</td>
      <td>160</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
# Task 3 - GroupBy and Pivot Tables

Group the parameters \"GPU\", \"CPU_core\" and \"Price\" to make a pivot
table and visualize this connection using the pcolor plot.
:::

::: {.cell .code execution_count="60"}
``` python
# Create the group
df_group =df[['Price','CPU_core','GPU']]
group = df_group.groupby(['CPU_core','GPU'],as_index=False).mean()
group
```

::: {.output .execute_result execution_count="60"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CPU_core</th>
      <th>GPU</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>1</td>
      <td>769.250000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>2</td>
      <td>785.076923</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3</td>
      <td>784.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>1</td>
      <td>998.500000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2</td>
      <td>1462.197674</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>3</td>
      <td>1220.680000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>1</td>
      <td>1167.941176</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>2</td>
      <td>1744.621622</td>
    </tr>
    <tr>
      <th>8</th>
      <td>7</td>
      <td>3</td>
      <td>1945.097561</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .code execution_count="66"}
``` python
# Create the Pivot table

pivot_df = group.pivot(index='GPU', columns='CPU_core')
pivot_df
```

::: {.output .execute_result execution_count="66"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">Price</th>
    </tr>
    <tr>
      <th>CPU_core</th>
      <th>3</th>
      <th>5</th>
      <th>7</th>
    </tr>
    <tr>
      <th>GPU</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>769.250000</td>
      <td>998.500000</td>
      <td>1167.941176</td>
    </tr>
    <tr>
      <th>2</th>
      <td>785.076923</td>
      <td>1462.197674</td>
      <td>1744.621622</td>
    </tr>
    <tr>
      <th>3</th>
      <td>784.000000</td>
      <td>1220.680000</td>
      <td>1945.097561</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .code execution_count="71"}
``` python
fig, ax = plt.subplots()
im = ax.pcolor(pivot_df, cmap='RdBu')

#label names
row_labels = pivot_df.columns.levels[1]
col_labels = pivot_df.index

#move ticks and labels to the center
ax.set_xticks(np.arange(pivot_df.shape[1]) + 0.5, minor=False)
ax.set_yticks(np.arange(pivot_df.shape[0]) + 0.5, minor=False)

#insert labels
ax.set_xticklabels(row_labels, minor=False)
ax.set_yticklabels(col_labels, minor=False)

fig.colorbar(im)
```

::: {.output .execute_result execution_count="71"}
    <matplotlib.colorbar.Colorbar at 0x21c817d8af0>
:::

::: {.output .display_data}
![](vertopal_5ddfe6d9de4a432cbc6ba7ac88db96a3/2c18dc7a274d7d0712fb24eab67d1e0c02b86a4c.png)
:::
:::

::: {.cell .markdown}
# Task 4 - Pearson Correlation and p-values

Use the `scipy.stats.pearsonr()` function to evaluate the Pearson
Coefficient and the p-values for each parameter tested above. This will
help you determine the parameters most likely to have a strong effect on
the price of the laptops.
:::

::: {.cell .code execution_count="85"}
``` python
# Write your code below and press Shift+Enter to execute



for cols in df[['CPU_core','GPU']]:
    cor, p =stats.pearsonr(df[cols],df['Price'])
    print(f'The correlation coef of {cols} and Price is {cor} and the p-value is {p}')
```

::: {.output .stream .stdout}
    The correlation coef of CPU_core and Price is 0.4593977773355117 and the p-value is 7.912950127009034e-14
    The correlation coef of GPU and Price is 0.28829819888814273 and the p-value is 6.166949698364282e-06
:::
:::

::: {.cell .code execution_count="1"}
``` python
jupyter nbconvert --execute --to markdown README.ipynb
```

::: {.output .error ename="SyntaxError" evalue="invalid syntax (3612450416.py, line 1)"}
      Cell In[1], line 1
        jupyter nbconvert --execute --to markdown README.ipynb
                ^
    SyntaxError: invalid syntax
:::
:::
