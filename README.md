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

```python
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

## Filter US Jobs

I'm focusing my analysis on the US Job Market, so I'll filter my dataset for jobs that are located in the US 

```python
df_us = df[df['job_country'] == 'United States']
```

# The Analysis

Each Jupyter notebook is aimed at investigating different aspects of the job market and is used to answer the guidiing questions. Here's how i approached each question. 

# 1. What are the most demanded skills for the top 3 most popular data roles

I first have to filter for the top 3 data roles and include the top 5 skills for each of the 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

For the detailed steps, click here: [2_Skill_Demand](/Project/2_Skill_Demand/)

## Visualing the Data 

```python
ig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)
    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
```
## Results 
![Visualisation1](/assets


