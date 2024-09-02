# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output

## STEP 1: Read the Data

```
import pandas as pd

data = pd.read_csv('Loan_data (1).csv')

# Display the first few rows to verify
data.head()

```
### Output :
![image](https://github.com/user-attachments/assets/3a86a34d-caf9-41f4-b2ae-ce1e212b1af2)

## STEP 2: Get the Information About the Data
```
# Get the information about the data
data.info()

```

### Output :
![image](https://github.com/user-attachments/assets/c8748d62-c1b5-4cbb-b4cf-0e4401f51f0e)

## STEP 3: Check for null values
```
# Check for null values 
data.isnull().sum()
```
### Output :
![image](https://github.com/user-attachments/assets/0ece57c1-674b-4402-91e9-587ac205f79e)

## STEP 4 : Handling missing values
```
# option 1 : to drop the null values
# Remove the null values from the data
clean_data = data.dropna()

# Verify if any null values remain
print(clean_data.isnull().sum())

```
### Output :
![image](https://github.com/user-attachments/assets/c5087278-2ecf-461f-beb7-af1a130c6e51)

```
# option 2 : to fill the missing values
# Fill missing values for numerical columns with the median
data['LoanAmount'].fillna(data['LoanAmount'].median(), inplace=True)
data['Loan_Amount_Term'].fillna(data['Loan_Amount_Term'].median(), inplace=True)
data['Credit_History'].fillna(data['Credit_History'].median(), inplace=True)

# Fill missing values for categorical columns with the mode
data['Gender'].fillna(data['Gender'].mode()[0], inplace=True)
data['Married'].fillna(data['Married'].mode()[0], inplace=True)
data['Dependents'].fillna(data['Dependents'].mode()[0], inplace=True)
data['Self_Employed'].fillna(data['Self_Employed'].mode()[0], inplace=True)
data['Education'].fillna(data['Education'].mode()[0], inplace=True)

# Verify if any null values remain
print(data.isnull().sum())

```

### Output :
![image](https://github.com/user-attachments/assets/6784e7ca-eaf6-4d78-87de-ae6b74e544dd)

## STEP 4: Save the Clean Data to a File

```
# Save the clean data to a new CSV file
clean_data.to_csv('clean_loan_data.csv', index=False)

# Verify the saved file
print("Clean data saved to 'clean_loan_data.csv'")

```
### Output :
![image](https://github.com/user-attachments/assets/ee71d728-3a50-4867-8fce-042301b0816a)

## STEP 5: Remove Outliers Using IQR (Interquartile Range)
```
# Define a function to remove outliers based on IQR
def remove_outliers_iqr(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    df = df[(df[column] >= lower_bound) & (df[column] <= upper_bound)]
    return df

# Apply IQR method to relevant numeric columns
columns_to_check = ['ApplicantIncome', 'CoapplicantIncome', 'LoanAmount']
for column in columns_to_check:
    clean_data = remove_outliers_iqr(clean_data, column)

# Verify the data after removing outliers
print(clean_data.describe())

```
### Output :
![image](https://github.com/user-attachments/assets/3f3cec00-3359-47e9-9772-e7f31e45995f)

## STEP 6: Remove Outliers Using Z-Score
```
from scipy import stats

# Define a function to remove outliers based on Z-score
def remove_outliers_zscore(df, column, threshold=3):
    df = df[(stats.zscore(df[column]) < threshold)]
    return df

for column in columns_to_check:
    clean_data = remove_outliers_zscore(clean_data, column)

# Verify the data after removing outliers
clean_data.describe()

```
### Output :
![image](https://github.com/user-attachments/assets/a99dd48d-317e-461c-adee-82d9ce369aad)

# Result
Therefore, successfully handled null values and performed outlier handling.
