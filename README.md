# Overview 

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was done with the intention to better understand the data job market, using the data source to draw out some key insights such as the top paying jobs and in-demand skills, to find the top job opportunities for data analysts. 

The data is sourced from Luke Barousse's Python course. Through a series of python scripts, I explore key questions such as the most demanded technical skills, salary trends and intersection of demand and salary in data analytics. 

# The Questions

These are the guiding questions I set out to answer with this project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying

# Tools I Used 

**Python:** The backbone of my analysis, used for analysis and visualisations. I also used the following libraries
  - Pandas Library: Used for data analysis
  - MatplotLib Library: Used to create visualisations of the data
  - Seaborn Library: Helped me to create more advanced visuals
**Jupyter Notebooks:** The tool I used to run my python scripts that easily allows me to include notes and analysis
**Visual Studio Code:** My go-to for executing my Python scripts.
**Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preperation and Clean up 

This section outlines the steps I took to clean and process the data used for my analysis. 

## Import and Clean Data

First step was to load in the necessary libraries and datasource, followed by cleaning it to ensure data quality.

```
# Importing libraries 
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Clean Data
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## 


