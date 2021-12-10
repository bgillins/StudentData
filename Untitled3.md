```python
import pandas as pd
import numpy as np
import matplotlib as plt 
import seaborn as sns
```


```python
def low_scores(math_class, assignment_column):
    low_scores = math_class[assignment_column].apply(lambda x: (x<7).sum(), axis = 1)
    return low_scores
def missing_assignments(math_class, assignment_column):
    missing_assignment = math_class[assignment_column].apply(lambda x: (x ==0).sum(), axis = 1)
    return missing_assignment
def group_classes_together(*arg):
    group_classes = pd.concat([*arg])
    return group_classes
def strip_math(col):
    col = col.str.strip('%')
    col = col.str.strip('A',)
    col = col.str.strip('B')
    col = col.str.strip('C')
    col = col.str.strip('D')
    col = col.str.strip('F')
    col = col.str.strip('-')
    col = col.str.strip('+')
    col = col.str.strip()
    return col.astype(int)
```

## Import Data
### Import each sheet into its own variable to import class period in next step



```python
df12_08 = r'filename'
```


```python
period_1a = pd.read_excel(df12_08, '1(A) Secondary Mathematics 2')
period_4a = pd.read_excel(df12_08, '4(A) Secondary Mathematics 2')
period_7a = pd.read_excel(df12_08, '7(A) Secondary Mathematics 2')
```
    

## Cleaning Data
### Need to get column names to plug into functions above. Will use the following information to calculuate missing and low score assignments. 


```python

column_names = list(period_1a.columns)
```


```python
print(column_names)
```

    ['Unnamed: 0', 'Q2', '1.1 - Quadratic Sheet', 'Active Recall 1.1', 'Active Recall 1.2', 'Adding Polynomials', 'Subtracting Polynomials', 'Active Recall 1.4', 'Unit 1 Test', 'Chapter 1 Review', 'Active Recall 2.2', 'Chapter 2 Test', 'Chapter 2 Review', 'Active Recall 3.1', 'Active Recall 3.2', 'HW 3.2 Spot Check', 'Active Recall 3.3', 'HW 3.3 Spot Check', 'Chapter 3 Review', 'Chapter Test 3', 'Active Recall 4.2', 'Active Recall 4.1', 'Transversal Practice', 'Active Recall 4.3', 'Active Recall 4.4', 'Active Recall 4.5', 'Chapter 4 Review', 'Chapter 4 Test', 'Active Recall 5.1', 'Assignment 5.1 (Orange)', 'Assignment 5.2 (Blue)', 'Active Recall 5.3', 'Active Recall 5.4', 'Assignment 5.3 (salmon)', 'Assignment 5.4 (salmon)', 'Active Recall 5.5', 'Assignment 5.5 (Pink)', 'Chapter 5 Test', 'Chapter 5 Review', 'Active Recall 6.1', 'Assignment 6.1 (pink)', 'Assignment 6.2 (Orange)', 'Active Recall 6.2', 'Assignment 6.3 (Yellow)', 'Active Recall 6.3', 'Assignment 6.4 (Green)', 'Active Recall 6.4', 'Assignment 6.5 (Blue)']
    


