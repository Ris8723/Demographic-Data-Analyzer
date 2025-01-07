# Demographic-Data-Analyzer
in this data we are anslysed  the data
How many people of each race are represented in this dataset? This should be a Pandas series with race names as the index labels. (race column)
What is the average age of men?
What is the percentage of people who have a Bachelor's degree?
What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?
What percentage of people without advanced education make more than 50K?
What is the minimum number of hours a person works per week?
What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
What country has the highest percentage of people that earn >50K and what is that percentage?
Identify the most popular occupation for those who earn >50K in India.?

ANS:-
import pandas as pd


def demographic_data_analysis():
    # Read data (replace "adult.data.csv" with the path to your dataset)
    df = pd.read_csv("adult.data.csv", header=None, names=[
        'age', 'workclass', 'fnlwgt', 'education', 'education-num', 'marital-status',
        'occupation', 'relationship', 'race', 'sex', 'capital-gain', 'capital-loss',
        'hours-per-week', 'native-country', 'salary'
    ])
    
    # 1. How many people of each race are represented in this dataset?
    race_count = df['race'].value_counts()

    # 2. What is the average age of men?
    average_age_men = round(df[df['sex'] == 'Male']['age'].mean(), 1)

    # 3. What is the percentage of people who have a Bachelor's degree?
    total_people = len(df)
    bachelor_count = len(df[df['education'] == 'Bachelors'])
    percentage_bachelors = round((bachelor_count / total_people) * 100, 1)

    # 4. What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?
    advanced_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
    advanced_education_rich = len(df[advanced_education & (df['salary'] == '>50K')])
    percentage_advanced_education_rich = round((advanced_education_rich / len(df[advanced_education])) * 100, 1)

    # 5. What percentage of people without advanced education make more than 50K?
    non_advanced_education = ~advanced_education
    non_advanced_education_rich = len(df[non_advanced_education & (df['salary'] == '>50K')])
    percentage_non_advanced_education_rich = round((non_advanced_education_rich / len(df[non_advanced_education])) * 100, 1)

    # 6. What is the minimum number of hours a person works per week?
    min_work_hours = df['hours-per-week'].min()

    # 7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    num_min_workers = df[df['hours-per-week'] == min_work_hours]
    rich_min_workers = len(num_min_workers[num_min_workers['salary'] == '>50K'])
    percentage_rich_min_workers = round((rich_min_workers / len(num_min_workers)) * 100, 1)

    # 8. What country has the highest percentage of people that earn >50K?
    countries = df[df['salary'] == '>50K']['native-country'].value_counts()
    total_countries = df['native-country'].value_counts()
    highest_earning_country_percentage = round((countries / total_countries * 100).max(), 1)
    highest_earning_country = (countries / total_countries * 100).idxmax()

    # 9. Identify the most popular occupation for those who earn >50K in India.
    india_occupations = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]['occupation']
    top_in_occupation = india_occupations.value_counts().idxmax()

    # Return all results
    return {
        "race_count": race_count,
        "average_age_men": average_age_men,
        "percentage_bachelors": percentage_bachelors,
        "percentage_advanced_education_rich": percentage_advanced_education_rich,
        "percentage_non_advanced_education_rich": percentage_non_advanced_education_rich,
        "min_work_hours": min_work_hours,
        "percentage_rich_min_workers": percentage_rich_min_workers,
        "highest_earning_country": highest_earning_country,
        "highest_earning_country_percentage": highest_earning_country_percentage,
        "top_in_occupation": top_in_occupation
    }

