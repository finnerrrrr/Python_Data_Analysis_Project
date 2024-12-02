# Overview 

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was done with the intention to better understand the data job market, using the data source to draw out some key insights such as the top paying jobs and in-demand skills, to find the top job opportunities for data analysts. 

The data is sourced from Luke Barousse's Python course. Through a series of python scripts, I explore key questions such as the most demanded technical skills, salary trends and intersection of demand and salary in data analytics. 

# The Questions

These are the guiding questions I set out to answer with this project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

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

For the detailed steps, click here: [2_Skill_Demand](/Project/2_Skill_Demand)

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
![Visualisation1](/assets/visualisation1.png)

## Insights

1. For Data Analyst and Data Engineers, SQL is the most sought after skill, with over 50% of job listings including the skill. For Data Scientists, Python is the most sought after skill by far, beating out SQL, the second-most sought after skill, by 20 percent.
   
2. Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
   
3. Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

# 2. How are the in-demand skills trending for Data Analysts

After finding out the top 5 skills for Data Analysts, I grouped the skills by the month of the job posting. THis gave me the frequency in which the skill was mentioned in job postings each month in 2023, allowing analyse how they trended in 2023. 

For the detailed steps, click here: [3_Skill_Trends](/Project/3_Skill_Trends)

## Viuslisation 

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_da_us_perc.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```
## Results

![visulisation2](/assets/visualisation2.png)

__Line graph of the top 5 skills trending over the months in 2023__

## Insights 

1. SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
   
2. Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
   
3. Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

# 3. How well do different Jobs and Skills pay for Data Analysts

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

For the detailed steps, click here: [4_Salary_Analysis](/Project/4_Salary_Analysis)

## Visualisation

```python
sns.boxplot(data=df_us_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
## Results
![visualisation3](/assets/visualisation3.png)

## Insights

1. There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

2. Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

4. The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## Highest Paid and Most in Demand Skill for Data Analyst

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

## Visualisation

![visualisation4](/assets/visualisation4.png)

__Horizontal bar charts displaying the salaries of the highest paid and most in-demand skills__

## Insights:

1. The top graph shows specialized technical skills like dplyr, Bitbucket, and Gitlab are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.

2. The bottom graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

3. There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

# 4. Most Optimal Skills to learn for Data Analysts

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

For the detailed steps, click here: [5_Optimal_Skills](/Project/5_Optimal_Skills)

## Visualisation 

![visualisation5](/assets/visualisation5.png)

__Scatter plot visualising most Optimal skills (highest demand & salary) for Data Analysts__

## Insights 

1. The skill Oracle appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.

2. More commonly required skills like Excel and SQL have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.

3. Skills such as Python, Tableau, and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

## Improving Visualisation 

![visualisation6](/assets/visualisation6.png)

__Scatter plot visualising most Optimal skills (highest demand & salary) for Data Analysts with colour labels for type of technology__

## Insights

1. The scatter plot shows that most of the programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

2. The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

3. Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# What I learned

Throughout this project, I deepened my understanding of the data job market and enhanced my data analysis skills using python, especially in data manipulation and visualisation. 

1. Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
   
3. Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
   
4. Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights
This project provided several general insights into the data job market for analysts:

1. Skill Demand and Salary Correlation: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
   
2. Market Trends: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
   
3. Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:

1. Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

2. Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

3. Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.

