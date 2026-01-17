# Introduction
 🔍 Explore the data job market! This project focuses on data analyst roles, highlighting 💰 top-paying positions, 📈 in-demand skills, and where high demand meets high salaries in data analytics.
 
 🔍SQL queries? Check them out here: [sql_project](/sql_project/)

# Background
This project aims to provide a clear view of the data analyst job market by uncovering the top-paying roles and the most in-demand skills, helping professionals identify the best career opportunities.

Packed with valuable insights, this project highlights job titles, salary trends, locations, and the essential skills required to excel.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying
jobs?
3. What skills are most in demand for data
analysts?
4. Which skills are associated with higher
salaries?
5. What are the most optimal skills to learn?

# Tools I Used
For my deep dive into the data analyst job market,
I harnessed the power of several key tools:

- **SQL**: The backbone of my analysis, allowing me to
query the database and unearth critical insights.
- **PostgreSQL**: The chosen database management
system, ideal for handling the job posting data.
- **Visual Studio Code**: My go-to for database
management and executing SQL queries.
- **Git & GitHub**: Essential for version control and
sharing my SQL scripts and analysis, ensuring
collaboration and project tracking.

# The Analysis
Each query for this project aimed at investigating
specific aspects of the data analyst job market.
Here's how I approached each question:

### 1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered
data analyst positions by average yearly salary
and location, focusing on remote jobs. This query
highlights the high paying opportunities in the
field.

```sql
SELECT
    job_id,
    job_title_short,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM    
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id    
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL                    
 ORDER BY 
    salary_year_avg DESC
LIMIT
     10;
```     
Here's the breakdown of the top data analyst jobs :
- **Significant Pay Variability:** The top-earning data analyst positions show a wide compensation spread, with salaries ranging roughly between $184,000 and $650,000, underscoring the high financial upside of the role. 

- **Employer Representation Across Sectors:** High salary offerings are not limited to a single industry, with organizations such as SmartAsset, Meta, and AT&T reflecting widespread adoption of data analytics across domains.

- **Range of Analytical Roles:** The variation in job titles - from entry and mid-level analyst roles to senior leadership positions like Director of Analytics- highlights the diverse responsibilities and career paths within the analytics ecosystem.

### 2. Skills for Top Paying Jobs
To understand what skills are required for the
top-paying jobs, I joined the job postings with
the skills data, providing insights into what
employers value for high-compensation roles.

```sql
WITH top_paying_jobs AS(
SELECT
    job_id,
    job_title_short,
    salary_year_avg,
    name AS company_name
FROM    
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id    
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL                    
 ORDER BY 
    salary_year_avg DESC
LIMIT
     10    
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id =skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

ORDER BY 
    salary_year_avg DESC
```
Here's the breakdown of the most demanded skills
for the top 10 highest paying data analyst jobs:
- **SQL** emerges as the most prominent skill, appearing in **8** instances across the analyzed roles.

- **Python** ranks just behind, with a presence in **7** roles, reinforcing its importance in analytics work.

- **Tableau** maintains strong relevance, being cited **6** times among high-demand positions.

- Additional tools such as **R**, **Snowflake**, **Pandas**, and **Excel** reflect varied but meaningful levels of market demand.

### 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id =skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
    job_title_short = 'Data Analyst' AND
    job_work_from_home = True
GROUP BY
    skills
ORDER BY
    demand_count DESC    
LIMIT 5;
```
Here's the breakdown of the most demanded skills for data analysts

- **SQL** and **Excel** continue to be fundamental requirements, reinforcing the importance of strong skills in data processing and spreadsheet-based analysis.

- Tools such as Python, Tableau, and Power BI are increasingly essential, reflecting the growing reliance on technical and visualization capabilities for data-driven insights and decision support.

| Skills    | Demand Count |
|-----------|--------------|
| SQL       | 7291         |
| Excel     | 4611         |
| Python    | 4330         |
| Tableau   | 3745         |
| Power BI  | 2609         |

*Table of the demand for the top 5 skills in data analyst job postings*

### 4. Skills Based on Salary

Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg),0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id =skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL AND
    job_location = 'Anywhere'
GROUP BY
    skills
ORDER BY
    avg_salary DESC    
LIMIT 25;
```
Here's a breakdown of the results for top paying skills for Data Analysts:

- **Advanced Data Processing and Predictive Skill Demand:** Strong demand exists for professionals with expertise in big data and machine learning technologies such as PySpark, Couchbase, DataRobot, Jupyter, and Python libraries like Pandas and NumPy, highlighting the premium placed on advanced data processing and predictive modeling capabilities.

- **Intersection of Analytics and Engineering Capabilities:** Knowledge of development and deployment tools including GitLab, Kubernetes, and Airflow reflects a valuable overlap between data analytics and engineering, particularly for roles emphasizing automation and efficient data pipeline management.

