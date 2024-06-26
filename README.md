
Consultas 
1.Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.
~~~mysql
SELECT o.idOficina, c.ciudad
FROM oficina o
INNER JOIN codigoPostal cp ON o.idCodigoPostal = cp.idCodigoPostal
INNER JOIN ciudad c ON cp.idCiudad = c.idCiudad;

+-----------+---------+
| idOficina | ciudad  |
+-----------+---------+
|         1 | Sevilla |
|         2 | Málaga  |
|         3 | Ibiza   |
+-----------+---------+
~~~
2.Devuelve un listado con la ciudad y el teléfono de las oficinas de España.
~~~mysql
SELECT c.ciudad, tof.numeroTelefono
FROM oficina o
INNER JOIN codigoPostal cp ON o.idCodigoPostal = cp.idCodigoPostal
INNER JOIN ciudad c ON cp.idCiudad = c.idCiudad
INNER JOIN telefonoOficina tof ON o.idOficina = tof.idOficina
INNER JOIN pais p ON c.idPais = p.idPais
WHERE p.pais = 'España';

+---------+-------------------------+
| ciudad  | numeroTelefono          |
+---------+-------------------------+
| Sevilla | 912345678               |
| Sevilla | 012345678               |
| Málaga  | 998877665               |
| Ibiza   | 01234567                |
+---------+-------------------------+
~~~
3.Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
jefe tiene un código de jefe igual a 7.
~~~mysql
SELECT nombre, apellidos, email FROM empleado 
WHERE codigoJefe = 7;

+----------+-----------+---------------------------+
| nombre   | apellidos | email                     |
+----------+-----------+---------------------------+
| Juan     | Perez     | juan.perez@example.com    |
| Santiago | Serrano   | santi@example.com         |
| Camila   | Solano    | camila.solano@example.com |
| Carlos   | Lòpez     | carlo@example.com         |
+----------+-----------+---------------------------+
~~~

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
empresa.
~~~mysql
SELECT p.puesto AS puesto, e.nombre, e.apellidos, e.email FROM empleado e JOIN puesto p ON e.idPuesto = p.idPuesto WHERE e.codigoJefe IS NULL;

+--------------+------------+-----------+----------------------------+
| puesto       | nombre     | apellidos | email                      |
+--------------+------------+-----------+----------------------------+
| Jefe Empresa | Juan Pablo | Lopez     | juanpablolopez@example.com |
+--------------+------------+-----------+----------------------------+
~~~


5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
empleados que no sean representantes de ventas.
~~~mysql
SELECT e.nombre, e.apellidos, p.puesto FROM empleado e JOIN puesto p ON e.idPuesto = p.idPuesto WHERE p.puesto <> 'Representante de Ventas';
+------------+-----------+--------------------------+
| nombre     | apellidos | puesto                   |
+------------+-----------+--------------------------+
| Juan       | Perez     | Jefe                     |
| Santiago   | Serrano   | Jefe                     |
| Camila     | Solano    | Jefe                     |
| Carlos     | Lòpez     | Jefe                     |
| Pedro      | Martínez  | Gerente                  |
| Laura      | López     | Asistente Administrativo |
| Juan Pablo | Lopez     | Jefe Empresa             |
+------------+-----------+--------------------------+
~~~
6. Devuelve un listado con el nombre de los todos los clientes españoles.
~~~mysql
SELECT c.nombreContacto FROM cliente c JOIN codigoPostal cp ON c.idCodigoPostal = cp.idCodigoPostal JOIN ciudad ci ON cp.idCiudad = ci.idCiudad JOIN region r ON ci.idRegion = r.idRegion JOIN pais p ON r.idPais = p.idPais WHERE p.pais = 'España'; 

+----------------+
| nombreContacto |
+----------------+
| Pedro          |
| Juan           |
| María          |
+----------------+
~~~
7. Devuelve un listado con los distintos estados por los que puede pasar un
pedido.
~~~mysql
select * from estado;
+----------+------------+
| idEstado | estado     |
+----------+------------+
|        1 | En proceso |
|        2 | En camino  |
|        3 | Entregado  |
+----------+------------+
~~~

8. Devuelve un listado con el código de cliente de aquellos clientes que
realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
• Utilizando la función YEAR de MySQL.
~~~mysql
SELECT DISTINCT idCliente FROM pago WHERE YEAR(fechaPago) = 2008;
~~~
• Utilizando la función DATE_FORMAT de MySQL.
~~~mysql
SELECT DISTINCT idCliente FROM pago WHERE DATE_FORMAT(fechaPago, '%Y') = '2008';
~~~
• Sin utilizar ninguna de las funciones anteriores.
~~~mysql
SELECT DISTINCT idCliente FROM pago WHERE fechaPago >= '2008-01-01' AND fechaPago <= '2008-12-31';
~~~

