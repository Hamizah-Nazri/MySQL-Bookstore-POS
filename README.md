# Project-2 (Bookstore POS System) using MySQL Workbench

* schema generation query
-- Create the customers table
CREATE TABLE `customers` (
  `id` int DEFAULT NULL,
  `name` text,
  `email` text,
  `tel` bigint DEFAULT NULL,
  `created_at` text,
  `updated_at` text
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- Create the invoices table
CREATE TABLE `invoices` (
  `id` int DEFAULT NULL,
  `number` int DEFAULT NULL,
  `sub_total` int DEFAULT NULL,
  `tax_total` int DEFAULT NULL,
  `total` int DEFAULT NULL,
  `customer_id` int DEFAULT NULL,
  `created_at` text,
  `updated_at` text
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- Create the invoices_lines table
CREATE TABLE `invoices_lines` (
  `id` int DEFAULT NULL,
  `description` text,
  `unit_price` int DEFAULT NULL,
  `quantity` int DEFAULT NULL,
  `sub_total` int DEFAULT NULL,
  `tax_total` int DEFAULT NULL,
  `total` int DEFAULT NULL,
  `tax_id` text,
  `sku_id` int DEFAULT NULL,
  `invoice_id` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

* number of customers purchasing more than 5 books
SELECT COUNT(DISTINCT A.customer_id) AS num_customers
FROM appendix_a.invoices AS A
JOIN (
    SELECT invoice_id, SUM(quantity) AS total_quantity
    FROM appendix_a.invoices_lines
    GROUP BY invoice_id
    HAVING SUM(quantity) > 5
) AS P ON A.id = P.invoice_id;

* list of customers who never purchased anything
SELECT DISTINCT C.id
FROM appendix_a.customers AS C
LEFT JOIN appendix_a.invoices AS I ON C.id = I.customer_id
WHERE I.customer_id IS NULL;

* list of book purchased with the users
SELECT 
    C.id AS 'CustomerID',
    C.name AS 'CustomerName',
    GROUP_CONCAT(DISTINCT L.description ORDER BY L.description SEPARATOR ', ') AS 'Books'
FROM 
    appendix_a.customers AS C
INNER JOIN 
    appendix_a.invoices AS I ON C.id = I.customer_id
INNER JOIN 
    appendix_a.invoices_lines AS L ON I.id = L.invoice_id
GROUP BY 
    C.id, C.name;
