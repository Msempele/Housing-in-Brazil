# Housing-in-Brazil

### Project Overview
The Housing in Brazil project aims to analyze regional variations in the real estate market across Brazil, focusing on homes for sale. This analysis seeks to uncover whether there are significant differences in home prices across various regions. Additionally, the project delves into the real estate market in southern Brazil to explore the potential relationship between home size and price.

By performing a comprehensive data analysis, this project provides insights into how location impacts home prices and investigates whether larger homes in southern Brazil tend to be priced higher. The results of this analysis can help inform homebuyers, real estate investors, and policymakers about regional market trends and factors influencing home prices.
### Table of Contents
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning / Preparation](#data-cleaning--preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results / Findings](#results--findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)

### Data Sources
1. brazil-real-estate-1.csv: This file contains the primary dataset with various attributes of real estate properties across different regions in Brazil.
2. brazil-real-estate-2.csv: This is an additional dataset that complements the primary file.
   
The datasets include key variables such as:
- property_type: Type of property (e.g., apartment, house)
- place_with_parent_names: Location details (city, state, country)
- region: The geographical region (e.g., Northeast, South)
- area_m2: The size of the property in square meters
- price_usd: The price of the property in USD
- lat-lon: Latitude and longitude coordinates for each property
- price_brl: The price of the property in Brazilian Reais (BRL).
### Tools
The following tools and technologies were used for data analysis and visualization:
1. Python: The core programming language used for data manipulation and analysis.
2. Jupyter Notebook: An interactive environment to write and execute Python code, ideal for data exploration and presenting the results.
3. Pandas: A Python library used for data manipulation and analysis. 
4. NumPy: A fundamental library for numerical computations in Python, used for handling arrays and mathematical operations.
5. Matplotlib & Plotly: Visualization libraries used to create graphs and plots, helping to better understand trends and relationships in the data.
6. CSV: The format of the datasets containing real estate data used for analysis.

### Data Cleaning / Preparation
In this section, we describe the steps taken to clean and prepare the data for analysis. This involves handling missing values, converting data types, and creating new columns to facilitate our analysis.

1. Loading the Data
   - The primary datasets, brazil-real-estate-1.csv and brazil-real-estate-2.csv, were loaded into DataFrames df1 and df2, respectively.

2. Handling Missing Values
   - For df1:
     - Latitude and Longitude: The lat-lon column was split into two separate columns, lat and lon. Rows with missing lat-lon values were removed.
     - Price Conversion: The price_usd column was converted from an object type to a float after removing non-numeric characters.
     - Column Removal: The original lat-lon and place_with_parent_names columns were dropped, as they were no longer needed after creating lat and lon columns and extracting the state column.
   - For df2:
     - Column Removal: The price_brl column was dropped, as it was no longer needed after converting prices to USD.
     - Missing Values: Any rows with NaN values were removed to ensure data integrity.
       
3. Data Transformation
   - Creating New Columns:
     - Latitude and Longitude: The lat-lon column was split into lat and lon, and their data types were converted to float.
   - State Extraction:
     - The place_with_parent_names column was used to extract the state name, creating a new state column.

4. Combining DataFrames
   - The cleaned DataFrames df1 and df2 were concatenated into a single DataFrame df. This combined DataFrame includes all relevant information from both sources, with a uniform structure for analysis.
### Exploratory Data Analysis
In this section, I explore the dataset to uncover patterns, trends, and relationships. EDA is crucial for understanding the data and forming hypotheses for further analysis. Here’s a summary of the key steps and findings:

#### Descriptive Statistics
I started by examining the basic statistics of the numerical columns in the dataset. This includes measures such as mean, median, standard deviation, minimum, and maximum values.
```python
# Summary statistics for area and price
summary_stats = df[["area_m2", "price_usd"]].describe()
print(summary_stats)
```
#### Findings:
- The area_m2 column shows the distribution of property sizes, with most properties falling in the 50–200m² range.
- The price_usd column reveals the range and distribution of property prices.
#### Data Distribution
I visualized the distribution of property sizes and prices using histograms and box plots. This helps to understand the frequency and spread of the data.

#### Histogram of Property Prices:
```python
import matplotlib.pyplot as plt

# Build histogram
plt.hist(df["price_usd"], bins=30, edgecolor='black')

# Label axes
plt.xlabel("Price [USD]")
plt.ylabel("Frequency")

# Add title
plt.title("Distribution of Home Prices")

# Save the plot
plt.savefig("images/1-5-12.png", dpi=150)
plt.show()

```
#### Findings:

The histogram indicates that home prices are right-skewed, meaning there are more lower-priced properties compared to higher-priced ones.

#### Box Plot of Property Sizes:
```python
# Build box plot
plt.boxplot(df["area_m2"], vert=False)

# Label x-axis
plt.xlabel("Area [sq meters]")

# Add title
plt.title("Distribution of Home Sizes")

# Save the plot
plt.savefig("images/1-5-13.png", dpi=150)
plt.show()

```

#### Findings:

The box plot reveals the spread of property sizes and identifies potential outliers. The right skew in the area_m2 distribution is evident.

#### Regional Analysis
I also examined how property prices vary across different regions of Brazil.
```python
import plotly.express as px

# Create scatter map
fig = px.scatter_mapbox(
    df,  # DataFrame with property data
    lat="lat",
    lon="lon",
    center={"lat": -15.78, "lon": -47.93},  # Center map on Brazil
    width=600,
    height=600,
    hover_data=["price_usd"],
)

# Update layout
fig.update_layout(mapbox_style="open-street-map")

# Show figure
fig.show()

```
#### Findings:

The scatter map visualizes the geographic distribution of properties and can highlight regional differences in pricing.
### Data Analysis

### Results / Findings

### Recommendations

### Limitations

### References