```python
all_assignments = ['1.1 - Quadratic Sheet', 'Active Recall 1.1', 'Active Recall 1.2', 'Adding Polynomials', 'Subtracting Polynomials', 'Active Recall 1.4', 'Chapter 1 Review', 'Active Recall 2.2', 'Chapter 2 Review', 'Active Recall 3.1', 'Active Recall 3.2', 'HW 3.2 Spot Check', 'Active Recall 3.3', 'HW 3.3 Spot Check', 'Chapter 3 Review', 'Active Recall 4.2', 'Active Recall 4.1', 'Transversal Practice', 'Active Recall 4.3', 'Active Recall 4.4', 'Active Recall 4.5', 'Chapter 4 Review', 'Active Recall 5.1', 'Assignment 5.1 (Orange)', 'Assignment 5.2 (Blue)', 'Active Recall 5.3', 'Active Recall 5.4', 'Assignment 5.3 (salmon)', 'Assignment 5.4 (salmon)', 'Active Recall 5.5', 'Assignment 5.5 (Pink)', 'Chapter 5 Review', 'Active Recall 6.1', 'Assignment 6.1 (pink)', 'Assignment 6.2 (Orange)', 'Active Recall 6.2', 'Assignment 6.3 (Yellow)', 'Active Recall 6.3', 'Assignment 6.4 (Green)', 'Active Recall 6.4', 'Assignment 6.5 (Blue)', 'Chapter 6 Test', 'Active Recall 2.3', 'Active Recall 2.3.1']

math_2_assignments =['Active Recall 4.2', 'Active Recall 4.1', 'Transversal Practice', 'Active Recall 4.3', 'Active Recall 4.4', 'Active Recall 4.5', 'Chapter 4 Review', 'Active Recall 5.1', 'Assignment 5.1 (Orange)', 'Assignment 5.2 (Blue)', 'Active Recall 5.3', 'Active Recall 5.4', 'Assignment 5.3 (salmon)', 'Assignment 5.4 (salmon)', 'Active Recall 5.5', 'Assignment 5.5 (Pink)', 'Chapter 5 Review', 'Active Recall 6.1', 'Assignment 6.1 (pink)', 'Assignment 6.2 (Orange)', 'Active Recall 6.2', 'Assignment 6.3 (Yellow)', 'Active Recall 6.3', 'Assignment 6.4 (Green)', 'Active Recall 6.4', 'Assignment 6.5 (Blue)']

math_2_test = ['Unit 1 Test', 'Chapter 2 Test',  'Chapter Test 3', 'Chapter 4 Test',  'Chapter 5 Test']

math_2_current_grade = ['Q2']

student_name = ['Unnamed: 0']
```

## Calculate Student Assignment Grade Average


```python
# Categorizing class periods before concating. 
period_1a['Period'] = 1
period_4a['Period'] = 4
period_7a['Period'] = 7
```


```python
#concat periods together
math_2 = group_classes_together(period_1a, period_4a, period_7a)
```


```python
# Change data type of Q2 Grades
math_2['Q2'] = strip_math(math_2['Q2'])
```


```python
#calculate periods together
math_2['Low Scores_3'] = low_scores(math_2, math_2_assignments)
math_2['Missing Assignments_3'] = missing_assignments(math_2, math_2_assignments)
```

    NumExpr defaulting to 8 threads.
    


```python
#drop all assignments
math_2.drop(all_assignments, axis=1, inplace=True)
```


```python
#rename column to Student Name
math_2.rename(columns={'Unnamed: 0': 'Student Name'}, inplace=True)
```

## Load Additional files
### Calulate missing and low scoring assignments. 


```python
df11_30= r'filename_2' 
period_1a_old = pd.read_excel(df11_30, '1(A) Secondary Mathematics 2')
period_4a_old = pd.read_excel(df11_30, '4(A) Secondary Mathematics 2')
period_7a_old = pd.read_excel(df11_30, '7(A) Secondary Mathematics 2')
```

    C:\Users\brand\anaconda3\lib\site-packages\openpyxl\styles\stylesheet.py:221: UserWarning: Workbook contains no default style, apply openpyxl's default
      warn("Workbook contains no default style, apply openpyxl's default")
    


```python
column_names_11_30 = list(period_1a_old.columns)
```


