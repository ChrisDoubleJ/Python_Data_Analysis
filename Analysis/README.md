# Data Analyst Career Trends & Insights

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

## 2. How are in-demand skills trending for Data Analysts in 2023?

Using data from job postings for data analyst positions, the top skills were identified and tracked throughout 2023. The results reveal insights into how the demand for these skills has shifted over the year.

### Insights:
- SQL: This skill has remained the most consistently demanded throughout the year, despite a slight decline towards the end.
- Excel: Demonstrated steady demand with minor fluctuations, maintaining its position as a critical skill for data analysts.
- Python: Saw an increase in demand towards the latter part of the year, indicating growing importance.
- Power BI: Experienced a notable rise in demand, especially in the final months of 2023.
- Tableau: Despite being essential, its demand has been relatively stable with some variability.

## Visualise Data
The line plot illustrates the likelihood of job postings mentioning each skill by month, providing a clear view of the trends over 2023.

Code Snippet for Visualization:

```python
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_UK_percent.iloc[:, :5]
line_palette = sns.color_palette('tab10', n_colors=len(df_plot.columns))

sns.lineplot(data=df_plot, dashes=False, palette=line_palette)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the UK')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

sorted_indices = df_plot.iloc[-1].sort_values(ascending=False).index
y_offsets = [0.7 * i for i in range(len(sorted_indices))]  # Adjust the offset as necessary

for i, (column_name, offset) in enumerate(zip(sorted_indices, y_offsets)):
    line_color = line_palette[df_plot.columns.get_loc(column_name)]
    plt.text(
        x=11.2,  # Position to the right end of the plot
        y=df_plot[column_name].iloc[-1] - offset,  # Adjust y position with offset
        s=column_name,  # Skill name
        color=line_color,  # Match text color to line color
        fontsize=12  # Adjust the font size as needed
    )

plt.show()
```

### Results
#Insert Picture of Trending Top Skills for Data Analysts in the UK
Bar graph visualszing the trending top skills for data analysts in the UK in 2023.


## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Wage_Analysis](4_Wage_Analysis.ipynb).

#### Visualise Data 

```python
sns.boxplot(data=df_UK_top6, x='salary_year_avg_gbp', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Salary Distributions of Data Jobs in the UK')
plt.xlabel('Yearly Salary (GBP)')
plt.ylabel('')
plt.xlim(0, 200000)  # Convert the x-axis limit to GBP
ticks_x = plt.FuncFormatter(lambda y, pos: f'£{int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results

#Insert Picture of Salary Distributions of Data Jobs in the UK
*Box plot visualizing the salary distributions for the top 6 data job titles.*

- There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to £200K, indicating the high value placed on advanced data skills and experience in the industry.
- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.
- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualise Data

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], palette='dark:g')
ax[0].legend().remove()
ax[0].set_title('Highest Paid Skills for Data Analysts in the UK')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'£{int(x/1000)}K'))

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], palette='light:g_r')
ax[1].legend().remove()
ax[1].set_title('Most In-Demand Skills for Data Analysts in the UK')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (GBP)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'£{int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```
#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the UK:

#Insert Picture of Salary Distributions of Data Jobs in the UK
*Two separate bar graphs visualising the highest paid skills and most in-demand skills for data analysts in the UK.*

### Insights from the Graph:

1. **Top Earning Skills**:
   - The top graph indicates that specialized technical skills like `dplyr`, `Bitbucket`, and `GitLab` are associated with higher salaries, some reaching up to £140K. This suggests that advanced technical proficiency can significantly increase earning potential.
   - Skills such as `Solidity`, `Hugging Face`, and `Couchbase` also contribute to higher median salaries, indicating a high market value for expertise in these areas.

2. **Most In-Demand Skills**:
   - The bottom graph highlights foundational skills like `Python`, `Tableau`, and `R` as the most in-demand, though they may not necessarily offer the highest salaries compared to the specialized skills. This underscores the importance of these core competencies for employability in data analysis roles.
   - Other in-demand skills include `SQL Server`, `Power BI`, and traditional tools like `Excel` and `PowerPoint`, reflecting their widespread use and essential role in data analysis.

3. **Career Strategy for Data Analysts**:
   - There's a distinct separation between the highest-paid skills and the most in-demand skills. For data analysts looking to maximize their career potential, it is crucial to develop a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.
   - By balancing expertise in high-value areas with proficiency in commonly used tools, data analysts can enhance their marketability and earning potential.

This analysis suggests that while niche technical skills can boost salary prospects, a strong foundation in widely used analytical tools is essential for securing employment and advancing in the data analysis field.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary (£GBP)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the UK')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```
### Results

#Insert Picture of Most Optimal Skills for Data Analysts in the UK
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the UK.*

### Insights:

1. **Highly Valued Skills**:
   - **Tableau** and **SQL** stand out with the highest median salaries, around £75K. These skills are also fairly prevalent in job listings, indicating they are both valuable and in demand.

2. **Common and Essential Skills**:
   - **Excel** and **SQL** appear to be the most common skills, required in approximately 40-45% of data analyst jobs. Their median salaries, around £60K-£70K, highlight their importance and demand in the job market.

3. **Emerging Skills with High Salaries**:
   - **Looker**, **Azure**, and **Power BI** are associated with high median salaries (around £70K) but are less common in job listings compared to Excel and SQL. This suggests that these skills might be more specialized or emerging, yet highly valued.

4. **Specialized Skills**:
   - **SAS** and **R** have decent median salaries (£55K-£60K) and are moderately common in job listings. These skills are crucial for statistical analysis and data manipulation.

5. **Lower Demand and Salary**:
   - Skills like **Word** and **Outlook** have the lowest median salaries (~£40K) and are less commonly required in job postings. This suggests that basic office skills are less valued for data analyst roles compared to specialized technical skills.

6. **Programming Skills**:
   - **Python** is noted for both a high median salary (~£65K) and a significant presence in job postings, indicating its critical role in data analysis and programming.

### Summary:

For data analysts in the UK, proficiency in Tableau and SQL can lead to the highest salaries. While common skills like Excel and SQL are essential, specialized skills such as Looker, Azure, and Power BI offer competitive salaries despite being less prevalent in job listings. Basic office skills like Word and Outlook are less relevant for high-paying data analyst roles.

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```python
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

# Prepare texts for adjustText
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='grey'))

# Set axis labels, title, and legend
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skills for Data Analysts in the UK')
plt.legend(title='Technology')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'£{int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

# Adjust layout and display plot 
plt.tight_layout()
plt.show()
```
#### Results

#Insert Picture of Most Optimal Skills for Data Analysts in the UK
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the UK with color labels for technology.*

## Key Takeaways:

- **Programming skills (blue)** generally offer higher salaries compared to other categories, indicating a strong demand for technical expertise.
- **Analyst tools (orange)** like Tableau, Power BI, and Looker not only appear in a significant percentage of job postings but also offer competitive salaries, emphasizing their importance in the data analytics field.
- **Cloud skills (green)** like Azure are becoming more valuable, reflected in their competitive salaries despite their lower frequency in job postings.

This analysis suggests that aspiring data analysts in the UK should focus on building strong programming skills, particularly in SQL and Python, while also gaining proficiency in key data visualization tools and cloud platforms to maximize their job opportunities and salary potential.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
