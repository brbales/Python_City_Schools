

```python
# modules
import numpy as np
import pandas as pd
```


```python
# read csv 1
schools_df = pd.read_csv("../raw_data/schools_complete.csv")
# rename column
schools_df.rename(columns={"name":"School Name"}, inplace=True)

# sample school data table
schools_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>School Name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
# snapshot school data cleanliness
schools_df.count()
```




    School ID      15
    School Name    15
    type           15
    size           15
    budget         15
    dtype: int64




```python
# read csv 
students_df = pd.read_csv("../raw_data/students_complete.csv")
# rename column
students_df.rename(columns={"name":"Student Name", "school":"School Name"}, inplace=True)

# sample students data table
students_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>Student Name</th>
      <th>gender</th>
      <th>grade</th>
      <th>School Name</th>
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




```python
# snapshot students data cleanliness
students_df.count()
```




    Student ID       39170
    Student Name     39170
    gender           39170
    grade            39170
    School Name      39170
    reading_score    39170
    math_score       39170
    dtype: int64




```python
# source data for district summary - table one
# define Total Schools
total_schools = schools_df["School ID"].count()
total_schools
```




    15




```python
# source data for district summary - table one
# define Total Students
total_students = students_df["Student ID"].count()
total_students
```




    39170




```python
# source data for district summary - table one
# define Total Budget
total_budget = schools_df["budget"].sum()
total_budget
```




    24649428




```python
# source data for district summary - table one
# define Avg. Math
avg_math = students_df["math_score"].mean()
avg_math
```




    78.98537145774827




```python
# source data for district summary - table one
# define Avg. Reading
avg_reading = students_df["reading_score"].mean()
avg_reading
```




    81.87784018381414




```python
# source data for district summary - table one
# define % Pass Math
mpass = (students_df[students_df["math_score"]>=60].count())
per_mpass = (mpass["math_score"]/total_students)*100
per_mpass
```




    92.445749297932096




```python
# source data for district summary - table one
# define % Pass Reading
rpass = (students_df[students_df["reading_score"]>=60].count())
per_rpass = (rpass["reading_score"]/total_students)*100
per_rpass
```




    100.0




```python
# source data for district summary - table one
# define Dist. Pass Rate
opass = (per_mpass+per_rpass)/2
opass
```




    96.222874648966041




```python
# make district summary - table one
dist_summary = pd.DataFrame({"Total Schools": [total_schools], "Total Students": [total_students], 
             "Total Budget": [total_budget], "Avg. Math Score": [avg_math], 
             "Avg. Reading Score": [avg_reading], "% Pass Math": [per_mpass], 
             "% Pass Reading": [per_rpass], "District Pass Rate": [opass]})
#cols1 = dist_summary.columns.tolist()
#cols1
dist_summary = dist_summary[['Total Schools', 'Total Students', 'Total Budget', 'Avg. Math Score', 
                             'Avg. Reading Score', '% Pass Math', '% Pass Reading', 'District Pass Rate']]
# format student and budget columns
dist_summary["Total Students"] = dist_summary["Total Students"].map("{:,}".format)
dist_summary["Total Budget"] = dist_summary["Total Budget"].map("${:.2f}".format)

# show table
dist_summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Avg. Math Score</th>
      <th>Avg. Reading Score</th>
      <th>% Pass Math</th>
      <th>% Pass Reading</th>
      <th>District Pass Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24649428.00</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>92.445749</td>
      <td>100.0</td>
      <td>96.222875</td>
    </tr>
  </tbody>
</table>
</div>




```python
# source data for school summary - table two
merge_df = pd.merge(schools_df, students_df, on="School Name", how="outer")
merge_df.head()

# define School Name 
# define School Type
# define Total School Budget
# define Per Student Budget
# define Average Math Score
# define Average Reading Score
# define % Passing Math
# define % Passing Reading
# define Overall Passing Rate

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>School Name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>Student Name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
# source data for school summary - table two
# define Total Students
stu_count = merge_df["School Name"].value_counts()
#stu_count
```


```python
stu_count_df = pd.DataFrame(stu_count)
#stu_count_df.rename(columns={"budget":"misc"}, inplace=True)
#stu_count_df
```