- **Growing Relevance of Cloud-Based Analytics Platforms:** Experience with cloud and data engineering platforms such as Elasticsearch, Databricks, and GCP underscores the growing reliance on cloud-driven analytics environments, indicating that cloud proficiency significantly enhances earning potential in data analytics roles.

| Skills         |     Average Salary ($)   |
|----------------|------------------|
| pyspark        | 2,08,172         |
| bitbucket      | 1,89,155         |
| couchbase      | 1,60,515         |
| watson         | 1,60,515         |
| datarobot      | 1,55,486         |
| gitlab         | 1,54,500         |
| swift          | 1,53,750         |
| jupyter        | 1,52,177         |
| pandas         | 1,51,821         |
| elasticsearch  | 1,45,000         |

*Table of the average salary for the top 10 paying skills for data analysts*

### 5. Most Optimal Skills to Learn

Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries,
offering a strategic focus for skill development.

```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim. job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25; 
```
| Skill ID | Skills       | Demand Count | Average Salary ($)    |
|----------|--------------|--------------|-----------------------|
| 8        | go           | 27           |  1,15,320             |
| 234      | confluence   | 11           |  1,14,210             |
| 97       | hadoop       | 22           |  1,13,193             |
| 80       | snowflake    | 37           |  1,12,948             |
| 74       | azure        | 34           |  1,11,225             |
| 77       | bigquery     | 13           |  1,09,654             |
| 76       | aws          | 32           |  1,08,317             |
| 4        | java         | 17           |  1,06,906             |
| 194      | ssis         | 12           |  1,06,683             |
| 233      | jira         | 20           |  1,04,918             |

*Table of the most optimal skills for data analyst sorted by salary*

Here's a breakdown of the most optimal skills for Data Analysts in 2023:

- **Prominence of Core Programming Languages:-** Python and R continue to show strong demand, with occurrence counts of 236 and 148 respectively. Their average compensation levels—approximately $101,397 for Python and $100,499 for R—suggest that while these skills are highly valued, they are also widely represented across the talent pool.

- **Rising Importance of Cloud and Big Data Platforms:** Specialized tools such as Snowflake, Azure, AWS, and BigQuery exhibit notable demand alongside comparatively higher average salaries, emphasizing the increasing reliance on cloud-based and large-scale data technologies in analytics roles.

- **Critical Role of Business Intelligence and Visualization:** Visualization and BI tools like Tableau and Looker, with demand counts of 230 and 49 and average salaries of around $99,288 and $103,795 respectively, underscore the importance of translating data into actionable business insights.

- **Sustained Demand for Database Expertise:** Skills in both traditional and NoSQL database technologies, including Oracle, SQL Server, and NoSQL systems, remain in demand with average salaries ranging from $97,786 to $104,534, reflecting the ongoing need for robust data storage and management capabilities.

# What I Learned   

Throughout this adventure, I've turbocharged my
SQL toolkit with some serious firepower:

- **Advanced Query Development:** Developed strong proficiency in writing complex SQL queries, including efficient table joins and the use of WITH clauses to manage temporary result sets.

- **Effective Data Summarization Techniques:** Gained hands-on experience with GROUP BY operations and aggregate functions such as COUNT() and AVG() to derive meaningful summaries from large datasets.

- **Applied Analytical Problem Solving:** Enhanced real-world analytical capabilities by translating business questions into structured, insight-driven SQL queries.

# Conclusions

### Insights
From the analysis, several general insights
emerged:

1. **High-Compensation Remote Opportunities:** The top-paying data analyst roles that support remote work display a broad salary spectrum, with compensation reaching as high as $650,000.

2. **Skill Requirements for Premium Roles:** Advanced expertise in SQL is a common requirement among the highest-paying data analyst positions, highlighting its importance for top-tier earnings.

3. **Market Demand for Core Skills:** SQL also emerges as the most frequently sought-after skill in the data analytics job market, reinforcing its necessity for aspiring analysts.

4. **Impact of Specialized Skill Sets:** Niche technologies such as SVN and Solidity are linked to the highest average salaries, suggesting a strong market premium for specialized expertise.

5. **Maximizing Job Market Competitiveness:** With both high demand and strong average compensation, SQL stands out as one of the most strategic skills for data analysts to develop to enhance their overall market value.

### Closing Thoughts
Working on this project helped me significantly improve my SQL skills while also giving me practical insight into the data analyst job market. The analysis made it easier to understand which skills matter most when planning learning paths and job searches. By focusing on skills that are both in high demand and well-paid, aspiring data analysts can better navigate a competitive market. Overall, this project reinforced the importance of continuous learning and staying adaptable as the field of data analytics continues to evolve.