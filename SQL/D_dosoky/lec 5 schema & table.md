# ملاحظات مختصرة عن السكيما في SQL

## نظرة عامة

**السكيما**: حاوية منطقية لكائنات قاعدة البيانات (جداول، عروض، إجراءات). تُنظم الكائنات وتتحكم في الوصول.

## إنشاء سكيما (CREATE SCHEMA)

### الصيغة

```sql
CREATE SCHEMA schema_name [AUTHORIZATION owner_name]
```

- `schema_name`: اسم السكيما.
- `AUTHORIZATION`: (اختياري) يحدد المالك.

### مثال

```sql
CREATE SCHEMA customer_services;
GO
```

### التحقق

عرض السكيمات:

```sql
SELECT s.name AS schema_name, u.name AS schema_owner 
FROM sys.schemas s 
INNER JOIN sys.sysusers u ON u.uid = s.principal_id 
ORDER BY s.name;
```

### إنشاء جدول

```sql
CREATE TABLE customer_services.jobs (
    job_id INT PRIMARY KEY IDENTITY,
    customer_id INT NOT NULL,
    description VARCHAR(200),
    created_at DATETIME2 NOT NULL
);
```

## حذف سكيما (DROP SCHEMA)

### الصيغة

```sql
DROP SCHEMA schema_name;
```

### مثال

```sql
DROP SCHEMA customer_services;
```

### ملاحظات

- السكيما يجب أن تكون فارغة.
- تحتاج صلاحيات (`CONTROL` أو `ALTER`).
- الحذف نهائي.

## أفضل الممارسات

- استخدم أسماء واضحة (مثل `sales`).
- عيّن مالكًا للتحكم في الوصول.
- نظّم الكائنات حسب الغرض.
- وثّق السكيمات.

## نصائح

- استعلم عن كائنات السكيما:
    
    ```sql
    SELECT name, type_desc 
    FROM sys.objects 
    WHERE schema_id = SCHEMA_ID('customer_services');
    ```
    
- السكيما الافتراضية تُسهل الاستعلامات.

---
# 6- table
# شرح مبسط للمفاهيم الغير واضحة في الجداول بـ SQL Server

هنا سأشرح المفاهيم التي قد تكون غير واضحة من الملاحظات السابقة بشكل مبسط مع أمثلة عملية، مع التركيز على تبسيط الأفكار لتكون سهلة الفهم.

## 1. مفهوم IDENTITY

### الشرح

- `IDENTITY` هو خاصية تُستخدم في عمود لتوليد أرقام تلقائية بشكل متسلسل (مثل 1، 2، 3...) كلما أُضيف سجل جديد.
- يُستخدم غالبًا للعمود الرئيسي (Primary Key) لضمان أن كل سجل له رقم فريد.
- الصيغة: `IDENTITY(بداية, زيادة)`، مثلاً `IDENTITY(1,1)` يبدأ من 1 ويزيد بـ1.

### مثال بسيط

لنفترض أنك تريد إنشاء جدول لتسجيل زيارات العملاء، وتريد رقمًا فريدًا لكل زيارة:

```sql
CREATE TABLE sales.visits (
    visit_id INT PRIMARY KEY IDENTITY(1,1), -- يولد 1، 2، 3... تلقائيًا
    customer_name VARCHAR(50)
);
```

عند إضافة سجلات:

```sql
INSERT INTO sales.visits (customer_name) VALUES ('أحمد');
INSERT INTO sales.visits (customer_name) VALUES ('سارة');
```

النتيجة:

|visit_id|customer_name|
|---|---|
|1|أحمد|
|2|سارة|

**لماذا هو مفيد؟** يوفر عليك إدخال الأرقام يدويًا ويضمن عدم التكرار.

---

## 2. مفهوم SEQUENCE

### الشرح

- `SEQUENCE` هو كائن منفصل في قاعدة البيانات يُولد أرقامًا متسلسلة (مثل `IDENTITY`)، لكن يمكن استخدامه في أكثر من جدول أو لأغراض أخرى.
- يُعطيك مرونة أكبر من `IDENTITY` لأنه ليس مرتبطًا بعمود معين.

### مثال بسيط

إنشاء تسلسل:

```sql
CREATE SEQUENCE sales.visit_seq
START WITH 1
INCREMENT BY 1;
```

استخدامه في جدول:

```sql
CREATE TABLE sales.visits (
    visit_id INT PRIMARY KEY,
    customer_name VARCHAR(50)
);
INSERT INTO sales.visits (visit_id, customer_name)
VALUES (NEXT VALUE FOR sales.visit_seq, 'أحمد');
```