```python
# define Average Math Score
school_mpass = merge_df.groupby("School Name")["math_score"].mean()
school_mpass
```




    School Name
    Bailey High School       77.048432
    Cabrera High School      83.061895
    Figueroa High School     76.711767
    Ford High School         77.102592
    Griffin High School      83.351499
    Hernandez High School    77.289752
    Holden High School       83.803279
    Huang High School        76.629414
    Johnson High School      77.072464
    Pena High School         83.839917
    Rodriguez High School    76.842711
    Shelton High School      83.359455
    Thomas High School       83.418349
    Wilson High School       83.274201
    Wright High School       83.682222
    Name: math_score, dtype: float64




```python
school_mpass_df = pd.DataFrame(school_mpass)
#school_mpass_df.head()
```


```python
# define Average Reading Score
school_rpass = merge_df.groupby("School Name")["reading_score"].mean()
#school_rpass
```


```python
school_rpass_df = pd.DataFrame(school_rpass)
#school_rpass_df.head()
```


```python
# define type
school_type = schools_df.groupby("School Name")["type"].describe()
```


```python
school_type_df = pd.DataFrame(school_type)
#school_type_df
```


```python
school_type_df2 = school_type_df.drop(["count", "unique", "freq"], axis=1)
school_type_df2.rename(columns={"top":"type"}, inplace=True)
#school_type_df2
```


```python
# school budget
school_budget = schools_df.groupby("School Name")["budget"].describe()
```


```python
school_budget_df = pd.DataFrame(school_budget)
#school_budget_df
```


```python
school_budget_df2 = school_budget_df.drop(["count", "mean", "std", "min", "25%", "50%", "75%"], axis=1)
school_budget_df2.rename(columns={"max":"budget"}, inplace=True)
#school_budget_df2["budget"] = school_budget_df2["budget"].map("${:.2f}".format)
#school_budget_df2
```


```python
# number of students
school_stu = schools_df.groupby("School Name")["size"].describe()
```


```python
school_stu_df = pd.DataFrame(school_stu)
school_stu_df
school_stu_df2 = school_stu_df.drop(["count", "mean", "std", "min", "25%", "50%", "75%"], axis=1)
school_stu_df2.rename(columns={"max":"# Students"}, inplace=True)
#school_stu_df2
```


```python
# seperate out only passing math scores
each_mpass = merge_df.loc[merge_df["math_score"]>=60]
#each_mpass.head()
#per_mpass = (mpass["math_score"]/total_students)*100
#per_mpass
```


```python
each_mpass = each_mpass.groupby("School Name").count()
each_mpass = each_mpass.drop(["School ID", "type", "size", "budget", "Student ID", "Student Name", 
                              "gender", "grade", "reading_score"], axis=1)
#each_mpass
```


```python
#each_mpass = (each_mpass["math_score"]/total_students)*100

merge_each_mpass = each_mpass.join(school_stu_df2["# Students"], how="left", sort=False)
#merge_each_mpass
```


```python
merge_each_mpass["% Pass Math"] = ((merge_each_mpass["math_score"])/(merge_each_mpass["# Students"]))*100
#merge_each_mpass
```


```python
# seperate out only passing reading scores
each_rpass = merge_df.loc[merge_df["reading_score"]>=60]
#each_rpass.head()
```


```python
each_rpass = each_rpass.groupby("School Name").count()
each_rpass = each_rpass.drop(["School ID", "type", "size", "budget", "Student ID", "Student Name", 
                              "gender", "grade", "math_score"], axis=1)
#each_rpass
```


```python
merge_each_rpass = each_rpass.join(school_stu_df2["# Students"], how="left", sort=False)
merge_each_rpass["% Pass Reading"] = ((merge_each_rpass["reading_score"])/(merge_each_rpass["# Students"]))*100

#merge_each_rpass
```


```python
school_pass = merge_each_rpass.join(merge_each_mpass["math_score"], how="left", sort=False)
#school_pass
```


```python
school_pass["% Pass Math"] = ((school_pass["math_score"])/(school_pass["# Students"]))*100
#school_pass
```


```python
school_pass["Pass Rate"] = ((school_pass["% Pass Reading"])+(school_pass["% Pass Math"]))/2
#school_pass
```


```python
school_summary = school_pass.drop(["reading_score", "math_score"], axis=1)
#school_summary
```


```python
school_summary = school_summary.join(school_type_df2["type"], how="left", sort=False)
#school_summary
```


```python
school_summary = school_summary.join(school_budget_df2["budget"], how="left", sort=False)
#school_summary
```