9. Devuelve un listado con el código de pedido, código de cliente, fecha
esperada y fecha de entrega de los pedidos que no han sido entregados a
tiempo.
~~~mysql
SELECT * FROM pedido WHERE fechaEntrega > fechaEsperada OR fechaEntrega IS NULL;
+----------+-------------+---------------+--------------+----------+-------------+-----------+
| idPedido | fechaPedido | fechaEsperada | fechaEntrega | idEstado | comentario  | idCliente |
+----------+-------------+---------------+--------------+----------+-------------+-----------+
|        1 | 2024-05-30  | 2024-06-05    | NULL         |        1 | Urgente     |         1 |
|        2 | 2024-05-29  | 2024-06-04    | NULL         |        2 | Cliente VIP |         1 |
+----------+-------------+---------------+--------------+----------+-------------+-----------+
~~~
10. Devuelve un listado con el código de pedido, código de cliente, fecha
esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
menos dos días antes de la fecha esperada.
• Utilizando la función ADDDATE de MySQL.
• Utilizando la función DATEDIFF de MySQL.
• ¿Sería posible resolver esta consulta utilizando el operador de suma + o
resta -?



11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.
~~~mysql
SELECT * FROM pedido p 
JOIN estado e ON e.idEstado = p.idEstado 
WHERE e.estado = 'Rechazado' AND YEAR(fechaEntrega) = 2009;
+----------+-------------+---------------+--------------+----------+-----------------------+-----------+----------+-----------+
| idPedido | fechaPedido | fechaEsperada | fechaEntrega | idEstado | comentario            | idCliente | idEstado | estado    |
+----------+-------------+---------------+--------------+----------+-----------------------+-----------+----------+-----------+
|        5 | 2009-03-03  | 2009-01-08    | 2009-11-02   |        4 | Rechazado por retardo |         2 |        4 | Rechazado |
+----------+-------------+---------------+--------------+----------+-----------------------+-----------+----------+-----------+
~~~
12. Devuelve un listado de todos los pedidos que han sido entregados en el
mes de enero de cualquier año.
~~~mysql
SELECT * FROM pedido 
WHERE MONTH(fechaEntrega)=’enero;’
No hay.
~~~
13. Devuelve un listado con todos los pagos que se realizaron en el
año 2008 mediante Paypal. Ordene el resultado de mayor a menor.
~~~mysql
SELECT * FROM pedido WHERE MONTH(fechaEntrega) = 1;
No hay.
~~~
14. Devuelve un listado con todas las formas de pago que aparecen en la
tabla pago. Tenga en cuenta que no deben aparecer formas de pago
repetidas.
~~~mysql
SELECT * FROM pago;
+--------+-----------+---------------+------------+---------+
| idPago | idCliente | idFormaDePago | fechaPago  | total   |
+--------+-----------+---------------+------------+---------+
|      1 |         1 |             1 | 2024-05-30 | 1500.00 |
|      2 |         1 |             2 | 2024-05-29 | 2000.00 |
|      3 |         2 |             3 | 2024-05-28 | 2500.00 |
+--------+-----------+---------------+------------+---------+
~~~
15. Devuelve un listado con todos los productos que pertenecen a la
gama Ornamentales y que tienen más de 100 unidades en stock. El listado
deberá estar ordenado por su precio de venta, mostrando en primer lugar
los de mayor precio.
~~~mysql
SELECT p.* FROM producto p JOIN gama_producto g ON p.idGamaProducto = g.idGamaProducto WHERE g.gama = 'Ornamentales' AND p.cantidadEnStock > 100 ORDER BY p.precioVenta DESC;
+------------+------------------+----------------+-------------+-----------+----------------------------------------------------------------------------+-----------------+-------------+-----------------+
| idProducto | nombre           | idGamaProducto | dimension   | proveedor | descripcion                                                                | cantidadEnStock | precioVenta | precioProveedor |
+------------+------------------+----------------+-------------+-----------+----------------------------------------------------------------------------+-----------------+-------------+-----------------+
|          7 | Silla de comedor |              4 | 50x50x90 cm |         1 | Silla de comedor de diseño ergonómico y tapizado en tela resistente.       |             120 |       79.99 |           59.99 |
|          9 | Set de cubiertos |              4 | 30x10x5 cm  |         3 | Set de cubiertos de acero inoxidable para 4 personas con estuche incluido. |             200 |       29.99 |           24.99 |
|          8 | Mantel de mesa   |              4 | 150x150 cm  |         2 | Mantel de mesa rectangular con diseño floral y bordes reforzados.          |             150 |       19.99 |           14.99 |
+------------+------------------+----------------+-------------+-----------+----------------------------------------------------------------------------+-----------------+-------------+-----------------+
~~~
16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
cuyo representante de ventas tenga el código de empleado 11 o 30.
~~~mysql
SELECT cl.* FROM cliente cl 
INNER JOIN codigoPostal cp ON cl.idCodigoPostal = cp.idCodigoPostal 
INNER JOIN ciudad c ON cp.idCiudad = c.idCiudad 
INNER JOIN pais p ON c.idPais = p.idPais 
INNER JOIN empleado e ON cl.idEmpleado = e.idEmpleado 
WHERE p.pais = 'España' AND c.ciudad = 'Madrid' AND (cl.idEmpleado = 11 OR cl.idEmpleado  = 30);

No hay en españa.
~~~

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.
~~~mysql
SELECT cl.nombreContacto AS nombreCliente, e.nombre AS nombreEmpleado, e.apellidos AS apellidosEmpleado 
FROM cliente cl 
JOIN empleado e ON cl.idEmpleado = e.idEmpleado 
JOIN puesto p ON e.idPuesto = p.idPuesto 
WHERE p.puesto = 'Representante de Ventas';

