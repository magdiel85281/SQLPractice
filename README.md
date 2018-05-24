# SQLPractice
SQL Practice for Galvanize
Simple Queries on a Single Table
*****************************
1. Use the WHERE clause to show the countries with a flag ratio of 2:3 (w_prop = 2 and l_prop = 3).

SELECT country FROM flags WHERE w_prop = 2 AND l_prop = 3;

*******************************
2. Use IN to check if an item is in a list and show the countries on a continent that is either Europe
or North America.

SELECT country FROM countries WHERE continent IN ("Europe", "North America");

**********************************
3. Use BETWEEN xxx AND xxx to show names of flags and countries that have width proportion
higher than 1 but lower than 8.


SELECT name FROM flags WHERE w_prop BETWEEN 1 AND 8;

***********************************
4. Use LIKE 'X%' to show countries that have an name that starts with ‘U’.

SELECT country FROM countries WHERE country LIKE "U%";

*************************************
5. Use CASE to show countries, their capital and a column to indicate whether the continent is
‘Eurasia’ (i.e. Europe or Asia) or ‘Americas’ (North or South America). Add a filter to select
countries with capitals that are at least 7 character long.

#Without filter

SELECT country, capital, 
  CASE WHEN continent = "Europe" THEN "Eurasia"
    WHEN continent = "Asia" THEN "Eurasia"
    WHEN continent = "North America" THEN "Americas"
    WHEN continent = "South America" THEN "Americas"
  ELSE NULL END AS quartersphere
  FROM countries;
  
#With Filter

SELECT country, capital, 
  CASE WHEN continent = "Europe" THEN "Eurasia"
    WHEN continent = "Asia" THEN "Eurasia"
    WHEN continent = "North America" THEN "Americas"
    WHEN continent = "South America" THEN "Americas"
  ELSE NULL END AS quartersphere
  FROM countries
  WHERE LENGTH(capital) >= 7;

***********
************
Build Queries with Aggregates
Aggregates include commands such as DISTINCT, COUNT, SUM, GROUP BY, HAVING, and ORDER
BY. Try using these commands on the following questions!
******************************
1. Use DISTINCT to list the continents in the countries table (each continent should appear only
once).

SELECT DISTINCT continent FROM countries;


***************************
2. Use COUNT to see how many countries are in Europe.

SELECT COUNT(country) FROM countries WHERE continent = "Europe";

*******************************
3. Use GROUP BY to count how many countries are in each continent, with continents
alphabetically ordered (hint: use ORDER BY).

SELECT continent, COUNT(country) FROM countries
  GROUP BY continent
  ORDER BY continent;

*********************************
4. Use HAVING to determine which continents are represented at least twice in the countries table.

SELECT continent, COUNT(country) FROM countries
  GROUP BY continent
  HAVING COUNT(country) >= 2
  ORDER BY continent;

*****************
*****************
Build Complex Queries on Multiple Tables
**********************************
1. Use JOIN to display the capital, the country, and the flag name.

SELECT countries.capital, countries.country, flags.name
  FROM countries
  LEFT JOIN flags ON countries.country = flags.country;
  
  
************************************
2. Use JOIN and WHERE to display the continents associated to the flags in the flags table when
the flag has a name (i.e. not ‘NA’).

SELECT flags.name, countries.continent
  FROM flags
  LEFT JOIN countries ON countries.country = flags.country
  WHERE flags.name != "NA";

******************************************
3. Use JOIN and HAVING to display continents that have at least 2 countries represented as well as
the average adoption date of the flag (as avg_date).

SELECT countries.continent, COUNT(countries.country), AVG(flags.adoption_date) AS avg_date
  FROM countries
  LEFT JOIN flags ON countries.country = flags.country
  GROUP BY continent
  HAVING COUNT(countries.country) >= 2;
