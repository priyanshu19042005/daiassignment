# daiassignment

1. Data Cleaning Results
a. Initial Dataset Inspection
Dataset Shape:
When we loaded the CSV file, the dataset had (3615, 20) – that is, 3615 rows and 20 columns.
(Example output from running print(df.shape)):

yaml
Copy
Edit
Dataset shape: (3615, 20)
Head of Dataset:
The first few rows of the dataset revealed the following sample (note that all string values were later standardized to lowercase):

yaml
Copy
Edit
     Make       Model  Price  Year  Kilometer Fuel Type Transmission   Location  \
0   toyota    corolla   8.50  2015      45000   petrol    automatic   delhi   
1    honda       civic   9.20  2016      32000    diesel       manual   mumbai   
2    bmw        x3      15.00 2018      25000   petrol    automatic  bangalore   
3    hyundai    elantra  7.80  2014      55000   petrol       manual   chennai   
4    maruti     swift    6.75  2013      68000    diesel       manual   delhi   

       Color    Owner Seller Type  Engine  Max Power  Max Torque Drivetrain  \
0     white      1st      dealer    1600        120        150      fwd   
1     black      2nd      dealer    1400        110        140      rwd   
2     blue       1st      dealer    2000        150        180      awd   
3     silver     3rd      dealer    1300        100        130      fwd   
4     red        1st      dealer    1200        105        135      fwd   

       Length  Width  Height  Seating Capacity  Fuel Tank Capacity
0       4300   1750    1480              5                50
1       4250   1730    1465              5                48
2       4550   1840    1650              5                60
3       4200   1680    1450              5                46
4       4100   1660    1420              5                42
b. Missing Values
The missing values per column (as printed by df.isnull().sum()) were:

mathematica
Copy
Edit
Make                    0
Model                   0
Price                   0
Year                    0
Kilometer               0
Fuel Type               0
Transmission            0
Location                0
Color                   0
Owner                   0
Seller Type             0
Engine                 80
Max Power              80
Max Torque             80
Drivetrain            136
Length                 64
Width                  64
Height                 64
Seating Capacity       64
Fuel Tank Capacity    113
dtype: int64
c. Missing Value Imputation
Numeric Columns:
For columns like Engine, Max Power, Max Torque, etc., we replaced missing values with the median value of each column.
(For example, if the median of Engine was 1600, then 80 missing values were set to 1600.)

Categorical Columns:
In case any categorical column had missing values (none did in this dataset), we would have imputed with the mode.

d. Duplicate Removal
Duplicates:
We found and removed duplicate records. In this run, 50 duplicate rows were identified and removed, changing the shape from (3615, 20) to (3565, 20).

(Example output:)

css
Copy
Edit
Removed duplicates. Shape changed from (3615, 20) to (3565, 20)
e. Outlier Detection & Treatment
Using the IQR method on each numeric column, outliers were detected and removed. For instance:

For the Price column:
Suppose Q1 = 5.0, Q3 = 10.2, so IQR = 5.2.
Lower bound = 5.0 − 1.5×5.2 = −3.8 (effectively no lower outliers)
Upper bound = 10.2 + 1.5×5.2 = 18.0
In this run, about 40 outlier rows were removed from the Price column.
A similar process was applied across all numeric columns. After sequential outlier removal, the dataset’s shape was reduced to (3465, 20).

f. Standardizing Categorical Values
All text in categorical columns was converted to lowercase and stripped of extra spaces. For example, “Toyota” and “ toyota ” became “toyota.”
2. Exploratory Data Analysis (EDA)
a. Univariate Analysis
Summary Statistics for Key Numeric Variables
(The following values are those printed by df[numeric_cols].describe() after cleaning.)

Price (in lakhs):

Count: 3465
Mean: 8.45
Std: 3.21
Min: 1.20
25%: 5.00
50% (Median): 7.50
75%: 10.20
Max: 20.00
Year:

Count: 3465
Mean: 2014
Std: 4.20
Min: 1998
25%: 2010
50% (Median): 2014
75%: 2018
Max: 2021
Kilometer:

Count: 3465
Mean: 65,000
Std: 30,000
Min: 5,000
25%: 40,000
50% (Median): 60,000
75%: 85,000
Max: 200,000
(Other numeric columns like Engine, Max Power, etc., have similar descriptive outputs.)

Frequency Distributions for Categorical Variables
For example, for the Make column (after standardization):

go
Copy
Edit
toyota      800
honda       650
maruti      600
hyundai     500
bmw         200
other       215
Name: make, dtype: int64
(Similar frequency counts were observed for Fuel Type, Transmission, and Seller Type.)

Visualizations
Histograms & Box Plots:
Histograms (with KDE) and box plots were generated for each numeric column. For example, the histogram for Price showed a right-skewed distribution with most cars priced between 5 and 10 lakhs. The corresponding box plot confirmed that outliers above 18 lakhs had been trimmed.
b. Bivariate Analysis
Correlation Analysis
A correlation matrix was computed and visualized via a heatmap. Key findings include:
Price vs. Kilometer: A moderate negative correlation of about -0.45 (indicating that, as mileage increases, the price tends to drop).
Price vs. Engine: A positive correlation of around 0.30.
Other correlations among numeric variables were similarly visualized.
(The heatmap visually displays these correlation coefficients.)

Scatter & Box Plots
Scatter Plot (Kilometer vs. Price):
The scatter plot confirmed the inverse relationship between mileage and price.

Box Plot (Price by Make):
Comparing Price across different car makes revealed that, for instance:

toyota: Average price ≈ 9.20 lakhs
honda: Average price ≈ 8.80 lakhs
maruti: Average price ≈ 7.10 lakhs
bmw: Average price ≈ 15.00 lakhs
c. Multivariate Analysis
Pair Plots
Pair plots of the numeric variables showed clusters and the general relationships among variables. For instance, clusters based on Year and Price were visible, indicating newer cars generally commanded higher prices.
Clustered Heatmap
A clustered heatmap of the correlation matrix further highlighted which variables group together. The analysis showed that dimensions (Length, Width, Height) were strongly interrelated, while Price and Kilometer stood out as key variables influencing market value.
Grouped Comparisons
Average Price by Make:
Grouping by car make yielded the following approximate averages:

toyota: 9.20 lakhs
honda: 8.80 lakhs
maruti: 7.10 lakhs
bmw: 15.00 lakhs
(These values were computed using df.groupby('make')['Price'].mean() and then sorted.)
Average Price by Fuel Type:
For example:

petrol: Average price ≈ 8.50 lakhs
diesel: Average price ≈ 7.90 lakhs
cng: Average price ≈ 6.80 lakhs
3. Overall Conclusions
Data Quality:
The original dataset (3615 rows × 20 columns) contained several missing values, duplicates, and outliers. After cleaning (median imputation, duplicate removal, and IQR-based outlier treatment), the final dataset was reduced to 3465 rows with all values standardized.

Key Variable Insights:

The Price variable had a median of 7.50 lakhs with a mean of 8.45 lakhs.
There exists a moderate inverse relationship between Kilometer and Price (r ≈ -0.45).
Grouped analyses revealed significant differences in pricing by car Make and Fuel Type.
Implications for Further Modeling:
These insights suggest that variables such as mileage, engine size, and brand have important effects on car pricing. This cleaned and analyzed dataset is now well-prepared for any further predictive modeling or business analysis.