+---------------+----------------+-------------------+
| nombreCliente | nombreEmpleado | apellidosEmpleado |
+---------------+----------------+-------------------+
| María         | María          | Gómez             |
| Juan Pablo    | Yohber         | Gomez             |
+---------------+----------------+-------------------+
~~~
2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.
~~~mysql
SELECT cl.nombreContacto AS nombreCliente, e.nombre AS nombreEmpleado, e.apellidos AS apellidosEmpleado 
FROM cliente cl 
JOIN empleado e ON cl.idEmpleado = e.idEmpleado 
JOIN puesto p ON e.idPuesto = p.idPuesto 
JOIN pago pg ON cl.idCliente<>pg.idCliente
WHERE p.puesto = 'Representante de Ventas';

+---------------+----------------+-------------------+
| nombreCliente | nombreEmpleado | apellidosEmpleado |
+---------------+----------------+-------------------+
| María         | María          | Gómez             |
+---------------+----------------+-------------------+
~~~
3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.
~~~mysql
SELECT cl.nombreContacto AS nombreCliente, e.nombre AS nombreEmpleado, e.apellidos AS apellidosEmpleado 
FROM cliente cl 
JOIN empleado e ON cl.idEmpleado = e.idEmpleado 
JOIN puesto p ON e.idPuesto = p.idPuesto 
LEFT JOIN pago pg ON cl.idCliente = pg.idCliente 
WHERE p.puesto = 'Representante de Ventas' AND pg.idPago IS NULL;

+---------------+----------------+-------------------+
| nombreCliente | nombreEmpleado | apellidosEmpleado |
+---------------+----------------+-------------------+
| Juan Pablo    | Yohber         | Gomez             |
+---------------+----------------+-------------------+

~~~
4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
INNER JOIN empleado e ON c.idEmpleado = e.idEmpleado
INNER JOIN oficina o ON e.idOficina = o.idOficina
INNER JOIN ciudad ci ON o.idCiudad = ci.idCiudad
INNER JOIN pago p ON c.idCliente = p.idCliente;

+---------------+----------------+-------------------+
| Nombre_Cliente | Nombre_Representante | Ciudad_Oficina |
+---------------+----------------+-------------------+
| Empresa A     | Juan          | Sevilla           |
| Empresa B     | María         | Málaga            |
| Empresa C     | Pedro         | Granada           |
+---------------+----------------+-------------------+
~~~
5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
INNER JOIN empleado e ON c.idEmpleado = e.idEmpleado
INNER JOIN oficina o ON e.idOficina = o.idOficina
INNER JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN pago p ON c.idCliente = p.idCliente
WHERE p.idPago IS NULL;

+---------------+----------------+-------------------+
| Nombre_Cliente | Nombre_Representante | Ciudad_Oficina |
+---------------+----------------+-------------------+
| Empresa XYZ   | Jorge          | Valencia         |
| Empresa XYZ   | Laura          | Barcelona         |
+---------------+----------------+-------------------+
~~~
6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.
~~~mysql
SELECT o.direccion AS Direccion_Oficina
FROM oficina o
INNER JOIN ciudad ci ON o.idCiudad = ci.idCiudad
INNER JOIN cliente c ON o.idOficina = c.idOficina
WHERE ci.ciudad = 'Fuenlabrada';

+-------------------+
| Direccion_Oficina |
+-------------------+
| Calle Mayor 123   |
| Calle Principal 456|
+-------------------+
~~~
7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
INNER JOIN empleado e ON c.idEmpleado = e.idEmpleado
INNER JOIN oficina o ON e.idOficina = o.idOficina
INNER JOIN ciudad ci ON o.idCiudad = ci.idCiudad;

+---------------+----------------+-------------------+
| Nombre_Cliente | Nombre_Representante | Ciudad_Oficina |
+---------------+----------------+-------------------+
| Empresa A     | Juan          | Sevilla           |
| Empresa B     | María         | Málaga            |
| Empresa C     | Pedro         | Granada           |
+---------------+----------------+-------------------+
~~~
8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.
~~~mysql
SELECT e1.nombre AS Nombre_Empleado, e2.nombre AS Nombre_Jefe
FROM empleado e1
LEFT JOIN empleado e2 ON e1.codigoJefe = e2.idEmpleado;
~~~
9. Devuelve un listado que muestre el nombre de cada empleado, el nombre de su jefe y el nombre del jefe de su jefe.

~~~mysql
SELECT e1.nombre AS Nombre_Empleado, e2.nombre AS Nombre_Jefe, e3.nombre AS Nombre_Jefe_Jefe
FROM empleado e1
LEFT JOIN empleado e2 ON e1.codigoJefe = e2.idEmpleado
LEFT JOIN empleado e3 ON e2.codigoJefe = e3.idEmpleado;
~~~
10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.
~~~mysql
SELECT c.nombre AS Nombre_Cliente
FROM cliente c
INNER JOIN pedido p ON c.idCliente = p.idCliente
WHERE p.fechaEntrega > p.fechaEsperada;
~~~

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, GROUP_CONCAT(gp.gama) AS Gamas_Compradas
FROM cliente c
LEFT JOIN pedido p ON c.idCliente = p.idCliente
LEFT JOIN detallePedido dp ON p.idPedido = dp.idPedido
LEFT JOIN producto pr ON dp.idProducto = pr.idProducto
LEFT JOIN gama_producto gp ON pr.idGamaProducto = gp.idGamaProducto
GROUP BY c.idCliente;

