# Introduction
📊 The data job market... The project focuses on the data job market, particularly the 📈 Machine Learning Roles. 💰 Top-paying jobs and 🔥 in-demand skills are explored. The Repo also answers ❓ the question: "What are the optimal skills for my role that I need to learn?".

🔎 The SQL queries? Look into [here](/sql_project/).

# The Main Questions I Want To Answer
1. What are the top-paying Machine Learning roles?
2. What skills are required for the top-paying Machine Learning roles?
3. What are the most in-demand skills for my role?
4. What are the top skills based on the salary for my role?
5. What are the most optimal skills to learn? 

# Tools I Used
The main tools that helped me make the analysis are:

- **SQL**: The backbone of the analysis, which made it possible to write the queries and get critical insight from the data.
- **PostgreSQL**: The Database System used to manage the job -related data in a reliable and robust fashion.
- **Visual Studio Code**: The go-to IDE for database management and querying the data.
- **Git & GitHub**: Used for version control and sharing the SQL scripts and analysis. 

# The analysis 
Each query was designed to answer a specific [question](Question Answered) about the Machine Learning (ML) job market. Below, a few examples are given on the approach:

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

![Top 10 Most Paid Remote Machine Learning Jobs](img\1_top_paying_remote_ml_roles.png)


By commenting the `job_location = 'Anywhere' AND` in the `WHERE` statement, one could see the top average salaries for all locations. Here is the plot:

![Top 10 Most Paid Machine Learning Jobs ](img\1_top_paying_ml_roles.png)
*The same analysis could be done for other data positions like Data Scientist, Data Engineer, etc.*

The three most important implications of the analysis are:

1. The salaries for Machine Learning Engineers are exceptionally high — $325K. The two leading ML roles in Harnham (Senior ML Engineer) and in Storm5 (Principal ML Engineer) are at the top of the market. 

2. The first two highest-paid positions are remote. 5 of the top 10 roles are fully remote. The on-site/hybrid roles are also top-paid. It is fascinating that the well-known companies like Nvidia prioritise the on-site roles.

3. It is obvious from the figures that the top-paid positions are for Senior developers. It could be аssumed with high certainty that more experienced and skilful developers receive higher salaries 

In the end, it should be mentioned that many companies do not give information about the salary in the job postings.

### 2. Top Paying Machine Learning Skills
There are skills that are mandatory for an ML role. There are other skills which could increase the salary of an ML Engineer. The next two graphs show this relation:

![Average Salary by Skills](img\2_avg_salary_per_skill.png)     
*The skills are grouped by specific category*

![Top 15 Skills Required For High Salary](img\2_avg_salary_per_skill.png)     

Three key insights from the figures are:

1. Python is the undisputed champion of top-paying ML roles. It is a must to craft Python skills in order to be successful for ML positions. The language appears in 13 out of the 15 top job postings. Python is the single highest-frequency requirement across the entire database.

2. The two dominant ML frameworks are PyTorch and TensorFlow. They appear almost equally in the job postings. Both of them are essential for pursuing an ML career.

3. Cloud skills (AWS, GCP, Azure) and Infrastructure/DevOps tools are features that every ML engineer should have in their bag. They could increase the salary of an engineer significantly.

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

The most demanded skills in 2023 are given in the figure below. The top language for an ML position is Python. The ML Engineer should also be good in specific libraries (PyTorch and TensorFlow), in at least one cloud technology (AWS/Azure) and have SQL skills. 

![Most Demanding Skills for ML Engineer](img\3_top_demanded_skills.png)     

A few key findings are:

1. Python is the dominant programming language for ML. It appears in 9685 postings, two times more than the next skill. PyTorch comes in second place in 4389 postings. TensorFlow is very close - it appears in 4307 postings.

2. Two out of ten spots - the cloud skills in the analysis. AWS is the leader - 3780 postings. Azure is in second place - appears in 2732 postings. 

3. SQL is demanded in many job postings. It is rank at 5th place from 3497 postings ahead of Azure, Spark and Docker. Mastering SQL could make the job candidate stand out in the interview. Hence, doing an SQL project for the job market wasn't a waste of time, was it?

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
Companies are paying dozens of bills for niche skills. The 2023 research shows that skills like haskel, chef, kotlin are the top candidates for high salary. However, for an ML Engineer it is better to focus on the basics in the beginning of the career, rather than in such niche technologies. First, learn the fundamentals, then tackle more "exquisite" tools.

One last note ... For the last year **[footnote](footnote)**, such niche high-paid skills are Hugging Face, LangChain and the LLM top models APIs (OpenAI, Claude, Gemini, etc).

### 5. Optimal Skills - Demand vs Salary

![Optimal Skills for ML Engineer - Demand vs Salary](img\5_optimal_skills.png)     

The last task is to compare the skills based ot demand versus salary. The figure once again shows Python as an optimal skill to learn. It is the highest-demand tool in the profession. Scala, on the other hand, is the highest paid and also very niche. There are also hidden gems - Spark, Airflow, Hadoop and Kubernetes. It is worth mentioning the R programming language - less paid and not highly demanded.

# What I Learned 
- **Advanced SQL Query Crafting** - Writing queries that combine `JOINS`, `CASE` expressions, `UNION` operators, Date function manipulation and more...

- **Sub-Queries and CTE** - Query the Database via `Sub-Queries` and `CTE` to combine the data for getting ultra-important insights.

- **Insights of the Machine Learning Job Market** - Find the top demand and high-paying skills, as well as optimal pathways to accelerate a career as a Machine Learning Engineer.

# Insights