```python
school_summary["Per Stu Budget"] = ((school_summary["budget"])/(school_summary["# Students"]))
#school_summary
```


```python
school_summary = school_summary.join(school_rpass_df["reading_score"], how="left", sort=False)
#school_summary
```


```python
school_summary = school_summary.join(school_mpass_df["math_score"], how="left", sort=False)
#school_summary
```


```python
# Final School Summary - table two
school_summary.rename(columns={"budget":"Budget", "type":"School Type", 
                               "math_score":"Avg Stu Math Score", 
                               "reading_score":"Avg Stu Reading Score"}, inplace=True)

school_summary = school_summary[["School Type", "# Students", "Budget", "Per Stu Budget", "Avg Stu Math Score", 
                              "% Pass Math", "Avg Stu Reading Score", "% Pass Reading", "Pass Rate"]]

# format student and budget columns
school_summary["Budget"] = school_summary["Budget"].map("${:.2f}".format)
#school_summary["Per Stu Budget"] = school_summary["Per Stu Budget"].map("${:.2f}".format)

school_summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th># Students</th>
      <th>Budget</th>
      <th>Per Stu Budget</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Name</th>
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
      <td>4976.0</td>
      <td>$3124928.00</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>89.529743</td>
      <td>81.033963</td>
      <td>100.0</td>
      <td>94.764871</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858.0</td>
      <td>$1081356.00</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>100.000000</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949.0</td>
      <td>$1884411.00</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>88.436758</td>
      <td>81.158020</td>
      <td>100.0</td>
      <td>94.218379</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739.0</td>
      <td>$1763916.00</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>89.302665</td>
      <td>80.746258</td>
      <td>100.0</td>
      <td>94.651333</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468.0</td>
      <td>$917500.00</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>100.000000</td>
      <td>83.816757</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635.0</td>
      <td>$3022020.00</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>89.083064</td>
      <td>80.934412</td>
      <td>100.0</td>
      <td>94.541532</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427.0</td>
      <td>$248087.00</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>100.000000</td>
      <td>83.814988</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917.0</td>
      <td>$1910635.00</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>88.858416</td>
      <td>81.182722</td>
      <td>100.0</td>
      <td>94.429208</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761.0</td>
      <td>$3094650.00</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>89.182945</td>
      <td>80.966394</td>
      <td>100.0</td>
      <td>94.591472</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962.0</td>
      <td>$585858.00</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>100.000000</td>
      <td>84.044699</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999.0</td>
      <td>$2547363.00</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>88.547137</td>
      <td>80.744686</td>
      <td>100.0</td>
      <td>94.273568</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761.0</td>
      <td>$1056600.00</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>100.000000</td>
      <td>83.725724</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635.0</td>
      <td>$1043130.00</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>100.000000</td>
      <td>83.848930</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283.0</td>
      <td>$1319574.00</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>100.000000</td>
      <td>83.989488</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800.0</td>
      <td>$1049400.00</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>100.000000</td>
      <td>83.955000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
school_summary.dtypes
```




    School Type               object
    # Students               float64
    Budget                    object
    Per Stu Budget           float64
    Avg Stu Math Score       float64
    % Pass Math              float64
    Avg Stu Reading Score    float64
    % Pass Reading           float64
    Pass Rate                float64
    dtype: object