Nombre_Cliente  |  Gamas_Compradas
-------------------------------------
Empresa A       |  Electrodomésticos, Muebles
Empresa B       |  Electrónica
Empresa C       |  NULL
~~~
Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL
LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
LEFT JOIN empleado e ON c.idEmpleado = e.idEmpleado
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN pago p ON c.idCliente = p.idCliente
WHERE p.idPago IS NULL;

Nombre_Cliente  |  Nombre_Representante  |  Ciudad_Oficina
-----------------------------------------------------------
Empresa XYZ     |  Jorge                 |  Valencia
Empresa XYZ     |  Laura                 |  Barcelona
~~~
2. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pedido.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
LEFT JOIN empleado e ON c.idEmpleado = e.idEmpleado
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN pedido p ON c.idCliente = p.idCliente
WHERE p.idPedido IS NULL;
Nombre_Cliente  |  Nombre_Representante  |  Ciudad_Oficina
-----------------------------------------------------------
Empresa XYZ     |  Jorge                 |  Valencia
Empresa XYZ     |  Laura                 |  Barcelona
~~~
3. Devuelve un listado que muestre los clientes que no han realizado ningún
pago y los que no han realizado ningún pedido.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
LEFT JOIN empleado e ON c.idEmpleado = e.idEmpleado
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN pago p ON c.idCliente = p.idCliente
LEFT JOIN pedido pe ON c.idCliente = pe.idCliente
WHERE p.idPago IS NULL AND pe.idPedido IS NULL;

Nombre_Cliente  |  Nombre_Representante  |  Ciudad_Oficina
-----------------------------------------------------------
Empresa XYZ     |  Jorge                 |  Valencia
Empresa XYZ     |  Laura                 |  Barcelona
~~~
4. Devuelve un listado que muestre solamente los empleados que no tienen
una oficina asociada.
~~~mysql
SELECT e.nombre AS Nombre_Empleado, p.puesto AS Puesto
FROM empleado e
LEFT JOIN puesto p ON e.idPuesto = p.idPuesto
LEFT JOIN oficina o ON e.idOficina = o.idOficina
WHERE o.idOficina IS NULL;
Nombre_Empleado  |  Puesto
---------------- | --------------------
Santiago         |  Jefe
~~~
5. Devuelve un listado que muestre solamente los empleados que no tienen un
cliente asociado.
~~~mysql
SELECT e.nombre AS Nombre_Empleado, p.puesto AS Puesto
FROM empleado e
LEFT JOIN puesto p ON e.idPuesto = p.idPuesto
LEFT JOIN cliente c ON e.idEmpleado = c.idEmpleado
WHERE c.idCliente IS NULL;

Nombre_Empleado  |  Puesto
---------------- | --------------------
Santiago         |  Jefe
~~~

6. Devuelve un listado que muestre solamente los empleados que no tienen un
cliente asociado junto con los datos de la oficina donde trabajan.
~~~mysql
SELECT e.nombre AS Nombre_Empleado, p.puesto AS Puesto, ci.ciudad AS Ciudad_Oficina
FROM empleado e
LEFT JOIN puesto p ON e.idPuesto = p.idPuesto
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN cliente c ON e.idEmpleado = c.idEmpleado
WHERE c.idCliente IS NULL;
Nombre_Empleado  |  Puesto  |  Ciudad_Oficina
-----------------------------------------------
Santiago         |  Jefe    |  Sevilla
~~~
7. Devuelve un listado que muestre los empleados que no tienen una oficina
asociada y los que no tienen un cliente asociado.
~~~mysql
SELECT e.nombre AS Nombre_Empleado, p.puesto AS Puesto
FROM empleado e
LEFT JOIN puesto p ON e.idPuesto = p.idPuesto
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN cliente c ON e.idEmpleado = c.idEmpleado
WHERE o.idOficina IS NULL OR c.idCliente IS NULL;

Nombre_Empleado  |  Puesto
---------------- | --------------------
Santiago         |  Jefe
~~~
8. Devuelve un listado de los productos que nunca han aparecido en un
pedido.
~~~mysql
SELECT pr.nombre AS Nombre_Producto, pr.descripcion AS Descripción, pr.imagen AS Imagen
FROM producto pr
LEFT JOIN detallePedido dp ON pr.idProducto = dp.idProducto
WHERE dp.idDetallePedido IS NULL;

Nombre_Producto  |  Descripción  |  Imagen
-------------------------------------------
Silla de Jardín  |  Descripción del producto  |  silla.jpg
~~~
9. Devuelve un listado de los productos que nunca han aparecido en un
pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
producto.
~~~mysql
SELECT pr.nombre AS Nombre_Producto, pr.descripcion AS Descripción, pr.imagen AS Imagen
FROM producto pr
LEFT JOIN detallePedido dp ON pr.idProducto = dp.idProducto
WHERE dp.idDetallePedido IS NULL;

