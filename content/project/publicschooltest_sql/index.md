
---
author: Tanguy Hodonou
categories:
- SQL
- Data Manipulation
- Programming
- Importing & Cleaning Data
date: "2022-12-14"
draft: false
excerpt: In this project, we will work with a SQL database containing test performance from NYC's public schools. We will look at how performance varies by borough, identify how many schools fail to report information, and find the top ten performing schools across the city!
layout: single
subtitle: sql project
tags:
- sql
title: Analyzing NYC Public School Test Result Scores
---


## 1. Inspecting the data
<p><img src="https://assets.datacamp.com/production/project_1416/img/schoolbus.jpg" alt="New York City schoolbus" height="300px" width="300px"></p>
<p>Photo by <a href="https://unsplash.com/@jannis_lucas">Jannis Lucas</a> on <a href="https://unsplash.com">Unsplash</a>.
<br></p>
<p>Every year, American high school students take SATs, which are standardized tests intended to measure literacy, numeracy, and writing skills. There are three sections - reading, math, and writing, each with a maximum score of 800 points. These tests are extremely important for students and colleges, as they play a pivotal role in the admissions process.</p>
<p>Analyzing the performance of schools is important for a variety of stakeholders, including policy and education professionals, researchers, government, and even parents considering which school their children should attend. </p>
<p>In this notebook, we will take a look at data on SATs across public schools in New York City. Our database contains a single table:</p>
<h1 id="schools"><code>schools</code></h1>
<table>
<thead>
<tr>
<th>column</th>
<th>type</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>school_name</code></td>
<td><code>varchar</code></td>
<td>Name of school</td>
</tr>
<tr>
<td><code>borough</code></td>
<td><code>varchar</code></td>
<td>Borough that the school is located in</td>
</tr>
<tr>
<td><code>building_code</code></td>
<td><code>varchar</code></td>
<td>Code for the building</td>
</tr>
<tr>
<td><code>average_math</code></td>
<td><code>int</code></td>
<td>Average math score for SATs</td>
</tr>
<tr>
<td><code>average_reading</code></td>
<td><code>int</code></td>
<td>Average reading score for SATs</td>
</tr>
<tr>
<td><code>average_writing</code></td>
<td><code>int</code></td>
<td>Average writing score for SATs</td>
</tr>
<tr>
<td><code>percent_tested</code></td>
<td><code>numeric</code></td>
<td>Percentage of students completing SATs</td>
</tr>
</tbody>
</table>
<p>Let's familiarize ourselves with the data by taking a looking at the first few schools!</p>


```sql
%%sql
postgresql:///schools
    
-- Select all columns from the database
-- Display only the first ten rows
SELECT *
FROM schools
LIMIT 10;
```

    10 rows affected.
    




<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>borough</th>
            <th>building_code</th>
            <th>average_math</th>
            <th>average_reading</th>
            <th>average_writing</th>
            <th>percent_tested</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>New Explorations into Science, Technology and Math High School</td>
            <td>Manhattan</td>
            <td>M022</td>
            <td>657</td>
            <td>601</td>
            <td>601</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Essex Street Academy</td>
            <td>Manhattan</td>
            <td>M445</td>
            <td>395</td>
            <td>411</td>
            <td>387</td>
            <td>78.9</td>
        </tr>
        <tr>
            <td>Lower Manhattan Arts Academy</td>
            <td>Manhattan</td>
            <td>M445</td>
            <td>418</td>
            <td>428</td>
            <td>415</td>
            <td>65.1</td>
        </tr>
        <tr>
            <td>High School for Dual Language and Asian Studies</td>
            <td>Manhattan</td>
            <td>M445</td>
            <td>613</td>
            <td>453</td>
            <td>463</td>
            <td>95.9</td>
        </tr>
        <tr>
            <td>Henry Street School for International Studies</td>
            <td>Manhattan</td>
            <td>M056</td>
            <td>410</td>
            <td>406</td>
            <td>381</td>
            <td>59.7</td>
        </tr>
        <tr>
            <td>Bard High School Early College</td>
            <td>Manhattan</td>
            <td>M097</td>
            <td>634</td>
            <td>641</td>
            <td>639</td>
            <td>70.8</td>
        </tr>
        <tr>
            <td>Urban Assembly Academy of Government and Law</td>
            <td>Manhattan</td>
            <td>M445</td>
            <td>389</td>
            <td>395</td>
            <td>381</td>
            <td>80.8</td>
        </tr>
        <tr>
            <td>Marta Valle High School</td>
            <td>Manhattan</td>
            <td>M025</td>
            <td>438</td>
            <td>413</td>
            <td>394</td>
            <td>35.6</td>
        </tr>
        <tr>
            <td>University Neighborhood High School</td>
            <td>Manhattan</td>
            <td>M446</td>
            <td>437</td>
            <td>355</td>
            <td>352</td>
            <td>69.9</td>
        </tr>
        <tr>
            <td>New Design High School</td>
            <td>Manhattan</td>
            <td>M445</td>
            <td>381</td>
            <td>396</td>
            <td>372</td>
            <td>73.7</td>
        </tr>
    </tbody>
