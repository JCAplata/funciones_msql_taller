# funciones_msql_taller
# **Taller de Funciones Definidas por el Usuario (UDFs) en MySQL** 🚀

## **📌 Introducción**

En este taller, desarrollarás varias funciones definidas por el usuario (UDFs) en MySQL para resolver problemas comunes en bases de datos. Aprenderás a crear funciones, utilizarlas en consultas y comprender en qué casos son útiles.

Cada ejercicio presenta un caso de estudio con datos y requerimientos específicos. ¡Ponte a prueba y mejora tus habilidades en SQL! 💡

------

## **🔹 Caso 1: Cálculo de Bonificaciones de Empleados**

### **Escenario**:

La empresa **TechCorp** otorga bonificaciones a sus empleados según su salario base. La bonificación se calcula así:

- Si el salario es menor a 2,000 USD → Bonificación del **10%**.
- Si el salario está entre 2,000 y 5,000 USD → Bonificación del **7%**.
- Si el salario es mayor a 5,000 USD → Bonificación del **5%**.
  #RTA:
  DELIMITER //

CREATE FUNCTION calculate_bonus(salary DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE bonus DECIMAL(10,2);

    IF salary < 2000 THEN
        SET bonus = salary * 0.10;
    ELSEIF salary <= 5000 THEN
        SET bonus = salary * 0.07;
    ELSE
        SET bonus = salary * 0.05;
    END IF;

    RETURN bonus;
END //

DELIMITER ;

CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    salary DECIMAL(10,2)
);

INSERT INTO employees (name, salary) VALUES
('camilo amezquita', 1500.00),
('sofia pinto', 3000.00),
('diego gasca', 6000.00);

SELECT 
    name,
    salary,
    calculate_bonus(salary) AS bonus
FROM employees;

#  Caso 2: Cálculo de Edad de Clientes**

### **Escenario**:

En la empresa **MarketShop**, se necesita calcular la edad de los clientes a partir de su fecha de nacimiento para determinar estrategias de marketing.
#RTA:
DELIMITER //

CREATE FUNCTION calculate_age(birth_date DATE) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE());
END //

DELIMITER ;

CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    birth_date DATE
);

INSERT INTO customers (name, birth_date) VALUES
('james amezquita', '19680-03-22'),
('emmanuel amezquita', '2013-06-07'),
('federico corso', '2012-04-11');

SELECT 
    name,
    birth_date,
    calculate_age(birth_date) AS age
FROM customers;

# Caso 3: Formatear Números de Teléfono**

### **Escenario**:

En **CallCenter Solutions**, los números de teléfono están almacenados sin formato y se necesita presentarlos en el formato `(XXX) XXX-XXXX`.

#RTA:

DELIMITER //

CREATE FUNCTION format_phone(number VARCHAR(10)) 
RETURNS VARCHAR(14)
DETERMINISTIC
BEGIN
    DECLARE formatted_phone VARCHAR(14);

    SET formatted_phone = CONCAT(
        '(', SUBSTRING(number, 1, 3), ') ',
        SUBSTRING(number, 4, 3), '-', 
        SUBSTRING(number, 7, 4)
    );

    RETURN formatted_phone;
END //

DELIMITER ;

CREATE TABLE contacts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    phone VARCHAR(10)
);

INSERT INTO contacts (name, phone) VALUES
('hair ceron', '3158178155'),
('geronimo ceron', '3158178155'),
('amelia carrera', '3158178155');

SELECT 
    name,
    phone,
    format_phone(phone) AS formatted_phone
FROM contacts;

# Caso 4: Clasificación de Productos por Precio**

### **Escenario**:

En la tienda **E-Shop**, los productos se categorizan en tres niveles según su precio:

- **Bajo**: Menos de 50 USD.
- **Medio**: Entre 50 y 200 USD.
- **Alto**: Más de 200 USD.

#RTA:
DELIMITER //

CREATE FUNCTION classify_price(price DECIMAL(10,2)) 
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE category VARCHAR(10);

    IF price < 50 THEN
        SET category = 'Low';
    ELSEIF price <= 200 THEN
        SET category = 'Medium';
    ELSE
        SET category = 'High';
    END IF;

    RETURN category;
END //

DELIMITER ;

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    price DECIMAL(10,2)
);

INSERT INTO products (name, price) VALUES
('USB 2000 GB', 25.00),
('Mechanical Keyboard Impact', 120.00),
('Gaming Laptop PC 360', 950.00);

SELECT 
    name,
    price,
    classify_price(price) AS category
FROM products;

