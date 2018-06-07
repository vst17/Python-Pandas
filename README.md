
# PythonCity School Analysis

1.	Schools that spend between 585-615 dollars per student have a better percent passing reading, percent passing math, and percent overall passing rate than schools that spend more than 615 dollars per student. Also, those schools that spend less per student are smaller schools with 2,000 students or less.
2.	It is also found that the schools with 2000 students or less have a higher percent passing reading, percent passing math, and percent overall passing rate than larger schools that have 2000 – 5000 students. Furthermore, the smaller the school, the less they spend per student.
3.	With that being said,Charter schools have a higher percent passing reading, percent passing math, and percent overall passing rate than District schools. Thus being the top five performing schools based on percent passing rates, whereas the bottom five performing schools based on percent passing rate are District schools.
4. In all 15 schools, the Average Reading Score is higher than the Average Math Score.
5. Lasly, for the top 5 performing schools, the percent passing math is about 20 percent lower than the percent passing reading. 


```python
# Dependencies
import pandas as pd
import numpy as np
```


```python
# The path to our csv file
schools_complete = "Resources/schools_complete.csv"
students_complete = "Resources/students_complete.csv"
```


```python
# create dataframe using mapping
schools_complete_df = pd.read_csv(schools_complete)
students_complete_df = pd.read_csv(students_complete)
schools_complete_df.head()
students_complete_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



# District Summary


```python
# finding total schools
total_schools = len(schools_complete_df['School ID'])

# The total number of students
total_students = schools_complete_df['size'].sum()

# Get the total budget
total_budget = schools_complete_df['budget'].sum()

# Get the Average Math Score
average_math_score = round(students_complete_df['math_score'].mean(),6)

# Get the Average Reading Score
average_reading_score = round(students_complete_df['reading_score'].mean(),5)

# Get the percentage of students passing math
students_passing_math = students_complete_df.loc[students_complete_df["math_score"] > 70,:]
percent_passing_math = round(float(students_passing_math['math_score'].count()/total_students)*100,6)

# Get the percentage of students passing reading
students_passing_reading = students_complete_df.loc[students_complete_df["reading_score"] >70,:]
percent_passing_reading = round(float(students_passing_reading["reading_score"].count()/total_students)*100,6)

# Get the overall passing rate for math and reading
overall_passing_rate =round((percent_passing_math + percent_passing_reading)/2,6)

# Create a disctric Summary Dataframe 
summary_df = pd.DataFrame({"Total Schools":[total_schools],
                          "Total Students":[total_students],
                          "Total Budget":[total_budget],
                          "Average Math Score":[average_math_score],
                          "Average Reading Score":[average_reading_score],
                          "Percent Passing Math":[percent_passing_math],
                          "Percent Passing Reading":[percent_passing_reading],
                          "Percent Overall Passing Rate":[overall_passing_rate]})
# summary_df
# to put it in the right format
district_summary_df = pd.DataFrame(summary_df, columns=["Total Schools",
                                                        "Total Students",
                                                        "Total Budget",
                                                        "Average Math Score",
                                                        "Average Reading Score",
                                                        "Percent Passing Math",
                                                        "Percent Passing Reading",
                                                       "Percent Overall Passing Rate"])
district_summary_df["Total Students"] = district_summary_df["Total Students"].map('{:,}'.format)
district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map('${:,.2f}'.format)

district_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percent Passing Math</th>
      <th>Percent Passing Reading</th>
      <th>Percent Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24,649,428.00</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>72.392137</td>
      <td>82.971662</td>
      <td>77.6819</td>
    </tr>
  </tbody>
</table>
</div>



# School Summary


```python
# Change header name from the dataframe from name to school
schools_complete_df = schools_complete_df.rename(columns={"name": "school"})
# schools_complete_df.columns
```


```python
# Merge the two data frames together on = school 
schools_complete_data_df = pd.merge(schools_complete_df, students_complete_df, on='school')
#testing
# schools_complete_data_df.head()

```


```python
# count number of students in each school
student_count = schools_complete_data_df['school'].value_counts()
# testing
# student_count.head()
```


```python
# changing the school type into a str
school_type = schools_complete_data_df.groupby('school')['type'].unique()
school_type = school_type.str[0]
#testing
# school_type
```


```python
# total budget for each school
budget_per_school = schools_complete_data_df.groupby('school')['budget'].count()
# budget_per_school.head()
# testing
# budget_per_school
```


```python
# budget for each student
budget_per_student = schools_complete_df.set_index('school')['budget']/schools_complete_df.set_index('school')['size']
# budget_per_student.head()
# testing
# budget_per_student
```


