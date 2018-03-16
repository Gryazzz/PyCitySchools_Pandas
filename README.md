
**PyCity Schools Analysis**


```python
import pandas as pd
```


```python
path1 = 'Resources/schools_complete.csv'
schools = pd.read_csv(path1, encoding='iso-8859-1', low_memory=False)
schools.head()
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
      <th>School ID</th>
      <th>name</th>
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
path2 = 'Resources/students_complete.csv'
students = pd.read_csv(path2, encoding='iso-8859-1', low_memory=False)
students.head()
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



**District Summary**


```python
total_schools = len(schools['School ID'].unique())
total_students = len(students['Student ID'].unique())
total_budget = schools['budget'].sum()

av_math_score = round((students['math_score'].mean()),2)
av_read_score = round((students['reading_score'].mean()),2)

math_pass_df = students.loc[students['math_score'] >= 70]
math_pass = round((len(math_pass_df) / total_students * 100),2)
read_pass_df = students.loc[students['reading_score'] >= 70]
read_pass = round((len(read_pass_df) / total_students * 100),2)
# Below overall passing rate = % of students who passed both math and reading, later it's just an average
ov_pass_df = pd.merge(math_pass_df, read_pass_df, on='Student ID')
ov_pass = round((len(ov_pass_df) / total_students * 100),2)
distr_df = pd.DataFrame({'Total Schools': [total_schools], 'Total Students': [total_students], 'Total Budget':
                         [total_budget], 'Average Math Score': [av_math_score], 'Average Reading Score':
                         [av_read_score], '% Passing Math': [math_pass], '% Passing Reading': [read_pass],
                         '% Overall Passing Rate': [ov_pass]})
distr_df['Total Budget'] = distr_df['Total Budget'].map("$ {:,.2f}".format)
distr_df_answ = distr_df[['Total Schools', 'Total Students', 'Total Budget', 'Average Math Score', 'Average Reading Score',
                         '% Passing Math', '% Passing Reading', '% Overall Passing Rate']]
distr_df_answ
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
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>$ 24,649,428.00</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>74.98</td>
      <td>85.81</td>
      <td>65.17</td>
    </tr>
  </tbody>
</table>
</div>



**School Summary**


