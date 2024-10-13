# Exploratory Data Analysis
## Overview
This project focuses on exploratory data analysis (EDA) of global job postings for data science jobs for the year 2023. It includes detailed insights into various aspects such as the locations of job postings, the organizations behind them, salary offerings, and additional relevant factors. Data originated from the Philippines will be prioritized in this project but can be changed to places like the United States should the data count presented is low.
## Objectives
1. Identify the most in-demand skills across various job postings in different data science jobs.
2. Analyze trends in the demand for specific job skills for Data Analysts over the course of the year.
3. Compare salary offers across different data science roles. Identify the highest-paying skills in data science and contrast them with the salary ranges associated with the most frequently requested skills.
4. Determine the optimal data science skills to learn by comparing their salary potential with their demand in the job market.
# Output
## Objectve #1
### Methodology
1. Filter the data to see only job postings in the Philippines.
2. Group the job titles with their corresponding job skills count.
3. Sort the values to identify which are the the top skills for different job titles.
4. Determine the percentage of job skills relative to the total job listing count.
   
View my notebook to see the detailed steps. [View Jupyter Notebook](https://github.com/JohnDataAnalytics/Exploratory-Data-Analysis-Using-Python/blob/main/Project%20Notebooks/1_Skill_Demand.ipynb)

### Data Visualization
```python
fig, ax = plt.subplots(len(job_titles), 1)

def to_percent(x, _):
    return f'{x:.0f}%'

for i, job_title in enumerate (job_titles):
    df_plot = df_skills_count_new[df_skills_count_new['job_title_short'] == job_title].head(top_no)
    sn.barplot(data=df_plot,x='%',y='job_skills',ax=ax[i],hue='skill_count')
    ax[i].set_title(job_title)
    ax[i].set_xlabel("")
    ax[i].set_ylabel("")
    ax[i].set_xlim(0,72)
    ax[i].xaxis.set_major_formatter(FuncFormatter(to_percent))
    ax[i].legend().set_visible(False)

    if i !=  len(job_titles)-1:
        ax[i].set_xticks([])

    for n, v in enumerate(df_plot['%']):
        ax[i].text(v+0.5, n, f'{v:.2f}%', va='center')

fig.suptitle('Most Demanded Skills in Different Analyst Roles in PH',fontsize=15)
plt.tight_layout()
plt.show()
```

### Results
![image](https://github.com/user-attachments/assets/cb707c17-dc79-471f-8663-1762e3387985)

*Bar graph that visualizes the demand for different data science skills for the top 3 data science jobs in the Philippines*

### Insights
- Unlike other countries, it is seen in the visualization that when it comes to Data Analysts and Business Analysts, excel, compared to SQL, is far more favored by the employers despite its fundamental nature in data analysis.
- Excel and SQL are critical across all three roles, with Excel being slightly more important for business and data analysts, while SQL is crucial for data engineers.
- Python is highly demanded for data engineers but less so for data analysts and business analysts.
- Data visualization tools like Tableau and Power BI are similarly important for both business and data analysts but not as relevant for data engineers.
- Cloud platforms like Azure and AWS are especially important for data engineers, reflecting the technical infrastructure needs of this role.

## Objective #2
### Methodology
1. Filter the dataframe for data analyst job postings in the Philippines
2. Determine the count of job postings and identify the top skills in terms of count.
3. Assign each job posting to the corresponding month
4. Determine the percenatage of each job skill listed.

View my notebook to see the detailed steps. [View Jupyter Notebook](https://github.com/JohnDataAnalytics/Exploratory-Data-Analysis-Using-Python/blob/main/Project%20Notebooks/2_Skills_Trend.ipynb)

### Data Visualization
```python
df_plot = df_percentages.iloc[: , :top_no]

ax = sns.lineplot(df_plot, dashes=False, palette= "tab10")
sns.despine()
plt.xlabel('Month')
plt.ylabel('Likelihood')
plt.legend().remove()
formatter = FuncFormatter(lambda x, _: f'{int(x)}%')
ax.yaxis.set_major_formatter(formatter)
plt.suptitle(f'Likelihood of the Top {top_no} Skills to be Demanded for Each Month in the Philippines', size=15)

plt.text(11.2, 50.1, df_percentages.columns[0])
plt.text(11.2, 33.2, df_percentages.columns[1])
plt.text(11.2, 23, df_percentages.columns[2])
plt.text(11.2, 25, df_percentages.columns[3])
plt.text(11.2, 26.9, df_percentages.columns[4])

plt.tight_layout()
plt.show()
```
### Results
![image](https://github.com/user-attachments/assets/60185b49-d6f2-4cdf-bc31-4b4d4a4cd85a)

*Line graph that shows the trend of the likelihood of the top data science skills to be demanded for each month throughout 2023*

### Insights
- Excel and SQL are core skills for data analysts, with stable demand throughout the year.
- Python, Power BI, and Tableau show more specialized or variable demand, suggesting that while they are important, their necessity might depend on specific roles or projects.
- Most job postings spike during July, and December.

## Objective #3
### Methodology
1. Filter the dataset to include only job postings that explicitly state salary ranges.
2. Sort and filter the data to show job postings for the top 6 most demanded data science roles.
3. Analyze and compare the salary ranges for these top 6 data science positions.
4. In the filtered dataset, determine which data science skills are most frequently demanded and which skills correlate with the highest salaries.
5. Create a chart comparing the most demanded skills with the highest paid skills.

View my notebook to see the detailed steps. [View Jupyter Notebook](https://github.com/JohnDataAnalytics/Exploratory-Data-Analysis-Using-Python/blob/main/Project%20Notebooks/3_Salary_Analysis.ipynb)

## Data Visualization
```python
sns.set_theme(style="ticks")
sns.boxplot(df_US_top6, x="salary_year_avg", y="job_title_short", order=job_order)#, hue="salary_year_avg")

plt.title("Salary Distribution of the Top 6 Data Science Jobs", size=15)
plt.xlabel("Yearly Salary")
plt.ylabel("")
def formatter(x, pos):
    return f'${int(x/1000)}k'

plt.gca().xaxis.set_major_formatter(FuncFormatter(formatter))

plt.show()
```
```python
fig, ax = plt.subplots(2,1)
sns.set_theme(style="ticks")

sns.barplot(top_paid_skills, x="median", y="job_skills" ,ax=ax[0], hue="median")
ax[0].set_title("Yearly Salary of the Top Paid Data Science Skills")
ax[0].set_xlabel("")
ax[0].set_xlim(0,225000)
ax[0].xaxis.set_major_formatter(FuncFormatter(formatter))
ax[0].set_ylabel("")
ax[0].legend().remove()

sns.barplot(most_popular_skills, x="median", y="job_skills", ax=ax[1], hue="median")
ax[1].set_title("Yearly Salary of the Most In-Demand Data Science Skills")
ax[1].set_xlim(0,225000)
ax[1].xaxis.set_major_formatter(FuncFormatter(formatter))
ax[1].set_xlabel("Yearly Salary (Median)")
ax[1].set_ylabel("")
ax[1].legend().remove()

plt.suptitle("Most In-Demand Data Science Skills Salary vs. Highest Paying Data Science Skills", size=14.5)
plt.tight_layout()
plt.show()
```

## Results
![image](https://github.com/user-attachments/assets/65e35c80-6719-4d92-9757-417dfb410167)

*Boxplot that visualizes the salary distribution of the top data science jobs*

![image](https://github.com/user-attachments/assets/f5ba8f91-d07c-4eb3-a03c-cfe2e57ee8bb)

*Two bar graphs that visualize the salary offerings for the highest paid data science skills and the most demanded data science skills*

## Insights
- Machine Learning Engineers command the highest median salary, with some exceeding $300k, indicating strong earning potential.
- Senior Data Scientists have a high median salary, typically ranging from $150k to $200k, with notable outliers earning more.
- Senior Data Engineers offer similar salaries to Senior Data Scientists but generally have higher top-end earnings, sometimes surpassing $250k.
- Data Scientists earn less than senior roles, with their top quartile approaching $200k, making it a competitive position.
- Data Engineers typically earn more than Data Analysts, with a consistent salary range around $100k to $150k.
- Data Analysts have the lowest median salary, with few high earners reaching close to $150k.
- Power BI surprisingly topped the highest median salary for job postings in 2023 surpassing skills like python and its data visualization competitor tableau.
- PyTorch represented the higest salary offering based on the data gathered in 2023.

## Objective #4
### Methodology
1. Filter the dataset to include only job postings that specify salary offerings.
2. Identify the most in-demand job skills based on the filtered dataset.
3. Sort the skills by their median salary.
4. Calculate the percentage of each skill relative to the total number of job postings.

View my notebook to see the detailed steps. [View Jupyter Notebook](https://github.com/JohnDataAnalytics/Exploratory-Data-Analysis-Using-Python/blob/main/Project%20Notebooks/4_Optimal_Skills.ipynb)

### Data Visualization
```python
sns.set_theme(style="ticks")
sns.scatterplot(df_plot, x="%", y="Median Salary", color="purple")
sns.despine()

def formatter(x, pos):
    return f'${int(x/1000)}k'
plt.gca().yaxis.set_major_formatter(FuncFormatter(formatter))

def formatter1(x, pos):
    return f'{int(x)}%'
plt.gca().xaxis.set_major_formatter(FuncFormatter(formatter1))

for i in range(10):
    plt.annotate(df_plot.index[i], (df_plot.iloc[i,2], df_plot.iloc[i,1]),textcoords="offset points", xytext=(5, 5), ha='left')

plt.legend().remove()
plt.xlabel('Likelihood')
plt.ylabel('Yearly Salary (Median)')
plt.suptitle("Likelihood of Job Skill to be Demanded vs Yearly Median Salary Offer")
plt.tight_layout()
plt.show()
```

### Results
![image](https://github.com/user-attachments/assets/dfd57bce-3dbf-4fda-8930-186f06f1ad08)

*Scatter plot that visualizes the differences of each skills relative to their likelihood of being demanded and their median salary offerings*

### Insights
- Python stands out as the only skill located in the first quadrant, indicating that it commands both a high median yearly salary and strong demand in data science job postings.
- SQL has the highest likelihood of being requested in data science job listings, reflecting its critical importance in the field.
- There is an intriguing distinction between data visualization tools: while Power BI offers higher salary potential, Tableau enjoys greater demand in the job market.
- Conversely, R, a programming language used for data analytics, has underperformed this year, placing it in the third quadrant, which signifies low demand and low salary offerings.

# Ending Remarks
Thanks for taking the time to see my project! This project basically is the fruition of my hardwork, passion, and determination for data analytics. Despite numerous troubleshooting and finding out where I went wrong, I enjoyed it regardless. Although looking back at all the programming I did, I sure have a lot more to go - which is actually exciting! This project would not be possible without Luke Barousse's course on Python for Data Analytics which he uploads for everyone to see on Youtube; so if ever you're reading this, Luke, thanks a lot!
