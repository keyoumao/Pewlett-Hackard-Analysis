# Pewlett-Hackard-Analysis

## Challenge

## 1. Introduction
### Technical Analysis Deliverable 1: 
Number of Retiring Employees by Title. We created three new tables, one showing number of [titles] retiring, one showing number of employees with each title, and one showing a list of current employees born between Jan. 1, 1952 and Dec. 31, 1955. New tables were exported as CSVs. 
### Technical Analysis Deliverable 2: 
Mentorship Eligibility. A table containing employees who are eligible for the mentorship program and the CSV containing the data are provided.

## 2. Methods
In your second paragraph, summarize the steps that you took to solve the problem, as well as the challenges that you encountered along the way. This is an excellent spot to provide examples and descriptions of the code that you used.
Include only the most recent titles in the table. 

**The code used for select the most recent titles:**
``` sql
-- Partition the data to show only most recent title per employee
SELECT emp_no,
 first_name,
 last_name,
 title,
 from_date,
 salary
INTO nameyourtable
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

![alt text](https://github.com/keyoumao/Pewlett-Hackard-Analysis/blob/master/EmployeeDB.PNG "Logo Title Text 1")


## 3. Results and Discussion
In your final paragraph, share the results of your analysis and discuss the data that you’ve generated. Have you identified any limitations to the analysis? What next steps would you recommend?


