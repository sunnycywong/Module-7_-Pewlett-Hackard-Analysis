--Retrieve the emp_no, first_name, and last_name columns from the Employees table.
SELECT
employees.emp_no,
employees.first_name,
employees.last_name
FROM employees;

--Retrieve the title, from_date, and to_date columns from the Titles table.
SELECT
titles.title,
titles.from_date,
titles.to_date
FROM titles;

--Deliverable 1
SELECT employees.emp_no,
     employees.first_name,
	 employees.last_name,
     titles.title,	 
     titles.from_date,
     titles.to_date
--Create a new table using the INTO clause.
INTO retirement_titles_info
FROM employees
--Join both tables on the primary key.
INNER JOIN titles
ON employees.emp_no = titles.emp_no
--Filter the data on the birth_date column to retrieve the employees who were born between 1952 and 1955. 
--Then, order by the employee number.
WHERE employees.birth_date BETWEEN '1952-01-01' AND '1955-12-31'
ORDER BY titles.emp_no;

--Retrieve the employee number, first and last name, and title columns from the Retirement Titles table.
SELECT
retirement_titles_info.emp_no,
retirement_titles_info.first_name,
retirement_titles_info.last_name,
retirement_titles_info.title
FROM retirement_titles_info;

--Use the DISTINCT ON statement to retrieve the first occurrence of the employee number 
--for each set of rows defined by the ON () clause.
SELECT DISTINCT ON (retirement_titles_info.emp_no)
retirement_titles_info.emp_no,
retirement_titles_info.first_name,
retirement_titles_info.last_name,
retirement_titles_info.title
FROM retirement_titles_info;

SELECT * FROM retirement_titles_info;

SELECT DISTINCT ON (retirement_titles_info.emp_no)
retirement_titles_info.emp_no,
retirement_titles_info.first_name,
retirement_titles_info.last_name,
retirement_titles_info.title
--Create a Unique Titles table using the INTO clause.
INTO unique_titles
FROM retirement_titles_info
--Exclude those employees that have already left the company by filtering on to_date 
--to keep only those dates that are equal to '9999-01-01'.
WHERE retirement_titles_info.to_date = '9999-01-01'
--Sort the Unique Titles table in ascending order by the employee number 
--and descending order by the last date (i.e., to_date) of the most recent title.
ORDER BY retirement_titles_info.emp_no ASC,
		 retirement_titles_info.to_date DESC;

SELECT * FROM unique_titles;

--Write another query in the Employee_Database_challenge.sql file 
--to retrieve the number of employees by their most recent job title who are about to retire.

--First, retrieve the number of titles from the Unique Titles table.
--Then, create a Retiring Titles table to hold the required information.
SELECT 
COUNT (unique_titles.title),
unique_titles.title
INTO retiring_titles
FROM unique_titles
--Group the table by title, then sort the count column in descending order.
GROUP BY unique_titles.title
ORDER BY unique_titles.count DESC;

SELECT * FROM retiring_titles;