</table>



## 2. Finding missing values
<p>It looks like the first school in our database had no data in the <code>percent_tested</code> column! </p>
<p>Let's identify how many schools have missing data for this column, indicating schools that did not report the percentage of students tested. </p>
<p>To understand whether this missing data problem is widespread in New York, we will also calculate the total number of schools in the database.</p>


```sql
%%sql

-- Count rows with percent_tested missing and total number of schools
SELECT COUNT(*) - COUNT(percent_tested) as num_tested_missing, COUNT(*) as num_schools
FROM schools;
```

     * postgresql:///schools
    1 rows affected.
    




<table>
    <thead>
        <tr>
            <th>num_tested_missing</th>
            <th>num_schools</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>20</td>
            <td>375</td>
        </tr>
    </tbody>
</table>



## 3. Schools by building code
<p>There are 20 schools with missing data for <code>percent_tested</code>, which only makes up 5% of all rows in the database.</p>
<p>Now let's turn our attention to how many schools there are. When we displayed the first ten rows of the database, several had the same value in the <code>building_code</code> column, suggesting there are multiple schools based in the same location. Let's find out how many unique school locations exist in our database. </p>


```sql
%%sql

-- Count the number of unique building_code values
SELECT COUNT(DISTINCT(building_code)) AS num_school_buildings
FROM schools; 
```

     * postgresql:///schools
    1 rows affected.
    




<table>
    <thead>
        <tr>
            <th>num_school_buildings</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>233</td>
        </tr>
    </tbody>
</table>



## 4. Best schools for math
<p>Out of 375 schools, only 233 (62%) have a unique <code>building_code</code>! </p>
<p>Now let's start our analysis of school performance. As each school reports individually, we will treat them this way rather than grouping them by <code>building_code</code>. </p>
<p>First, let's find all schools with an average math score of at least 80% (out of 800). </p>


```sql
%%sql

-- Select school and average_math
-- Filter for average_math 640 or higher
-- Display from largest to smallest average_math
SELECT school_name, average_math
FROM schools
WHERE average_math >= 640
ORDER BY average_math DESC;
```

     * postgresql:///schools
    10 rows affected.
    




<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>average_math</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Stuyvesant High School</td>
            <td>754</td>
        </tr>
        <tr>
            <td>Bronx High School of Science</td>
            <td>714</td>
        </tr>
        <tr>
            <td>Staten Island Technical High School</td>
            <td>711</td>
        </tr>
        <tr>
            <td>Queens High School for the Sciences at York College</td>
            <td>701</td>
        </tr>
        <tr>
            <td>High School for Mathematics, Science, and Engineering at City College</td>
            <td>683</td>
        </tr>
        <tr>
            <td>Brooklyn Technical High School</td>
            <td>682</td>
        </tr>
        <tr>
            <td>Townsend Harris High School</td>
            <td>680</td>
        </tr>
        <tr>
            <td>High School of American Studies at Lehman College</td>
            <td>669</td>
        </tr>
        <tr>
            <td>New Explorations into Science, Technology and Math High School</td>
            <td>657</td>
        </tr>
        <tr>
            <td>Eleanor Roosevelt High School</td>
            <td>641</td>
        </tr>
    </tbody>
</table>



## 5. Lowest reading score
<p>Wow, there are only ten public schools in New York City with an average math score of at least 640!</p>
<p>Now let's look at the other end of the spectrum and find the single lowest score for reading. We will only select the score, not the school, to avoid naming and shaming!</p>


```sql
%%sql

-- Find lowest average_reading
SELECT MIN(average_reading) AS lowest_reading
FROM schools;
```

     * postgresql:///schools
    1 rows affected.
    




<table>
    <thead>
        <tr>
            <th>lowest_reading</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>302</td>
        </tr>
    </tbody>
</table>



## 6. Best writing school
<p>The lowest average score for reading across schools in New York City is less than 40% of the total available points! </p>
<p>Now let's find the school with the highest average writing score.</p>


```sql
%%sql

-- Find the top score for average_writing
-- Group the results by school
-- Sort by max_writing in descending order
-- Reduce output to one school
SELECT school_name, MAX(average_writing) AS max_writing
FROM schools
GROUP BY school_name
ORDER BY max_writing DESC
LIMIT 1;
```

     * postgresql:///schools
    1 rows affected.
    




