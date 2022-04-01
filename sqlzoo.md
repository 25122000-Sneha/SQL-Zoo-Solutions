# SQL-Zoo-Solutions
Solutions to questions on (http://sqlzoo.net/wiki/SQL_Tutorial).  

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT Basics:

1.
```sql
  SELECT population FROM world
    WHERE name = 'Germany'
```
2.
```sql
  SELECT name, gdp/population FROM world
    WHERE area > 5000000
```
3.
```sql
  SELECT name, population FROM world
    WHERE name IN ('Ireland','Iceland','Denmark');
```
4.
```sql
  SELECT name, area FROM world
    WHERE area BETWEEN 200000 AND 250000
```
## SELECT from WORLD
1.
```sql
SELECT name, continent, population FROM world
```
2.
```sql
SELECT name FROM world
WHERE population>200000000
```
3.
```sql
SELECT name, gdp/population FROM world
  WHERE population > 200000000
```
4.
```sql
SELECT name, population/1000000 FROM world
  WHERE continent = 'South America'
```
5.
```sql
SELECT name,population FROM world
  WHERE name IN ('France','Germany','Italy')
```
6.
```sql
SELECT name FROM world
  WHERE name LIKE '%United%'
```
7.
```sql
select name, population, area from world
  where population > 250000000 or area > 3000000
```
8.
```sql
SELECT name,population,area 
FROM world
WHERE (area > 3000000 AND population < 250000000) 
OR (area <3000000 AND population > 250000000);
```
9.
```sql
select name, ROUND(population/1000000,2), ROUND(gdp/1000000000,2) from world
  where continent = 'South America'
```
10.
```sql
select name, ROUND(gdp/population,-3) from world
  where gdp > 1000000000000
```

## SELECT FROM nobel
1.
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
 ```
 2.
 ```sql
 SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
   ```
 3.
 ```sql
 SELECT yr, subject
FROM nobel
WHERE winner LIKE 'Albert Einstein';
```
4.
```sql
SELECT winner
FROM nobel
WHERE subject LIKE 'Peace' AND yr>=2000;
```
5.
```sql
SELECT yr, subject, winner 
FROM nobel
WHERE subject LIKE 'Literature' and yr>=1980 and yr<=1989;
```
6.
```sql
SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```
7.
```sql
Select winner FROM nobel
WHERE winner LIKE 'John%';
```
8.
```sql
SELECT yr, subject, winner FROM nobel
WHERE (subject LIKE 'Physics' and yr=1980) OR (subject LIKE 'Chemistry' and yr=1984);
```
9.
```sql
SELECT * FROM nobel
WHERE yr=1980 AND subject NOT IN ('Chemistry', 'Medicine');
```
10.
```sql
SELECT * FROM nobel
WHERE (subject LIKE 'Medicine' AND yr < 1910)
OR (subject LIKE 'Literature' AND yr>=2004);
```
### Harder Questions:
11.
```sql
SELECT yr,subject,winner FROM nobel
WHERE winner LIKE 'Peter Gr_nberg';
```
12.
```sql
SELECT * FROM nobel
WHERE winner LIKE 'Eugene O''Neill';
```
13.
```sql
SELECT winner, yr, subject 
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner;
```
14.
```sql
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY subject IN('Physics', 'Chemistry'),subject,winner;
 ```
 
## Select in Select
1.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
2.
```sql
SELECT name 
FROM world
WHERE gdp/population > 
(SELECT gdp/population FROM world
WHERE name LIKE 'United Kingdom') AND
continent LIKE 'Europe';
```
3.
```sql
SELECT name,continent 
FROM world
WHERE continent IN 
(SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'));
```
4.
```sql
SELECT name, population FROM world
WHERE population > 
(SELECT population FROM world WHERE name LIKE 'Canada') AND 
population < 
(SELECT population FROM world WHERE name LIKE 'Poland');
```
5.
```sql
SELECT name, 
CONCAT(CAST(ROUND((population/(SELECT population FROM world 
WHERE name LIKE 'Germany'))*100, 0) AS INT), '%') "percentage"
FROM world
WHERE continent LIKE 'Europe';
```
6.
```sql
SELECT name FROM world
WHERE gdp >
ALL(SELECT gdp FROM world 
WHERE continent LIKE 'Europe' AND gdp>0);
```
###Correlated Subqueries
7.
```sql
SELECT continent, name,area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
8.
```sql
SELECT continent, name 
FROM world x 
WHERE name <= ALL(SELECT name FROM world y 
WHERE x.continent = y.continent);
```
9.
```sql
select name, continent, population
from world x
where 25000000 >= all(select population from world y where x.continent=y.continent);
```
10.
```sql
select name, continent
from world x
where x.population > all(select 3*y.population from world y
where x.continent = y.continent and x.name!=y.name);
```
