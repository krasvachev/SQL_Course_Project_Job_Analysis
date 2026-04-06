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
Each query was designed to answer a specific [question](Question Aswerd) about Machine Learning (ML) job market. Below, a few examples are given on the approach:

### 1. Top Paying Machine Learning Jobs
The top ML positions were ranked based on the average yearly salary and the location. The emphasis was put on the remote jobs. The query shows a way to answer the question:

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

The analysis of the top 10 most paying remote ML position is given below:

![Top 10 Most Payed Remote Machine Learning Jobs ](img\1_top_paying_remote_ml_roles.png)
*comment about the graph*

By commenting the `job_location = 'Anywhere' AND` in the `WHERE` statement one could see top average salaries for all locations. Here is the plot:

![Top 10 Most Payed Machine Learning Jobs ](img\1_top_paying_ml_roles.png)
*comment about the graph*

The three most important implications of the analysis are:

1. The salaries for Machine Learning Engineers are exceptionally high — $325K. The two leading ML roles Harnham (Senior ML Engineer) and Storm5 (Principal ML Engineer) achieve the top of the market. Should be mentioned that many companies do not give information about the salary in the job postings.

2. The first two highest payed positions are remote. 5 of the top 10 roles are fully remote. The on-site/hybrid roles are also top payed. It is facinating that the well-known companies in the ranking prioritise the on-site roles.

3. It is obvious from the figures that the top payed positions are for Senior developers. It could be аssumed with high certanty that more experienced and skillful developers, receive higher salaries 

### 2. Top Paying Machine Learning Skills
There are skills that are mandatory for an ML role. There are other skill which could increase a lot the salary of an ML Engineer. The next two graphs shows this relation:

![Average Salary by Skills](img\2_avg_salary_per_skill.png)     
*comment about the graph*

![Top 15 Skills Required For High Salary](img\2_avg_salary_per_skill.png)     
*comment about the graph*

Three key insights from the figures are:

1. Python is the undisputed chapmpion of top-paying ML roles. It is a must to craft the Python skills, to be successful for ML positions. The language appears in 13 out of the top job postings. Python is the single highest-frequency requirement across the entire database.

2. The two dominant ML frameworks are PyTorch and TensorFlow are the but PyTorch. They appear almost equally in the job postings. Both of them are essential for pursuating a ML career.

3. Cloud skills (AWS, GCP, Azure) and Infrastructure/DevOps tools are features that every ML engineer should have in his bag. They could increase the salary of an engineer significantly.

The query which was implemented to generate the results is:
```sql
WITH top_paying_jobs AS (
    SELECT 
        job_id,
        job_title,
        salary_year_avg,
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
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

### 3. Top Demanded Skills

The most demanded skills in 2023 are given in the figure bellow. The top language for a ML position is Python. The ML Engineer should also be good in specific libraries (PyTorch and TensorFlow), in at least one cloud technology (AWS/Azure) and have SQL skills. 

![Most Demanding Skills for ML Engineer](img\3_top_demanded_skills.png)     
*comment about the graph*

A few key findings are:

1. Python is the dominant programing language for ML. It appears in 9685 postings, two times more than the next skill. PyTorch comes in second place in 4389 postings. TensorFlow is very close - it appears in 4307 postings.

2. Two out of ten spots - the cloud skills in the analysis. AWS is the leader - 3780 postings. Azure is on second place - appears in 2732 posting. 

3. SQL is demanded in many job postings. It is rank at 5 place 3497 postings ahead of Azure, Spark and Docker. Mastering SQL could make the job candidate to stand out in the interview. Hence, doing an SQL project wasn't a waste of time, was it?

This is the query that generates these results:

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
    job_title_short = 'Machine Learning Engineer'
GROUP BY 
    skills
ORDER BY
    demand_count DESC
LIMIT 10
```

### 4. Top Paying Niche Skills
Companies are paying dozens of bills for a niche skills. The 2023 research shows that skills like haskel, chef, kotlin are the top candidates for high salary. However, for ML Engineer is better to focus on the basics in the beginning of the career, rather than in such niche skills. First, learn the fundamentials, then tackle more "exquisite" tools.

One last note... For the last year, such niche high payed skills are Hugging Face, LangChain and the LLM top models APIs (OpenAI, Claude, Gemini, etc).

### 5. Optimal Skills - Demand vs Salary

![Optimal Skills for ML Engineer - Demand vs Salary](img\5_optimal_skills.png)     
*comment about the graph*

The last task is to comparare the skills based ot demand versus salary. The figure once again shows Python as an optimal skill to learn. It is the highest demand tool in the profession. Scala, on the other hand, is the highest paid, also very niche. There are also a hidden gems - Spark, Airflow, Hadoop and Kubernetes. It worths mentioning the R programing language - less paid and not highly demand.

# What I Learned 
- **Advanced SQL Query Crafting** - Writing a queries that combine `JOINS`, `CASE` expressions, `UNION` operators, Date function manipulation and more...

- **Sub-Queries and CTE** - Query the Database via `Sub-Queries` and `CTE` to combine the data end get ultra-important insights.

- **Insights of the Machine Learning Job Market** - Find the top demand and high paying skills, also optimal pathways to accelarate a career as a Machine Learning Engineer.

### Insights