```python
print(column_names_11_30)
```

    ['Unnamed: 0', 'Q2', '1.1 - Quadratic Sheet', 'Active Recall 1.1', 'Active Recall 1.2', 'Adding Polynomials', 'Subtracting Polynomials', 'Active Recall 1.4', 'Unit 1 Test', 'Chapter 1 Review', 'Active Recall 2.2', 'Chapter 2 Test', 'Chapter 2 Review', 'Active Recall 3.1', 'Active Recall 3.2', 'HW 3.2 Spot Check', 'Active Recall 3.3', 'HW 3.3 Spot Check', 'Chapter 3 Review', 'Chapter Test 3', 'Active Recall 4.2', 'Active Recall 4.1', 'Transversal Practice', 'Active Recall 4.3', 'Active Recall 4.4', 'Active Recall 4.5', 'Chapter 4 Review', 'Chapter 4 Test', 'Active Recall 5.1', 'Assignment 5.1 (Orange)', 'Assignment 5.2 (Blue)', 'Active Recall 5.3', 'Active Recall 5.4', 'Assignment 5.3 (salmon)', 'Assignment 5.4 (salmon)', 'Active Recall 5.5', 'Assignment 5.5 (Pink)', 'Chapter 5 Test', 'Chapter 5 Review', 'Active Recall 6.1', 'Assignment 6.1 (pink)', 'Assignment 6.2 (Orange)', 'Active Recall 6.2', 'Assignment 6.3 (Yellow)', 'Active Recall 6.3', 'Assignment 6.4 (Green)', 'Active Recall 6.4', 'Assignment 6.5 (Blue)']
    


```python
math_2_assignments_old = ['Active Recall 4.2', 'Active Recall 4.1', 'Transversal Practice', 'Active Recall 4.3', 'Active Recall 4.4', 'Active Recall 4.5', 'Chapter 4 Review', 'Active Recall 5.1', 'Assignment 5.1 (Orange)', 'Assignment 5.2 (Blue)', 'Active Recall 5.3', 'Active Recall 5.4', 'Assignment 5.3 (salmon)', 'Assignment 5.4 (salmon)', 'Active Recall 5.5', 'Assignment 5.5 (Pink)', 'Chapter 5 Review', 'Active Recall 6.1', 'Assignment 6.1 (pink)', 'Assignment 6.2 (Orange)', 'Active Recall 6.2', 'Assignment 6.3 (Yellow)', 'Active Recall 6.3', 'Assignment 6.4 (Green)', 'Active Recall 6.4', 'Assignment 6.5 (Blue)']

math_2_old_grade = ['Q2']
```


```python
# Categorizing class periods before concating. 
period_1a_old['Period'] = 1
period_4a_old['Period'] = 4
period_7a_old['Period'] = 7
```


```python
#concat periods together
math_2_old = group_classes_together(period_1a_old, period_4a_old, period_7a_old)
```


```python

```

## Import CSV 
### File was prepared on my tablet and saved as a CSV.


```python
csv = r'cbs_filename'
grades_csv = pd.read_csv(csv)
```

# Merge Documents 


```python
#calculate periods together
math_2['Low Scores_2'] = low_scores(math_2_old, math_2_assignments_old)
math_2['Missing Assignments_2'] = missing_assignments(math_2_old, math_2_assignments_old)
math_2['Q2 11.30'] =strip_math(math_2_old['Q2'])
```


```python
#calculate periods together
math_2['Low Scores'] = grades_csv['Low Scores']
math_2['Missing Assignments'] = grades_csv['Missing Assignments']
math_2['Missing Assignments_2'] = missing_assignments(math_2_old, math_2_assignments_old)
math_2['Q2 11.15'] =strip_math(grades_csv['Q2'])
```


```python
math_2
```




