# Wedge Co-Op Transaction Data Project

This project offers hands-on experience with data engineering, focusing on cleaning, processing, and analyzing transaction data from the Wedge Co-Op, the largest cooperative grocery store in the US, located in Minneapolis, MN. The project involves transforming raw data into a format suitable for analysis using Google BigQuery and building summary tables in a relational database.

## Introduction

Data engineering plays a crucial role in data analysis, as shaping raw data into consumable formats can take up to 80% of the time spent on analytics work. This project helps develop the skills required to define, clean, and process large datasets into meaningful structures, preparing you for real-world data engineering tasks.

The Wedge Co-Op transaction data spans from **January 2010 to January 2017**, capturing every transaction logged at the store. With about **75% of transactions tied to co-op owners**, this dataset provides a detailed view of shopping habits and changes over time.

However, the dataset also contains non-purchase records, such as payments, taxes, discounts, and charity donations, adding complexity to the cleaning process. This project helps you overcome those challenges and transform the raw POS data into an organized format for analysis.

## Data

The dataset includes detailed transaction records with fields such as:

- **Transaction Date and Time:** When the transaction occurred.
- **Employee and Register Info:** Details about the employee and register used.
- **Item Description and Quantity:** What items were purchased and in what amounts.
- **Total Transaction Cost:** The total cost of the transaction.

The raw data is provided in **CSV files** with mixed delimiters (commas and semicolons) and various representations for null values (`"NULL"`, `"\N"`, `"\\N"`). 

## Project Goals

### Task 1: Building a Transaction Database in Google BigQuery

- Upload all Wedge transaction records to BigQuery.  
- Ensure proper data types and handle null values correctly.

### Task 2: A Sample of Owners

- Generate a file containing every record for a subset of co-op owners.  
- Target a sample size of around 250 MB.

### Task 3: Building Summary Tables

- Create summary tables to quickly answer questions about sales trends, popular items, and top-spending owners.  
- Output the summary data into an SQLite database with tables for:
  - Sales by date
  - Sales by owner
  - Sales by product
