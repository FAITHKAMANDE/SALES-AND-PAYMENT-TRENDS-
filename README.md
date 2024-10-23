# SALES AND PAYMENT TRENDS
## 1.INTRODUCTION
This report presents a comprehensive analysis of the sales and payment data to uncover key trends, customer preferences, and performance insights. By examining transaction volumes, payment methods, and distribution patterns, we aim to provide a data-driven understanding of the business’s financial health and customer behavior. The report highlights significant findings such as the dominant role of MPESA as a payment method, the skewed distribution of sales and payments towards lower ranges, and the overall reports in both sales and payments over time.

The analysis serves as a foundation for strategic recommendations that can help the business enhance its operations. These include encouraging the use of preferred payment methods, targeting high-value customers, addressing declining trends, and optimizing credit card usage. By implementing these recommendations, the business can improve revenue consistency, reduce outstanding balances, and better meet customer needs.

## 2. TOOLS AND LIBRARIES
### TOOLS
1) Python
2) Jupyter Notebook
3) Pandas
### LIBRARIES
- **Libraries Used**: 
  - `pandas` for data manipulation
  - `numpy` for numerical operations
  - `matplotlib` and `seaborn` for visualization
  - `scipy.stats` for statistical analysis
  - `plotly.express` for interactive plots
  - `datetime` for working with date data
  - `penpyxl` for Excel file processing
  <img width="1256" alt="Screenshot 2024-10-18 at 17 08 16" src="https://github.com/user-attachments/assets/ce307bdc-32ff-4d69-ad6d-6b4c21ff4fd7">
## 3. READING AND UNDERSTANDING THE DATA
- The datasets are imported from CSV files. The two payment dataset contains payment information, while the sales dataset (sales.csv) tracks the sales transactions
- These datasets will be the basis of the analysis.
   ### UNDERSTANDING THE DATA
  - This section provides a quick overview of the data by displaying the first few rows of each dataset. This step helps in understanding:
    1) The structure of the data (e.g., number of rows and columns),the general information fo the data.
    2) Column names and whether they match expectations or need renaming for clarity.
    3) Potential data quality issues, such as missing values, inconsistent formatting, or outliers.
    4) The presence of any redundant columns that may need to be dropped.
    5) Types of variables (e.g., categorical, numerical, date) to determine appropriate cleaning and analysis methods.
  - By reviewing the first few rows, we can also identify if further preprocessing is required before diving into deeper analysis, ensuring that the data is clean, consistent, and ready for meaningful insights.
## 4. CLEANING THE DATA PROCESS 
1) DROPPING THE COLUMNS WITH NAN VALUES
- The data cleaning process is a crucial step to ensure that the dataset is structured and ready for analysis. In this stage i dropped the irrelevant columns: Columns Unnamed: 5 and Unnamed: 6 and number which contained no useful information. 
- This reduces noise and ensures only relevant data is retained.
  <img width="848" alt="Screenshot 2024-10-23 at 11 01 07" src="https://github.com/user-attachments/assets/c6db877a-2c7d-4515-93de-5539160311be">
2) DROPPING THE NUMBER COLUMNS
  ```python
  # DROPPING THE NUMBER COLUMN
  payments_copy.drop(columns = 'NUMBER',inplace = True)
  ```
  ```python
  # CHECKING IF  'NUMBER' IS IN COLUMNS
  if 'NUMBER' in payments_copy.columns:
    print("The 'NUMBER' column exists.")
  else:
    print("The 'NUMBER' column does not exist.")
  ```  
3) REMOVING SPACES
- Column names were stripped of unnecessary spaces to standardize and avoid any potential referencing issues when manipulating the data.
<img width="1099" alt="Screenshot 2024-10-23 at 11 43 14" src="https://github.com/user-attachments/assets/ef3b117f-772d-43d1-92a3-34eba047794c">

4) CONVERTING THE DATA TYPES.
- The DATE columns in all datasets were converted to datetime format. This ensures that all dates are consistently formatted and allows for accurate date-based analysis.
<img width="797" alt="Screenshot 2024-10-23 at 13 34 41" src="https://github.com/user-attachments/assets/712ce170-9c8c-4782-b8de-8987d7d9634a">

```Python
  sales_copy["DATE"]= pd.to_datetime(sales_copy["DATE"],format = '%d/%m/%Y')
  sales_copy.head()
```
- While changing the data type of the second payment i realized that some dates (year) was a two digit number and others were four digit number ,i needed pandas to interprate them as year.
- The row had date value that could not be interpretated ,i identifyed it and coerced pandas to read as dayfirst
```Python
two_digit_years = second_payments_copy[second_payments_copy['DATE'].str.len() == 8]
two_digit_years

second_payments_copy['DATE'] = pd.to_datetime(second_payments_copy['DATE'], errors='coerce', dayfirst=True)
second_payments_copy['DATE']

missing_dates = second_payments_copy[second_payments_copy['DATE'].isna()]
missing_dates

second_payments_copy.at[24, 'DATE'] = pd.to_datetime('30/12/2023', format='%d/%m/%Y')

missing_dates = second_payments_copy[second_payments_copy['DATE'].isna()]
missing_dates
```
## 5. ANALYSIS
- Since i had two payments data that had the same values I concatenated them to make one data to work with.
  <img width="730" alt="Screenshot 2024-10-23 at 14 11 21" src="https://github.com/user-attachments/assets/e0d14534-9da6-4924-91a8-3fb56f89aebb">