```python
# Top Performing Schools (pass rate) - table three
top_school_summary = school_summary.sort_values(["Pass Rate","Avg Stu Math Score"], ascending=False)
top_school_summary.head()
# 6 schools have 100% pass rate, so Avg Stu Math Score is used as secondary condition
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th># Students</th>
      <th>Budget</th>
      <th>Per Stu Budget</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Name</th>
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
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962.0</td>
      <td>$585858.00</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>100.0</td>
      <td>84.044699</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427.0</td>
      <td>$248087.00</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>100.0</td>
      <td>83.814988</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800.0</td>
      <td>$1049400.00</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>100.0</td>
      <td>83.955000</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635.0</td>
      <td>$1043130.00</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>100.0</td>
      <td>83.848930</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761.0</td>
      <td>$1056600.00</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>100.0</td>
      <td>83.725724</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Bottom Performing Schools (pass rate) - table four
bottom_school_summary = school_summary.sort_values(["Pass Rate","Avg Stu Math Score"], ascending=True)
bottom_school_summary.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th># Students</th>
      <th>Budget</th>
      <th>Per Stu Budget</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Name</th>
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
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949.0</td>
      <td>$1884411.00</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>88.436758</td>
      <td>81.158020</td>
      <td>100.0</td>
      <td>94.218379</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999.0</td>
      <td>$2547363.00</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>88.547137</td>
      <td>80.744686</td>
      <td>100.0</td>
      <td>94.273568</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917.0</td>
      <td>$1910635.00</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>88.858416</td>
      <td>81.182722</td>
      <td>100.0</td>
      <td>94.429208</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635.0</td>
      <td>$3022020.00</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>89.083064</td>
      <td>80.934412</td>
      <td>100.0</td>
      <td>94.541532</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761.0</td>
      <td>$3094650.00</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>89.182945</td>
      <td>80.966394</td>
      <td>100.0</td>
      <td>94.591472</td>
    </tr>
  </tbody>
</table>
</div>




```python
# start scores by grade level by school
# drop excess columns from merge_df
merge_grades = merge_df.drop(["type", "size", "budget", "Student ID", "Student Name", "gender", 
                              "School ID"], axis=1)
#merge_grades.head()
```


```python
nine_df = merge_grades.loc[merge_grades["grade"] == "9th",:]

nine_df2 = nine_df.groupby("School Name").describe()

#nine_df2.head()
```


```python
#nine_df3 = nine_df2.drop(["count", "std", "min", "25%", "50%", "75%", 
#                              "max"], axis=1)
nine_df3 = nine_df2.iloc[:,[1, 9]]
#nine_df3.head()

```


```python
nine_df3["9th Math Avg"] = nine_df3[("math_score", "mean")]
nine_df3["9th Reading Avg"] = nine_df3[("reading_score", "mean")]
#nine_df3.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app
    


```python
nine_df4 = nine_df3.iloc[:,[2, 3]]
#nine_df4.head()
```


```python
ten_df = merge_grades.loc[merge_grades["grade"] == "10th",:]

ten_df2 = ten_df.groupby("School Name").describe()

#ten_df2.head()
```


```python
ten_df3 = ten_df2.iloc[:,[1, 9]]
#ten_df3.head()

```


```python
ten_df3["10th Math Avg"] = ten_df3[("math_score", "mean")]
ten_df3["10th Reading Avg"] = ten_df3[("reading_score", "mean")]
#ten_df3.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app
    


```python
ten_df4 = ten_df3.iloc[:,[2, 3]]
#ten_df4.head()
```


```python
eleven_df = merge_grades.loc[merge_grades["grade"] == "11th",:]

eleven_df2 = eleven_df.groupby("School Name").describe()

#eleven_df2.head()
```


```python
eleven_df3 = eleven_df2.iloc[:,[1, 9]]
#eleven_df3.head()

```


```python
eleven_df3["11th Math Avg"] = eleven_df3[("math_score", "mean")]
eleven_df3["11th Reading Avg"] = eleven_df3[("reading_score", "mean")]
#eleven_df3.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app
    


```python
eleven_df4 = eleven_df3.iloc[:,[2, 3]]
#eleven_df4.head()
```


```python
twelve_df = merge_grades.loc[merge_grades["grade"] == "12th",:]

twelve_df2 = twelve_df.groupby("School Name").describe()

#twelve_df2.head()
```


```python
twelve_df3 = twelve_df2.iloc[:,[1, 9]]
#twelve_df3.head()

```


```python
twelve_df3["12th Math Avg"] = twelve_df3[("math_score", "mean")]
twelve_df3["12th Reading Avg"] = twelve_df3[("reading_score", "mean")]
#twelve_df3.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app
    


```python
twelve_df4 = twelve_df3.iloc[:,[2, 3]]
#twelve_df4.head()
```


```python
grade_math_school = nine_df3.iloc[:,[2]]
#grade_math_school.head()
```


```python
grade_math_school = grade_math_school.join(ten_df3["10th Math Avg"], how="left", sort=False)
#grade_math_school.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\pandas\core\reshape\merge.py:551: UserWarning: merging between different levels can give an unintended result (2 levels on the left, 1 on the right)
      warnings.warn(msg, UserWarning)
    


```python
grade_math_school = grade_math_school.join(eleven_df3["11th Math Avg"], how="left", sort=False)
#grade_math_school.head()
```


