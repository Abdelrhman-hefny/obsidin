### PRIMARY KEY
هناك طريقتين لاضافه primary key


### أولًا: في العمود 

```sql
customer_id INT NOT NULL PRIMARY KEY
```

دي طريقة **مباشرة** لتعريف العمود مع تحديد إنه **Primary Key** في نفس السطر.

- بسيطة وسريعة.
    
- مناسبة لما يكون عندك مفتاح أساسي واحد فقط.
    
- ما بتديكش فرصة تسمي القيود (constraints) بشكل واضح.
    

---

### ثانيًا: داخل constraint 

```sql
customer_id INT NOT NULL,

CONSTRAINT customer_pk PRIMARY KEY (customer_id)
```

دي طريقة **تعريف منفصل للـ Constraint** باستخدام الكلمة المحجوزة `CONSTRAINT`، وبتديك شوية مميزات:

- تقدر تسمي الـ constraint (`customer_pk` هنا).
    
- أسهل في التعديل والإدارة لاحقًا (مفيد في قواعد بيانات كبيرة).
    
- ضرورية لو عايز تعمل **مفتاح أساسي مركّب** (Composite Primary Key) من أكتر من عمود.
    
---
## - Constraints -Unique Constraint

طبعًا، هشرحلك الـ `UNIQUE` Constraint بشكل بسيط وعملي 👇

---

## ✅ يعني إيه `UNIQUE Constraint`؟

الـ `UNIQUE` constraint بيمنعك إنك تدخل **قيم مكررة** في العمود، لكنه **بيسمح بالقيم الفارغة (`NULL`)** (إلا في بعض قواعد البيانات اللي بتتصرف بشكل مختلف زي Oracle).

يعني:

- كل قيمة في العمود لازم تكون **فريدة** (ما تتكررش).
    
- ينفع تدخل قيمة فاضية `NULL` مرة واحدة (أو أكتر حسب نوع قاعدة البيانات).
    

---


## 📌 الفرق بين `PRIMARY KEY` و `UNIQUE`

|الخاصية|`PRIMARY KEY`|`UNIQUE`|
|---|---|---|
|يمنع التكرار|✅|✅|
|يسمح بـ NULL|❌ (ما ينفعش NULL)|✅ (يسمح بـ NULL)|
|لكل جدول|مفتاح أساسي واحد بس|ممكن أكتر من `UNIQUE` واحد|
|الغرض|تعيين هوية الصف (record)|التأكد من أن القيمة مش مكررة|


---

## 🧪 مثال عملي

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    username VARCHAR(50) UNIQUE
);
```

هنا:

- `user_id`: المفتاح الأساسي، لازم يكون فريد ومش فاضي.
    
- `email`: مينفعش يتكرر، لكن ممكن يبقى `NULL`.
    
- `username`: نفس الفكرة.
    

---

## ✏️ تقدر تضيف `UNIQUE` كـ Constraint باسم مخصص:

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_code VARCHAR(20),
    CONSTRAINT unique_product_code UNIQUE (product_code)
);
```

كده لو حد حاول يضيف `product_code` مكرر، هتظهر له رسالة خطأ زي:

```
ERROR: duplicate key value violates unique constraint "unique_product_code"
```

---

## 📌 ملخص سريع:

- `UNIQUE` يمنع التكرار في العمود.
    
- تقدر تحطه في أكتر من عمود في نفس الجدول.
    
- ينفع تعرّفه داخل السطر أو بعدين باستخدام `CONSTRAINT`.
    

---


##  Constraints - Check Constraint


الـ `CHECK` Constraint بيستخدم علشان **يحدد شرط معين لازم القيمة تلتزم بيه** عند إدخال البيانات في الجدول.

يعني بمنتهى البساطة:

> "ما تدخلش أي قيمة إلا لو كانت مطابقة للشرط اللي أنا محدده."

---

## 🎯 مثال عملي

خلينا نقول إن عندك جدول للموظفين، وعايز تتأكد إن العمر يكون **18 سنة أو أكتر**:

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18)
);
```

### 💡 هنا:

- أي محاولة لإدخال موظف عمره أقل من 18 هترجعلك خطأ.
    

---

## ✍️ مثال تاني بشروط متعددة:

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) CHECK (price > 0),
    stock INT CHECK (stock >= 0)
);
```

- `price` لازم يكون أكبر من 0.
    
