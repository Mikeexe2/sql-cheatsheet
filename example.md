## example solution

--4 tables

--1. country: id, country_name
--2. city: id,ci_name,country_id(refer to country.id)
--3. customer: id, city_id (tefer to city.id)
--4. invoice: id,customer_id (refer to customer.id), total_price

--Write a query, for each country, display the country name, total number of invoices, and their average amount. Format the average as floating point number with 6 decimal palces. Return only those countries where their invoice amount is greater than the average invoice amount over all invoices.

```
SELECT
c.country_name AS Country_Name,
COUNT(DISTINCT i.id) AS Total_invoices_No,
ROUND(AVG(i.total_price), 6) AS Average_Amount
FROM country c
LEFT JOIN city ci ON c.id = ci.country_id
LEFT JOIN customer cu ON ci.id = cu.city_id
LEFT JOIN invoice i ON cu.id = i.customer_id
WHERE i.total_price > (SELECT AVG(total_price) FROM invoice)
GROUP BY c.country_name;
```


--For each pair of city and product, return the names for the city and product, and the total amount spent on the product to 2 decimal places. Order the result by amount spent from high to low then by city name and product name in ascending order.

--5 tables:
--city : id, city_name, country_id
--customer: id, city_id(refer to city.id), 
--invoice: id, customer_id (refer to customer.id)
--invoice_item: id, invoice_id(refer to invoice.id), product_id(refer to product.id), line_total_price
--product: id, product_name

```
SELECT
ci.city_name AS city_name,p.product_name AS product_name
ROUND(SUM(ii.line_total_price), 2) AS total_amount_spent
FROM city ci
JOIN customer cu ON ci.id = cu.city_id
JOIN invoice i ON cu.id = i.customer_id
JOIN invoice_item ii ON i.id = ii.invoice_id
JOIN product p ON ii.product_id = p.id
GROUP BY ci.city_name,p.product_name
ORDER BY total_amount_spent DESC,ci.city_name ASC,p.product_name ASC;
```