```python
grade_math_school = grade_math_school.join(twelve_df3["12th Math Avg"], how="left", sort=False)
#grade_math_school.head()
```


```python
# rename 9th to match format
# final Math per Grade by School - table five
grade_math_school.rename(columns={('9th Math Avg', ''):'9th Math Avg'}, inplace=True)
grade_math_school.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th Math Avg</th>
      <th>10th Math Avg</th>
      <th>11th Math Avg</th>
      <th>12th Math Avg</th>
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
  </tbody>
</table>
</div>




```python
grade_reading_school = nine_df3.iloc[:,[3]]
#grade_reading_school.head()
```


```python
grade_reading_school = grade_reading_school.join(ten_df3["10th Reading Avg"], how="left", sort=False)
#grade_reading_school.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\pandas\core\reshape\merge.py:551: UserWarning: merging between different levels can give an unintended result (2 levels on the left, 1 on the right)
      warnings.warn(msg, UserWarning)
    


```python
grade_reading_school = grade_reading_school.join(eleven_df3["11th Reading Avg"], how="left", sort=False)
#grade_reading_school.head()
```


```python
grade_reading_school = grade_reading_school.join(twelve_df3["12th Reading Avg"], how="left", sort=False)
#grade_reading_school.head()
```


```python
list(grade_reading_school)
```




    [('9th Reading Avg', ''),
     '10th Reading Avg',
     '11th Reading Avg',
     '12th Reading Avg']




```python
# rename 9th to match format
# final Reading per Grade by School - table six
grade_reading_school.rename(columns={('9th Reading Avg', ''):'9th Reading Avg'}, inplace=True)
grade_reading_school.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th Reading Avg</th>
      <th>10th Reading Avg</th>
      <th>11th Reading Avg</th>
      <th>12th Reading Avg</th>
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
  </tbody>
</table>
</div>




```python
# create scores (pass rate) by school spending per stu
school_perstu = school_summary.iloc[:,[3,4,5,6,7,8]]
school_perstu.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Per Stu Budget</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Name</th>
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
      <td>628.0</td>
      <td>77.048432</td>
      <td>89.529743</td>
      <td>81.033963</td>
      <td>100.0</td>
      <td>94.764871</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>582.0</td>
      <td>83.061895</td>
      <td>100.000000</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>639.0</td>
      <td>76.711767</td>
      <td>88.436758</td>
      <td>81.158020</td>
      <td>100.0</td>
      <td>94.218379</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>644.0</td>
      <td>77.102592</td>
      <td>89.302665</td>
      <td>80.746258</td>
      <td>100.0</td>
      <td>94.651333</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>625.0</td>
      <td>83.351499</td>
      <td>100.000000</td>
      <td>83.816757</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#school_perstu["Per Stu Budget"] = pd.to_numeric(school_perstu["Per Stu Budget"])
school_perstu.dtypes
```




    Per Stu Budget           float64
    Avg Stu Math Score       float64
    % Pass Math              float64
    Avg Stu Reading Score    float64
    % Pass Reading           float64
    Pass Rate                float64
    dtype: object




```python
# create bin and bin names
bins = [0, 585, 615, 645, 675]

group_names = ["<$585", "$585-$615", "$615-$645", "$645-$675"]
```


```python
pd.cut(school_perstu["Per Stu Budget"], bins, labels=group_names)
```




    School Name
    Bailey High School       $615-$645
    Cabrera High School          <$585
    Figueroa High School     $615-$645
    Ford High School         $615-$645
    Griffin High School      $615-$645
    Hernandez High School    $645-$675
    Holden High School           <$585
    Huang High School        $645-$675
    Johnson High School      $645-$675
    Pena High School         $585-$615
    Rodriguez High School    $615-$645
    Shelton High School      $585-$615
    Thomas High School       $615-$645
    Wilson High School           <$585
    Wright High School           <$585
    Name: Per Stu Budget, dtype: category
    Categories (4, object): [<$585 < $585-$615 < $615-$645 < $645-$675]




```python
school_perstu["Est Per Stu Spending"] = pd.cut(school_perstu["Per Stu Budget"], bins, labels=group_names)
#school_perstu.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    