<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>max_writing</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Stuyvesant High School</td>
            <td>693</td>
        </tr>
    </tbody>
</table>



## 7. Top 10 schools
<p>An average writing score of 693 is pretty impressive! </p>
<p>This top writing score was at the same school that got the top math score, Stuyvesant High School. Stuyvesant is widely known as a perennial top school in New York. </p>
<p>What other schools are also excellent across the board? Let's look at scores across reading, writing, and math to find out.</p>


```sql
%%sql

-- Calculate average_sat
-- Group by school_name
-- Sort by average_sat in descending order
-- Display the top ten results
SELECT school_name, SUM(average_math + average_reading + average_writing) AS average_sat
FROM schools
GROUP BY school_name
ORDER BY average_sat DESC
LIMIT 10;
```

     * postgresql:///schools
    10 rows affected.
    




<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>average_sat</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Stuyvesant High School</td>
            <td>2144</td>
        </tr>
        <tr>
            <td>Staten Island Technical High School</td>
            <td>2041</td>
        </tr>
        <tr>
            <td>Bronx High School of Science</td>
            <td>2041</td>
        </tr>
        <tr>
            <td>High School of American Studies at Lehman College</td>
            <td>2013</td>
        </tr>
        <tr>
            <td>Townsend Harris High School</td>
            <td>1981</td>
        </tr>
        <tr>
            <td>Queens High School for the Sciences at York College</td>
            <td>1947</td>
        </tr>
        <tr>
            <td>Bard High School Early College</td>
            <td>1914</td>
        </tr>
        <tr>
            <td>Brooklyn Technical High School</td>
            <td>1896</td>
        </tr>
        <tr>
            <td>Eleanor Roosevelt High School</td>
            <td>1889</td>
        </tr>
        <tr>
            <td>High School for Mathematics, Science, and Engineering at City College</td>
            <td>1889</td>
        </tr>
    </tbody>
</table>



## 8. Ranking boroughs
<p>There are four schools with average SAT scores of over 2000! Now let's analyze performance by New York City borough. </p>
<p>We will build a query that calculates the number of schools and the average SAT score per borough!</p>


```sql
%%sql

-- Select borough and a count of all schools, aliased as num_schools
-- Calculate the sum of average_math, average_reading, and average_writing, divided by a count of all schools, aliased as average_borough_sat
-- Organize results by borough
-- Display by average_borough_sat in descending order
SELECT borough, count(school_name) AS num_schools, SUM(average_math +average_reading+average_writing)/count(school_name) AS average_borough_sat
FROM schools
GROUP BY borough
ORDER BY average_borough_sat DESC;


```

     * postgresql:///schools
    5 rows affected.
    




<table>
    <thead>
        <tr>
            <th>borough</th>
            <th>num_schools</th>
            <th>average_borough_sat</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Staten Island</td>
            <td>10</td>
            <td>1439</td>
        </tr>
        <tr>
            <td>Queens</td>
            <td>69</td>
            <td>1345</td>
        </tr>
        <tr>
            <td>Manhattan</td>
            <td>89</td>
            <td>1340</td>
        </tr>
        <tr>
            <td>Brooklyn</td>
            <td>109</td>
            <td>1230</td>
        </tr>
        <tr>
            <td>Bronx</td>
            <td>98</td>
            <td>1202</td>
        </tr>
    </tbody>
</table>



## 9. Brooklyn numbers
<p>It appears that schools in Staten Island, on average, produce higher scores across all three categories. However, there are only 10 schools in Staten Island, compared to an average of 91 schools in the other four boroughs!</p>
<p>For our final query of the database, let's focus on Brooklyn, which has 109 schools. We wish to find the top five schools for math performance.</p>


```sql
%%sql

-- Select school and average_math
-- Filter for schools in Brooklyn
-- Aggregate on school_name
-- Display results from highest average_math and restrict output to five rows
SELECT school_name, average_math
FROM schools
WHERE borough = 'Brooklyn'
GROUP BY school_name
ORDER BY average_math DESC
LIMIT 5;
```

     * postgresql:///schools
    5 rows affected.
    




<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>average_math</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Brooklyn Technical High School</td>
            <td>682</td>
        </tr>
        <tr>
            <td>Brooklyn Latin School</td>
            <td>625</td>
        </tr>
        <tr>
            <td>Leon M. Goldstein High School for the Sciences</td>
            <td>563</td>
        </tr>
        <tr>
            <td>Millennium Brooklyn High School</td>
            <td>553</td>
        </tr>
        <tr>
            <td>Midwood High School</td>
            <td>550</td>
        </tr>
    </tbody>
</table>


