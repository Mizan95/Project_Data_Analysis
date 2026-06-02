# Data Analysis Project for Data Jobs in 2023


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


## Approach
Like with all data analyses, projects or pieces of work, it requires a multi-stage approach to get to the desired result. Each stage usually has multiple steps. And the desired result is usually a clean visualisation that tells a story.

### Stage 1 - Importing Relevant Libraries and Data cleaning
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

- in the addendum, turning dictionaries into lists of tuples so they can be exploded and 'unpivoted'.  
This involved a 6-step process. And to better show the effect of each step on the manipulated dataframe, I exported the output of each step into separate CSV files. The CSV files can be accessed [here](files_csv_convert_dictionaries_to_explodable_list/)
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

### Stage 2 - Group by aggregations
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

### Stage 3 - Pivot tables
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


### Stage 4 - Create a subset of data
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


### Stage 5 - Plot the data
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

# Analysis

## Introductory analysis
Link to file can be found [here](Introductory-analysis.ipynb)  

I found that the top 5 data jobs were: Data Engineer, Data Analyst, Data Scientist, Senior Data Engineer and Senior Data Scientist.

![](assets\001.png)  

This suggests that data engineering and data analyst roles lead because companies must build infrastructure and find immediate business value before they can run complex AI models. Most UK businesses are still in the early stages of data adoption, creating a massive foundational demand for these two specific skillsets.      

## What skills are in in demand for data jobs?
Link to file can be found [here](Core-analysis-2_Trend_Skills_in_Year.ipynb)  
I found that the top 5 skills in demand for data jobs were: SQL, Python, Azure, AWS and Excel.

![](assets\002.png) 

This suggests that SQL and Python are the top two skills because they are the universal languages of data, required across all data engineering, analysis, and science roles. With Azure and AWS competing for demand of cloud technologies.

## What are the salaries for the most in demand skills?
Link to file can be found [here](Core-analysis-3_Salaries_Most_Indemand_Skills.ipynb)  
I found that the top 5 skills with the highest median salaries were: AWS, Python, Tableu, SQL and Excel.

![](assets\003.png) 

This suggests that companies are willing to pay a premium for cloud based skills such as AWS commanding a premium of $131,000. This is whilst foundational skills such as Python and SQL still make it into the top 5 with median salaries of $111,000 and $105,000 respectively.

## What are the most valuable skills? Valuable means that they have a combination of high demand and salary.
Link to file can be found [here](Core-analysis-4_Most_Valuable_Skills.ipynb)  
I found that the most valuable skills are a mixture of foundational tools (Python, SQL), cloud technologies (AWS, Azure, GCP) and analyst tools (Power BI, Tableu, Excel).

![](assets\004.png) 

This suggests that a competent data professional needs to have foundational tools such as Python and SQL as they take up 59% and 56% of job postings. Whilst companies are willing to pay higher for more specialist cloud technologies such as AWS and Azure. Analyst tools such as Power BI and Tableu also feature indicating that a competent data analyst should look to having experience with these specific analyst tools.

## Addendum: What types of skills appear in Data Analyst, Data Engineer and Data Scientist jobs? And how does this correlate to their salary?
Link to file can be found [here](Addendum_Most_Valuable_Skill_Types.ipynb)  
I found that core programming skills are the absolute priority across all three roles, serving as the highest-demand skill type overall. Data analysts rely heavily on analyst tools which reflects their dominance in the analyst tools space. Whilst data engineers have the dominance in cloud type skills which reflects their role in building scalable cloud infrastructure.

![](assets\005.png)  

### How does this correlate to their salary?
I found that for data analysts, analyst tools and programming dominate the vast majority of job postings commanding a median salary of between $80,000 to $100,000 respectively. Moreover, databases along with cloud technologies command a higher pay and have few jobs indicating their niche yet valuable skill type.

For data engineers, cloud and programming skill types dominate with salaries of between $100,000 and $110,000. It also seems useful for a cloud engineer to know analyst tools as although they consist of only 15% of jobs, they command a high median salary of nearly $140,000.

For data scientists, programming skills dominate taking nearly half of job posting mentions at 49% and with a median salary of $91,000. It is also valuable for a data scientist to know cloud technologies as they consist of 19% of jobs and with a median salary of $113,000.

![](assets\006.png) 

# Challenges I faced
I faced a number of challenges which provided very ideal opportunities to learn and advance myself:

* Unclean data: Managing missing or incomplete data required  careful consideration from me in the approach to building the visualisation. A good example of this is where in the addendum I had to build a wrapper function to detect whether values were NA or a string and thereby return a dictionary object.

* Complex Visualisations: I had to use a matter of good judgement to decide on which visualisation fitted the purpose of the story. It can be easy to get carried away in building complex visualisations. However, in my opinion, simple visualisations are usually the best in conveying clear and compelling stories.

* Resilience: This project took over a month to complete and defining a well-thought layout and direction of the project was a really good learning experience in planning and executing this project.

# Conclusion
In conclusion, this analysis into the job market proved incredibly informative highlighting the skills needed for data professionals. It shows that having a grasp of the fundamental tools such as Python and SQL is beneficial across the board for a data professional aspiring to be either a data analyst, engineer or scientist. Whilst specialising in more specialised tools is beneficial to progress in any of those three fields.
This project also gave me a stellar opportunity to showcase my skills in cleaning, analysing and presenting complex datasets with Python and the adjoining tools such as Matplotlib, Pandas and Seaborn. It was also a pleasure in writing up the README file demonstrating my writing and explanatory skills.