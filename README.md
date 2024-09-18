

# SQLProyect (Franco,Angel,David)

1. Estructura de la base de datos de un almacen con sus tablas, sus campos y sus relaciones (Realizado en drawDB).

![imagen](https://github.com/user-attachments/assets/1f3d970d-4e19-4bd1-bb73-30c94c0e014d)


2. Creacion de las Tablas en lenguage SQL. (Ordenados para que no den conflictos de relaciones).
  ```sql
  
   -- Categories Table
   CREATE TABLE Categories (
       category_id INT PRIMARY KEY AUTO_INCREMENT,
       name VARCHAR(50) NOT NULL,z
       description TEXT
   );
   > esta tabla no tiene relaciones

   -- Suppliers Table
   CREATE TABLE Suppliers (
       supplier_id INT PRIMARY KEY AUTO_INCREMENT,
       name VARCHAR(100) NOT NULL,
       contact_person VARCHAR(100),
       phone VARCHAR(20),
       email VARCHAR(100),
       address TEXT
   );
 --  Products Table
 CREATE TABLE Products (
       product_id INT PRIMARY KEY AUTO_INCREMENT,
       name VARCHAR(100) NOT NULL,
       description TEXT,
       category_id INT,
       supplier_id INT,
       unit_price DECIMAL(10, 2) NOT NULL,
       FOREIGN KEY (category_id) REFERENCES Categories(category_id),
       FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
   );
   
   -- Inventory Table
   CREATE TABLE Inventory (
       inventory_id INT PRIMARY KEY AUTO_INCREMENT,
       product_id INT,
       quantity INT NOT NULL,
       location VARCHAR(50),
       last_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
       FOREIGN KEY (product_id) REFERENCES Products(product_id)
   );
   
   -- Orders Table
   CREATE TABLE Orders (
       order_id INT PRIMARY KEY AUTO_INCREMENT,
       supplier_id INT,
       order_date DATE NOT NULL,
       total_amount DECIMAL(10, 2) NOT NULL,
       status VARCHAR(20) NOT NULL,
       FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
   );
   
   -- Order_Items Table
   CREATE TABLE Order_Items (
       order_item_id INT PRIMARY KEY AUTO_INCREMENT,
       order_id INT,
       product_id INT,
       quantity INT NOT NULL,
       unit_price DECIMAL(10, 2) NOT NULL,
       FOREIGN KEY (order_id) REFERENCES Orders(order_id),
       FOREIGN KEY (product_id) REFERENCES Products(product_id)
   );
 ``` 

3. Codigos de inserción de datos (Ordenados por tablas)
 
  ```sql
 INSERT INTO Categories (name, description) VALUES
 ('Electronics', 'Devices like phones, laptops, and gadgets'),
 ('Furniture', 'Home and office furniture'),
 ('Clothing', 'Apparel for men, women, and children'),
 ('Food & Beverages', 'Edible items and drinks'),
 ('Sports Equipment', 'Gear and equipment for sports');
 
 INSERT INTO Suppliers (name, contact_person, phone, email, address) VALUES
 ('Tech Supplies Ltd.', 'John Doe', '123-456-7890', 'johndoe@techsupplies.com', '123 Tech Street, Silicon Valley, CA'),
 ('Furniture Co.', 'Jane Smith', '234-567-8901', 'janesmith@furnitureco.com', '456 Wood Avenue, Oak City, TX'),
 ('Fashion Hub', 'Emily White', '345-678-9012', 'emily@fashionhub.com', '789 Style Road, New York, NY'),
 ('Fresh Foods Inc.', 'Michael Brown', '456-789-0123', 'michael@freshfoods.com', '101 Farm Lane, Greenfield, IL'),
 ('Sports World', 'David Green', '567-890-1234', 'david@sportsworld.com', '202 Arena Blvd, Denver, CO');
 
 INSERT INTO Products (name, description, category_id, supplier_id, unit_price) VALUES
 ('iPhone 13', 'Latest model of Apple iPhone', 1, 1, 999.99),
 ('Laptop', 'High performance laptop for gaming and work', 1, 1, 1299.50),
 ('Office Chair', 'Ergonomic office chair with lumbar support', 2, 2, 199.99),
 ('Sofa', '3-seater comfortable sofa for living room', 2, 2, 499.99),
 ('T-shirt', 'Cotton T-shirt for men', 3, 3, 19.99),
 ('Running Shoes', 'Lightweight running shoes', 5, 3, 79.99),
 ('Organic Apples', 'Fresh organic apples, 1kg pack', 4, 4, 4.99),
 ('Orange Juice', '1L fresh orange juice', 4, 4, 3.49),
 ('Basketball', 'Official size basketball', 5, 5, 29.99),
 ('Tennis Racket', 'Professional tennis racket', 5, 5, 149.99);
 
 INSERT INTO Inventory (product_id, quantity, location) VALUES
 (1, 50, 'Warehouse A'),
 (2, 30, 'Warehouse B'),
 (3, 20, 'Warehouse C'),
 (4, 15, 'Warehouse A'),
 (5, 100, 'Warehouse B'),
 (6, 75, 'Warehouse C'),
 (7, 200, 'Warehouse A'),
 (8, 180, 'Warehouse B'),
 (9, 60, 'Warehouse C'),
 (10, 40, 'Warehouse A');
 
 INSERT INTO Orders (supplier_id, order_date, total_amount, status) VALUES
 (1, '2024-09-01', 1999.98, 'Shipped'),
 (2, '2024-09-05', 699.98, 'Pending'),
 (3, '2024-09-10', 999.95, 'Delivered'),
 (4, '2024-09-12', 15.96, 'Processing'),
 (5, '2024-09-15', 299.98, 'Shipped');
 
 INSERT INTO Order_Items (order_id, product_id, quantity, unit_price) VALUES
 (1, 1, 2, 999.99),
 (2, 3, 2, 199.99),
 (3, 5, 5, 19.99),
 (4, 7, 4, 3.99),
 (5, 9, 10, 29.99);
 
 ``` 
4. Querys a la base de datos para obtener datos relevantes.

 4.1. Productos con una media de precio por unidad mayor a 100.
 ```sql
 SELECT category_id, AVG(unit_price) AS avg_price
 FROM Products
 GROUP BY category_id
 HAVING avg_price > 100;
 ```
 4.2.Proovedores con 2 o más productos
 ```sql
 SELECT s.name, COUNT(p.product_id) AS product_count
 FROM Suppliers s
 JOIN Products p ON s.supplier_id = p.supplier_id
 GROUP BY s.name
 HAVING product_count >= 2;
 ```
 4.3.Nombre y precio unidad teniendo en cuenta su id y el precio (en este caso precio mayor que 50 y que el id sea 1)
 ```sql
 SELECT name, unit_price
 FROM Products
 WHERE unit_price > 50 AND category_id = 1;
 ```
 4.4. Cuantos registros hay en el inventario (aunque solo muestra el id del producto,cantidad y localización) teniendo en cuenta que la cantidad es menor que 50
 ```sql
 SELECT product_id, quantity, location
 FROM Inventory
 WHERE quantity < 50;
 ```
 4.5. Muestra el nombre y el precio por unidad de los 5 productos mas caros.
 ```sql
 SELECT name, unit_price
 FROM Products
 ORDER BY unit_price DESC
 LIMIT 5;
 ```
 4.6. Muestra el top 3 proovedores agrupandolo por nombre de proovedor y ordenandolo por ganancia total.
 ```sql
 SELECT s.name, SUM(oi.quantity * oi.unit_price) AS total_revenue
 FROM Suppliers s
 JOIN Products p ON s.supplier_id = p.supplier_id
 JOIN Order_Items oi ON p.product_id = oi.product_id
 GROUP BY s.name
 HAVING total_revenue > 0
 ORDER BY total_revenue DESC
 LIMIT 3;
 
 ```
 4.7. Muestra el id de los pedidos, el estado y su coste total teniendo en cuenta que su coste total es mayor que 500 y el cual su estado sea Shipped.
 ```sql
 SELECT o.order_id, o.total_amount, o.status
 FROM Orders o
 WHERE o.total_amount > 500
 HAVING o.status = 'Shipped';
 ```
 4.8.Muestrame los 10 primeros registros del nombre de los productos y la cantidad del inventario teniendo en cuenta su relación y que la catidad tiene que ser mayor que 20
 ```sql
 SELECT p.name, i.quantity
 FROM Products p
 JOIN Inventory i ON p.product_id = i.product_id
 WHERE i.quantity > 20
 LIMIT 10;
 ```
 4.9.Muestra el número total de productos en el inventario. 
 ```sql
 SELECT COUNT(*) AS total_products_in_inventory
 FROM Inventory;
 
 ```
 4.10. Muestra la cantidad de productos, del mismo tipo, que existen en el inventario.
 ```sql
 SELECT p.name, SUM(i.quantity) AS total_quantity
 FROM Products p
 JOIN Inventory i ON p.product_id = i.product_id
 GROUP BY p.name;
 
 ```
 4.11. Precio medio de cada producto por categorías.
 ```sql
 SELECT c.name AS category_name, AVG(p.unit_price) AS avg_unit_price
 FROM Products p
 JOIN Categories c ON p.category_id = c.category_id
 GROUP BY c.name;
 
 ```
 4.12. Muestra los productos ofrecidos por cada provedor, ordenándolos de más caros a más baratos. 
 ```sql
 SELECT s.name AS supplier_name, MAX(p.unit_price) AS max_price
 FROM Products p
 JOIN Suppliers s ON p.supplier_id = s.supplier_id
 GROUP BY s.name;
 
 ```
 4.13. Muestra los productos más baratos por cada categoría.
 ```sql
 SELECT c.name AS category_name, MIN(p.unit_price) AS min_price
 FROM Products p
 JOIN Categories c ON p.category_id = c.category_id
 GROUP BY c.name;
 
 ```
 4.14. Muestra los tres provedores que más ganacia que generan.
 ```sql
 SELECT s.name AS supplier_name, SUM(oi.quantity * oi.unit_price) AS total_revenue
 FROM Suppliers s
 JOIN Products p ON s.supplier_id = p.supplier_id
 JOIN Order_Items oi ON p.product_id = oi.product_id
 GROUP BY s.name order by total_revenue desc limit 3;
 ```
 4.15. Muestra el numero totaal de productos agrupados por categoria 
 ```sql
 SELECT c.name AS category_name, SUM(oi.quantity * oi.unit_price) AS total_revenue
 FROM Products p
 JOIN Categories c ON p.category_id = c.category_id
 JOIN Order_Items oi ON p.product_id = oi.product_id
 GROUP BY c.name;
 ```