```python
# Score summary based on school per student budget - table seven
school_spending = school_perstu.groupby("Est Per Stu Spending")
school_spending.mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Per Stu Budget</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>Est Per Stu Spending</th>
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
      <th>&lt;$585</th>
      <td>581.000000</td>
      <td>83.455399</td>
      <td>100.000000</td>
      <td>83.933814</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>$585-$615</th>
      <td>604.500000</td>
      <td>83.599686</td>
      <td>100.000000</td>
      <td>83.885211</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>$615-$645</th>
      <td>635.166667</td>
      <td>79.079225</td>
      <td>92.636050</td>
      <td>81.891436</td>
      <td>100.0</td>
      <td>96.318025</td>
    </tr>
    <tr>
      <th>$645-$675</th>
      <td>652.333333</td>
      <td>76.997210</td>
      <td>89.041475</td>
      <td>81.027843</td>
      <td>100.0</td>
      <td>94.520737</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create scores (pass rate) by school size
school_size = school_summary.iloc[:,[1, 4, 5, 6, 7, 8]]
school_size.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th># Students</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Name</th>
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
      <td>4976.0</td>
      <td>77.048432</td>
      <td>89.529743</td>
      <td>81.033963</td>
      <td>100.0</td>
      <td>94.764871</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>1858.0</td>
      <td>83.061895</td>
      <td>100.000000</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>2949.0</td>
      <td>76.711767</td>
      <td>88.436758</td>
      <td>81.158020</td>
      <td>100.0</td>
      <td>94.218379</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>2739.0</td>
      <td>77.102592</td>
      <td>89.302665</td>
      <td>80.746258</td>
      <td>100.0</td>
      <td>94.651333</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>1468.0</td>
      <td>83.351499</td>
      <td>100.000000</td>
      <td>83.816757</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create bin and bin names
bins2 = [0, 1000, 2000, 5000]

group_names2 = ["Small(<1000 stu)", "Medium (1000-2000 stu)", "Large (2000-5000 stu)"]
```


```python
pd.cut(school_size["# Students"], bins2, labels=group_names2)
```




    School Name
    Bailey High School        Large (2000-5000 stu)
    Cabrera High School      Medium (1000-2000 stu)
    Figueroa High School      Large (2000-5000 stu)
    Ford High School          Large (2000-5000 stu)
    Griffin High School      Medium (1000-2000 stu)
    Hernandez High School     Large (2000-5000 stu)
    Holden High School             Small(<1000 stu)
    Huang High School         Large (2000-5000 stu)
    Johnson High School       Large (2000-5000 stu)
    Pena High School               Small(<1000 stu)
    Rodriguez High School     Large (2000-5000 stu)
    Shelton High School      Medium (1000-2000 stu)
    Thomas High School       Medium (1000-2000 stu)
    Wilson High School        Large (2000-5000 stu)
    Wright High School       Medium (1000-2000 stu)
    Name: # Students, dtype: category
    Categories (3, object): [Small(<1000 stu) < Medium (1000-2000 stu) < Large (2000-5000 stu)]




```python
school_size["School Size"] = pd.cut(school_size["# Students"], bins2, labels=group_names2)
#school_size.head()
```

    C:\Users\brbal\AppData\Local\conda\conda\envs\PythonData\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    


```python
# Score summary based on school size - table eight
school_size_df = school_size.groupby("School Size")
school_size_df.mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th># Students</th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
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
      <th>Small(&lt;1000 stu)</th>
      <td>694.500</td>
      <td>83.821598</td>
      <td>100.000000</td>
      <td>83.929843</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Medium (1000-2000 stu)</th>
      <td>1704.400</td>
      <td>83.374684</td>
      <td>100.000000</td>
      <td>83.864438</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Large (2000-5000 stu)</th>
      <td>3657.375</td>
      <td>77.746417</td>
      <td>90.367591</td>
      <td>81.344493</td>
      <td>100.0</td>
      <td>95.183795</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create scores (pass rate) by school type
school_type = school_summary.iloc[:,[0, 4, 5, 6, 7, 8]]
#school_type.head()
```


```python
# Score summary based on school type - table eight
school_type_df = school_type.groupby("School Type")
school_type_df.mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg Stu Math Score</th>
      <th>% Pass Math</th>
      <th>Avg Stu Reading Score</th>
      <th>% Pass Reading</th>
      <th>Pass Rate</th>
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
      <td>100.000000</td>
      <td>83.896421</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>88.991533</td>
      <td>80.966636</td>
      <td>100.0</td>
      <td>94.495766</td>
    </tr>
  </tbody>
</table>
</div>


