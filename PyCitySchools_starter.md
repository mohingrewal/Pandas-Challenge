# PyCity Schools Analysis

- Your analysis here
  
---


```python
# Dependencies and Setup
import pandas as pd
from pathlib import Path

# File to Load (Remember to Change These)
school_data_to_load = Path("Resources/schools_complete.csv")
student_data_to_load = Path("Resources/students_complete.csv")

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete.head()

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
      <th>student_name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school_name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>



## District Summary


```python
# Calculate the total number of unique schools
unique_school_name = school_data_complete['school_name'].unique()
total_school_number=len(unique_school_name)
total_school_number

```




    15




```python
# Calculate the total number of students
total_student_number = school_data_complete['student_name'].count()
total_student_number


```




    39170




```python
# Calculate the total budget
total_budget = school_data['budget'].sum()
total_budget

```




    24649428




```python
# Calculate the average (mean) math score
average_math_score = school_data_complete["math_score"].mean()
average_math_score

```




    78.98537145774827




```python
# Calculate the average (mean) reading score
average_reading_score = school_data_complete["reading_score"].mean()
average_reading_score

```




    81.87784018381414




```python
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)
student_passing_math = school_data_complete.loc[school_data_complete["math_score"] >= 70]
number_students_passing_math = student_passing_math["Student ID"].count()
percent_passing_math = (number_students_passing_math / total_student_number) * 100
percent_passing_math
```




    74.9808526933878




```python
# Calculate the percentage of students who passed reading (hint: look at how the math percentage was calculated)
passing_reading_count = school_data_complete.loc[school_data_complete["reading_score"] >=70]
number_students_passing_reading = passing_reading_count["Student ID"].count()
passing_reading_percentage = (number_students_passing_reading / total_student_number) * 100
passing_reading_percentage