```python
# Average math and readin scores for each school
school_average_math = round(schools_complete_data_df.groupby('school')['math_score'].mean(),2)
school_average_reading = round(schools_complete_data_df.groupby('school')['reading_score'].mean(),2)
```


```python
# dataframe with reading and math passing only scores
passing_df = schools_complete_data_df.loc[(schools_complete_data_df['math_score'] >=70) 
                                          & (schools_complete_data_df['reading_score'] >=70)]
passing_math_df = schools_complete_data_df.loc[(schools_complete_data_df['math_score'] >=70)]
passing_reading_df = schools_complete_data_df.loc[(schools_complete_data_df['reading_score'] >=70)]

# testing
# passing_math_df.head()
# passing_reading_df.head()
```


```python
# percentage for students passing math and reading 
percent_passing_math = round((passing_math_df.groupby('school')['math_score'].count()/student_count)*100,1)
percent_passing_reading = round((passing_reading_df.groupby('school')['reading_score'].count()/student_count)*100,1)
# percent_passing_reading
# percent_passing_math
```


```python
# overall passing percentage
overall_passing_percent = round((percent_passing_math + percent_passing_reading)/2, 2)
# overall_passing_percent
```


```python
# school summary dataframe

school_summary_df = pd.DataFrame({"School Type":school_type,
                                "Total Students":student_count,
                                "Total School Budget":budget_per_school,
                                "Budget Per Student":budget_per_student,
                                "Average Math Score": school_average_math,
                                "Average Reading Score": school_average_reading,
                                "% Passing Math": percent_passing_math,
                                "% Passing Reading": percent_passing_reading,
                                "% Overall Passing Rate":overall_passing_percent},columns=["School Type",
                                                        "Total Students",
                                                        "Total School Budget",
                                                        "Budget Per Student",
                                                        "Average Math Score",
                                                        "Average Reading Score",
                                                        "% Passing Math",
                                                       "% Passing Reading",
                                                        "% Overall Passing Rate"])
#testing
# school_summary_df.head()
school_summary_df["Total Students"] = school_summary_df["Total Students"].map('{:,}'.format)
school_summary_df["Total School Budget"] = school_summary_df["Total School Budget"].map('${:,.2f}'.format)
school_summary_df["Budget Per Student"] = school_summary_df["Budget Per Student"].map('${:,.2f}'.format)
school_summary_df
 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Budget Per Student</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4,976</td>
      <td>$4,976.00</td>
      <td>$628.00</td>
      <td>77.05</td>
      <td>81.03</td>
      <td>66.7</td>
      <td>81.9</td>
      <td>74.30</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,858.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.1</td>
      <td>97.0</td>
      <td>95.55</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$2,949.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>66.0</td>
      <td>80.7</td>
      <td>73.35</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2,739</td>
      <td>$2,739.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.3</td>
      <td>79.3</td>
      <td>73.80</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1,468</td>
      <td>$1,468.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.4</td>
      <td>97.1</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4,635</td>
      <td>$4,635.00</td>
      <td>$652.00</td>
      <td>77.29</td>
      <td>80.93</td>
      <td>66.8</td>
      <td>80.9</td>
      <td>73.85</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$427.00</td>
      <td>$581.00</td>
      <td>83.80</td>
      <td>83.81</td>
      <td>92.5</td>
      <td>96.3</td>
      <td>94.40</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$2,917.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.7</td>
      <td>81.3</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$4,761.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.1</td>
      <td>81.2</td>
      <td>73.65</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$962.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.6</td>
      <td>95.9</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$3,999.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.4</td>
      <td>80.2</td>
      <td>73.30</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1,761</td>
      <td>$1,761.00</td>
      <td>$600.00</td>
      <td>83.36</td>
      <td>83.73</td>
      <td>93.9</td>
      <td>95.9</td>
      <td>94.90</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,635.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.3</td>
      <td>97.3</td>
      <td>95.30</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$2,283.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.9</td>
      <td>96.5</td>
      <td>95.20</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1,800</td>
      <td>$1,800.00</td>
      <td>$583.00</td>
      <td>83.68</td>
      <td>83.96</td>
      <td>93.3</td>
      <td>96.6</td>
      <td>94.95</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools by Passing Rate


```python
# the top schools ( only top 5 )
top_performing_schools= school_summary_df.sort_values("% Overall Passing Rate", ascending=False, inplace=False)
top_performing_schools.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Budget Per Student</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,858.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.1</td>
      <td>97.0</td>
      <td>95.55</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,635.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.3</td>
      <td>97.3</td>
      <td>95.30</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1,468</td>
      <td>$1,468.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.4</td>
      <td>97.1</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$962.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.6</td>
      <td>95.9</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$2,283.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.9</td>
      <td>96.5</td>
      <td>95.20</td>
    </tr>
  </tbody>
