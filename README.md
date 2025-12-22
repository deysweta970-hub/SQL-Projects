# SQL-Projects
<a href="https://github.com/deysweta970-hub/SQL-Projects/commit/db7f878f4a10677b73c76099532c7d702e6c116c">SQL</a>
------------------------TABLE 1
CREATE TABLE "BOOKS1"(
"Book_ID" Serial  Primary key,
"Title" VARCHAR ,
"Author" VARCHAR,
"Genre" VARCHAR,
"Published_Year" int,
"Price" Decimal,
"Stock" int
);
SELECT * FROM "BOOKS1";

COPY "BOOKS1" ("Book_ID","Title","Author","Genre","Published_Year","Price",	"Stock"
)
From 'E:\30 Day - SQL Practice Files\BOOK.csv'
delimiter','
csv header;

-------------------TABLE 2
CREATE TABLE "CUSTOMER1"(
    "Customer_ID" Serial PRIMARY KEY,
    "Name" VARCHAR,
    "Email" VARCHAR,
    "Phone" VARCHAR,
    "City" VARCHAR,
    "Country" VARCHAR
);
Select * from "CUSTOMER1";
COPy "CUSTOMER1"("Customer_ID","Name","Email","Phone","City","Country")
from 'E:\30 Day - SQL Practice Files\CUSTOMERSS.csv'
DELIMITER ','
CSV HEADER;

------------------------ TABLE 3
CREATE TABLE "ORDER1"(
"Order_ID" Serial primary key,
"Customer_ID" int references    "CUSTOMER1"("Customer_ID"),
"Book_ID" int references "BOOKS1"("Book_ID"),
"Order_Date" date,
"Quantity" int,
"Total_Amount" decimal);

Select * from "ORDER1"
Copy "ORDER1"("Order_ID","Customer_ID",	"Book_ID",	"Order_Date",	"Quantity",	"Total_Amount")
FROM 'E:\30 Day - SQL Practice Files\ORDERSS.csv'
delimiter','
csv header;

--1 Show all customers from Japan.--
select * from "CUSTOMER1"
where"Country" = 'Japan';
--2 Find the total number of orders.--
SELECT COUNT("Order_ID") AS "Total_Order"
FROM "ORDER1";
-- 3 List all orders placed in April 2024.--
Select * from "ORDER1"
Where "Order_Date"  between '2024.04.01' and '2024.04.30'
-- 4 Show the highest priced product.--
 Select  MAX("Price") as "Highest_Price"
From "BOOKS1"
 -- 5 Count how many customers signed up in 2024.--
 Select distinct "City"
 From "CUSTOMER1"
 --6 Show all orders with customer name and Author name.--
 Select O."Order_ID",C."Name",B."Author"
 From"ORDER1" O
 JOIN "CUSTOMER1" C on O."Customer_ID"=C."Customer_ID"
 JOIN "BOOKS1" B On O."Book_ID"=B."Book_ID"
 --7 Find total sales amount for each customer.--
 Select C."Name", Sum("Total_Amount") As"Total_Sales_Amount"
 From"ORDER1" O
 Join "CUSTOMER1" C On O."Customer_ID"=C."Customer_ID"
 GROUP BY C."Name"
 -- 8 Find total sales amount for each genre.--
 Select B."Genre" , Sum("Total_Amount") As "New"
 From "ORDER1" O
 Join "BOOKS1" B on O."Book_ID"=B."Book_ID"
 Group by B."Genre"
--9 Show customers who have placed at least one order.--
SELECT DISTINCT C."Name"
FROM"CUSTOMER1" C
JOIN "ORDER1" O On O."Customer_ID"=C."Customer_ID"
--10 Find customers who have never placed an order.--
Select C."Name"
 From"CUSTOMER1" C
LEFT JOIN "ORDER1" O On O."Customer_ID"= C."Customer_ID"
 Where "Order_ID" Is Null
--11 Show orders greater than > 300 with customer names.--
 Select C."Name",O."Total_Amount"
 From "ORDER1" O
 Join"CUSTOMER1"C On O."Customer_ID"=C."Customer_ID"
 Where "Total_Amount" >300;
 --12 Get city-wise customer order count including zero.
 select C."City",count(O."Order_ID") as"New_Order"
 From"ORDER1" O
 Left Join "CUSTOMER1"C ON C."Customer_ID"=O."Customer_ID"
 Group by C."City"
-- 13  Rank customer orders by amount.
SELECT C."Name", O."Total_Amount",
RANK() OVER (PARTITION BY C."Name" ORDER BY O."Total_Amount" DESC) AS rnk
FROM  "CUSTOMER1" C
JOIN "ORDER1" O ON C."Customer_ID"=O."Customer_ID";
--14  Author whose name starts with 'S'
Select  *  From"BOOKS1"
Where  "Author" Like 'D%'
-- 15 Highest customer Per City
SELECT "City", "Name",Highest_Amount
FROM (
    SELECT C."City", C."Name",
           SUM(O."Total_Amount") AS Highest_Amount,
           DENSE_RANK() OVER (
               PARTITION BY C."City"
               ORDER BY SUM(O."Total_Amount") DESC
           ) AS rnk
    FROM "CUSTOMER1" C
    JOIN "ORDER1"O ON O."Customer_ID"=C."Customer_ID"
    GROUP BY C."City", C."Name"
) ;
--16 Identify top 3  Country & customers by total spending.
Select C."Name", "Country",Sum(O."Total_Amount") AS "Top_3"
From"CUSTOMER1" C
JOIN "ORDER1" O On O."Customer_ID"=C."Customer_ID"
Group by C."Name",C."Country"
Order by "Top_3" Desc
Limit 3

![Screenshot (1412)](https://github.com/user-attachments/assets/53bf8686-701c-4b8d-8780-8f6e3ec31fa0)