```




    85.80546336482001




```python
# Use the following to calculate the percentage of students that passed math and reading
Overall_passing_rate = school_data_complete[(school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
]['Student ID'].count()/total_student_number*100
Overall_passing_rate

```




    65.17232575950983




```python
# Create a high-level snapshot of the district's key metrics in a DataFrame
district_summary = pd.DataFrame({
    "Total Schools": total_school_number,
    "Total Students": f"{total_student_number:,}",
    "Total Budget": f"${total_budget:,.2f}",
    "Average Math Score": f"{average_math_score:.6f}",
    "Average Reading Score": f"{average_reading_score:.5f}",
    "% Passing Math": f"{percent_passing_math:.6f}",
    "% Passing Reading": f"{passing_reading_percentage:.6f}",
    "% Overall Passing": f"{Overall_passing_rate: .6f}"
}, index=[0])

# Display the DataFrame
district_summary

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
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
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
      <td>74.980853</td>
      <td>85.805463</td>
      <td>65.172326</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary


```python
#Group by school name
school_name = school_data_complete.set_index('school_name').groupby(['school_name'])
```


```python
# Use the code provided to select the type per school from school_data
school_type = school_data.set_index(['school_name'])['type']

```


```python
# Calculate the total student count per school from school_data
total_student = school_name['Student ID'].count()

```


```python
# Calculate the total school budget and per student budget spending per school from school_data
total_school_budget = school_data.set_index('school_name')['budget']
budget_per_student = (school_data.set_index('school_name')['budget']/school_data.set_index('school_name')['size'])

```


```python
# Calculate the average test scores per school from school_data_complete
per_school_math = school_name['math_score'].mean()
per_school_reading = school_name['reading_score'].mean()

```


```python
# Calculate the number of students per school with math scores of 70 or higher from school_data_complete
students_passing_math = school_data_complete[school_data_complete['math_score'] >=70].groupby('school_name')['Student ID'].count()/total_student*100


```


```python
# Calculate the number of students per school with reading scores of 70 or higher from school_data_complete
students_passing_reading = school_data_complete[school_data_complete['reading_score'] >=70].groupby('school_name')['Student ID'].count()/total_student*100


```


```python
# Use the provided code to calculate the number of students per school that passed both math and reading with scores of 70 or higher
students_passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
school_students_passing_math_and_reading = students_passing_math_and_reading.groupby(["school_name"]).size()

```


```python
# Use the provided code to calculate the passing rates
overall_passing_rate = school_data_complete[(school_data_complete['reading_score'] >=70) & (school_data_complete['math_score'] >=70)].groupby('school_name')['Student ID'].count()/total_student*100 

```


```python
# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame({
     "School Type": school_type,
    "Total Students": total_student,
    "Per Student Budget": budget_per_student,
    "Total School Budget": total_school_budget,
    "Average Math Score": per_school_math,
    "Average Reading Score": per_school_reading,
    '% Passing Math': students_passing_math,
    '% Passing Reading': students_passing_reading,
    "% Overall Passing": overall_passing_rate
})

#munging
per_school_summary = per_school_summary[['School Type', 
                          'Total Students', 
                          'Total School Budget', 
                          'Per Student Budget', 
                          'Average Math Score', 
                          'Average Reading Score',
                          '% Passing Math',
                          '% Passing Reading',
                          '% Overall Passing']]


# Formatting
per_school_summary.style.format({'Total Students': '{:}',
                                  "Total School Budget": "${:,.2f}",
                                 "Per Student Budget": "${:.2f}",
                                  'Average Math Score': "{:6f}", 
                                  'Average Reading Score': "{:6f}", 
                                  "% Passing Math": "{:6f}", 
                                  "% Passing Reading": "{:6f}"})


# Display the DataFrame
per_school_summary

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
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>54.642283</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>54.289887</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>89.227166</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>53.539172</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>90.540541</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>89.892107</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>90.948012</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>90.333333</td>
    </tr>
  </tbody>
</table>
</div>



## Highest-Performing Schools (by % Overall Passing)


```python
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows.
top_schools = per_school_summary.sort_values("% Overall Passing", ascending = False)
top_schools.head().style.format({'Total Students':'{:}',
                                "Total School Budget": "${:,.2f}",
                                "Per Student Budget": "${:.2f}",
                                 "% Passing Math": "{:6f}",
                                 "% Passing Reading": "{:6f}",
                                 "% Overall Passing": "{:6f}"
                                })

```




<style type="text/css">
</style>
<table id="T_14040">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_14040_level0_col0" class="col_heading level0 col0" >School Type</th>
      <th id="T_14040_level0_col1" class="col_heading level0 col1" >Total Students</th>
      <th id="T_14040_level0_col2" class="col_heading level0 col2" >Total School Budget</th>
      <th id="T_14040_level0_col3" class="col_heading level0 col3" >Per Student Budget</th>
      <th id="T_14040_level0_col4" class="col_heading level0 col4" >Average Math Score</th>
      <th id="T_14040_level0_col5" class="col_heading level0 col5" >Average Reading Score</th>
      <th id="T_14040_level0_col6" class="col_heading level0 col6" >% Passing Math</th>
      <th id="T_14040_level0_col7" class="col_heading level0 col7" >% Passing Reading</th>
      <th id="T_14040_level0_col8" class="col_heading level0 col8" >% Overall Passing</th>
    </tr>
    <tr>
      <th class="index_name level0" >school_name</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
      <th class="blank col5" >&nbsp;</th>
      <th class="blank col6" >&nbsp;</th>
      <th class="blank col7" >&nbsp;</th>
      <th class="blank col8" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_14040_level0_row0" class="row_heading level0 row0" >Cabrera High School</th>
      <td id="T_14040_row0_col0" class="data row0 col0" >Charter</td>
      <td id="T_14040_row0_col1" class="data row0 col1" >1858</td>
      <td id="T_14040_row0_col2" class="data row0 col2" >$1,081,356.00</td>
      <td id="T_14040_row0_col3" class="data row0 col3" >$582.00</td>
      <td id="T_14040_row0_col4" class="data row0 col4" >83.061895</td>
      <td id="T_14040_row0_col5" class="data row0 col5" >83.975780</td>
      <td id="T_14040_row0_col6" class="data row0 col6" >94.133477</td>
      <td id="T_14040_row0_col7" class="data row0 col7" >97.039828</td>
      <td id="T_14040_row0_col8" class="data row0 col8" >91.334769</td>
    </tr>
    <tr>
      <th id="T_14040_level0_row1" class="row_heading level0 row1" >Thomas High School</th>
      <td id="T_14040_row1_col0" class="data row1 col0" >Charter</td>
      <td id="T_14040_row1_col1" class="data row1 col1" >1635</td>
      <td id="T_14040_row1_col2" class="data row1 col2" >$1,043,130.00</td>
      <td id="T_14040_row1_col3" class="data row1 col3" >$638.00</td>
      <td id="T_14040_row1_col4" class="data row1 col4" >83.418349</td>
      <td id="T_14040_row1_col5" class="data row1 col5" >83.848930</td>
      <td id="T_14040_row1_col6" class="data row1 col6" >93.272171</td>
      <td id="T_14040_row1_col7" class="data row1 col7" >97.308869</td>
      <td id="T_14040_row1_col8" class="data row1 col8" >90.948012</td>
    </tr>
    <tr>
      <th id="T_14040_level0_row2" class="row_heading level0 row2" >Griffin High School</th>
      <td id="T_14040_row2_col0" class="data row2 col0" >Charter</td>
      <td id="T_14040_row2_col1" class="data row2 col1" >1468</td>
      <td id="T_14040_row2_col2" class="data row2 col2" >$917,500.00</td>
      <td id="T_14040_row2_col3" class="data row2 col3" >$625.00</td>
      <td id="T_14040_row2_col4" class="data row2 col4" >83.351499</td>
      <td id="T_14040_row2_col5" class="data row2 col5" >83.816757</td>
      <td id="T_14040_row2_col6" class="data row2 col6" >93.392371</td>
      <td id="T_14040_row2_col7" class="data row2 col7" >97.138965</td>
      <td id="T_14040_row2_col8" class="data row2 col8" >90.599455</td>
    </tr>
    <tr>
      <th id="T_14040_level0_row3" class="row_heading level0 row3" >Wilson High School</th>
      <td id="T_14040_row3_col0" class="data row3 col0" >Charter</td>
      <td id="T_14040_row3_col1" class="data row3 col1" >2283</td>
      <td id="T_14040_row3_col2" class="data row3 col2" >$1,319,574.00</td>
      <td id="T_14040_row3_col3" class="data row3 col3" >$578.00</td>
      <td id="T_14040_row3_col4" class="data row3 col4" >83.274201</td>
      <td id="T_14040_row3_col5" class="data row3 col5" >83.989488</td>
      <td id="T_14040_row3_col6" class="data row3 col6" >93.867718</td>
      <td id="T_14040_row3_col7" class="data row3 col7" >96.539641</td>
      <td id="T_14040_row3_col8" class="data row3 col8" >90.582567</td>
    </tr>
    <tr>
      <th id="T_14040_level0_row4" class="row_heading level0 row4" >Pena High School</th>
      <td id="T_14040_row4_col0" class="data row4 col0" >Charter</td>
      <td id="T_14040_row4_col1" class="data row4 col1" >962</td>
      <td id="T_14040_row4_col2" class="data row4 col2" >$585,858.00</td>
      <td id="T_14040_row4_col3" class="data row4 col3" >$609.00</td>
      <td id="T_14040_row4_col4" class="data row4 col4" >83.839917</td>
      <td id="T_14040_row4_col5" class="data row4 col5" >84.044699</td>
      <td id="T_14040_row4_col6" class="data row4 col6" >94.594595</td>
      <td id="T_14040_row4_col7" class="data row4 col7" >95.945946</td>
      <td id="T_14040_row4_col8" class="data row4 col8" >90.540541</td>
    </tr>
  </tbody>
</table>




## Bottom Performing Schools (By % Overall Passing)


```python
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
bottom_schools = top_schools.tail()
bottom_schools = bottom_schools.sort_values('% Overall Passing')
bottom_schools.style.format({'Total Students':'{:}',
                             "Total School Budget": "${:,.2f}", 
                             "Per Student Budget": "${:.2f}", 
                             "% Passing Math": "{:6f}", 
                           "% Passing Reading": "{:6f}", 
                           "% Overall Passing": "{:6f}"})

```




<style type="text/css">
</style>
<table id="T_77db6">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_77db6_level0_col0" class="col_heading level0 col0" >School Type</th>
      <th id="T_77db6_level0_col1" class="col_heading level0 col1" >Total Students</th>
      <th id="T_77db6_level0_col2" class="col_heading level0 col2" >Total School Budget</th>
      <th id="T_77db6_level0_col3" class="col_heading level0 col3" >Per Student Budget</th>
      <th id="T_77db6_level0_col4" class="col_heading level0 col4" >Average Math Score</th>
      <th id="T_77db6_level0_col5" class="col_heading level0 col5" >Average Reading Score</th>
      <th id="T_77db6_level0_col6" class="col_heading level0 col6" >% Passing Math</th>
      <th id="T_77db6_level0_col7" class="col_heading level0 col7" >% Passing Reading</th>
      <th id="T_77db6_level0_col8" class="col_heading level0 col8" >% Overall Passing</th>
    </tr>
    <tr>
      <th class="index_name level0" >school_name</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
      <th class="blank col5" >&nbsp;</th>
      <th class="blank col6" >&nbsp;</th>
      <th class="blank col7" >&nbsp;</th>
      <th class="blank col8" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_77db6_level0_row0" class="row_heading level0 row0" >Rodriguez High School</th>
      <td id="T_77db6_row0_col0" class="data row0 col0" >District</td>
      <td id="T_77db6_row0_col1" class="data row0 col1" >3999</td>
      <td id="T_77db6_row0_col2" class="data row0 col2" >$2,547,363.00</td>
      <td id="T_77db6_row0_col3" class="data row0 col3" >$637.00</td>
      <td id="T_77db6_row0_col4" class="data row0 col4" >76.842711</td>
      <td id="T_77db6_row0_col5" class="data row0 col5" >80.744686</td>
      <td id="T_77db6_row0_col6" class="data row0 col6" >66.366592</td>
      <td id="T_77db6_row0_col7" class="data row0 col7" >80.220055</td>
      <td id="T_77db6_row0_col8" class="data row0 col8" >52.988247</td>
    </tr>
    <tr>
      <th id="T_77db6_level0_row1" class="row_heading level0 row1" >Figueroa High School</th>
      <td id="T_77db6_row1_col0" class="data row1 col0" >District</td>
      <td id="T_77db6_row1_col1" class="data row1 col1" >2949</td>
      <td id="T_77db6_row1_col2" class="data row1 col2" >$1,884,411.00</td>
      <td id="T_77db6_row1_col3" class="data row1 col3" >$639.00</td>
      <td id="T_77db6_row1_col4" class="data row1 col4" >76.711767</td>
      <td id="T_77db6_row1_col5" class="data row1 col5" >81.158020</td>
      <td id="T_77db6_row1_col6" class="data row1 col6" >65.988471</td>
      <td id="T_77db6_row1_col7" class="data row1 col7" >80.739234</td>
      <td id="T_77db6_row1_col8" class="data row1 col8" >53.204476</td>
    </tr>
    <tr>
      <th id="T_77db6_level0_row2" class="row_heading level0 row2" >Huang High School</th>
      <td id="T_77db6_row2_col0" class="data row2 col0" >District</td>
      <td id="T_77db6_row2_col1" class="data row2 col1" >2917</td>
      <td id="T_77db6_row2_col2" class="data row2 col2" >$1,910,635.00</td>
      <td id="T_77db6_row2_col3" class="data row2 col3" >$655.00</td>
      <td id="T_77db6_row2_col4" class="data row2 col4" >76.629414</td>
      <td id="T_77db6_row2_col5" class="data row2 col5" >81.182722</td>
      <td id="T_77db6_row2_col6" class="data row2 col6" >65.683922</td>
      <td id="T_77db6_row2_col7" class="data row2 col7" >81.316421</td>
      <td id="T_77db6_row2_col8" class="data row2 col8" >53.513884</td>
    </tr>
    <tr>
      <th id="T_77db6_level0_row3" class="row_heading level0 row3" >Hernandez High School</th>
      <td id="T_77db6_row3_col0" class="data row3 col0" >District</td>
      <td id="T_77db6_row3_col1" class="data row3 col1" >4635</td>
      <td id="T_77db6_row3_col2" class="data row3 col2" >$3,022,020.00</td>
      <td id="T_77db6_row3_col3" class="data row3 col3" >$652.00</td>
      <td id="T_77db6_row3_col4" class="data row3 col4" >77.289752</td>
      <td id="T_77db6_row3_col5" class="data row3 col5" >80.934412</td>
      <td id="T_77db6_row3_col6" class="data row3 col6" >66.752967</td>
      <td id="T_77db6_row3_col7" class="data row3 col7" >80.862999</td>
      <td id="T_77db6_row3_col8" class="data row3 col8" >53.527508</td>
    </tr>
    <tr>
      <th id="T_77db6_level0_row4" class="row_heading level0 row4" >Johnson High School</th>
      <td id="T_77db6_row4_col0" class="data row4 col0" >District</td>
      <td id="T_77db6_row4_col1" class="data row4 col1" >4761</td>
      <td id="T_77db6_row4_col2" class="data row4 col2" >$3,094,650.00</td>
      <td id="T_77db6_row4_col3" class="data row4 col3" >$650.00</td>
      <td id="T_77db6_row4_col4" class="data row4 col4" >77.072464</td>
      <td id="T_77db6_row4_col5" class="data row4 col5" >80.966394</td>
      <td id="T_77db6_row4_col6" class="data row4 col6" >66.057551</td>
      <td id="T_77db6_row4_col7" class="data row4 col7" >81.222432</td>
      <td id="T_77db6_row4_col8" class="data row4 col8" >53.539172</td>
    </tr>
  </tbody>
</table>




## Math Scores by Grade


```python
# Use the code provided to separate the data by grade level average math scores for each school
ninth_graders = school_data_complete.loc[school_data_complete['grade'] == '9th'].groupby('school_name')["math_score"].mean()
tenth_graders = school_data_complete.loc[school_data_complete['grade'] == '10th'].groupby('school_name')["math_score"].mean()
eleventh_graders = school_data_complete.loc[school_data_complete['grade'] == '11th'].groupby('school_name')["math_score"].mean()
twelfth_graders = school_data_complete.loc[school_data_complete['grade'] == '12th'].groupby('school_name')["math_score"].mean()

math_scores = pd.DataFrame({
        "9th": ninth_graders,
        "10th": tenth_graders,
        "11th": eleventh_graders,
        "12th": twelfth_graders
})
math_scores = math_scores[['9th', '10th', '11th', '12th']]
math_scores.index.name = "School Name"

#show and format
math_scores.style.format({'9th': '{:.6f}', 
                          "10th": '{:.6f}', 
                          "11th": "{:.6f}", 
                          "12th": "{:.6f}"})
# Display the DataFrame
math_scores
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



## Reading Score by Grade 


```python
#creates grade level average reading scores for each school
ninth_graders = school_data_complete.loc[school_data_complete['grade'] == '9th'].groupby('school_name')["reading_score"].mean()
tenth_graders = school_data_complete.loc[school_data_complete['grade'] == '10th'].groupby('school_name')["reading_score"].mean()
eleventh_graders = school_data_complete.loc[school_data_complete['grade'] == '11th'].groupby('school_name')["reading_score"].mean()
twelfth_graders = school_data_complete.loc[school_data_complete['grade'] == '12th'].groupby('school_name')["reading_score"].mean()

#merges the reading score averages by school and grade together
reading_scores = pd.DataFrame({
        "9th": ninth_graders,
        "10th": tenth_graders,
        "11th": eleventh_graders,
        "12th": twelfth_graders
})
reading_scores = reading_scores[['9th', '10th', '11th', '12th']]
reading_scores.index.name = "School Name"

#format
reading_scores.style.format({'9th': '{:.6f}', 
                             "10th": '{:.6f}', 
                             "11th": "{:.6f}", 
                             "12th": "{:.6f}"})

# Display the DataFrame
reading_scores

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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Spending


```python

# Establish the bins
bins = [0, 584, 629, 644, 675]
group_name = ["<$584", "$585-629", "$630-644", "$645-675"]

avg_math = by_spending['math_score'].mean()
avg_read = by_spending['reading_score'].mean()
pass_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
pass_read = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
          
#reorder columns
scores_by_spend = scores_by_spend[[
    "Average Math Score",
    "Average Reading Score",
    "% Passing Math",
    "% Passing Reading",
    "% Overall Passing"
]]
#formatting
scores_by_spend.style.format({'Average Math Score': '{:.2f}', 
                              'Average Reading Score': '{:.2f}', 
                              '% Passing Math': '{:.2f}', 
                              '% Passing Reading':'{:.2f}', 
                              '% Overall Passing': '{:.2f}'})

spending_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]


