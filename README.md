# Data Analysis Project for Data Jobs in 2023



---------------------------------
# Introduction
Welcome to my Python project showing a data analysis of the UK data job market in 2023. This project is comprised of an introductory analysis, four core analyses and an addendum.

## Tools I used:
* Python - the foundational tool used to code my analysis. I also used the following libraries:
    * Pandas - to analyse the dataset
    * Matplotlib - to visualise the data
    * Seaborn - to build more aesthetically pleasing visualisations
* Jupyter Notebooks - to run my code in separate cells and include my notes and headings for organisation
* VS Code - the industry standard code editor
* Git/Github - essential for version-control and publicising my work

---------------
Perhaps remove this?
## Questions I explore
In the introductory analysis, I explore the following questions:
- Which companies are most hiring in the UK?
- What percentage of jobs are work from home?
- What are the top 5 data jobs?
- What are the median salaries of the top 5 data jobs?
- How many data jobs are posted throughout the year?

In the first core analysis, I explore the following questions:
- Which skills have the highest demand across Data Analyst, Data Engineer and Data Scientist jobs?
- What is the likelihood of these skills appearing in the above job postings?

In the second core analysis, I explore the following questions:
- How does the demand for the top 5 skills trend throughout the year?
- How do the top 5 skills trend throughout the year percentage wise?

In the third core analysis, I explore the following questions:
- What are the average salaries for the top 5 skills?

In the fourth core analysis, I explore the following questions:
- What are the most valuable skills for a data professional to acquire?
A valuable skill is one that has a combination on high demand and a high salary

In the addendum, I explore the following questions:
- What are the most valuable skill types to acquire for a data professional?
- What is the demand for these skill types compared to their average salaries?
---------------

## Approach
Like with all data analyses, projects or pieces of work, it requires a multi-stage approach to get to the desired result. Each stage usually has multiple steps. And the desired result is usually a clean visualisation that tells a story.

### Stage 1 Importing Relevant Libraries and Data cleaning
The most vital step in data analysis work. Data cleaning usually relies on importing the relevant libraries to perform the necessary tasks.
If the process of data cleaning is done inadequately, it can lead to skewed data resulting in misleading visualisations. Misleading visualisations can lead to incorrect business decisions hindering business progress. In essence, business decisions need to be made on visualisation stories that are factually true and accurate. And factually true and accurate visualisation stories rely on clean data. 
So, hereunder are a few examples where I imported the relevant libraries and performed data cleaning throughout my project:
NB: I have added comments to aid understanding.
- importing the relevant libraries
``` python
# Importing Libraries
import ast # For string conversion into list object, and string conversion into dictionary object.
import pandas as pd
from datasets import load_dataset
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick # To add percentage sign to y axis ticks

plt.close('all') # close all open plots

import seaborn as sns

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()
```

- turning string values into python date object which can be further manipulated and analysed
``` python
# Data Cleanup
# make datetime string into datetime object
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])

```

- in core-analysis-1, turning string values in list objects so they can be exploded such as values in the job_skills column
``` python
# in column job_skills, make list from string into panda series (list) object using ast.literal_eval
df["job_skills"] = df["job_skills"].apply(lambda skill_list: ast.literal_eval(skill_list) if pd.notna(skill_list) else None)
```

- in the addendum, turning dictionaries into lists of tuples so they can be exploded and 'unpivoted'
``` python
# This essential wrapper function checks if there are strings in column job_type_skills and catches if there are na values
# Essential when running ast.literal_eval on column as it contains na values
def safe_parse_dict(val):
    # Only parse if the value is a non-null string
    if pd.notna(val) and isinstance(val, str):
        return ast.literal_eval(val)
    # Return an empty dictionary (or None) if the data is missing
    return {} 
 # Apply safe_parse_dict function to values in column job_type_skills
df_UK_copy["job_type_skills"] = df_UK_copy["job_type_skills"].apply(safe_parse_dict)
```
- in core-analysis-2, extracting month number from a converted Python date object to chronologically order my data
``` python
df_UK["job_posted_month_no"] = df_UK["job_posted_date"].dt.month
```

### Stage 2 Group by aggregations
Group by aggregations are a useful way of quickly aggregating relevant data to uncover insights. At times, group by aggregations can be plotted straight away for simple analyses. However, if a more stringent approach is required to analyse the data, then pivot tables offer a precise solution for that.
Below are a few examples where I performed group by aggregations throughout my project:
- in introductory-analysis, grouping the companies by the number of jobs they have posted
``` python
df_companies = df_UK.groupby("company_name").size()

# reset index to convert into dataframe
df_companies = df_companies.reset_index(name="count")

df_companies.sort_values(by="count", ascending=False, inplace=True)

df_companies
```

- in core-analysis-1, grouping skills and job titles by the number of skills mentioned within the job titles
``` python
# Group by to see how many jobs per skill grouped by job title
df_skills_count = df_skills.groupby(["job_skills", "job_title_short"]).size()

# reset index to convert into dataframe
df_skills_count = df_skills_count.reset_index(name="skill_count")

df_skills_count.sort_values(by="skill_count", ascending=False, inplace=True)
```

- in core-analysis-3, grouping skills by their count and adding a calculation for their median salaries
``` python
# Wrap groupby in parantheses to allow multi-line code block
df_UK_top_skills = (df_UK_exploded
                    .groupby("job_skills")["salary_year_avg"]
                    .agg(["count", "median"])
                    .sort_values(by="count", ascending=False)
                )
```