- `stock` لازم يكون صفر أو أكتر (يعني ماينفعش قيمة سالبة).
    

---

## ✅ ممكن تستخدم `CHECK` مع `CONSTRAINT` واسم:

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50),
    grade INT,
    CONSTRAINT grade_check CHECK (grade BETWEEN 0 AND 100)
);
```

---

## ❗ملاحظات مهمة:

|النقطة|التوضيح|
|---|---|
|مرونة|تقدر تستخدم شروط زي `>`, `<`, `=`, `BETWEEN`, `IN`, `LIKE` ... إلخ.|
|مش دايمًا مدعومة في كل قواعد البيانات بنفس القوة|بعض الأنظمة بتتجاهل الـ CHECK أحيانًا أو بتطبقها بشكل محدود.|
|ممكن تستخدم أكتر من CHECK في نفس الجدول|عادي جدًا.|

---

## 📌 خلاصة:

`CHECK` Constraint = بوابة مرور لأي قيمة.  
لو القيمة مطابقة للشرط → تدخل.  
لو مش مطابقة → **Error**.

---

## - Constraints - Foreign Key Constraint


## 🔗 Constraints - **Foreign Key Constraint**

### ✅ يعني إيه Foreign Key؟

الـ **Foreign Key** (المفتاح الأجنبي) هو **علاقة بين جدولين**، وظيفته إنه يربط عمود في جدول بعمود في جدول تاني (غالبًا العمود ده بيكون `PRIMARY KEY` أو `UNIQUE` في الجدول التاني).

> بكلمات بسيطة:  
> الـ Foreign Key بيقولك: "القيمة اللي هتتحط هنا لازم تكون موجودة هناك."

---

## 🎯 مثال عملي

### عندك جدولين:

- `customers`: فيه بيانات العملاء.
    
- `orders`: فيه بيانات الطلبات.
    

كل طلب لازم يكون مرتبط بـ **عميل** معين، صح؟ يبقى لازم جدول `orders` يعرف مين العميل، بس مش أي رقم عميل... لازم عميل موجود فعلًا في جدول `customers`.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    CONSTRAINT fk_customer FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

### ✅ اللي حصل هنا:

- `customer_id` في جدول `orders` هو **Foreign Key**.
    
- بيشير إلى `customer_id` في جدول `customers`.
    

يعني لو حاولت تضيف `customer_id = 7` في الطلبات، وهو مش موجود في جدول العملاء → ❌ **ERROR**!

---

## 🧠 ليه بنستخدم Foreign Key؟

|السبب|الفائدة|
|---|---|
|التأكد من صحة البيانات|يمنعك من إدخال قيم غير موجودة.|
|الحفاظ على العلاقات بين الجداول|زي لما تربط طلب بعميل، أو موظف بقسم...|
|ترتيب العمليات عند الحذف أو التعديل|تقدر تحدد لو البيانات هتتحذف، هل نحذف اللي مرتبط بيها ولا لأ؟ (هنشوف تحت ⬇️)|

---

## ⚙️ خصائص مهمة:

### 1. **ON DELETE / ON UPDATE**

بتحدد إيه اللي يحصل لو الصف المرتبط اتعدل أو اتمسح.

مثال:

```sql
CONSTRAINT fk_customer FOREIGN KEY (customer_id)
REFERENCES customers(customer_id)
ON DELETE CASCADE
ON UPDATE CASCADE;
```

| الحالة                    | معناها                                                    |
| ------------------------- | --------------------------------------------------------- |
| `CASCADE`                 | لو العميل اتمسح، يتم مسح كل طلباته.                       |
| `SET NULL`                | لو العميل اتمسح، نخلي قيمة `customer_id` في الطلب `NULL`. |
| `NO ACTION` أو `RESTRICT` | امنع الحذف أو التعديل لو في طلبات مرتبطة.                 |

---

## 📌 ملخص الكلام:

|نقطة|توضيح|
|---|---|
|هو إيه؟|عمود بيربط جدول بجدول تاني.|
|بيربط إيه؟|عمود بـ `PRIMARY KEY` أو `UNIQUE` في جدول تاني.|
|فائدته؟|الحفاظ على العلاقات ومنع القيم العشوائية أو غير المرتبطة.|
|بيديك خيارات؟|آه، زي `ON DELETE`, `ON UPDATE` علشان تتحكم في السلوك.|

---