```

    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\4114915280.py:7: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      pass_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\4114915280.py:8: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      pass_read = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\4114915280.py:9: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()*100
    


```python
# Create a copy of the school summary since it has the "Per Student Budget"
school_spending_df = per_school_summary.copy()
scores_by_spend.index.name = "Per Student Budget"

```


```python

school_data_complete['spending_bins'] = pd.cut(school_data_complete['budget']/school_data_complete['size'], bins, labels = group_name)
school_spending_df

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
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>54.642283</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>54.289887</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>89.227166</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>53.539172</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>90.540541</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>89.892107</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>90.948012</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>90.333333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#  Calculate averages for the desired columns.
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()

```


```python
# Assemble into DataFrame
spending_summary =

# Display results
spending_summary

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
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
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
      <td>83.455399</td>
      <td>83.933814</td>
      <td>93.460096</td>
      <td>96.610877</td>
      <td>90.369459</td>
    </tr>
    <tr>
      <th>$585-630</th>
      <td>81.899826</td>
      <td>83.155286</td>
      <td>87.133538</td>
      <td>92.718205</td>
      <td>81.418596</td>
    </tr>
    <tr>
      <th>$630-645</th>
      <td>78.518855</td>
      <td>81.624473</td>
      <td>73.484209</td>
      <td>84.391793</td>
      <td>62.857656</td>
    </tr>
    <tr>
      <th>$645-680</th>
      <td>76.997210</td>
      <td>81.027843</td>
      <td>66.164813</td>
      <td>81.133951</td>
      <td>53.526855</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Size


