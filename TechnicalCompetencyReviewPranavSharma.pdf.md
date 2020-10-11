Technical Competnecy Test.                                                                      Page 1
Name - Pranav Sharma 
Email Address - contactpranavs@gmail.com

#Solution to Question 1.1

SELECT DISTINCT name, 
COUNT(*) AS [CountOfCustomers]

FROM region 
INNER JOIN customer_region 
ON region.id = customer_region.region_id 

GROUP BY name;

![](1.jpg)
#Solution to Question 1.2

SELECT name, 
sum(loan_amount) AS "TotalLoanValue" 

FROM
customers c
INNER JOIN
loans l
ON c.id = l.customer_id

WHERE name like '%Will%'

GROUP BY name
ORDER BY TotalLoanValue DESC;


#Solution to Question 1.3

SELECT SUM(NonMatchingRows) AS CountNoLoans
FROM (
  SELECT COUNT(*) AS NonMatchingRows
  FROM customers
  WHERE id NOT IN ( SELECT DISTINCT customer_id FROM loans )
  UNION ALL
  SELECT COUNT(*) AS NonMatchingRows
  FROM loans
  WHERE customer_id NOT IN ( SELECT DISTINCT id FROM customers )
)


#Solution to Question 1.4

SELECT TopDefaultingRegion, max(totaldefault) as CustomerCount, AverageDefault
FROM(
    SELECT count(defaulted) as totaldefault, name AS TopDefaultingRegion, AverageDefault
FROM(
    SELECT DISTINCT l.customer_id, defaulted, region_id, name, round(avg(loan_amount), 2) as AverageDefault
    FROM customer_region c
    INNER JOIN loans l ON c.customer_id = l.customer_id
    INNER JOIN region r ON c.region_id = r.id  
WHERE defaulted = 1
GROUP BY l.customer_id)
GROUP BY name)

