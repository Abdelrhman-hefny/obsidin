
---

### 🧠 ما هي `GROUP BY`؟

`GROUP BY` تُستخدم لتجميع الصفوف التي تشترك في نفس القيمة في عمود أو أكثر، وغالبًا مع دوال تجميع مثل:

- `SUM()` لحساب المجموع
    
- `AVG()` لحساب المتوسط
    
- `COUNT()` لحساب عدد الصفوف
    
- `MAX()` / `MIN()` لأكبر وأصغر قيمة
    

---

### 🟦 مثال 1: الأساسيات

**الجدول: Orders**

|OrderID|Customer|Amount|
|---|---|---|
|1|Ali|100|
|2|Ali|150|
|3|Sara|200|
|4|Sara|50|
|5|Hani|90|

**المطلوب:** مجموع المبيعات لكل عميل

```sql
SELECT Customer, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY Customer;
```

**النتيجة:**

|Customer|TotalAmount|
|---|---|
|Ali|250|
|Sara|250|
|Hani|90|

---

### 🟨 ما هي `HAVING`؟

`HAVING` تُستخدم لتصفية النتائج بعد استخدام `GROUP BY`، بينما `WHERE` تُستخدم قبل التجميع ولا تقبل دوال تجميع.

---

### 🟨 مثال 2: تصفية باستخدام `HAVING`

**المطلوب:** العملاء الذين مجموع طلباتهم > 200

```sql
SELECT Customer, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY Customer
HAVING SUM(Amount) > 200;
```

**النتيجة:**

|Customer|TotalAmount|
|---|---|
|Ali|250|
|Sara|250|

---

### 🟥 مثال 4: `GROUP BY` على أكثر من عمود

**الجدول: Sales**

|SaleID|Product|Region|Amount|
|---|---|---|---|
|1|TV|East|1000|
|2|TV|West|800|
|3|Phone|East|500|
|4|TV|East|1200|
|5|Phone|West|600|

**المطلوب:** مجموع المبيعات لكل منتج في كل منطقة

```sql
SELECT Product, Region, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Product, Region;
```

**النتيجة:**

|Product|Region|TotalSales|
|---|---|---|
|TV|East|2200|
|TV|West|800|
|Phone|East|500|
|Phone|West|600|

---

### 🟪 مثال 5: `HAVING` + `GROUP BY` + `ORDER BY`

**المطلوب:** المنتجات التي مبيعاتها > 1000، مرتبة تنازليًا

```sql
SELECT Product, Region, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Product, Region
HAVING SUM(Amount) > 1000
ORDER BY TotalSales DESC;
```

**النتيجة:**

|Product|Region|TotalSales|
|---|---|---|
|TV|East|2200|

---

### 🧩 `JOIN` مع `GROUP BY`

نستخدم `JOIN` لربط جدولين معًا، ثم نستخدم `GROUP BY` لتجميع النتائج بناءً على عمود من أحد الجدولين.

---

```sql
SELECT brand_name, 
       COUNT(*) AS ProductCount, 
       MAX(list_price) AS MaxPrice, 
       MIN(list_price) AS MinPrice
FROM production.brands b
JOIN production.products p ON b.brand_id = p.brand_id
GROUP BY brand_name;
```

---

- `JOIN`: ربط بين جدول **brands** (العلامات التجارية) و **products** (المنتجات) باستخدام `brand_id`.
- `GROUP BY brand_name`: نجمع البيانات حسب اسم العلامة التجارية.
- `COUNT(*)`: نحسب عدد المنتجات لكل علامة تجارية.
- `MAX(list_price)`: أعلى سعر منتج.
- `MIN(list_price)`: أقل سعر منتج.

---
### 📋 النتيجة :

|brand_name|ProductCount|MaxPrice|MinPrice|
|---|---|---|---|
|Nike|10|300.00|99.00|
|Adidas|8|250.00|80.00|
|Puma|5|200.00|120.00|

> (القيم تقريبية – تعتمد على البيانات الفعلية في قاعدة البيانات)
