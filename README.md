# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, i filtered out those positions by which ones were the most popular, and got the top 5 skills for the top 3 roles.
This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_skills_count.ipynb](3_project\2_skills_count.ipynb)

### Visualize Data

````python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style="ticks")

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc["job_title_short"] == job_title].head(5)
    # df_plot.plot(kind="barh", x="job_skills", y="skill_percent", ax=ax[i], title=job_title)
    sns.barplot(data=df_plot, x="skill_percent", y="job_skills", ax=ax[i], hue="skill_count", palette="dark:b_r")
    ax[i].set_title(job_title)
    ax[i].set_ylabel("")
    ax[i].set_xlabel("")
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0, 78)
    ax[i].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"{int(x)}%"))
    
    for n, v in enumerate(df_plot["skill_percent"]): # n is index, v is value
        ax[i].text(v + 1, n, f"{v:.0f}%", va="center") # wir wollen das v als Floatingpoint, und 0 Nachkommastellen, das rundet die Zahl automatisch um

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle("Likelihood of Skills Requested in US Job Postings", fontsize=15)

plt.tight_layout(h_pad=0.5)
plt.show()
````

### Results

![Visualization of Top Skills for Data Nerds](3_project\images\skill_demand_data_roles.png)

### Insights

- SQL and Python are the dominant baseline technologies required across all three data

- Data Analyst: Focuses primarily on data querying (SQL: 51%) alongside spreadsheet and visualization tools (Excel: 41%, Tableau: 28%).

- Data Engineer: Heavily relies on backend data processing (SQL: 68%, Python: 65%) paired with cloud and big data infrastructure (AWS: 43%, Azure: 32%, Spark: 32%).

- Data Scientist: Python is the single most in-demand skill (72%), followed by SQL (51%) and statistical programming (R: 44%).

- Specialization Patterns: Business intelligence tools (Excel/Tableau) lead for Analysts, cloud technologies (AWS/Azure) for Engineers, and programming languages (Python/R) for Data Scientists.  

## 2. How are in-demand skills trending for Data Analyst

````python
df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data = df_plot, dashes="", palette="tab10")

plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{int(y)}%"))

plt.show()
````

### Results

![Trending Top Skills for Data Analysts in the US](3_project\images\skill_per_month_percentage.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights
- SQL remains the most consistently demanded skill thoughout the year, altough it shows a gradual decrease in demand
- excel experienced a significant increase in demand starting around september, surpassing both python and Tableau by the end of the year

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Jobs

#### Visualize Data

````python
sns.boxplot(data=df_US_top6, x="salary_year_avg", y="job_title_short", order=job_ordered)

ticks_x = plt.FuncFormatter(lambda y, pos: f"${int(y/1000)}K")
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
````

#### Results

![Salary Distribution of Data Jobs in the US](3_project\images\median_salary_per_role.png)

#### Insights

- There is a significant variation in salarys ranges across different job titles. Senior Data Scientists roles show a considerable number of outliners on the high end of salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles
- The median salaries increase with the seniority and specialization of the roles.

#### Visualize Data:

````python
# Top 10 Hoghest Paid SKills for Data Analysts
sns.barplot(data=df_DA_top_pay, x="median", y=df_DA_top_pay.index, ax=ax[0], hue="median", palette="dark:b_r")

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x="median", y=df_DA_skills.index, ax=ax[1], hue="median", palette="light:b")

plt.show()
````
#### Results:

![The Highest Paif & Most In-Demand Skills for Data Analysts in the US](3_project\images\highest_paid_vs_most_demand.png)

#### Insights:

- High Pay for Niche & Specialized Skills: The highest-paying skills (led by dplyr, bitbucket, gitlab, and solidity) command median salaries between $150K and $195K, reflecting high compensation for specialized engineering, DevOps, blockchain, and advanced analytics tools.

- Core Analytics Skills Drive High Demand: The most demanded skills focus heavily on foundational data tools like python, tableau, r, sql, and power bi.

- Salary Trade-Off (Demand vs. Pay): Most in-demand skills yield lower median salaries (~$80K to $98K) compared to niche tools, as widespread market supply stabilizes compensation for core analyst tools.

## 4. What is the most optimal skill to learn for Data Analysts?

### Visualize Data

````python
df_DA_skills_high_demand.plot(kind="scatter", x="skill_percent", y="median_salary", figsize=(8, 6))
````

![Most Optimal Skills for Data AAnalysts in the US](3_project\images\most_optimal_skills.png)
*A scatter plot visualizing the most optimal skills for data analysts in the US.*

#### Insights:

- High Demand vs. High Salary Sweet Spot: Python and Tableau occupy the optimal top-right quadrant, combining high demand (featured in over 30% of job postings) with above-average median salaries ($93K–$97.5K).

- Essential Baseline Skill: SQL is by far the most in-demand skill (found in nearly 60% of job ads), while offering a solid median salary around $91K.

- High-Paying Niche Skills: Specialized technologies like Oracle, SQL Server, and Go command high salaries ($90K–$97K), but appear in fewer than 10% of job listings.

- Lower Premium for Standard Office Tools: Foundational tools like Excel maintain high market presence (~41%) but yield lower median compensation ($84.4K). Word and PowerPoint trail at the bottom in both demand and salary.

- Balanced Mid-Range Options: Power BI, SAS, and R cluster closely around 20% job demand and $90K–$92.5K median salaries, serving as balanced, high-value core skills.