</table>
</div>



#  Bottom Performing Schools (By Passing Rate)



```python
# only showing bottom 5
Bottom_performing_schools= school_summary_df.sort_values("% Overall Passing Rate", ascending=True, inplace=False)
Bottom_performing_schools.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Budget Per Student</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$3,999.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.4</td>
      <td>80.2</td>
      <td>73.30</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$2,949.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>66.0</td>
      <td>80.7</td>
      <td>73.35</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$2,917.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.7</td>
      <td>81.3</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$4,761.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.1</td>
      <td>81.2</td>
      <td>73.65</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2,739</td>
      <td>$2,739.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.3</td>
      <td>79.3</td>
      <td>73.80</td>
    </tr>
  </tbody>
</table>
</div>



#  Math Scores by Grade


```python
from pandas.api.types import CategoricalDtype
#Reset the grade order in the original students data frame "students_complete_df". 
students_complete_df["grade"] = students_complete_df['grade'].astype(CategoricalDtype(["9th", "10th","11th","12th"]))
# students_complete_df

math_scores_grade = round(students_complete_df.pivot_table(index="school", columns="grade", values="math_score"),2)
math_scores_grade.index.name = None 
math_scores_grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.08</td>
      <td>77.00</td>
      <td>77.52</td>
      <td>76.49</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.09</td>
      <td>83.15</td>
      <td>82.77</td>
      <td>83.28</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.40</td>
      <td>76.54</td>
      <td>76.88</td>
      <td>77.15</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.36</td>
      <td>77.67</td>
      <td>76.92</td>
      <td>76.18</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.04</td>
      <td>84.23</td>
      <td>83.84</td>
      <td>83.36</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.44</td>
      <td>77.34</td>
      <td>77.14</td>
      <td>77.19</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.79</td>
      <td>83.43</td>
      <td>85.00</td>
      <td>82.86</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.03</td>
      <td>75.91</td>
      <td>76.45</td>
      <td>77.23</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.19</td>
      <td>76.69</td>
      <td>77.49</td>
      <td>76.86</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.63</td>
      <td>83.37</td>
      <td>84.33</td>
      <td>84.12</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.86</td>
      <td>76.61</td>
      <td>76.40</td>
      <td>77.69</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.42</td>
      <td>82.92</td>
      <td>83.38</td>
      <td>83.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.59</td>
      <td>83.09</td>
      <td>83.50</td>
      <td>83.50</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.09</td>
      <td>83.72</td>
      <td>83.20</td>
      <td>83.04</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.26</td>
      <td>84.01</td>
      <td>83.84</td>
      <td>83.64</td>
    </tr>
  </tbody>
</table>
</div>



# Reading scores by grade


```python
#Reset the grade order in the original students data frame "students_complete_df". 
students_complete_df['grade'] = students_complete_df['grade'].astype(CategoricalDtype(["9th", "10th","11th","12th"]))
# students_complete_df

reading_scores_grade = round(students_complete_df.pivot_table(index="school", columns="grade", values="reading_score"),2)
reading_scores_grade.index.name = None
reading_scores_grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.30</td>
      <td>80.91</td>
      <td>80.95</td>
      <td>80.91</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.68</td>
      <td>84.25</td>
      <td>83.79</td>
      <td>84.29</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.20</td>
      <td>81.41</td>
      <td>80.64</td>
      <td>81.38</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.63</td>
      <td>81.26</td>
      <td>80.40</td>
      <td>80.66</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.37</td>
      <td>83.71</td>
      <td>84.29</td>
      <td>84.01</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.87</td>
      <td>80.66</td>
      <td>81.40</td>
      <td>80.86</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.68</td>
      <td>83.32</td>
      <td>83.82</td>
      <td>84.70</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.29</td>
      <td>81.51</td>
      <td>81.42</td>
      <td>80.31</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.26</td>
      <td>80.77</td>
      <td>80.62</td>
      <td>81.23</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.81</td>
      <td>83.61</td>
      <td>84.34</td>
      <td>84.59</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.99</td>
      <td>80.63</td>
      <td>80.86</td>
      <td>80.38</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.12</td>
      <td>83.44</td>
      <td>84.37</td>
      <td>82.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.73</td>
      <td>84.25</td>
      <td>83.59</td>
      <td>83.83</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.94</td>
      <td>84.02</td>
      <td>83.76</td>
      <td>84.32</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.83</td>
      <td>83.81</td>
      <td>84.16</td>
      <td>84.07</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending


```python
# Create the bins ( school spending )
spending_bins = [0, 585, 615, 645, 675]

