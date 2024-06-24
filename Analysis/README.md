# Data Analyst Job Market Analysis

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created to better navigate and understand the job market. It delves into top-paying and in-demand skills to help identify optimal job opportunities for data analysts.

The data is sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python), which provides a foundation for this analysis. It includes detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

## Research Questions

This project aims to answer the following questions:

1. What are the most in-demand skills for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

## Tools and Technologies

For an in-depth analysis of the data analyst job market, I used several key tools:

- **Python:** The core of my analysis, enabling data processing and insights. Key Python libraries used include:
    - **Pandas:** For data manipulation and analysis.
    - **Matplotlib:** For data visualization.
    - **Seaborn:** For creating advanced visualizations.
- **Jupyter Notebooks:** To run Python scripts, include notes, and present analysis effectively.
- **Visual Studio Code:** For developing and executing Python scripts.
- **Git & GitHub:** For version control and sharing the code and analysis, facilitating collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare and clean the data for analysis, ensuring accuracy and usability.

---

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter UK Jobs
To focus my analysis on the UK job market, I apply filters to the dataset, narrowing down to roles based in the United Kingdom.

```python
df_UK = df[df['job_country'] == 'United Kingdom']
df_skills = df_UK.explode('job_skills')
```

# The Analysis
Each Jupyter notebook for this project aims to investigate specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?
To identify the most demanded skills for the top 3 most popular data roles, I filtered out the positions by popularity and extracted the top 5 skills for these roles. This query highlights the most popular job titles and their top skills, showing which skills are crucial depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand](2_Skills_Demand.ipynb).

## Visualise Data
Counts of Skills Requested in UK Job Postings

```python
fig, ax = plt.subplots(len(job_titles), 1, figsize=(10, 15))

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], palette='dark:g')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 75)
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in UK Job Postings', fontsize=15)
fig.tight_layout(h_pad=.5)
plt.show()
```
### Results
#Insert Picture of Likelihood of Skills Requested in UK Job Postings

### Insights:

- SQL is the most requested skill for Data Analysts and Data Engineers, with it appearing in 43% and 60% of job postings, respectively. For Data Scientists, Python is the most sought-after skill, appearing in 69% of job postings.
- Data Engineers require more specialized technical skills (Azure 41%, AWS 33%, Spark 18%) compared to Data Analysts and Data Scientists, who are expected to be proficient in more general data management and analysis tools (Excel 41%, Power BI 27%, Tableau 16%).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (69%) and Data Engineers (55%).

## 1. How are in-demand skills trending for Data Analysts?

