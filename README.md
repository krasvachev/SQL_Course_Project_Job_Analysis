# Introduction
The data job market... The project focuses on the data job market, and especially the Machine Learning Roles. Top-paying jobs and skills, in-demand skills are explored. The Repo also answers the question: What are the optimal skills that I need to learn for my role.

The SQL queries? Look into [here](/project_sql/).

# The Main Questions I Want To Answer
1. What are the top paying Machine Learning roles?
2. What skills are required for the the top paying Machine Learning roles?
3. What are the most in-demand skills for my role?
4. What are the top skills based on the salary for my role?
5. What are the most optimal skills to learn? 

# Tools I Used
The main tools that helped me make the analysis are:

- **SQL**: The backbone of the analysis, that made possible writing the queries and getting critical insight from the data.
- **PostgreSQL**: The Database System used to manage the job related data in realiable and robust fashion.
- **Visual Studio Code**: The go-to IDE for the database managing and quering the data.
- **Git & GitHub**: Used for version control and sharing the SQL scripts and analysis 

# The analysis 
Each query was designed to answer a specific [question](Question Aswerd) about Machine Learning job market. Below, a few examples are given on the approach:

### 1. Top Paying Machine Learning Jobs
The top Machine Learning positions were ranked based on the average yearly salary and the location. The emphasis was put on the remote jobs. The query shows a way to answer the question:

```sql 
SELECT 
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Machine Learning Engineer' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY 
    salary_year_avg DESC
LIMIT 10
```

Here is the analysis of the market:
![Caption](url_to_image)
*comment about the graph*

**Do it for the other 4 questions**

# What I Learned 
- **Advanced SQL Query Crafting** - 

- **Sub-Queries and CTE**

- **Insights of the Machine Learning Job Market** - 

# Conclusions

### Insights

### 



