# Pewlett-Hackard-Analysis

## Challenge

## 1. Introduction

### Technical Analysis Deliverable 1

Number of Retiring Employees by Title. We created three new tables, one showing number of [titles] retiring, one showing number of employees with each title, and one showing a list of current employees born between Jan. 1, 1952 and Dec. 31, 1955. New tables were exported as CSVs.

### Technical Analysis Deliverable 2

Mentorship Eligibility. A table containing employees who are eligible for the mentorship program and the CSV containing the data are provided.

## 2. Methods

In your second paragraph, summarize the steps that you took to solve the problem, as well as the challenges that you encountered along the way. This is an excellent spot to provide examples and descriptions of the code that you used.
Include only the most recent titles in the table. 

### 2.1 The code used for creating a new table Number of Retiring Employeess by Title

``` sql
SELECT ri.emp_no,
 ri.first_name,
 ri.last_name,
 ti.title,
 ti.from_date,
 s.salary
INTO retirement_title
FROM retirement_info as ri
INNER JOIN titles AS ti
ON (ri.emp_no = ti.emp_no)
INNER JOIN salaries AS s
ON (ti.emp_no = s.emp_no)
```

#### 2.1.1 The code used for number of [titles] retiring

```sql
-- Group by titles: number of [titles] retiring.
SELECT re.emp_no, COUNT(re.title)
INTO number_titles_retiring
FROM retirement_title as re
GROUP BY re.emp_no;
```

#### 2.1.2 The code used for number of employees with each title

``` sql
-- Group by titles: Number of employees with each title.
SELECT re.title, COUNT(re.emp_no)
INTO number_emp_title
FROM retirement_title as re
GROUP BY re.title;
```

#### 2.1.3 The code used for selecting the most recent titles

``` sql
-- Partition the data to show only most recent title per employee
SELECT emp_no,
 first_name,
 last_name,
 title,
 from_date,
 salary
INTO most_recent_title
FROM
 (SELECT emp_no,
 first_name,
 last_name,
 title,
 from_date,
 salary, ROW_NUMBER() OVER
 (PARTITION BY (emp_no)
 ORDER BY from_date DESC) rn
 FROM retirement_title
 ) tmp WHERE rn = 1
ORDER BY emp_no;
```

#### 2.1.4 The code used for a list of current employees born between Jan. 1, 1952 and Dec. 31, 1955

``` sql
SELECT e.emp_no,
 e.first_name,
e.last_name,
 e.gender,
 s.salary,
 de.to_date
INTO emp_info
FROM employees as e
INNER JOIN salaries as s
ON (e.emp_no = s.emp_no)
INNER JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
     AND (e.hire_date BETWEEN '1985-01-01' AND '1988-12-31')
   AND (de.to_date = '9999-01-01');
```

### 2.2 The code used for determining Mentorship Eligibility

``` sql
SELECT e.emp_no,
 e.first_name,
e.last_name,
 ti.title,
 ti.from_date,
 ti.to_date
INTO mentorship_eligibility
FROM employees as e
INNER JOIN titles AS ti
ON (e.emp_no = ti.emp_no)
INNER JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
   AND (ti.to_date = '9999-01-01');
```

![alt text](https://github.com/keyoumao/Pewlett-Hackard-Analysis/blob/master/EmployeeDB.PNG "Logo Title Text 1")

## 3. Results and Discussion

In your final paragraph, share the results of your analysis and discuss the data that you’ve generated. Have you identified any limitations to the analysis? What next steps would you recommend?