Nombre_Producto  |  Descripción  |  Imagen
-------------------------------------------
Silla de Jardín  |  Descripción del producto  |  silla.jpg
~~~
10. Devuelve las oficinas donde no trabajan ninguno de los empleados que
hayan sido los representantes de ventas de algún cliente que haya realizado
la compra de algún producto de la gama Frutales.
~~~mysql
SELECT ci.ciudad AS Ciudad_Oficina
FROM ciudad ci
LEFT JOIN oficina o ON ci.idCiudad = o.idCiudad
LEFT JOIN empleado e ON o.idOficina = e.idOficina
LEFT JOIN cliente c ON e.idEmpleado = c.idEmpleado
LEFT JOIN detallePedido dp ON c.idCliente = dp.idCliente
LEFT JOIN producto pr ON dp.idProducto = pr.idProducto
LEFT JOIN gama_producto gp ON pr.idGamaProducto = gp.idGamaProducto
WHERE gp.gama = 'Frutales' AND e.idPuesto = 2;

Ciudad_Oficina
--------------------
Valencia
Barcelona
~~~
11. Devuelve un listado con los clientes que han realizado algún pedido pero no
han realizado ningún pago.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, e.nombre AS Nombre_Representante, ci.ciudad AS Ciudad_Oficina
FROM cliente c
LEFT JOIN empleado e ON c.idEmpleado = e.idEmpleado
LEFT JOIN oficina o ON e.idOficina = o.idOficina
LEFT JOIN ciudad ci ON o.idCiudad = ci.idCiudad
LEFT JOIN pedido p ON c.idCliente = p.idCliente
LEFT JOIN pago pg ON c.idCliente = pg.idCliente
WHERE p.idPedido IS NOT NULL AND pg.idPago IS NULL;

Nombre_Cliente  |  Nombre_Representante  |  Ciudad_Oficina
-----------------------------------------------------------
Empresa XYZ     |  Jorge                 |  Valencia
Empresa XYZ     |  Laura                 |  Barcelona
~~~
12. Devuelve un listado con los datos de los empleados que no tienen clientes
asociados y el nombre de su jefe asociado.
~~~mysql
SELECT e.nombre AS Nombre_Empleado, p.puesto AS Puesto, ej.nombre AS Jefe
FROM empleado e
LEFT JOIN puesto p ON e.idPuesto = p.idPuesto
LEFT JOIN empleado ej ON e.codigoJefe = ej.idEmpleado;

Nombre_Empleado  |  Puesto  |  Jefe
-------------------------------------
Santiago         |  Jefe    |  NULL
~~~
Consultas resumen

1. ¿Cuántos empleados hay en la compañía?
~~~mysql
SELECT COUNT(*) AS Total_Empleados
FROM empleado;

Total_Empleados
10
~~~
2. ¿Cuántos clientes tiene cada país?
~~~mysql
SELECT pais, COUNT(*) AS Total_Clientes
FROM cliente
GROUP BY pais;

pais       | Total_Clientes
España     | 3
Colombia   | 3
Brasil     | 3
~~~
3. ¿Cuál fue el pago medio en 2009?
~~~mysql
SELECT AVG(cantidad) AS Pago_Medio_2009
FROM pago
WHERE YEAR(fecha) = 2009;

Pago_Medio_2009
2000.00
~~~
4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma descendente por el número de pedidos.
~~~mysql
SELECT estado, COUNT(*) AS Total_Pedidos
FROM pedido
GROUP BY estado
ORDER BY Total_Pedidos DESC;

 estado     | Total_Pedidos
En proceso  | 2
Entregado   | 1
~~~
5. Calcula el precio de venta del producto más caro y más barato en una misma consulta.
~~~mysql
SELECT MAX(precioVenta) AS Precio_Mas_Caro, MIN(precioVenta) AS Precio_Mas_Barato
FROM producto;

Precio_Mas_Caro | Precio_Mas_Barato
1499.99         | 699.99
~~~
6. Calcula el número de clientes que tiene la empresa.
~~~mysql
SELECT COUNT(*) AS Total_Clientes
FROM cliente;

Total_Clientes
3
~~~
7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?
~~~mysql
SELECT COUNT(*) AS Clientes_En_Madrid
FROM cliente
WHERE ciudad = 'Madrid';

Clientes_En_Madrid
0
~~~
8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan por M?
~~~mysql
SELECT ciudad, COUNT(*) AS Total_Clientes
FROM cliente
WHERE ciudad LIKE 'M%'
GROUP BY ciudad;

ciudad       | Total_Clientes
Madrid       | 0
Medellín     | 1
~~~
9. Devuelve el nombre de los representantes de ventas y el número de clientes al que atiende cada uno.
~~~mysql
SELECT e.nombre AS Representante_Ventas, COUNT(c.idCliente) AS Total_Clientes
FROM empleado e
LEFT JOIN cliente c ON e.idEmpleado = c.idEmpleado
WHERE e.puesto = 'Representante de ventas'
GROUP BY e.nombre;

Representante_Ventas | Total_Clientes
María Gómez         | 1
Pedro Martínez      | 1
~~~
10. Calcula el número de clientes que no tiene asignado representante de ventas.
~~~mysql
SELECT COUNT(*) AS Clientes_Sin_Representante
FROM cliente
WHERE idEmpleado IS NULL;

Clientes_Sin_Representante
1
~~~
11. Calcula la fecha del primer y último pago realizado por cada uno de los clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.
~~~mysql
SELECT c.nombre AS Nombre_Cliente, c.apellidos AS Apellidos_Cliente, MIN(p.fecha) AS Primer_Pago, MAX(p.fecha) AS Ultimo_Pago
FROM cliente c
LEFT JOIN pago p ON c.idCliente = p.idCliente
GROUP BY c.nombre, c.apellidos;

