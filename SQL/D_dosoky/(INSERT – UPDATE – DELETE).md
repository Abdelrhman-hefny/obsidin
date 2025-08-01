
### ✅ 1. `INSERT` – إضافة بيانات جديدة في الجدول

#### 📌 الصيغة:

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

#### 🧠 مثال:

```sql
INSERT INTO students (name, age, grade, email)
VALUES ('Omar', 23, 'B', 'omar@mail.com');
```

> 🟡 يضيف صف جديد لجدول `students` بقيم الطالب "Omar".

---

### ✅ 2. `UPDATE` – تعديل بيانات موجودة

#### 📌 الصيغة:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

#### 🧠 مثال:

```sql
UPDATE students
SET grade = 'A', age = 21
WHERE name = 'Mariam';
```

> 🟡 يعدل درجة وعمر الطالبة "Mariam".

> ⚠️ **مهم جدًا**: لازم تحط `WHERE` علشان ما يتعدلش كل الصفوف.

---

### ✅ 3. `DELETE` – حذف بيانات من الجدول

#### 📌 الصيغة:

```sql
DELETE FROM table_name
WHERE condition;
```

#### 🧠 مثال:

```sql
DELETE FROM students
WHERE id = 6;
```

> 🟡 يحذف الطالب اللي رقمه 6.

> ⚠️ **بدون `WHERE`؟** كل البيانات هتتحذف:

```sql
DELETE FROM students; -- خطر جدًا
```

---

### ✅ 4. ملاحظات مهمة:

- لو عندك عمود `IDENTITY` (يعني رقم تلقائي)، مش لازم تكتبه في `INSERT`.
    
- دايمًا اختبر أوامر `UPDATE` و `DELETE` بإنك تعمل `SELECT` بنفس الـ `WHERE` الأول.
    

---

### ✅ 5. تمارين تطبيقية:

#### 🧪 على جدول `students`

1. أضف طالب اسمه "Nour"، عمره 20، درجته 'C'، وإيميله `nour@mail.com`.
    
2. عدل عمر الطالب "Youssef" إلى 23.
    
3. غير درجة الطالب اللي رقمه 4 إلى 'B'.
    
4. احذف الطالب اللي اسمه "Mostafa".
    
5. احذف كل الطلاب اللي أعمارهم أقل من 18.
    

---