### Stage 3 Pivot tables
Pivot tables offer a very granular way to analyse data. Columns can be rearranged to become the index or remain as columns. Calculations can also be applied to the cells of a pivot table along with the option to merge pivot tables in the way tables are merged in SQL. With such a massive level of customisation, pivot tables are a common tool for an effective data professional, just as a kitchen knife is to a chef.
So, hereunder are a few examples where I utilised pivot tables throughout my project:
- In core-analysis-2, pivoting the data to uncover the number of skill mentions throughout the year
``` python
df_UK_exploded_pivot = df_UK_exploded.pivot_table(
    index="job_posted_month_no",
    columns="job_skills",
    aggfunc="size",
    fill_value=0, # fill empty cells with 0 instead of NaN
    
)
```

- In the addendum, to gain insight into the number of skill mentions in jobs for a target list of data profession job titles
``` python
skills_job_pivot = df_UK_group.pivot_table(
    index="skill_type",
    columns="job_title_short",
    values="job_count",
    aggfunc="sum",
    fill_value=0,

    margins=True,
    margins_name="Total"
)
```


### Stage 4 Create a subset of data
This involves reducing the dataset in the pivot table to only what values I wish to plot and assigning it to a variable. For example, out of a list of 100 skills, it is usually preferred to plot the top 10 skills. Using subsets of data reduces clutter on the final plot and allows ease of reading and accessibility. 
Subsets are also essential as they remove the need to directly change the code blocks that plot the analysis.
I used the subsets of data approach throughout my project. Below are some examples:
- in 1_introductory-analysis I created a subset of data to filter a dataframe by a target list of job titles:
``` python
job_list = ["Data Analyst", "Data Engineer", "Data Scientist"]

df_median_salaries_filtered = df_median_salaries[df_median_salaries["job_title_short"].isin(job_list)]
```

- in core-analysis-2, I created a subset of data highlighting the top 5 skills with the highest counts using the .iloc method
``` python
data_subset = df_UK_exploded_pivot.iloc[:, :5]

data_subset
```

- in core-analysis-4, I extract a subset of data comprising of the top 10 most valuable skills using the .head method. 
``` python
df_top_10 = df_UK_top_skills.head(10)

df_top_10
```


### Stage 5 Plot the data
I primarily used the industry standard Seaborn library to plot my analyses. This allows more visually pleasing plots akin to styled graphs in Microsoft Excel. However, there have been challenges in the case of adding data labels to scatter plots in particular. In a wysiwyg program such as Microsoft Excel, adding data labels is a matter of a few clicks. Whereas, using the Seaborn library (or Matplotlib for that matter), the operation can be more than a few lines of code. A good example of this added complexity is the subplots plotted in the final analysis of the addendum.
I plotted data throughout my project using the subsets of data approach outlined above. Below are some examples:

- in core-analysis-2 I plotted the top 5 skills on a line plot.
``` python
# Plot top 5 skills on line graph using Seaborn

sns.set_theme(style="ticks")

sns.lineplot(
    data=data_subset, # pass through subset of data
    dashes=False,
)

plt.title("What are the Top 5 Skills for Data Professionals throughout the Year?")
plt.legend(title="Skills", bbox_to_anchor=(1.05, 1), loc='upper left') # Make legend sit outside plot on top right by anchoring top left of legend box

plt.xlabel("Month")
plt.ylabel("Number of Jobs");
```
- in core-analysis-3 I plotted the median salaries of the top 5 in demand skills
``` python
sns.set_theme(style="whitegrid", font_scale=1.1)
plt.figure(figsize=(9, 5))

sns.barplot(
    data=df_salaries, # reset index to turn into dataframe, otherwise plot fails
    x="median",
    y="job_skills",

    hue="job_skills",
    palette="dark:teal_r"
     
);

plt.gca().xaxis.set_major_formatter(FuncFormatter(pounds_k)) # Format x axis ticks as £K

plt.title("Salaries of Top 5 skills")
plt.ylabel("")
plt.ylabel("Skill")
plt.tight_layout()
```

- in core-analysis-1, I plotted the highest skills in data job postings
``` python
# Set up grid layout and figure size
fig, ax = plt.subplots(len(job_list), 1, figsize=(7, 10))

# Use for loop to interate over job_list and bring top 5 skills for each job title
for i, job_name in enumerate(job_list):
    df_plot = df_skills_count[df_skills_count["job_title_short"] == job_name].head(5)
    sns.barplot(
        data=df_plot,
        y="job_skills", # Swap x and y values to make it horizontal bar plot
        x="skill_count",
        ax=ax[i],
        legend=False,

        hue="skill_count",
        palette="dark:b_r"
    )
    ax[i].set_title(job_name)
    ax[i].set_ylabel("")
    ax[i].set_xlabel("")
    ax[i].set_xlim(0, 7000) # Ensure uniformity in x axis values for ease of comparison

fig.suptitle("Highest skills in Data Job Postings in the UK", fontsize=15)
fig.tight_layout() # Prevent overlap between plots
plt.show();
```


Next I will describe my findings in this project.
perhaps summarise findings.
## Intro analysis
- Write findings, comment about approach if needed. 

# What skills are in in demand for data jobs?

# How do skills trend throughout the year?

# What are the salaries for the most in demand skills? 

# What is the likelihood of a skill appearing in a job? And how does this correlate to its salary?


# Addendum: What types of skills appear in jobs? And how does this correlate to its salary?
do a bar chart showing count and percentage etc.


---
# Top 5 in-demand skills for Data jobs in the UK
In this analysis, I found that:
* for Data Analyst roles, SQL and Excel had the highest demand with 43% and 41% respectively
* for Data Engineer roles, SQL and Python had the highest demand with 43% and 41% respectively
* for Data Scientist roles, Python and SQL had the highest demand with 60% and 55% respectively
* for all three roles, Python and SQL continously feature within the most in-demand skills
* In Data Engineer and Data Scientist roles, cloud technologies feature as the next most-in demand skills after geenral tools such as Python and SQL.