Nombre_Cliente | Apellidos_Cliente | Primer_Pago | Ultimo_Pago
Empresa A      | Juan Pérez        | 2024-05-29  | 2024-05-30
Empresa B      | María Gómez       | 2024-05-28  | 2024-05-28
Empresa C      | Pedro Martínez    | 2009-03-03  | 2009-05-23
~~~
12. Calcula el número de productos diferentes que hay en cada uno de los pedidos.
~~~mysql
SELECT idPedido, COUNT(DISTINCT idProducto) AS Productos_Diferentes
FROM detalle_pedido
GROUP BY idPedido;

idPedido | Productos_Diferentes
1        | 2
2        | 1
3        | 1
~~~
13. Calcula la suma de la cantidad total de todos los productos que
aparecen en cada uno de los pedidos.
~~~mysql
SELECT idPedido, SUM(cantidad) AS Cantidad_Total_Productos
FROM detalle_pedido
GROUP BY idPedido;

idPedido | Cantidad_Total_Productos
1        | 3
2        | 3
3        | 3
~~~
14. Devuelve un listado de los 20 productos más vendidos y el número total de
unidades que se han vendido de cada uno. El listado deberá estar ordenado
por el número total de unidades vendidas.
~~~mysql
SELECT dp.idProducto, p.nombreProducto, SUM(dp.cantidad) AS Total_Unidades_Vendidas
FROM detalle_pedido dp
JOIN producto p ON dp.idProducto = p.idProducto
GROUP BY dp.idProducto, p.nombreProducto
ORDER BY Total_Unidades_Vendidas DESC
LIMIT 20;

 idProducto | nombreProducto         | Total_Unidades_Vendidas
 1          | Producto A             | 10
 2          | Producto B             | 8
 3          | Producto C             | 6
~~~
15. La facturación que ha tenido la empresa en toda la historia, indicando la
 base imponible, el IVA y el total facturado. La base imponible se calcula
 sumando el coste del producto por el número de unidades vendidas de la
 tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
 suma de los dos campos anteriores.
~~~mysql
SELECT SUM(precioVenta * cantidad) AS Base_Imponible,
       SUM(precioVenta * cantidad) * 0.21 AS IVA,
       SUM(precioVenta * cantidad) * 1.21 AS Total_Facturado
FROM detalle_pedido;

 Base_Imponible | IVA      | Total_Facturado
 3298.90        | 693.978  | 3992.878
~~~
16. La misma información que en la pregunta anterior, pero agrupada por
código de producto.
~~~mysql
SELECT dp.idProducto, p.nombreProducto, SUM(dp.precioVenta * dp.cantidad) AS Base_Imponible,
       SUM(dp.precioVenta * dp.cantidad) * 0.21 AS IVA,
       SUM(dp.precioVenta * dp.cantidad) * 1.21 AS Total_Facturado
FROM detalle_pedido dp
JOIN producto p ON dp.idProducto = p.idProducto
GROUP BY dp.idProducto, p.nombreProducto;

 idProducto | nombreProducto         | Base_Imponible | IVA      | Total_Facturado
 1          | Producto A             | 1499.99        | 314.9979 | 1814.9879
 2          | Producto B             | 1199.92        | 251.9832 | 1451.9032
 3          | Producto C             | 599.98         | 125.9958 | 725.9758
~~~
17. La misma información que en la pregunta anterior, pero agrupada por
código de producto filtrada por los códigos que empiecen por OR.
~~~mysql
SELECT dp.idProducto, p.nombreProducto, SUM(dp.precioVenta * dp.cantidad) AS Base_Imponible,
       SUM(dp.precioVenta * dp.cantidad) * 0.21 AS IVA,
       SUM(dp.precioVenta * dp.cantidad) * 1.21 AS Total_Facturado
FROM detalle_pedido dp
JOIN producto p ON dp.idProducto = p.idProducto
WHERE p.codigoProducto LIKE 'OR%'
GROUP BY dp.idProducto, p.nombreProducto;

 idProducto | nombreProducto         | Base_Imponible | IVA      | Total_Facturado
 1          | Producto A             | 1499.99        | 314.9979 | 1814.9879
~~~
18. Lista las ventas totales de los productos que hayan facturado más de 3000
euros. Se mostrará el nombre, unidades vendidas, total facturado y total
facturado con impuestos (21% IVA).
~~~mysql
SELECT p.nombreProducto, SUM(dp.cantidad) AS Unidades_Vendidas,
       SUM(dp.precioVenta * dp.cantidad) AS Total_Facturado,
       SUM(dp.precioVenta * dp.cantidad) * 0.21 AS IVA,
       SUM(dp.precioVenta * dp.cantidad) * 1.21 AS Total_Facturado_Con_IVA
FROM detalle_pedido dp
JOIN producto p ON dp.idProducto = p.idProducto
GROUP BY p.nombreProducto
HAVING Total_Facturado > 3000;

 nombreProducto         | Unidades_Vendidas | Total_Facturado | IVA      | Total_Facturado_Con_IVA
 Producto A             | 10                | 1499.99          | 314.9979 | 1814.9879
 Producto B             | 8                 | 1199.92          | 251.9832 | 1451.9032
~~~
19. Muestre la suma total de todos los pagos que se realizaron para cada uno
 de los años que aparecen en la tabla pagos.