النتيجة: `visit_id` يأخذ القيمة 1، ثم 2، وهكذا.

**الفرق عن IDENTITY؟** `SEQUENCE` يمكن استخدامه في عدة جداول أو لأغراض أخرى، بينما `IDENTITY` مرتبط بجدول واحد.

---

## 3. مفهوم Computed Columns (الأعمدة المحسوبة)

### الشرح

- عمود محسوب هو عمود يتم حساب قيمته تلقائيًا بناءً على تعبير (مثل جمع أو ضرب أعمدة أخرى).
- لا تحتاج إلى إدخال بيانات يدويًا لهذا العمود؛ القيمة تُحسب تلقائيًا.

### مثال بسيط

لنفترض أن لديك جدول طلبات يحتوي على الكمية والسعر، وتريد عمودًا يحسب الإجمالي:

```sql
CREATE TABLE sales.orders (
    order_id INT PRIMARY KEY,
    quantity INT,
    price DECIMAL(10,2),
    total AS (quantity * price) -- عمود محسوب
);
```

إضافة سجل:

```sql
INSERT INTO sales.orders (order_id, quantity, price)
VALUES (1, 5, 10.00);
```

النتيجة:

|order_id|quantity|price|total|
|---|---|---|---|
|1|5|10.00|50.00|

**لماذا هو مفيد؟** يوفر الوقت ويضمن دقة الحسابات (مثل إجمالي الفاتورة).

---

## 4. مفهوم Temporary Tables (الجداول المؤقتة)

### الشرح

- الجداول المؤقتة هي جداول تُنشأ مؤقتًا لتخزين بيانات أثناء جلسة العمل، ثم تُحذف تلقائيًا.
- نوعان:
    - **محلية (#)**: تُرى فقط في الجلسة الحالية.
    - **عالمية (##)**: تُرى من جميع الجلسات، لكن تُحذف عند إغلاق الجلسة التي أنشأتها.

### مثال بسيط

إنشاء جدول مؤقت محلي:

```sql
CREATE TABLE #temp_visits (
    visit_id INT,
    customer_name VARCHAR(50)
);
INSERT INTO #temp_visits VALUES (1, 'أحمد');
SELECT * FROM #temp_visits;
```

النتيجة: يظهر السجل في الجلسة الحالية فقط، ويختفي الجدول عند إغلاق الجلسة.

**لماذا هي مفيدة؟** تُستخدم لتخزين بيانات مؤقتة أثناء معالجة استعلامات معقدة.

---

## 5. مفهوم TRUNCATE vs DROP

### الشرح

- `TRUNCATE TABLE`: يحذف جميع البيانات من الجدول، لكنه يحتفظ بهيكل الجدول (الأعمدة والقيود).
- `DROP TABLE`: يحذف الجدول بالكامل (البيانات والهيكل).
- الفرق: `TRUNCATE` أسرع ويُستخدم عندما تريد إعادة استخدام الجدول، بينما `DROP` يزيل الجدول نهائيًا.

### مثال بسيط

لديك جدول `sales.visits`:

- تفريغ البيانات:
    
    ```sql
    TRUNCATE TABLE sales.visits;
    ```
    
    الجدول يبقى فارغًا، لكنه موجود.
    
- حذف الجدول:
    
    ```sql
    DROP TABLE sales.visits;
    ```
    
    الجدول يختفي تمامًا.
    

**متى تستخدم كل واحد؟**

- `TRUNCATE`: إذا كنت تريد تفريغ البيانات فقط (مثل تنظيف الجدول).
- `DROP`: إذا لم تعد بحاجة إلى الجدول.

---

## نصائح عامة لتبسيط العمل مع الجداول

1. **ابدأ بتصميم واضح**: حدد الأعمدة وأنواع البيانات بعناية قبل إنشاء الجدول.
2. **استخدم قيودًا**: مثل `NOT NULL` و`FOREIGN KEY` لضمان سلامة البيانات.
3. **اختبر التغييرات**: قبل حذف أو تعديل جدول، جرب الأوامر على قاعدة بيانات تجريبية.
4. **احتفظ بنسخ احتياطية**: خاصة قبل استخدام `DROP` أو `TRUNCATE`.

## ملاحظة إضافية

- إذا واجهت أي أمر غير مفهوم، جرب تشغيله على قاعدة بيانات تجريبية (مثل `tempdb`) لفهم تأثيره.
- يمكنك دائمًا استخدام `SELECT * FROM sys.tables` لمعرفة الجداول الموجودة في قاعدة البيانات.


---
