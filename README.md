# Data Analysis Project for Data Jobs in 2023



---------------------------------
# Introduction
Welcome to my Python project showing a data analysis of the UK data job market in 2023. This project is comprised of an introductory analysis, four core analyses and an addendum.
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

## Tools I used:
* Python - 
    * Pandas
    * Matplotlib
    * Seaborn
* Jupyter Notebooks - 
* VS Code - 
* Git/Github - 


## Approach
data cleaning
- turning string values into python objects such as values in job_posted_date column
- turning string values in list objects so they can be exploded such as values in the job_skills column
- turning dictionaries into lists of tuples so they can be exploded
- extract month number from


group by aggregation
Aggregate count of jobs over the year
can be plotted straight away for fundamental analyses, but at times, a more stringent approach is required in the form of pivot tables. Pivot tables provide a more precise approach to aggregating data.

pivot tables

create a subset of data
This is an essential maintainable code solution as it removes the need to directly change the code blocks that plot the analysis.

 plot
 I primarily used the industry standard Seaborn library to plot my analyses. This allows more visually pleasing plots akin to styled graphs in Microsoft Excel. However, there have been challenges in the case of adding data labels to scatter plots in particular. In a wysiwyg program such as Microsoft Excel, adding data labels is a matter of a a few clicks. Whereas, using the Seaborn library or Matplotlib for that matter, the operation can be more than a few lines of code. A good example of this is the subplots plotted in the final analysis of the addendum.



# Intro analysis
- Write findings, comment about approach.

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
