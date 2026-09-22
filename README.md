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

## 3.