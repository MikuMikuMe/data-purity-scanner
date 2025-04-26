# data-purity-scanner

Creating a "data-purity-scanner" can be a comprehensive task depending on the exact requirements and depth of analysis needed. Below is a simplified version of such a tool in Python. This script can serve as a basic framework to check for some common data quality issues in CSV files typically used in ETL pipelines. It includes comments and error handling for a clearer understanding of each part of the code.

```python
import pandas as pd
import numpy as np

class DataPurityScanner:
    def __init__(self, file_path):
        self.file_path = file_path
        self.dataframe = None

    def load_data(self):
        """Load data from a CSV file into a pandas DataFrame."""
        try:
            self.dataframe = pd.read_csv(self.file_path)
            print(f"Data loaded successfully from {self.file_path}")
        except FileNotFoundError:
            print(f"Error: File {self.file_path} not found.")
        except pd.errors.EmptyDataError:
            print("Error: The file is empty.")
        except pd.errors.ParserError:
            print("Error: Could not parse the file.")
        except Exception as e:
            print(f"An unexpected error occurred: {str(e)}")

    def check_missing_values(self):
        """Check for missing values in the DataFrame and output the results."""
        if self.dataframe is not None:
            missing_values = self.dataframe.isnull().sum()
            print("Missing values in each column:")
            print(missing_values[missing_values > 0])
        else:
            print("Error: Data not loaded. Cannot check for missing values.")

    def check_duplicates(self):
        """Check for duplicate rows in the DataFrame."""
        if self.dataframe is not None:
            duplicates = self.dataframe.duplicated().sum()
            print(f"Number of duplicate rows: {duplicates}")
        else:
            print("Error: Data not loaded. Cannot check for duplicates.")

    def check_anomalies(self):
        """Identify possible anomalies in numerical data."""
        if self.dataframe is not None:
            # Assuming numerical anomalies are outliers using z-score; this can be changed as needed
            numerical_data = self.dataframe.select_dtypes(include=[np.number])
            if not numerical_data.empty:
                z_scores = np.abs((numerical_data - numerical_data.mean()) / numerical_data.std())
                anomalies = np.where(z_scores > 3)
                print(f"Anomalies detected at positions: {anomalies}")
            else:
                print("No numerical data to check for anomalies.")
        else:
            print("Error: Data not loaded. Cannot check for anomalies.")

    def run_scan(self):
        """Run all data purity checks."""
        self.load_data()
        self.check_missing_values()
        self.check_duplicates()
        self.check_anomalies()

if __name__ == "__main__":
    # Replace 'file.csv' with the path to your actual CSV file
    scanner = DataPurityScanner('file.csv')
    scanner.run_scan()
```

### Key Points:
- **File Loading:** The program attempts to load a CSV file into a pandas DataFrame, handling various potential loading errors.
  
- **Data Quality Checks:**
  - **Missing Values:** It checks for missing values in each column.
  - **Duplicates:** It identifies duplicate rows within the dataset.
  - **Anomalies:** It detects anomalies in numerical data using the Z-score method; this can be adjusted based on specific requirements.

### Usage:
- Ensure you have the required packages installed (`pandas`, `numpy`). You can install them using pip:
  ```bash
  pip install pandas numpy
  ```
- Update `'file.csv'` with the path to your CSV file and run the program.

This script provides a starting point. Depending on the application, more sophisticated anomaly detection, data type validation, and domain-specific checks may be added.