```python
# Establish the bins.
size_bins = [0, 1000, 1999, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
school_data_complete['size_bins'] = pd.cut(school_data_complete['size'], bins, labels = group_name)

#group by spending
by_size = school_data_complete.groupby('size_bins')

#calculations 
average_math_score = by_size['math_score'].mean()
average_reading_score = by_size['math_score'].mean()
pass_math_percent = school_data_complete[school_data_complete['math_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100
pass_read_percent = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100
overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100

            
# df build            
scores_by_size = pd.DataFrame({
    "Average Math Score": average_math_score,
    "Average Reading Score": average_reading_score,
    '% Passing Math': pass_math_percent,
    '% Passing Reading': pass_read_percent,
    '% Overall Passing': overall
    })
            
#reorder columns
scores_by_size = scores_by_size[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    '% Overall Passing'
]]

scores_by_size.index.name = "Total Students"
scores_by_size = scores_by_size.reindex(group_name)

#formating
scores_by_size.style.format({'Average Math Score': '{:.6f}', 
                              'Average Reading Score': '{:.6f}', 
                              '% Passing Math': '{:.6f}', 
                              '% Passing Reading':'{:.6f}', 
                              '% Overall Passing': '{:.6f}'})
```

    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\1373625192.py:7: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      by_size = school_data_complete.groupby('size_bins')
    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\1373625192.py:12: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      pass_math_percent = school_data_complete[school_data_complete['math_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100
    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\1373625192.py:13: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      pass_read_percent = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100
    C:\Users\mohin\AppData\Local\Temp\ipykernel_10828\1373625192.py:14: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()*100
    




<style type="text/css">
</style>
<table id="T_b8b2f">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_b8b2f_level0_col0" class="col_heading level0 col0" >Average Math Score</th>
      <th id="T_b8b2f_level0_col1" class="col_heading level0 col1" >Average Reading Score</th>
      <th id="T_b8b2f_level0_col2" class="col_heading level0 col2" >% Passing Math</th>
      <th id="T_b8b2f_level0_col3" class="col_heading level0 col3" >% Passing Reading</th>
      <th id="T_b8b2f_level0_col4" class="col_heading level0 col4" >% Overall Passing</th>
    </tr>
    <tr>
      <th class="index_name level0" >Total Students</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_b8b2f_level0_row0" class="row_heading level0 row0" ><$584</th>
      <td id="T_b8b2f_row0_col0" class="data row0 col0" >83.803279</td>
      <td id="T_b8b2f_row0_col1" class="data row0 col1" >83.803279</td>
      <td id="T_b8b2f_row0_col2" class="data row0 col2" >92.505855</td>
      <td id="T_b8b2f_row0_col3" class="data row0 col3" >96.252927</td>
      <td id="T_b8b2f_row0_col4" class="data row0 col4" >89.227166</td>
    </tr>
    <tr>
      <th id="T_b8b2f_level0_row1" class="row_heading level0 row1" >$585-629</th>
      <td id="T_b8b2f_row1_col0" class="data row1 col0" >nan</td>
      <td id="T_b8b2f_row1_col1" class="data row1 col1" >nan</td>
      <td id="T_b8b2f_row1_col2" class="data row1 col2" >nan</td>
      <td id="T_b8b2f_row1_col3" class="data row1 col3" >nan</td>
      <td id="T_b8b2f_row1_col4" class="data row1 col4" >nan</td>
    </tr>
    <tr>
      <th id="T_b8b2f_level0_row2" class="row_heading level0 row2" >$630-644</th>
      <td id="T_b8b2f_row2_col0" class="data row2 col0" >nan</td>
      <td id="T_b8b2f_row2_col1" class="data row2 col1" >nan</td>
      <td id="T_b8b2f_row2_col2" class="data row2 col2" >nan</td>
      <td id="T_b8b2f_row2_col3" class="data row2 col3" >nan</td>
      <td id="T_b8b2f_row2_col4" class="data row2 col4" >nan</td>
    </tr>
    <tr>
      <th id="T_b8b2f_level0_row3" class="row_heading level0 row3" >$645-675</th>
      <td id="T_b8b2f_row3_col0" class="data row3 col0" >nan</td>
      <td id="T_b8b2f_row3_col1" class="data row3 col1" >nan</td>
      <td id="T_b8b2f_row3_col2" class="data row3 col2" >nan</td>
      <td id="T_b8b2f_row3_col3" class="data row3 col3" >nan</td>
      <td id="T_b8b2f_row3_col4" class="data row3 col4" >nan</td>
    </tr>
  </tbody>
</table>





```python
# Categorize the spending based on the bins
# Use `pd.cut` on the "Total Students" column of the `per_school_summary` DataFrame.

per_school_summary["School Size"] =
per_school_summary

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
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
      <th>School Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$3,124,928.00</td>
      <td>$628.00</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>54.642283</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
      <td>Medium (1000-2000)</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>54.289887</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
      <td>Medium (1000-2000)</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>89.227166</td>
      <td>Small (&lt;1000)</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>53.539172</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>90.540541</td>
      <td>Small (&lt;1000)</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>52.988247</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>89.892107</td>
      <td>Medium (1000-2000)</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>90.948012</td>
      <td>Medium (1000-2000)</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>90.582567</td>
      <td>Large (2000-5000)</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>90.333333</td>
      <td>Medium (1000-2000)</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Type


```python
# group by type of school
school_type = school_data_complete.groupby("type")

#calculations 
average_math_score = schoo_type['math_score'].mean()
average_reading_score = schoo_type['math_score'].mean()
pass_math_percent = school_data_complete[school_data_complete['math_score'] >= 70].groupby('type')['Student ID'].count()/schoo_type['Student ID'].count()*100
pass_read_percent = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('type')['Student ID'].count()/schoo_type['Student ID'].count()*100
overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('type')['Student ID'].count()/schoo_type['Student ID'].count()*100

# df build            
scores_school_type = pd.DataFrame({
    "Average Math Score": average_math_score,
    "Average Reading Score": average_reading_score,
    '% Passing Math': pass_math_percent,
    '% Passing Reading': pass_read_percent,
    "% Overall Passing": overall})
    
#reorder columns
scores_school_type = scores_school_type[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "% Overall Passing"
]]
scores_school_type.index.name = "Type of School"


#formating
scores_school_type.style.format({'Average Math Score': '{:.6f}', 
                              'Average Reading Score': '{:.6f}', 
                              '% Passing Math': '{:.6f}', 
                              '% Passing Reading':'{:.6f}', 
                              '% Overall Passing': '{:.6f}'})
```




<style type="text/css">
</style>
<table id="T_efb53">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_efb53_level0_col0" class="col_heading level0 col0" >Average Math Score</th>
      <th id="T_efb53_level0_col1" class="col_heading level0 col1" >Average Reading Score</th>
      <th id="T_efb53_level0_col2" class="col_heading level0 col2" >% Passing Math</th>
      <th id="T_efb53_level0_col3" class="col_heading level0 col3" >% Passing Reading</th>
      <th id="T_efb53_level0_col4" class="col_heading level0 col4" >% Overall Passing</th>
    </tr>
    <tr>
      <th class="index_name level0" >Type of School</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_efb53_level0_row0" class="row_heading level0 row0" >Charter</th>
      <td id="T_efb53_row0_col0" class="data row0 col0" >83.406183</td>
      <td id="T_efb53_row0_col1" class="data row0 col1" >83.406183</td>
      <td id="T_efb53_row0_col2" class="data row0 col2" >93.701821</td>
      <td id="T_efb53_row0_col3" class="data row0 col3" >96.645891</td>
      <td id="T_efb53_row0_col4" class="data row0 col4" >90.560932</td>
    </tr>
    <tr>
      <th id="T_efb53_level0_row1" class="row_heading level0 row1" >District</th>
      <td id="T_efb53_row1_col0" class="data row1 col0" >76.987026</td>
      <td id="T_efb53_row1_col1" class="data row1 col1" >76.987026</td>
      <td id="T_efb53_row1_col2" class="data row1 col2" >66.518387</td>
      <td id="T_efb53_row1_col3" class="data row1 col3" >80.905249</td>
      <td id="T_efb53_row1_col4" class="data row1 col4" >53.695878</td>
    </tr>
  </tbody>
</table>





```python
# Assemble the new data by type into a DataFrame called `type_summary`
type_summary =

# Display results
type_summary

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
      <th>% Overall Passing</th>
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
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>90.432244</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>53.672208</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
