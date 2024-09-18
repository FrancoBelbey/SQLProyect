

# SQLProyect (Franco,Angel,David)

Estructura de la base de datos de un almacen con sus tablas, sus campos y sus relaciones

![imagen](https://github.com/user-attachments/assets/1f3d970d-4e19-4bd1-bb73-30c94c0e014d)


1. Productos con una media de precio por unidad mayor a 100.
```sql
SELECT category_id, AVG(unit_price) AS avg_price
FROM Products
GROUP BY category_id
HAVING avg_price > 100;
```
2.Proovedores con 2 o más productos
```sql
SELECT s.name, COUNT(p.product_id) AS product_count
FROM Suppliers s
JOIN Products p ON s.supplier_id = p.supplier_id
GROUP BY s.name
HAVING product_count >= 2;
```
3.Nombre y precio unidad teniendo en cuenta su id y el precio (en este caso precio mayor que 50 y que el id sea 1)
```sql
SELECT name, unit_price
FROM Products
WHERE unit_price > 50 AND category_id = 1;
```
4. Cuantos registros hay en el inventario (aunque solo muestra el id del producto,cantidad y localización) teniendo en cuenta que la cantidad es menor que 50
```sql
SELECT product_id, quantity, location
FROM Inventory
WHERE quantity < 50;
```
5. Muestra el nombre y el precio por unidad de los 5 productos mas caros.
```sql
SELECT name, unit_price
FROM Products
ORDER BY unit_price DESC
LIMIT 5;
```
6. Muestra el top 3 proovedores agrupandolo por nombre de proovedor y ordenandolo por ganancia total.
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
7. Muestra el id de los pedidos, el estado y su coste total teniendo en cuenta que su coste total es mayor que 500 y el cual su estado sea Shipped.
```sql
SELECT o.order_id, o.total_amount, o.status
FROM Orders o
WHERE o.total_amount > 500
HAVING o.status = 'Shipped';
```
8.Muestrame los 10 primeros registros del nombre de los productos y la cantidad del inventario teniendo en cuenta su relación y que la catidad tiene que ser mayor que 20
```sql
SELECT p.name, i.quantity
FROM Products p
JOIN Inventory i ON p.product_id = i.product_id
WHERE i.quantity > 20
LIMIT 10;
```
9.Muestra el número total de productos en el inventario. 
```sql
SELECT COUNT(*) AS total_products_in_inventory
FROM Inventory;

```
10. Muestra la cantidad de productos, del mismo tipo, que existen en el inventario.
```sql
SELECT p.name, SUM(i.quantity) AS total_quantity
FROM Products p
JOIN Inventory i ON p.product_id = i.product_id
GROUP BY p.name;

```
11. Precio medio de cada producto por categorías.
```sql
SELECT c.name AS category_name, AVG(p.unit_price) AS avg_unit_price
FROM Products p
JOIN Categories c ON p.category_id = c.category_id
GROUP BY c.name;

```
12. Muestra los productos ofrecidos por cada provedor, ordenándolos de más caros a más baratos. 
```sql
SELECT s.name AS supplier_name, MAX(p.unit_price) AS max_price
FROM Products p
JOIN Suppliers s ON p.supplier_id = s.supplier_id
GROUP BY s.name;

```
13. Muestra los productos más baratos por cada categoría.
```sql
SELECT c.name AS category_name, MIN(p.unit_price) AS min_price
FROM Products p
JOIN Categories c ON p.category_id = c.category_id
GROUP BY c.name;

```
14. Muestra los tres provedores que más ganacia que generan.
```sql
SELECT s.name AS supplier_name, SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM Suppliers s
JOIN Products p ON s.supplier_id = p.supplier_id
JOIN Order_Items oi ON p.product_id = oi.product_id
GROUP BY s.name order by total_revenue desc limit 3;
```
15. Muestra el numero totaal de productos agrupados por categoria 
```sql
SELECT c.name AS category_name, SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM Products p
JOIN Categories c ON p.category_id = c.category_id
JOIN Order_Items oi ON p.product_id = oi.product_id
GROUP BY c.name;
```
