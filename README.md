# Rename-Clean-CSV-Files-in-a-Folder-Automation-Task-
Loops through all .csv files in a folder Cleans up missing values Adds a timestamp column Saves the file with a new name (prefixed with cleaned_) Great for automating pre-processing in ETL pipelines
import os
import pandas as pd
from datetime import datetime

# Folder containing CSV files
folder_path = "./eligibility_files/"

# Process each CSV in the folder
for filename in os.listdir(folder_path):
    if filename.endswith(".csv"):
        full_path = os.path.join(folder_path, filename)

        # Read the CSV
        df = pd.read_csv(full_path)

        # Basic data cleaning
        df.dropna(subset=['MemberID'], inplace=True)  # Drop rows missing key field
        df['UploadDate'] = datetime.now().strftime('%Y-%m-%d')  # Add processing date

        # Save cleaned file with new name
        cleaned_name = f"cleaned_{filename}"
        df.to_csv(os.path.join(folder_path, cleaned_name), index=False)

        print(f"Processed: {filename} → {cleaned_name}")
