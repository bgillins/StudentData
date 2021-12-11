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
