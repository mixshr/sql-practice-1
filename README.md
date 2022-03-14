# SQL PRACTICE

### SQL Practice №1 | The computer store

#### Relational shema
![Relation shema](https://upload.wikimedia.org/wikipedia/commons/b/b2/Computer-store-db.png)

#### The creation code
``` sql
CREATE TABLE Manufacturers (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL 
);

CREATE TABLE Products (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL ,
	Price REAL NOT NULL ,
	Manufacturer INTEGER NOT NULL 
		CONSTRAINT fk_Manufacturers_Code REFERENCES Manufacturers(Code)
);
```
#### Sample datase
``` sql
INSERT INTO Warehouses (Code, Location, Capacity) VALUES (1, 'Chicago', 3);
INSERT INTO Warehouses (Code, Location, Capacity) VALUES (2, 'Chicago', 4);
INSERT INTO Warehouses (Code, Location, Capacity) VALUES (3, 'New York', 7);
INSERT INTO Warehouses (Code, Location, Capacity) VALUES (4, 'Los Angeles', 2);
INSERT INTO Warehouses (Code, Location, Capacity) VALUES (5, 'San Francisco', 8);

INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('0MN7', 'Rocks', 180,3);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4H8P', 'Rocks', 250,1);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4RT3', 'Scissors', 190,4);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('7G3H', 'Rocks', 200,1);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8JN6', 'Papers', 75,1);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8Y6U', 'Papers', 50,3);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('9J6F', 'Papers', 175,2);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('LL08', 'Rocks', 140,4);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P0H6', 'Scissors', 125,1);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P2T6', 'Scissors', 150,2);
INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('TU55', 'Papers', 90,5);
```
#### Exercises
1. Select the names of all the products in the store.
2. Select the names and the prices of all the products in the store.
3. Select the name of the products with a price less than or equal to $200.
4. Select all the products with a price between $60 and $120.
5. elect the name and price in cents (i.e., the price must be multiplied by 100).
6. Compute the average price of all the products.
7. Compute the average price of all products with manufacturer code equal to 2.
8. Compute the number of products with a price larger than or equal to $180.
9. Select the name and price of all products with a price larger than or equal to $180, and sort first by price (in descending order), and then by name (in ascending order).
10. Select all the data from the products, including all the data for each product's manufacturer.
11. Select the product name, price, and manufacturer name of all the products.
12. Select the average price of each manufacturer's products, showing only the manufacturer's code.
13. Select the average price of each manufacturer's products, showing the manufacturer's name.
14. Select the names of manufacturer whose products have an average price larger than or equal to $150.
15. Select the name and price of the cheapest product.
16. Select the name of each manufacturer along with the name and price of its most expensive product.
17. Select the name of each manufacturer which have an average price above $145 and contain at least 2 different products.
18. Add a new product: Loudspeakers, $70, manufacturer 2.
19. Update the name of product 8 to "Laser Printer".
20. Apply a 10% discount to all products.
21. Apply a 10% discount to all products with a price larger than or equal to $120.

``` sql
1. SELECT name FROM products;

2. SELECT name, price FROM products;

3. SELECT name FROM products WHERE price <= 200;

4. SELECT * FROM products WHERE price between 60 AND 120;

5. SELECT name, price * 100 as price FROM products;

6. SELECT AVG(price) AS price FROM products;

7. SELECT AVG(price) AS price FROM products WHERE Manufacturer = 2;

8. SELECT count(*) AS number FROM products WHERE price >= 180;

9. SELECT name, price FROM products WHERE price >= 180 ORDER BY price DESC, name;

10. SELECT * FROM products LEFT JOIN manufacturers m on m.code = products.manufacturer;

11. SELECT p.name, price, m.name as manufacturer FROM products AS p LEFT JOIN manufacturers m on m.code = p.manufacturer;

12. SELECT avg(p.price) AS price, m.name FROM products AS p LEFT JOIN manufacturers m on m.code = p.manufacturer GROUP BY m.name;

13. SELECT AVG(p.price), p.manufacturer FROM products AS p GROUP BY p.manufacturer;

14. SELECT AVG(p.price) AS price, m.name FROM products AS p LEFT JOIN manufacturers m on m.code = p.manufacturer GROUP BY m.name;

15. SELECT name FROM manufacturers AS m JOIN (SELECT manufacturer, avg(price) AS avg_price FROM products GROUP BY manufacturer) AS p ON p.manufacturer = m.code WHERE p.avg_price >= 150;

16. SELECT name, price FROM products WHERE code = (SELECT code FROM products GROUP BY code ORDER BY min(price) limit 1);

17. SELECT A.Name, A.Price, F.Name FROM Products A INNER JOIN Manufacturers F ON A.Manufacturer = F.Code AND A.Price = (SELECT MAX(A.Price) FROM Products A WHERE A.Manufacturer = F.Code);

18. WITH table_avg_price AS (SELECT AVG(price) AS avg_price, count(manufacturer) AS count_products, manufacturer FROM products GROUP BY manufacturer) SELECT * FROM table_avg_price WHERE avg_price > 145 AND count_products >= 2;

19. insert into products(code, name, price, manufacturer) values (11, 'Loudspeakers', 70, 2);

20. UPDATE products SET name = 'laser printer' WHERE code = 8;

21. UPDATE products SET  price = price - price * 0.1;

22. UPDATE products SET price = price - price * 0.1 WHERE price >= 120 ```