```python
# Form first 5 answer columns
school_sum = schools[['name', 'type', 'size', 'budget']]
school_sum = school_sum.assign(Per_Student_Budget = (school_sum['budget'] / school_sum['size']))
school_sum.columns = ['School name', 'School Type', 'Total Students', 'Total School Budget', 'Per Student Budget']

# Form average reading and math scores
students_extr = students[['school', 'reading_score', 'math_score']]
stud_sum = students_extr.groupby(['school'])
stud_sum_df = pd.DataFrame(stud_sum.mean())
stud_sum_df.reset_index(inplace=True)
stud_sum_df.columns = ['School name', 'Average Reading Score', 'Average Math Score']

# Form supplemental columns to calculate % Passing math
students_extr_math = students[['school', 'math_score']]
math_all_df = students_extr_math.loc[students_extr_math['math_score'] >= 70]
math_sum = math_all_df.groupby(['school'])
math_sum_df = pd.DataFrame(math_sum.count())
math_sum_df.reset_index(inplace=True)
math_sum_df.columns = ['School name', 'Passed Math Count']

# Form supplemental columns to calculate % Passing reading
students_extr_read = students[['school', 'reading_score']]
read_all_df = students_extr_read.loc[students_extr_read['reading_score'] >= 70]
read_sum = read_all_df.groupby(['school'])
read_sum_df = pd.DataFrame(read_sum.count())
read_sum_df.reset_index(inplace=True)
read_sum_df.columns = ['School name', 'Passed Read Count']

# Merge everything
new_sum1 = pd.merge(school_sum, stud_sum_df, on='School name')
new_sum2 = pd.merge(new_sum1, math_sum_df, on='School name')
new_sum = pd.merge(new_sum2, read_sum_df, on='School name')
# Calculate all 3 % Passing
new_sum['% Passing Math'] = round((new_sum['Passed Math Count'] / new_sum['Total Students'] * 100),2)
new_sum['% Passing Reading'] = round((new_sum['Passed Read Count'] / new_sum['Total Students'] * 100),2)
new_sum['% Overall Passing Rate'] = (new_sum['% Passing Math'] + new_sum['% Passing Reading'])/2
# Make the answer beautiful
distr_df_new = new_sum[['School name', 'School Type', 'Total Students', 'Total School Budget', 'Per Student Budget',
                     'Average Reading Score', 'Average Math Score','% Passing Reading', '% Passing Math',
                     '% Overall Passing Rate']]
distr_answ = distr_df_new.set_index('School name')
distr_answ['Total School Budget'] = distr_answ['Total School Budget'].map("$ {:,.2f}".format)
distr_answ['Per Student Budget'] = distr_answ['Per Student Budget'].map("$ {:,.2f}".format)
distr_answ
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
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School name</th>
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
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$ 1,910,635.00</td>
      <td>$ 655.00</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>81.32</td>
      <td>65.68</td>
      <td>73.500</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$ 1,884,411.00</td>
      <td>$ 639.00</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>80.74</td>
      <td>65.99</td>
      <td>73.365</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$ 1,056,600.00</td>
      <td>$ 600.00</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>95.85</td>
      <td>93.87</td>
      <td>94.860</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$ 3,022,020.00</td>
      <td>$ 652.00</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>80.86</td>
      <td>66.75</td>
      <td>73.805</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$ 917,500.00</td>
      <td>$ 625.00</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>97.14</td>
      <td>93.39</td>
      <td>95.265</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$ 1,319,574.00</td>
      <td>$ 578.00</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>96.54</td>
      <td>93.87</td>
      <td>95.205</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$ 1,081,356.00</td>
      <td>$ 582.00</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>97.04</td>
      <td>94.13</td>
      <td>95.585</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$ 3,124,928.00</td>
      <td>$ 628.00</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>81.93</td>
      <td>66.68</td>
      <td>74.305</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$ 248,087.00</td>
      <td>$ 581.00</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>96.25</td>
      <td>92.51</td>
      <td>94.380</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$ 585,858.00</td>
      <td>$ 609.00</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>95.95</td>
      <td>94.59</td>
      <td>95.270</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$ 1,049,400.00</td>
      <td>$ 583.00</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>96.61</td>
      <td>93.33</td>
      <td>94.970</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$ 2,547,363.00</td>
      <td>$ 637.00</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>80.22</td>
      <td>66.37</td>
      <td>73.295</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$ 3,094,650.00</td>
      <td>$ 650.00</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>81.22</td>
      <td>66.06</td>
      <td>73.640</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$ 1,763,916.00</td>
      <td>$ 644.00</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>79.30</td>
      <td>68.31</td>
      <td>73.805</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$ 1,043,130.00</td>
      <td>$ 638.00</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>97.31</td>
      <td>93.27</td>
      <td>95.290</td>
    </tr>
  </tbody>
</table>
</div>



**Top Performing Schools (By Passing Rate)**


```python
top_schools = distr_df_new.sort_values('% Overall Passing Rate', ascending=False)
top = top_schools.set_index('School name')
top['Total School Budget'] = top['Total School Budget'].map("$ {:,.2f}".format)
top['Per Student Budget'] = top['Per Student Budget'].map("$ {:,.2f}".format)
top5 = top.head(5)
top5
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
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School name</th>
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
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$ 1,081,356.00</td>
      <td>$ 582.00</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>97.04</td>
      <td>94.13</td>
      <td>95.585</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$ 1,043,130.00</td>
      <td>$ 638.00</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>97.31</td>
      <td>93.27</td>
      <td>95.290</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$ 585,858.00</td>
      <td>$ 609.00</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>95.95</td>
      <td>94.59</td>
      <td>95.270</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$ 917,500.00</td>
      <td>$ 625.00</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>97.14</td>
      <td>93.39</td>
      <td>95.265</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$ 1,319,574.00</td>
      <td>$ 578.00</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>96.54</td>
      <td>93.87</td>
      <td>95.205</td>
    </tr>
  </tbody>
</table>
</div>



**Top Bottom Schools (By Passing Rate)**


```python
bottom_schools = distr_df_new.sort_values('% Overall Passing Rate', ascending=True)
bottom = bottom_schools.set_index('School name')
bottom['Total School Budget'] = bottom['Total School Budget'].map("$ {:,.2f}".format)
bottom['Per Student Budget'] = bottom['Per Student Budget'].map("$ {:,.2f}".format)
bottom5 = bottom.head(5)
bottom5
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
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School name</th>
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
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$ 2,547,363.00</td>
      <td>$ 637.00</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>80.22</td>
      <td>66.37</td>
      <td>73.295</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$ 1,884,411.00</td>
      <td>$ 639.00</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>80.74</td>
      <td>65.99</td>
      <td>73.365</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$ 1,910,635.00</td>
      <td>$ 655.00</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>81.32</td>
      <td>65.68</td>
      <td>73.500</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$ 3,094,650.00</td>
      <td>$ 650.00</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>81.22</td>
      <td>66.06</td>
      <td>73.640</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$ 3,022,020.00</td>
      <td>$ 652.00</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>80.86</td>
      <td>66.75</td>
      <td>73.805</td>
    </tr>
  </tbody>
</table>
</div>



**Math Scores by Grade**


```python
grade_both = students.groupby(['school','grade'])
grade_both_df = pd.DataFrame(grade_both.mean())
grade_both_df.drop('Student ID', axis=1, inplace=True)
grade_both_df.reset_index(inplace=True)
piv_math = grade_both_df.pivot(index='school', columns='grade', values='math_score') #pivot function,very convenient
math_grade = piv_math[['9th', '10th', '11th', '12th']]
math_grade
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
    <tr>
      <th>school</th>
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