<div><div id=fbf26eed-ae74-4572-9abd-9873099a1911 style="display:none; background-color:#9D6CFF; color:white; width:200px; height:30px; padding-left:5px; border-radius:4px; flex-direction:row; justify-content:space-around; align-items:center;" onmouseover="this.style.backgroundColor='#BA9BF8'" onmouseout="this.style.backgroundColor='#9D6CFF'" onclick="window.commands?.execute('create-mitosheet-from-dataframe-output');">See Full Dataframe in Mito</div> <script> if (window.commands.hasCommand('create-mitosheet-from-dataframe-output')) document.getElementById('fbf26eed-ae74-4572-9abd-9873099a1911').style.display = 'flex' </script> <table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student Name</th>
      <th>Q2</th>
      <th>Unit 1 Test</th>
      <th>Chapter 2 Test</th>
      <th>Chapter Test 3</th>
      <th>Chapter 4 Test</th>
      <th>Chapter 5 Test</th>
      <th>Period</th>
      <th>Low Scores_3</th>
      <th>Missing Assignments_3</th>
      <th>Low Scores_2</th>
      <th>Missing Assignments_2</th>
      <th>Q2 11.30</th>
      <th>Low Scores</th>
      <th>Missing Assignments</th>
      <th>Q2 11.15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvey, Kayden</td>
      <td>74</td>
      <td>40%</td>
      <td>91.0</td>
      <td>88.0</td>
      <td>93.0</td>
      <td>48.0</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>74</td>
      <td>4</td>
      <td>3</td>
      <td>89</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Anderson, Grant</td>
      <td>99</td>
      <td>100%</td>
      <td>93.0</td>
      <td>101.0</td>
      <td>102.0</td>
      <td>96.0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>99</td>
      <td>1</td>
      <td>1</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Brinkerhoff, Melaina</td>
      <td>6</td>
      <td>95%</td>
      <td>76.0</td>
      <td>74.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>16</td>
      <td>16</td>
      <td>7</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Coonfer, Cash</td>
      <td>91</td>
      <td>82%</td>
      <td>76.0</td>
      <td>29.0</td>
      <td>93.0</td>
      <td>95.0</td>
      <td>1</td>
      <td>6</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>92</td>
      <td>2</td>
      <td>2</td>
      <td>92</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cordova, Richie</td>
      <td>85</td>
      <td>97%</td>
      <td>83.0</td>
      <td>104.0</td>
      <td>96.0</td>
      <td>96.0</td>
      <td>1</td>
      <td>15</td>
      <td>15</td>
      <td>13</td>
      <td>13</td>
      <td>86</td>
      <td>11</td>
      <td>11</td>
      <td>81</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Perkins, Sophia</td>
      <td>84</td>
      <td>89%</td>
      <td>95.0</td>
      <td>90.0</td>
      <td>90.0</td>
      <td>74.0</td>
      <td>7</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>85</td>
      <td>8</td>
      <td>8</td>
      <td>74</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Pincock, Trevor</td>
      <td>72</td>
      <td>76%</td>
      <td>66.0</td>
      <td>96.0</td>
      <td>67.0</td>
      <td>76.0</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>74</td>
      <td>6</td>
      <td>3</td>
      <td>82</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Spencer, McKenna</td>
      <td>88</td>
      <td>89%</td>
      <td>93.0</td>
      <td>79.0</td>
      <td>96.0</td>
      <td>78.0</td>
      <td>7</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>89</td>
      <td>3</td>
      <td>2</td>
      <td>98</td>
    </tr>
    <tr>
      <th>28</th>
      <td>West, Gavin</td>
      <td>78</td>
      <td>70%</td>
      <td>86.0</td>
      <td>62.0</td>
      <td>82.0</td>
      <td>82.0</td>
      <td>7</td>
      <td>11</td>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>80</td>
      <td>4</td>
      <td>4</td>
      <td>94</td>
    </tr>
    <tr>
      <th>29</th>
      <td>White, Brody</td>
      <td>24</td>
      <td>60%</td>
      <td>76.0</td>
      <td>74.0</td>
      <td>36.0</td>
      <td>0.0</td>
      <td>7</td>
      <td>14</td>
      <td>12</td>
      <td>11</td>
      <td>9</td>
      <td>25</td>
      <td>4</td>
      <td>3</td>
      <td>96</td>
    </tr>
  </tbody>
</table></div>




```python
#drop student names due to privacy. 
math_2 = math_2.drop('Student Name', axis =1)
```


```python
math_2.to_excel('Math 2 Grades Report 1210.xlsx')
```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