### 1) Understanding the summary statistics
- Understanding the summary statistics is essential for gaining insights into the central tendencies, variability, and distribution patterns within a dataset, helping to uncover important trends and relationships.
<img width="657" alt="Screenshot 2024-10-23 at 14 22 51" src="https://github.com/user-attachments/assets/8203dfa6-7961-45d4-8ba7-9c69bc8a41d5">

*After the summary statistics the minimum transaction date is in year 2004 ,since my data contains recent transactions i immediately recognized the typing error in the date*

```Python
  #INDENTIFYING THE ROW WITH THE YEAR 2004.
  old_date = total_payments[total_payments['DATE'] == '2004-02-11']
  old_date 
```
```Python
  # CORRECTING THE YEAR FROM 2004 TO 2024
  old_date = total_payments['DATE'] == pd.Timestamp('2004-02-11')
  total_payments.loc[old_date, 'DATE'] = total_payments.loc[old_date, 'DATE'].apply(lambda x: x.replace(year=2024))
  print(total_payments[total_payments['DATE'] == '2024-02-11']) 
```
### 2) Understand the distribution of payment amounts
```Python
sns.histplot(total_payments['AMOUNT'], kde=True,bins=30,alpha = 0.7)
plt.figure(figsize=(16, 10))
plt.show()
```
<img width="691" alt="Screenshot 2024-10-23 at 14 36 45" src="https://github.com/user-attachments/assets/7ac3fb23-04de-442d-9a20-2c27d27a792a">

- The amount distribution is heavily right-skewed,indicating that most payments amounts are below 5000 with a few large payments exting the tail to the right
- The highest frequency of payment falls within the range of 0-2500,suggesting this is the most common payment amount range.
- There are large payments amounts exceeding 20000.
### 3) Understand the distribution of sales
```Python
sns.histplot(sales_copy['SALES'], kde=True)
plt.show()
```

- The sales distribution is also right-skewed,indicating that most sales are concentrated in the lower range between 0-2500 with a few large sales exceeding 10000
- The highest frequency of sales falls within the range of 0 to 2500 ,suggesting this is the most common sales amount.
- There are severall large sales exceeding 10000.
<img width="786" alt="Screenshot 2024-10-23 at 14 43 34" src="https://github.com/user-attachments/assets/166a12a6-d106-42ed-abdc-056b25b9890e">

### 4) Identifying the overall trends in payments over time
```Python
payments_monthly = total_payments.resample('ME', on='DATE').sum().sort_values(by='AMOUNT', ascending=True)
payments_monthly
```
```
payments_monthly = total_payments.resample('ME', on='DATE').sum().sort_values(by='AMOUNT', ascending=True)
payments_monthly = payments_monthly.drop(columns=['NAME', 'METHOD'])
payments_monthly.index = payments_monthly.index.strftime('%B')

payments_monthly
```
<img width="1053" alt="Screenshot 2024-10-23 at 15 15 27" src="https://github.com/user-attachments/assets/8233e6b2-e768-4a1b-af7b-9b0d239540b1">

### 5)  Identifying the overall trends in sales over time
```
#TOTAL SALES PER END-MONTH
sales_monthly = sales_copy.resample('ME', on='DATE').sum()
sales_monthly
```
```
sales_monthly = sales_copy.resample('ME', on='DATE').sum().sort_values(by='SALES', ascending=True)
sales_monthly = sales_monthly.drop(columns=['NAMES'])
sales_monthly.index = sales_monthly.index.strftime('%B')

sales_monthly
```
<img width="884" alt="Screenshot 2024-10-23 at 15 34 54" src="https://github.com/user-attachments/assets/5f2910f5-ad2c-4d48-904b-52410e4725e6">

### 6) VISUALIZATION OF THE PAYMENTS AND SALES TRENDS

```Python
plt.figure(figsize=(14, 6))
plt.plot(payments_monthly.index, payments_monthly['AMOUNT'], label='Payments')
plt.plot(sales_monthly.index, sales_monthly['SALES'], label='Sales')
plt.legend()
plt.show()
 ```
<img width="1135" alt="Screenshot 2024-10-23 at 15 43 34" src="https://github.com/user-attachments/assets/a0e9e115-e261-4e89-b0c3-1433aea30a68">

- The sales trend shows a decline throught the year ,starting high and steadly decreasing.
- The payment trend also shows a decline same as the sales trend
- The trends suggests a strong correlation between the sales and the payments indicating the sales are equally linked to the payments.
- The two lines intersect around july suggesting a point where the sales equaled the payments ,before this point,sales where higher and the payments were slagging suggesting there were outstanding payments.
- After the intersection the payments exceeded the sales indicating customers were paying off the oustanding balances.
  
### 7) Analyzing the distribution of different payment methods

``` Python
total_payments['METHOD'] = total_payments['METHOD'].apply(lambda x: 'MPESA' if x.startswith('S')or x.startswith('R')or x.startswith('A') else x)
total_payments['METHOD'].value_counts()
```
``` Python
total_payments['METHOD'] = total_payments['METHOD'].replace({'CREDIT CARD': 'CREDITCARD'})
total_payments['METHOD'].value_counts()
```
| Method | Count |
| ----------- | ----------- |
| MPESA | 1027 |
| CASH  | 258  |
|CREDITCARD| 74|

- MPESA is the most frequently used payment method, accounting for 1,027 transactions. This indicates a strong preference for mobile money payments, likely due to its convenience and widespread use in the region.
- CASH payments come in second, with 258 transactions, showing that a notable portion of customers still prefer traditional payment methods.
- CREDIT CARD payments are the least used, with only 74 transactions. This suggests either limited credit card usage among the customer base or a preference for other payment methods.