**Reading Scores by Grade**


```python
piv_read = grade_both_df.pivot(index='school', columns='grade', values='reading_score')
read_grade = piv_read[['9th', '10th', '11th', '12th']]
read_grade
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
    <tr>
      <th>school</th>
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



**Scores by School Spending**


```python
print(distr_answ['Per Student Budget'].min())
print(distr_answ['Per Student Budget'].max())
```

    $ 578.00
    $ 655.00



```python
bins = [0, 585, 610, 635, 660]
labels_budg = ['<$ 585', '$ 585-610', '$ 610-635', '$ 635-660']
# It's important to use .copy(), so the initial df won't be changed
budg_df = distr_df_new.copy()
budg_df['Spending Ranges (Per Student)'] = pd.cut(budg_df["Per Student Budget"],bins,labels=labels_budg)
budg_df.drop('Per Student Budget', axis=1, inplace=True)
budg_df.drop('Total School Budget', axis=1, inplace=True)
budg_df.drop('Total Students', axis=1, inplace=True)
budg_grouped = budg_df.groupby(['Spending Ranges (Per Student)'])
budg_grouped_df = pd.DataFrame(budg_grouped.mean())
budg_grouped_df
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
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
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
      <th>&lt;$ 585</th>
      <td>83.933814</td>
      <td>83.455399</td>
      <td>96.610000</td>
      <td>93.460000</td>
      <td>95.035000</td>
    </tr>
    <tr>
      <th>$ 585-610</th>
      <td>83.885211</td>
      <td>83.599686</td>
      <td>95.900000</td>
      <td>94.230000</td>
      <td>95.065000</td>
    </tr>
    <tr>
      <th>$ 610-635</th>
      <td>82.425360</td>
      <td>80.199966</td>
      <td>89.535000</td>
      <td>80.035000</td>
      <td>84.785000</td>
    </tr>
    <tr>
      <th>$ 635-660</th>
      <td>81.368774</td>
      <td>77.866721</td>
      <td>82.995714</td>
      <td>70.347143</td>
      <td>76.671429</td>
    </tr>
  </tbody>
</table>
</div>



**Scores by School Size**


```python
print(schools['size'].max())
print(schools['size'].min())
```

    4976
    427



```python
bins = [0, 1000, 3000, 5000]
labels_size = ['Small (<1000)', 'Medium (1000-3000)', 'Large (>3000)']
size_df = distr_df_new.copy()
size_df['School Size'] = pd.cut(size_df['Total Students'],bins,labels=labels_size)
size_df.drop('Per Student Budget', axis=1, inplace=True)
size_df.drop('Total School Budget', axis=1, inplace=True)
size_df.drop('Total Students', axis=1, inplace=True)
size_grouped = size_df.groupby(['School Size'])
size_grouped_df = pd.DataFrame(size_grouped.mean())
size_grouped_df
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
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
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
      <th>Small (&lt;1000)</th>
      <td>83.929843</td>
      <td>83.821598</td>
      <td>96.100000</td>
      <td>93.550000</td>
      <td>94.825000</td>
    </tr>
    <tr>
      <th>Medium (1000-3000)</th>
      <td>82.933187</td>
      <td>81.176821</td>
      <td>91.316667</td>
      <td>84.648889</td>
      <td>87.982778</td>
    </tr>
    <tr>
      <th>Large (&gt;3000)</th>
      <td>80.919864</td>
      <td>77.063340</td>
      <td>81.057500</td>
      <td>66.465000</td>
      <td>73.761250</td>
    </tr>
  </tbody>
</table>
</div>



**Scores by School Type**


```python
school_type = distr_df_new.copy()
type_grouped = school_type.groupby('School Type')
type_grouped_df = pd.DataFrame(type_grouped.mean())
type_grouped_df.drop('Total School Budget', axis=1, inplace=True)
type_grouped_df.drop('Total Students', axis=1, inplace=True)
type_grouped_df['Per Student Budget'] = type_grouped_df['Per Student Budget'].map("$ {:,.2f}".format)
type_grouped_df
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
      <th>Per Student Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
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
      <th>Charter</th>
      <td>$ 599.50</td>
      <td>83.896421</td>
      <td>83.473852</td>
      <td>96.586250</td>
      <td>93.620000</td>
      <td>95.103125</td>
    </tr>
    <tr>
      <th>District</th>
      <td>$ 643.57</td>
      <td>80.966636</td>
      <td>76.956733</td>
      <td>80.798571</td>
      <td>66.548571</td>
      <td>73.673571</td>
    </tr>
  </tbody>
</table>
</div>



**PySchools Trend Analysis**

1. Charter school students are significantly more successful that students from District schools.
2. Meanwhile per student budget in average Charter school is smaller, than in District one, so there is no direct relation between money and quality of education, or probably there is an inefficient spending of a budget.
3. School size alse determines the scores - smaller schools have better passing rates, than large ones.