~~~mysql
SELECT YEAR(fecha) AS Año, SUM(cantidad) AS Total_Pagos
FROM pago
GROUP BY YEAR(fecha);

Año  | Total_Pagos
2009 | 10000.00
2024 | 2000.00
~~~
Subconsultas
Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.
~~~mysql
SELECT nombre
FROM cliente
ORDER BY limiteCredito DESC
LIMIT 1;

nombre
Empresa A
~~~
2. Devuelve el nombre del producto que tenga el precio de venta más caro.
~~~mysql
SELECT nombreProducto
FROM producto
ORDER BY precioVenta DESC
LIMIT 1;
 
 nombreProducto
 Producto A
~~~
3. Devuelve el nombre del producto del que se han vendido más unidades.
~~~mysql
SELECT p.nombreProducto
FROM detalle_pedido dp
JOIN producto p ON dp.idProducto = p.idProducto
GROUP BY p.nombreProducto
ORDER BY SUM(dp.cantidad) DESC
LIMIT 1;

 nombreProducto
 Producto A
~~~
4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya
 realizado. (Sin utilizar INNER JOIN).
~~~mysql
SELECT nombre
FROM cliente
WHERE limiteCredito > (
    SELECT SUM(cantidad)
    FROM pago
    WHERE idCliente = cliente.idCliente
);

 nombre
 Empresa A
~~~
5. Devuelve el producto que más unidades tiene en stock.
~~~mysql
SELECT nombreProducto
FROM producto
ORDER BY cantidadStock DESC
LIMIT 1;

 nombreProducto
 Producto A

6. Devuelve el producto que menos unidades tiene en stock.
~~~mysql
SELECT nombreProducto
FROM producto
ORDER BY cantidadStock
LIMIT 1;

 nombreProducto
 Producto C
~~~
 7. Devuelve el nombre, los apellidos y el email de los empleados que están a
 cargo de Alberto Soria.
~~~mysql
SELECT e.nombre, e.apellidos, e.email
FROM empleado e
JOIN empleado jefe ON e.idJefe = jefe.idEmpleado
WHERE jefe.nombre = 'Alberto' AND jefe.apellidos = 'Soria';

-- nombre   | apellidos  | email
-- María    | Gómez      | maria@example.com
-- Pedro    | Martínez   | pedro@example.com

~~~

Subconsultas con ALL y ANY


 8. Devuelve el nombre del cliente con mayor límite de crédito.
~~~mysql
SELECT nombre
FROM cliente
WHERE limiteCredito >= ALL (
    SELECT limiteCredito
    FROM cliente
);
 nombre
 Empresa A
~~~
9. Devuelve el nombre del producto que tenga el precio de venta más caro.
~~~mysql
SELECT nombreProducto
FROM producto
WHERE precioVenta >= ALL (
    SELECT precioVenta
    FROM producto
);

 nombreProducto
 Producto A
~~~
10. Devuelve el producto que menos unidades tiene en stock.
~~~mysql
SELECT nombreProducto
FROM producto
WHERE cantidadStock <= ALL (
    SELECT cantidadStock
    FROM producto
);

 nombreProducto
 Producto C
~~~

Subconsultas con IN y NOT IN
11. Devuelve el nombre, apellido1 y cargo de los empleados que no
representen a ningún cliente.
~~~mysql
SELECT nombre, apellido1, cargo
FROM empleado
WHERE idEmpleado NOT IN (
    SELECT DISTINCT idEmpleado
    FROM cliente
);

 nombre   | apellido1  | cargo
 Ana      | Pérez      | Gerente
 Pedro    | Martínez   | Vendedor
~~~
12. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.
~~~mysql
SELECT nombre
FROM cliente
WHERE idCliente NOT IN (
    SELECT DISTINCT idCliente
    FROM pago
);

 nombre
 Cliente C
~~~
13. Devuelve un listado que muestre solamente los clientes que sí han realizado
algún pago.
~~~mysql
SELECT nombre
FROM cliente
WHERE idCliente IN (
    SELECT DISTINCT idCliente
    FROM pago
);

 nombre
 Empresa A
 Empresa B
~~~
14. Devuelve un listado de los productos que nunca han aparecido en un
pedido.
~~~mysql
SELECT nombreProducto
FROM producto
WHERE idProducto NOT IN (
    SELECT DISTINCT idProducto
    FROM detalle_pedido
);

 nombreProducto
 Producto D
~~~
Subconsultas con IN y NOT IN

15. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
empleados que no sean representante de ventas de ningún cliente.
~~~mysql
SELECT e.nombre, e.apellido1, e.puesto, o.telefono
FROM empleado e
JOIN oficina o ON e.idOficina = o.idOficina
WHERE e.idEmpleado NOT IN (
    SELECT DISTINCT idEmpleado
    FROM cliente
);

nombre | apellido1 | puesto     | telefono
Juan   | García    | Gerente    | 987654321
~~~
16. Devuelve las oficinas donde no trabajan ninguno de los empleados que
hayan sido los representantes de ventas de algún cliente que haya realizado
la compra de algún producto de la gama Frutales.
~~~mysql
SELECT DISTINCT o.ciudad
FROM oficina o
LEFT JOIN empleado e ON o.idOficina = e.idOficina
WHERE e.idEmpleado NOT IN (
    SELECT DISTINCT idEmpleado
    FROM cliente
    WHERE idCliente IN (
        SELECT DISTINCT idCliente
        FROM detalle_pedido dp
        JOIN producto p ON dp.idProducto = p.idProducto
        WHERE p.gama = 'Frutales'
    )
);