# Names for the bins
spending_range = ["<$585", "$585-615", "$615-645", "$645-675"]

#schools_spending_df = school_summary_df
school_summary_df["Spending Ranges(Per Student)"] = pd.cut(budget_per_student, bins=spending_bins, labels=spending_range)
school_summary_df.head()

spending_math_score = school_summary_df.groupby(["Spending Ranges(Per Student)"])["Average Math Score"].mean()
spending_reading_score = school_summary_df.groupby(["Spending Ranges(Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_summary_df.groupby(["Spending Ranges(Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_summary_df.groupby(["Spending Ranges(Per Student)"])['% Passing Reading'].mean()
overall_passing_rate = (spending_passing_math + spending_passing_reading)/2

# Creating dataframe
scores_by_spending = pd.DataFrame ({"Average Math Score": spending_math_score,
    "Average Reading Score": spending_reading_score,
    "% Passing Math": spending_passing_math,
    "% Passing Reading": spending_passing_reading,
    "Overall Passing Rate": overall_passing_rate})
scores_by_spending

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges(Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$585</th>
      <td>93.450000</td>
      <td>96.600000</td>
      <td>83.452500</td>
      <td>83.935000</td>
      <td>95.025000</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>94.250000</td>
      <td>95.900000</td>
      <td>83.600000</td>
      <td>83.885000</td>
      <td>95.075000</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>75.683333</td>
      <td>86.083333</td>
      <td>79.078333</td>
      <td>81.891667</td>
      <td>80.883333</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>66.200000</td>
      <td>81.133333</td>
      <td>76.996667</td>
      <td>81.026667</td>
      <td>73.666667</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by Schools Size


```python
# Create the bins ( school size)
size_bins = [0, 1000, 2000, 5000]

# Names for the bins
group_names = ["small (<1000)", "Medium(1000-2000)", "Large(2000-5000)"]

school_size_df = school_summary_df
school_size_df["School Size"] = pd.cut(student_count, bins=size_bins, labels=group_names)
school_size_df

spending_math_score = school_size_df.groupby(["School Size"])["Average Math Score"].mean()
spending_reading_score = school_size_df.groupby(["School Size"])["Average Reading Score"].mean()
spending_passing_math = school_size_df.groupby(["School Size"])["% Passing Math"].mean()
spending_passing_reading = school_size_df.groupby(["School Size"])['% Passing Reading'].mean()
overall_passing_rate = (spending_passing_math + spending_passing_reading)/2

# Creating dataframe
scores_by_size = pd.DataFrame ({"Average Math Score": spending_math_score,
    "Average Reading Score": spending_reading_score,
    "% Passing Math": spending_passing_math,
    "% Passing Reading": spending_passing_reading,
    "Overall Passing Rate": overall_passing_rate})
scores_by_size

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>small (&lt;1000)</th>
      <td>93.5500</td>
      <td>96.10</td>
      <td>83.820</td>
      <td>83.92500</td>
      <td>94.82500</td>
    </tr>
    <tr>
      <th>Medium(1000-2000)</th>
      <td>93.6000</td>
      <td>96.78</td>
      <td>83.374</td>
      <td>83.86800</td>
      <td>95.19000</td>
    </tr>
    <tr>
      <th>Large(2000-5000)</th>
      <td>69.9875</td>
      <td>82.75</td>
      <td>77.745</td>
      <td>81.34375</td>
      <td>76.36875</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Type


```python
#* Repeat the above breakdown, but this time group schools based on school type (Charter vs. District)*
scores_school_type = school_summary_df[["School Type","Average Math Score",
                                        "Average Reading Score", 
                                        "% Passing Math", 
                                        "% Passing Reading", 
                                        "% Overall Passing Rate"]]
scores_school_type = scores_school_type.groupby('School Type').mean()
scores_school_type
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.472500</td>
      <td>83.897500</td>
      <td>93.625000</td>
      <td>96.575000</td>
      <td>95.100000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.955714</td>
      <td>80.965714</td>
      <td>66.571429</td>
      <td>80.785714</td>
      <td>73.678571</td>
    </tr>
  </tbody>
</table>
</div>


