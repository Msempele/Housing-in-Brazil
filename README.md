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
In this phase, I performed two key analyses:

#### 1. Regional Price Comparison
The aim was to determine whether there are significant differences in home prices across Brazil’s regions. I calculated the average price for each region and visualized the results using a bar chart.
```python
# Group by region and calculate the mean price
regional_price_avg = df.groupby("region")["price_usd"].mean().sort_values(ascending=False)

# Plot average prices by region
regional_price_avg.plot(kind='bar', color='skyblue')

# Label axes and add title
plt.xlabel("Region")
plt.ylabel("Average Price [USD]")
plt.title("Average Home Prices by Region")

# Show the plot
plt.show()

```
#### Findings:

- The average price of homes varies significantly between regions.
- Certain regions, such as the Southeast, tend to have higher home prices compared to others like the North and Northeast.

#### 2. Relationship Between Home Size and Price in Southern Brazil
I also examined the relationship between home size and price specifically in the Southern region of Brazil, where a significant part of the analysis was focused.

Code Example:
```python
# Filter for properties in the South region
south_df = df[df["region"] == "South"]

# Calculate correlation between home size and price
correlation = south_df["area_m2"].corr(south_df["price_usd"])
print(f"Correlation between home size and price in Southern Brazil: {correlation}")

# Visualize the relationship using a scatter plot
plt.scatter(south_df["area_m2"], south_df["price_usd"], alpha=0.5)
plt.xlabel("Area [sq meters]")
plt.ylabel("Price [USD]")
plt.title("Home Size vs. Price in Southern Brazil")
plt.show()

```
#### Findings:

- The correlation between home size and price in Southern Brazil was found to be 0.5316779420492636.
- The scatter plot shows that larger homes tend to be priced higher, although there is some variability.

### Results / Findings
Based on the exploratory data analysis and deeper investigation into the dataset, the following key insights were uncovered:

#### 1. Regional Price Differences
There are significant variations in home prices across different regions of Brazil. The Southeast region tends to have the highest average home prices, while the North and Northeast regions have more affordable homes.

- Southeast Region: Highest average home prices, likely due to economic concentration and urbanization.
- North and Northeast Regions: Lower average home prices, possibly due to less developed infrastructure and economic activity.
  
This regional disparity suggests that location is a crucial factor influencing home prices in Brazil, with homes in more economically developed areas commanding higher prices.

#### 2. Relationship Between Home Size and Price in Southern Brazil
Focusing on the Southern region, I explored whether larger homes tend to have higher prices. The correlation analysis revealed a positive relationship between home size (area in square meters) and price.

- Correlation Coefficient: 0.5316779420492636, indicating a moderate/strong positive relationship between home size and price in the South.
  
While larger homes do tend to be priced higher, the scatter plot indicates some variability, suggesting that other factors (such as location, amenities, and property type) also play a role in determining home prices.

#### 3. Skewness in Home Size and Price Distribution
The distribution of home prices is right-skewed, indicating that most properties are in the lower price range, with a few outliers at the high end. Similarly, the distribution of home sizes shows that most properties are within the 50–200m² range, with a smaller number of larger homes.

- Home Price Distribution: Right-skewed, with a concentration of properties at the lower end of the price range.
- Home Size Distribution: Also right-skewed, with most homes falling in the 50–200m² range.
- 
These skewed distributions indicate that the Brazilian real estate market contains more affordable, smaller properties, with fewer large and expensive homes.

#### 4. Geographical Distribution of Properties
The scatter map visualization of property prices across Brazil shows a clear clustering of higher-priced homes in economically developed regions, such as major cities (e.g., São Paulo and Rio de Janeiro).

- Clustering: High-priced properties are concentrated around urban centers, indicating that city locations play a major role in driving up real estate prices.
- Southern Brazil: Homes in the South generally follow a more moderate pricing trend compared to the Southeast.
### Recommendations
Based on the findings from the analysis of Brazil’s housing market, I offer the following recommendations:

#### 1. For Homebuyers
- Consider Regional Price Disparities: Homebuyers should carefully assess which region best aligns with their budget. For those seeking more affordable options, the North and Northeast regions offer lower average prices. However, those prioritizing economic opportunities or urban amenities may need to consider the higher prices in the Southeast region.

- Southern Brazil for Moderate Prices: Homebuyers looking for a balance between affordability and quality of life may find the South region a good compromise. While home prices are moderately high, they are lower than in the Southeast, with a wider selection of medium-sized homes.

#### 2. For Real Estate Investors
- Urban Areas for High ROI: Investors looking to maximize returns should focus on major cities like São Paulo and Rio de Janeiro, where property values are consistently high and in demand. These areas present more opportunities for price appreciation and rental income.

- Southern Region for Emerging Growth: While the Southeast region is already highly developed, the Southern region shows potential for growth. Investors could consider purchasing larger homes in the South, as the positive correlation between size and price suggests future price increases for larger properties in this area.

#### 3. For Policymakers
- Regional Investment in Infrastructure: To balance the real estate market, policymakers should invest in developing the infrastructure of the North and Northeast regions. Improving transportation, public services, and economic opportunities in these regions could attract more residents and investors, thereby stabilizing home prices.

- Affordable Housing Initiatives: The skewed distribution of home prices, with a majority of affordable properties, indicates a need for policies that ensure continued availability of affordable housing. Policymakers should focus on affordable housing programs, particularly in regions where price growth is rapid.

#### 4. Future Considerations
- Monitor Market Trends: Both homebuyers and investors should monitor future market trends, especially as regional infrastructure projects and urban development could shift demand and prices in various regions.

- Evaluate Other Factors: While the analysis highlights the importance of home size and location, other factors like neighborhood amenities, proximity to schools or public transportation, and environmental conditions should also be considered when making buying or investment decisions.