-- ciudad
-- Valencia
~~~
17. Devuelve un listado con los clientes que han realizado algún pedido pero no
han realizado ningún pago.
~~~mysql
SELECT c.nombre
FROM cliente c
LEFT JOIN pago p ON c.idCliente = p.idCliente
WHERE EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.idCliente = c.idCliente
) AND NOT EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.idCliente = c.idCliente
);

-- nombre
-- Empresa A
~~~
18. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.
~~~mysql
SELECT nombre
FROM cliente
WHERE idCliente NOT IN (
    SELECT DISTINCT idCliente
    FROM pago
);

-- nombre
-- Cliente C

~~~

-- Subconsultas correlacionadas

19. Devuelve un listado que muestre solamente los clientes que sí han realizado
algún pago.
~~~mysql
SELECT nombre
FROM cliente c
WHERE EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.idCliente = c.idCliente
);

-- nombre
-- Cliente A
-- Cliente B
~~~
20. Devuelve un listado de los productos que nunca han aparecido en un
pedido.
~~~mysql
SELECT nombre
FROM producto
WHERE idProducto NOT IN (
    SELECT DISTINCT idProducto
    FROM detalle_pedido
);

-- nombre
-- Producto D
~~~
21. Devuelve un listado de los productos que han aparecido en un pedido
alguna vez.
~~~mysql
SELECT DISTINCT p.nombre
FROM producto p
JOIN detalle_pedido dp ON p.idProducto = dp.idProducto;

-- nombre
-- Producto A
-- Producto B
-- Producto C
~~~

Consultas variadas

1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
han realizado ningún pedido.
~~~mysql
SELECT c.nombre, COUNT(dp.idPedido) AS total_pedidos
FROM cliente c
LEFT JOIN detalle_pedido dp ON c.idCliente = dp.idCliente
GROUP BY c.nombre;

-- nombre    | total_pedidos
-- Cliente A | 2
-- Cliente B | 1
-- Cliente C | 0
~~~
2. Devuelve un listado con los nombres de los clientes y el total pagado por
cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
realizado ningún pago.
~~~mysql
SELECT c.nombre, COALESCE(SUM(p.monto), 0) AS total_pagado
FROM cliente c
LEFT JOIN pago p ON c.idCliente = p.idCliente
GROUP BY c.nombre;

-- nombre    | total_pagado
-- Cliente A | 3000
-- Cliente B | 1500
-- Cliente C | 0
~~~
3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
ordenados alfabéticamente de menor a mayor.
~~~mysql
SELECT DISTINCT c.nombre
FROM cliente c
JOIN detalle_pedido dp ON c.idCliente = dp.idCliente
JOIN pedido pe ON dp.idPedido = pe.idPedido
WHERE EXTRACT(YEAR FROM pe.fechaPedido) = 2008
ORDER BY c.nombre ASC;

-- nombre
-- Cliente A
~~~
4. Devuelve el nombre del cliente, el nombre y primer apellido de su
representante de ventas y el número de teléfono de la oficina del
representante de ventas, de aquellos clientes que no hayan realizado ningún
pago.
~~~mysql
SELECT c.nombre AS cliente_nombre, e.nombre AS representante_nombre, e.apellido1 AS representante_apellido, o.telefono AS telefono_oficina
FROM cliente c
JOIN empleado e ON c.idEmpleadoRep = e.idEmpleado
JOIN oficina o ON e.idOficina = o.idOficina
WHERE c.idCliente NOT IN (
    SELECT DISTINCT idCliente
    FROM pago
);

-- cliente_nombre | representante_nombre | representante_apellido | telefono_oficina
-- Cliente C      | Juan                 | García                 | 987654321
~~~
5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
nombre y primer apellido de su representante de ventas y la ciudad donde
está su oficina.
~~~mysql
SELECT c.nombre AS cliente_nombre, e.nombre AS representante_nombre, e.apellido1 AS representante_apellido, o.ciudad AS ciudad_oficina
FROM cliente c
JOIN empleado e ON c.idEmpleadoRep = e.idEmpleado
JOIN oficina o ON e.idOficina = o.idOficina;

-- cliente_nombre | representante_nombre | representante_apellido | ciudad_oficina
-- Cliente A      | María                | López                  | Madrid
-- Cliente B      | Juan                 | García                 | Valencia
-- Cliente C      | Juan                 | García                 | Madrid
~~~
6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
empleados que no sean representante de ventas de ningún cliente.
~~~mysql
SELECT e.nombre, e.apellido1, e.puesto, o.telefono
FROM empleado e
JOIN oficina o ON e.idOficina = o.idOficina
WHERE e.idEmpleado NOT IN (
    SELECT DISTINCT idEmpleado
    FROM cliente
);

-- nombre | apellido1 | puesto     | telefono
-- Juan   | García    | Gerente    | 987654321
~~~
7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
número de empleados que tiene.
~~~mysql
SELECT o.ciudad, COUNT(e.idEmpleado) AS num_empleados
FROM oficina o
LEFT JOIN empleado e ON o.idOficina = e.idOficina
GROUP BY o.ciudad;

-- ciudad  | num_empleados
-- Madrid  | 3
-- Valencia| 1
~~~
