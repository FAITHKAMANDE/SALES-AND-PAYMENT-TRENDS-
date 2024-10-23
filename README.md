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

