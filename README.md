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


## Approach
Like with all data analysis, projects or pieces of work, it requires a multi-stage approach to get to the desired result. The desired result is usually a clean visualisation that tells a story. And each stage usually has multiple steps.
### Stage 1 Data cleaning
A most vital step in data analysis work. If done inadequately, it can lead to skewed data resulting in misleading visualisations. In essence, business decisions need to be made on visualisation stories that are factually true and accurate. So, hereunder are a few examples where I performed data cleaning throughout my project:
- turning string values into python objects such as values in job_posted_date column
- turning string values in list objects so they can be exploded such as values in the job_skills column
- turning dictionaries into lists of tuples so they can be exploded and 'unpivoted'
- extracting month number from a converted Python date object to chronologically order my data

### Stage 2 Group by aggregations
Group by aggregations are a useful way of quickly aggregating relevant data to uncover insights. At times, group by aggregations can be plotted straight away for simple analyses. However, if a more stringent approach is required to analyse the data, that's where pivot tables come in.
Below are a few examples where I performed group by aggregations throughout my project:
- 

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
