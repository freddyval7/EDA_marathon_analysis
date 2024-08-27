# Overview

Welcome to the analysis of the Ultra-Marathon running, the project was maded with the desire of obtain answers to important and frecuent questions, searching, filtering and comparing the data.

The dataset sourced from [The big dataset of ultra-marathon running](https://www.kaggle.com/datasets/aiaiaidavid/the-big-dataset-of-ultra-marathon-running/discussion/420633), wich provide a huge collection of over 7M race records registered between 1798 and 2022.

# The Questions

Below are the questions that i wanted to answer in my project:

1. Which marathon has recorded the fastest and slowest runner?
2. What is the average age of the runners?
3. What is the average speed of the runners in 50km races?
4. What is the average speed of the runners according to distance and gender?
5. The runners are slowest in a particular season of the year?

# Tools I Used

- Python: The basis of the analysis, letting me to explore, filter and find every insight, combined with some libraries, like:
  - Pandas Library: To analyse the data.
  - Matplotlib: To show the data in plots.
  - Seaborn: To more personalization in plots.
- Jupyter Notebooks: To run the Python scripts.
- Visual Studio Code: The IDE for executing the scripts.
- Git and Github: To control and share the Python code.

# The Analysis

## 1. Which marathon has recorded the fastest and slowest runner?
To obtain the answer, i filtered the data according to the average speed, searching for the max and min value and then concatenate it.

```python
fastest_runner = df2[df2["avg_speed"] == df2["avg_speed"].max()]

slowest_runner = df2[df2["avg_speed"] == df2["avg_speed"].min()]

df_fastest_slowest = pd.concat([fastest_runner, slowest_runner])
```

## Results
![Visualization of the pivot table](/visualizations/fastest_slowest_runner.png)

## Insights
- The fastest participation was a male, with a huge different average speed over the female.
- The fastest runner is half the age of the slowest.
- The Pemberton Trail 50km had a significantly higher number of finishers (67) compared to the Lord Hill Trail Run (32).

## What is the average age of the runners?
This answer can be easily showed with a histogram.

```python
sns.histplot(df2["athlete_age"])

plt.ylabel("Number of Athletes")
plt.xlabel("Age")
plt.show()
```

## Results
![Visualization of the average age](/visualizations/avg_age.png)

## Insights
- The histogram shows a normal distribution of age, indicating that the majority of athletes are cloustered around a central age.
- The peak of the distribution appears between 30 and 40 years old.
- The age range includes between 20 and 70 years old, with a significant number of participants in 20s, 30s and 40s, so the athletes frequently are youth.

## What is the average speed of the runners in 50km races?
Like in the last point, we can use a histogram to show this.

```python
sns.displot(df2[df2["distance"] == "50km"]["avg_speed"], kde=True)

plt.ylabel("Number of Athletes")
plt.xlabel("Average Speed")
plt.show()
```

## Results
![Visualization of average speed](/visualizations/avg_speed.png)

## Insights
- The graph shows a peak between 7 to 8 km/h, suggesting that the majority of the athletes have this speed range.
- A significant number of participants have an average speed between 6 to 10 km/h.
- The distribution is slightly skewed to the right, meaning there is a longer tail on the right side. This indicates that there are a few athletes with significantly higher average speeds than the majority.

## 4. What is the average speed of the runners according to distance and gender?

Using a violinplot, and adding some parameters to the easily visualization, i could obtain the answer.

```python
sns.violinplot(data=df2, x="distance", hue="gender", y="avg_speed", split=True, inner="quartile", linewidth=1)

plt.xlabel("Distance")
plt.ylabel("Average Speed")
plt.show()
```

## Results
![Visualization of the average speed according to distance and gender](/visualizations/avg_speed_gender.png)

## Insights

- The plot clearly shows a difference in average speed between male and female participants for both distances (50km and 50mi). Further investigations could explore the factors contributing to this difference.
- The male participants generally have a higher average speed than the female, in both distances.
- The violin plot show a similar shape, with a central peak, this can indicate that the distribution of average speed in each group is relatively normal.

## 5. The runners are slowest in a particular season of the year?
To obtain the answer, the first i did was obtain the month of the race, with that, i could get the season of the year according with that month and then grouping the data by the season and calculating the mean.

```python
df2["date"] = df2["date"].str.replace(".", "/")

df2["month"] = df2["date"].str.split("/").str[1].astype(int)

df2["season"] = df2["month"].apply(lambda x: "Winter" if x > 11 else "Fall" if x > 8 else "Summer" if x > 5 else "Spring")

avg_speed_seasons = df2.groupby("season")["avg_speed"].agg(["mean", "count"]).sort_values("mean", ascending=False)

sns.barplot(data=avg_speed_seasons, x=avg_speed_seasons.index, y="mean", palette="tab10", hue="count", legend=False, width=0.5)
```

## Results
![Visualization of the speed season](/visualizations/speed_season.png)

## Insights

- Spring has the highest speed over the year, suggesting that is the best season to achieve a high performance.
- Summer has the lowest average speed, indicating it might pose challenges for achieving high speeds.
- Different seasons bring varying weather conditions that could impact performance. For instance, colder temperatures and snow in winter might affect speed, while hot and humid weather in summer could be a factor.

# What I learned
In this project, i was allowed to practice more of cleaning up data with python and pandas, i understood more about 
management of data. Indeed, i learned more about the stats, like the positive and negative skew. Likewise, i learned much about ultra-marathon races, before this, my knowledge was practically null.

# Challenges I Faced

- Inconsistent Data: Some data was null or unclean, so i had to face some issues cleaning it.

# Conclusions
My research in the records of the ultra-marathon runnings was productive, because i could get some interesting answers, and learning more about this world. This project can help anyone to understand some things about the functioning and nature of the marathons.