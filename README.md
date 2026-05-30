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

### Stage 1 Data cleaning
The most vital step in data analysis work. If done inadequately, it can lead to skewed data resulting in misleading visualisations. In essence, business decisions need to be made on visualisation stories that are factually true and accurate. So, hereunder are a few examples where I performed data cleaning throughout my project:
- turning string values into python objects such as values in job_posted_date column
- in core-analysis-1, turning string values in list objects so they can be exploded such as values in the job_skills column
- in the addendum, turning dictionaries into lists of tuples so they can be exploded and 'unpivoted'
- in core-analysis-2, extracting month number from a converted Python date object to chronologically order my data

### Stage 2 Group by aggregations
Group by aggregations are a useful way of quickly aggregating relevant data to uncover insights. At times, group by aggregations can be plotted straight away for simple analyses. However, if a more stringent approach is required to analyse the data, then pivot tables offer a precise solution for that.
Below are a few examples where I performed group by aggregations throughout my project:
- in introductory-analysis, grouping the companies by the number of jobs they have posted
- in core-analysis-1, grouping skills and job titles by the number of skills mentioned within the job titles
- in core-analysis-3, grouping skills by their count and adding a calculation for their median salaries

### Stage 3 Pivot tables
Pivot tables offer a very granular way to analyse data. Columns can be rearranged to become the index or remain as columns. Calculations can also be applied to the cells of a pivot table along with the option to merge pivot tables in the way tables are merged in SQL. With such a massive level of customisation, pivot tables are a common tool for an effective data professional, just as a kitchen knife is to a chef.
So, hereunder are a few examples where I utilised pivot tables throughout my project:
- In core-analysis-2, pivoting the data to uncover the number of skill mentions throughout the year
- In the addendum, to gain insight into the number of skill mentions in jobs for a target list of data profession job titles
- In core-analysis-2, uncovering the percentage of skills mentioned throughout the year in job postings.

### Stage 4 Create a subset of data
This involves reducing the dataset in the pivot table to only what values I wish to plot and assigning it to a variable. For example, out of a list of 100 skills, it is usually preferred to plot the top 10 skills. These top 10 skills can be extracted using the .head() method and assigned to a variable called top_10_skills. 
By using this approach, this reduces clutter on the final plot and allows ease of reading and accessibility. Creating a subset of data is an essential solution as it also removes the need to directly change the code blocks that plot the analysis. One can simply pass in the subset of data into the plot as opposed to the entire pivot table.
I used the subsets of data approach throughout my project.

### Stage 5 Plot the data
I primarily used the industry standard Seaborn library to plot my analyses. This allows more visually pleasing plots akin to styled graphs in Microsoft Excel. However, there have been challenges in the case of adding data labels to scatter plots in particular. In a wysiwyg program such as Microsoft Excel, adding data labels is a matter of a few clicks. Whereas, using the Seaborn library (or Matplotlib for that matter), the operation can be more than a few lines of code. A good example of this is the subplots plotted in the final analysis of the addendum.
I plotted data throughout my project using the subsets of data approach outlined above.

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
