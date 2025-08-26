# --------------  Wildcard Characters ------------


#### إمتى نستخدمها؟

 تُستخدم مع شرط **`LIKE`** في جملة `WHERE` عشان نعمل **بحث عن نمط** داخل نصوص (أسماء، عناوين، أيميلات...). الصيغة العامة:

```sql
SELECT column FROM Table
WHERE column LIKE 'pattern';
```

---

## 1) `_` (underscore) — يمثل **حرفاً واحداً فقط**

- يعني: أي حرف واحد مكان `_`.
- مثال : نبحث عن أسماء يكون الحرف الثاني فيها هو `m`:
    
```sql
SELECT St_Fname
FROM Student
WHERE St_Fname LIKE '_m%';
```

-  `_` يطالب بحرف واحد (أي حرف) في البداية، ثم `m` كالحرف الثاني، ثم `%` أي شيء بعده.
- أمثلة نتيجة (إذا كانت الأسماء موجودة): `Amr`, `Emad`, `Amira` — لأن حرفهم الثاني هو `m` أو `M` (اعتمادًا على حساسية الحروف في القاعدة).

**ملاحظة:** `_a` يعني اسم مكوّن من حرفين وينتهي بـ `a` (مثال افتراضي: `Ma`).

## 2) `%` — يمثل **صفر أو أكثر من الأحرف**

- يعني: أي سلسلة (حتى فارغة).
- أمثلة:
    
    - `s%` → جميع الأسماء التي **تبدأ** بـ `s` (مثل `Samy`, `Shaimaa`).
    - `%a` → جميع الأسماء التي **تنتهي** بـ `a` (مثل `Sama`, `Amira`).
    - `%am%` → أي اسم يحتوي `am` في أي مكان (`Samar`, `Yassmin`).
        
```sql
SELECT St_Fname FROM Student WHERE St_Fname LIKE 's%';
SELECT St_Fname FROM Student WHERE St_Fname LIKE '%a';
SELECT St_Fname FROM Student WHERE St_Fname LIKE '%am%';
```

---

## 3) `[]` — مجموعة أحرف أو نطاق

- نكتب داخل الأقواس مجموعة من الحروف أو نطاق مثل `[a-m]`.
- `'[sa]%'` → يبدأ بـ `s` أو `a`.
- `'[a-m]%'` → يبدأ بأي حرف من a إلى m.
    - `'[0-9]%'` → يبدأ برقم (0 إلى 9).

```sql
SELECT St_Fname FROM Student WHERE St_Fname LIKE '[sa]%';
SELECT St_Fname FROM Student WHERE St_Fname LIKE '[a-m]%';
```

-  نتائج متوقعة: `'Ahmed'`, `'Shaimaa'`, `'Samy'` (لـ `[sa]%`).

---

## 4) `[^]` أو `[!...]` — نفي المجموعة (يعني **ليس** تلك الأحرف)

- `'[^sa]%'` → يبدأ بأي حرف **غير** s أو a.

```sql
SELECT St_Fname FROM Student WHERE St_Fname LIKE '[^sa]%';
```

- سيعيد أسماء مثل `Mohamed`, `Marwa`, `Essam` (لا تبدأ بـ s أو a).

---

## 5) أمثلة خاصة: تطابق محارف البدل نفسها (escape)

لو عايز تبحث عن نص فيه `%` أو `_` حرفيًا (مش كـ wildcard) تقدر:
- تستخدم الأقواس: `[%]` أو `[_]` داخل `LIKE` (في SQL Server هذا يعمل).

```sql
-- مثال: اختيار الصفوف التي تحتوي على علامة % داخل النص
SELECT St_Fname FROM Student WHERE St_Fname LIKE '%[%]%';
```

- أو تستخدم `ESCAPE` لتحديد حرف هروب:
```sql
-- نبحث عن السلاسل التي تحتوي على "50%" حرفياً
SELECT Col FROM T
WHERE Col LIKE '%50\%%' ESCAPE '\';
```

---

## 6) مسألة حساسية الحروف (Case sensitivity)

- في SQL Server: حساسية الحروف تعتمد على **collation**. غالبًا تكون غير حساسة (A = a). لو بدك إجبار حساسية:
```sql
SELECT St_Fname FROM Student
WHERE St_Fname COLLATE Latin1_General_CS_AS LIKE 'A%';
```

---


---

## تمارين بسيطة

1. أوجد كل الأسماء التي تبدأ بحرف `S` أو `s`.  
 2. أوجد الأسماء التي تحتوي `am` في أي مكان.  
3. أوجد الأسماء التي يبدأ حرفها الأول بين `a` و `m`.  
4. أوجد الأسماء التي تحتوي حرف `%` فعليًا داخلها (مثال: `Word%`).  

---

# ------عرض البيانات بدون تكرار (DISTINCT) ---------

##  1 — **عرض البيانات بدون تكرار (DISTINCT)**

### الفكرة

`DISTINCT` بتخلي نتيجة الاستعلام تحذف الصفوف المكررة، يعني ترجع لك كل قيمة فريدة مرة واحدة.

### التركيب العام

```sql
SELECT DISTINCT column1, column2
FROM [dbo].[Student];
```

### مثال عملي (من عندك)

```sql
select Distinct St_Fname 
from [dbo].[Student];
```

### توضيح بمثال صغير (تخيلي)

جدول `Student` يحتوي على الصفوف:

```
St_Fname
---------
Ahmed
Mohamed
Ahmed
Samar
Mohamed
```

`SELECT DISTINCT St_Fname FROM Student;`  
النتيجة ستكون:

```
St_Fname
---------
Ahmed
Mohamed
Samar
```

### نقاط مهمة 

- **DISTINCT على أكثر من عمود** → يزيل التكرار بناءً على _مجموع القيم_ في الأعمدة المحددة (أي: تركيبات القيم).
    
    ```sql
    SELECT DISTINCT St_Fname, St_Address FROM Student;
    ```
    
    هذا يعيد كل تركيبة اسم-عنوان مرة واحدة.
    
- **NULLs**: القيم `NULL` تُعامل كقيمة واحدة من حيث الـ `DISTINCT` (تُعرض `NULL` مرة واحدة إذا كانت موجودة مرات متعددة).
    
- **DISTINCT + ORDER BY**: إذا استخدمت `DISTINCT` في SQL Server، عمود الـ `ORDER BY` يجب أن يكون ضمن قائمة الأعمدة في `SELECT DISTINCT` (أو يمكنك استخدام الرقم الترتيبي للعمود). مثال صحيح:
    
    ```sql
    SELECT DISTINCT St_Fname
    FROM Student
    ORDER BY St_Fname;
    ```
    
- **DISTINCT vs GROUP BY**: ممكن تستخدم `GROUP BY` للحصول على نتائج فريدة أيضاً، لكن `GROUP BY` يستخدم عادة مع دوال تجميعية (مثل `COUNT`, `SUM`). كلاهما يزيل التكرار لكن الغرض قد يختلف.
    
- **أداء**: `DISTINCT` قد يحتاج لفرز أو تجميع داخلي في قاعدة البيانات (costly على جداول كبيرة). إن أمكن، اعمل فهرس على العمود اللي بتعمل عليه `DISTINCT` لتحسين الأداء.
    

---
# ----------- **ترتيب النتائج (ORDER BY)** ----------------

ا `ORDER BY` بترتب ناتج الاستعلام بحسب عمود أو أكثر، تصاعدي أو تنازلي.

### التركيب العام

```sql
SELECT column1, column2
FROM [dbo].[Student]
ORDER BY column1 ASC, column2 DESC;
```

- `ASC` = تصاعدي (افتراضي).
- `DESC` = تنازلي.
### أمثلة عملية

1. ترتيب أسماء الطلاب أبجدياً:
```sql
select St_Fname from [dbo].[Student]
order by St_Fname;
```

2. ترتيب بالعكس (من ز إلى أ):
```sql
select St_Fname from [dbo].[Student]
order by St_Fname desc;
```

3. ترتيب حسب المدينة ثم الاسم (المدينة تصاعدي، الاسم تنازلي):
```sql
select St_Fname, St_Address
from [dbo].[Student]
order by St_Address asc, St_Fname desc;
```

4. استخدام ترتيب بالرقم الترتيبي للعمود في SELECT:
```sql
select St_Fname, St_Address
from [dbo].[Student]
order by 2, 1; -- هنا 2 يعني St_Address ثم 1 يعني St_Fname
```

### (حالات خاصة )

- **الفرق بين نصوص وأرقام**: إذا العمود مخزن كنص لكنه في الواقع أرقام (`'2'`, `'10'`)، الترتيب الأبجدي سيعطي `'10'` قبل `'2'`. لو عايز ترتيب رقمي، حوّل النوع:
    ```sql
    SELECT col FROM T
    ORDER BY CAST(col AS INT);
    ```
    
- **NULLs في الترتيب**: قواعد البيانات تختلف. في SQL Server عادةً NULL تُعامل كقيمة صغيرة فتظهر أولاً عند `ORDER BY col ASC`. لو عايز تضمن مكان NULL، استعمل `CASE`:
    
    ```sql
    -- لو عايز NULL تظهر في الآخر
    SELECT St_Fname, St_Address
    FROM Student
    ORDER BY CASE WHEN St_Address IS NULL THEN 1 ELSE 0 END, St_Address;
    ```
    
- **الأداء**: `ORDER BY` يؤدي إلى عملية فرز يمكن أن تكون مكلفة على مجموعات كبيرة. لو عندك فهرس (index) مرتب بنفس ترتيب الأعمدة، الـ DB engine ممكن يستفيد منه ويتفادى الفرز.
    
- **TOP مع ORDER BY** (SQL Server): تستخدم `ORDER BY` مع `TOP` للحصول على أعلى/أدنى N صف:
    
    ```sql
    SELECT TOP 5 St_Fname FROM Student ORDER BY St_Fname;
    ```
    

---

## أمثلة مركّبة (DISTINCT + ORDER BY)

```sql
-- أسماء فريدة مرتبة أبجدياً
SELECT DISTINCT St_Fname
FROM [dbo].[Student]
ORDER BY St_Fname;
```

مهم: عند استخدام `DISTINCT` و `ORDER BY` تأكد إن عمود الترتيب موجود في قائمة الـ SELECT (SQL Server يطلب ذلك).

---

## تمارين قصيرة 

1. أوجد كل المدن الفريدة من جدول الطلاب ورتبها تصاعدياً.  
2. أوجد أسماء الطلاب الفريدة مع عناوينهم (كل تركيبة مرة واحدة) واذهب أولاً للطلاب الذين عنوانهم ليس `NULL`.  
3. أوجد أول 3 أسماء أبجدياً.  
4. عندك عمود `phone` مخزن كنص، كيف ترجع الأرقام مرتبة ترتيبًا رقميًا؟  

---

# ------------------ **Joins &  Self Join**  ---------------------

## مقدمة سريعة

الـ **JOINs** بتخلّينا نربط بين جدولين (أو أكثر) علشان نجيب بيانات مركبة — مثلاً اسم الطالب + اسم القسم بتاعه. كل نوع join ليه قاعدة عمل مختلفة بتتحكم أي صفوف هترجع.

### بيانات تجريبية بسيطة

**جدول Student**

```
St_Id | St_Fname | Dept_Id | St_super
------------------------------------
1     | Ahmed    | 10      | NULL
2     | Samy     | 20      | 1
3     | Mariam   | NULL    | 2
4     | Omar     | 30      | NULL
```

**جدول Department**

```
Dept_Id | Dept_Name
-------------------
10      | SD
20      | IT
40      | HR
```

ملاحظات: Dept_Id = 30 لا توجد في Department (حتى نُظهر حالات لا تطابق). Dept_Id = 40 (HR) ليس له طلاب.

---

# 4.1 Cross Join (الضرب الكارتيزي)

**الفكرة:** يطبع كل تركيبة من صفوف الجدولين — كل صف من A مع كل صف من B.

**الصيغة القديمة:**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s, [dbo].[Department] d;
```

**الصيغة الجديدة (أوضح):**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
CROSS JOIN [dbo].[Department] d;
```

**نتيجة بالمثال:** (4 طلاب × 3 أقسام = 12 صف)

```
Ahmed SD
Ahmed IT
Ahmed HR
Samy  SD
Samy  IT
...
Omar  HR
```

**متى تُستخدم؟** قليلًا ما تُستخدم في الحالات العادية — مفيدة لو محتاج تولّد كل التركيبات الممكنة (combinations). **تحذير:** ممكن تنتج عدد صفوف ضخم — احذر على قواعد بيانات كبيرة.

---

# 4.2 Inner Join (الربط الداخلي)

**الفكرة:** يرجع الصفوف التي **تطابق** شرط الربط فقط (الاشتراك في القيمة).

**الصيغة القديمة (استخدمت WHERE للربط):**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s, [dbo].[Department] d
WHERE d.Dept_Id = s.Dept_Id;
```

**الصيغة الجديدة (أنسب و أوضح):**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
INNER JOIN [dbo].[Department] d
  ON d.Dept_Id = s.Dept_Id;
```

**مثال مع فلترة:**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
INNER JOIN [dbo].[Department] d
  ON d.Dept_Id = s.Dept_Id
WHERE d.Dept_Name = 'SD';
```

**نتيجة من بيانات المثال:**

- Ahmed — SD  
    (لأن Ahmed هو الوحيد اللي Dept_Id بتاعه 10 والـ Dept_Name للـ10 = SD)
    

**متى تُستخدم؟** لما تحتاج **فقط** الصفوف المرتبطة (مثلاً: طلاب لديهم قسم معروف). هذه هي أكثر أنواع الـ JOIN استخداماً.

---

# 4.3 Outer Joins (الربط الخارجي)

## أ) Left Outer Join (LEFT JOIN)

**الفكرة:** يرجع كل صف من الجدول اليساري (left)، ومعاه صفوص مطابقة من الجدول اليميني (right) إن وُجدت. لو ما فيش تطابق، قيم الجدول اليميني بتكون `NULL`.

**مثال:**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
LEFT JOIN [dbo].[Department] d
  ON d.Dept_Id = s.Dept_Id;
```

**نتيجة (من بياناتنا):**

```
Ahmed   | SD      -- تطابق
Samy    | IT      -- تطابق
Mariam  | NULL    -- Dept_Id = NULL => لا تطابق
Omar    | NULL    -- Dept_Id = 30 => no dept found
```

**متى تُستخدم؟** لما تريد _كل_ صفوف الجدول الأول (اليسار) حتى لو ما عندهاش تطابق في الجدول الثاني (مثلاً: قائمة كل الطلاب + اسم القسم إن وُجد).

---

## ب) Right Outer Join (RIGHT JOIN)

**الفكرة:** عكس الـ LEFT JOIN — يرجع كل صف من الجدول اليميني ومعاه التطابقات من الجدول اليساري أو `NULL` إن لم توجد.

**مثال:**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
RIGHT JOIN [dbo].[Department] d
  ON d.Dept_Id = s.Dept_Id;
```

**نتيجة (من بياناتنا):**

```
Ahmed  | SD     -- مطابقة
Samy   | IT     -- مطابقة
NULL   | HR     -- قسم HR (Dept_Id=40) ليس له طلاب
```

**متى تُستخدم؟** لو المهتم بكل صفوف الجدول الثاني (اليميني) حتى لو ما عنده طلاب/مقابلات.

---

## ج) Full Outer Join (FULL JOIN)

**الفكرة:** يجمع بين LEFT و RIGHT: كل صفوف كليهما، ومكان ما لا يوجد تطابق تُعبأ بالقيم `NULL`.

**مثال:**

```sql
SELECT s.St_Fname, d.Dept_Name
FROM [dbo].[Student] s
FULL OUTER JOIN [dbo].[Department] d
  ON d.Dept_Id = s.Dept_Id;
```

**نتيجة (من بياناتنا):**

```
Ahmed   | SD
Samy    | IT
Mariam  | NULL
Omar    | NULL
NULL    | HR
```

**متى تُستخدم؟** لو عايز رؤية كاملة لأي اختلافات بين الجدولين — من يساعد في مصفوفات التطابق/اللاّتطابق.

---

### تحذير/مشكلة شائعة — شروط WHERE بعد Outer Join

لو عملت `LEFT JOIN` وبعد كده وضعت شرط في `WHERE` على عمود من الجدول اليميني، الشرط ممكن يقضي على صفوف `NULL` ويحوّل النتيجة فعليًا إلى `INNER JOIN`. مثال:

```sql
-- ممكن يظهر كـ INNER JOIN لأنه بعد الفرز نقفل أي صف يكون d.Dept_Name IS NULL
SELECT s.St_Fname, d.Dept_Name
FROM Student s
LEFT JOIN Department d ON d.Dept_Id = s.Dept_Id
WHERE d.Dept_Name = 'SD';
```

**البديل للحفاظ على الـ LEFT JOIN:**

- حط شرط التصفية داخل الـ `ON` بدل `WHERE`:
    

```sql
SELECT s.St_Fname, d.Dept_Name
FROM Student s
LEFT JOIN Department d
  ON d.Dept_Id = s.Dept_Id
  AND d.Dept_Name = 'SD';
```

بهذه الطريقة تحتفظ بكل الطلاب، لكن د.Dept_Name سيكون 'SD' للذين تطابقوا ومع `NULL` للآخرين.

---

# 5. ----------- Self Join (ربط الجدول بنفسه) ---------------


**الفكرة:** لما الصف الواحد في الجدول مرتبط بصف آخر في نفس الجدول — مثال شائع: جدول موظفين كل موظف له مشرف (supervisor) والـ supervisor هو أيضاً صف في نفس الجدول.

**بيانات توضيحية بسيطة (مأخوذة من الStudent أعلاه):**

```
1 | Ahmed   | St_super = NULL
2 | Samy    | St_super = 1    -- Samy مشرفه Ahmed
3 | Mariam  | St_super = 2    -- Mariam مشرفها Samy
4 | Omar    | St_super = NULL
```

**استعلام لإظهار اسم الطالب واسم المشرف (Inner -> فقط من لهم مشرف):**

```sql
SELECT stud.St_Id  AS 'رقم_الطالب',
       stud.St_Fname AS 'اسم_الطالب',
       supervisor.St_Fname AS 'اسم_المشرف'
FROM [dbo].[Student] stud
INNER JOIN [dbo].[Student] supervisor
  ON supervisor.St_Id = stud.St_super;
```

**نتيجة (Inner):**

```
2 | Samy   | Ahmed
3 | Mariam | Samy
```

**لو عايز كل الطلاب ومع اسم المشرف إن وُجد (Left Self Join):**

```sql
SELECT stud.St_Id, stud.St_Fname, supervisor.St_Fname AS 'اسم_المشرف'
FROM [dbo].[Student] stud
LEFT JOIN [dbo].[Student] supervisor
  ON supervisor.St_Id = stud.St_super;
```

**نتيجة (Left):**

```
1 | Ahmed   | NULL
2 | Samy    | Ahmed
3 | Mariam  | Samy
4 | Omar    | NULL
```

**متى تُستخدم؟** لعلاقات هرمية داخل نفس الجدول: مشرف/مرؤوس، مدير/موظف، أجزاء/مكوّنات، شجرة عائلة، الخ.

---

##  أفضل ممارسات

- **استخدم الصيغة الجديدة** (`JOIN ... ON`) لأنها أوضح وأسهل للصيانة.
    
- **استخدم ألقاب (aliases)** مثل `s`, `d` لتحديد الأعمدة وتفادي الالتباس.
    
- **لا تنسَ شرط ON** — نسيان `ON` مع `JOIN` يؤدي إلى نتيجة خاطئة (أو cartesian).
    
- **انتبه للـ WHERE بعد الـ OUTER JOIN** — يمكنه تحويل الـ outer إلى inner كما شرحنا.
    
- **لا تستخدم `SELECT *` في استعلامات إنتاجية**، اكتب الأعمدة صراحةً لتحسن الأداء وتقلل البيانات المرسلة.
    
- **فهرس أعمدة الربط (join columns)** مثل `Dept_Id` لتحسين أداء الربط على جداول كبيرة.
    
- **اختبر على بيانات صغيرة أولاً** لتتأكد من النتيجة قبل تطبيقها على قاعدة كبيرة.
    

---

## تمارين سريعة للمراجعة

1. اعرض كل الطلاب مع أسماء أقسامهم (حتى لو القسم مفقود).  
2. اعرض كل الأقسام (حتى لو ما عندها طلاب).  
3. اعرض اسم الطالب واسم المشرف له، واظهر أيضاً الطلاب بدون مشرف.  
4. ماذا يحدث لو كتبت `FROM Student s, Department d` بدون `WHERE` أو `ON`؟ جرب واحسب عدد الصفوف.  




+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



# -------------- **المعاملات (Transactions) وعمليات الحذف الآمنة (Temporary / Safe Delete)** ---------------------------


##  ليه نستخدم Transactions؟

لما تعمل تغييرات بيانات (INSERT / UPDATE / DELETE) ممكن تحصل أخطاء. `TRANSACTION` يسمح لك تجا معاملة كاملة كـ «عملية واحدة»: أو تتنفَّذ كلها (`COMMIT`) أو لو حصل خطأ ترجِّع كل حاجة زي ما كانت (`ROLLBACK`). مفيد جدًا للحماية من الحذف الخاطئ أو التغييرات الجزئية.

---
## الأوامر الأساسية

- `BEGIN TRANSACTION` — نبدأ المعاملة.
- `COMMIT` — نحفظ التغييرات نهائياً.
- `ROLLBACK` — نرجع التغييرات (تلغي كل شيء بعد الـ `BEGIN`).
- `@@ROWCOUNT` — دالة تعطيك عدد الصفوف المتأثرة من آخر أمر (مثل DELETE أو UPDATE).
    

---

## مثال عملي آمن (خطوة بخطوة)

هذا مثال عملي مأخوذ من كودك — نحذف موظفين مولودين في شهر 5 (مايو) لكن بطريقة آمنة نقدر نراجع قبل الحفظ:

```sql
USE CompanyG01;
-- 1) افحص أولًا أي صفوف هتتأثر
SELECT * 
FROM Employees
WHERE MONTH(BirthDate) = 5;

-- 2) ابدأ المعاملة
BEGIN TRANSACTION;

-- 3) احذف (لكن التغيير مؤقت لحد ما تعمل COMMIT)
DELETE FROM Employees
WHERE MONTH(BirthDate) = 5;

-- 4) اعرف كام صف اتحذف
SELECT @@ROWCOUNT AS RowsDeleted;

-- 5) لو النتائج كويسة:
COMMIT; -- يحفظ التغييرات
-- لو حصل خطأ أو مش عايز الحذف:
-- ROLLBACK; -- يسترجع البيانات كما كانت
```

**تفسير خطوة بخطوة:**

1. `SELECT` قبل الحذف: دا أفضل ممارسات — شوف الصفوف اللي هتتأثر قبل ما تمسحها.
2. `BEGIN TRANSACTION` يبدأ وضع مؤقت: الحذف ينفّذ لكنه غير نهائي.
3. `DELETE ...` ينفّذ ويؤثر على صفوف داخل المعاملة.
4. `SELECT @@ROWCOUNT` يعطي عدد الصفوف التي حُذفت (يمكن تستخدمه لاتخاذ قرار).
5. لو تم يعمل `COMMIT`و يحفظ. لو لا، `ROLLBACK` يرجع كل شيء.
    

---

## مثال تلقائي بالتحقق والإرجاع (TRY/CATCH)

لو بتحب تخلي العملية تلقائية (لو حصل خطأ ترجع تلقائياً)، تقدر تستخدم البلوك التالي في SQL Server:

```sql
USE CompanyG01;
BEGIN TRY
  BEGIN TRANSACTION;

  DELETE FROM Employees
  WHERE MONTH(BirthDate) = 5;

  SELECT @@ROWCOUNT AS RowsDeleted;

  -- مثال قرار: لو مفيش صفوف اتحذفت ممكن نرمي خطأ
  IF @@ROWCOUNT = 0
  BEGIN
    RAISERROR('No employees deleted - aborting.', 16, 1);
  END

  COMMIT TRANSACTION;
END TRY
BEGIN CATCH
  ROLLBACK TRANSACTION;
  SELECT ERROR_NUMBER() AS ErrNo, ERROR_MESSAGE() AS ErrMsg;
END CATCH;
```

---

## استخدام `OUTPUT` لعرض/حفظ الصفوف المحذوفة

مفيد لو عايز تحتفظ بسجل (audit) أو تعرض ما تم حذفه:

```sql
USE CompanyG01;
DECLARE @DeletedRows TABLE (EmpId INT, Fname NVARCHAR(100), BirthDate DATE);

BEGIN TRANSACTION;

DELETE FROM Employees
OUTPUT deleted.EmpId, deleted.Fname, deleted.BirthDate INTO @DeletedRows
WHERE MONTH(BirthDate) = 5;

SELECT * FROM @DeletedRows;  -- تشوف الصفوف اللي ات حذفِت

-- بعد المراجعة:
COMMIT;
-- أو لو مش عاجبك:
-- ROLLBACK;
```

---

## نقاط مهمة 

- **لا تقم بعمل `DELETE` بدون `WHERE`** إلا لو فعلاً تريد تمسح كل الصفوف.
- **افحص أولاً** بالـ `SELECT` ثم نفّذ الحذف داخل `TRANSACTION`.
- **@@ROWCOUNT** تفيدك لمعرفة كم صف اتأثر فورًا بعد الأمر.
- **تذكر أن التغييرات غير المنفّذة بـ `COMMIT` عادةً غير مرئية للجلسات الأخرى (اعتمادًا على isolation level)** — لكن لا تعتمد على ذلك للأمان، دا للـ DB engine.
- **استخدام الدوال في WHERE (مثل MONTH(BirthDate)=5)** يمنع استخدام الفهارس على `BirthDate` عادةً — لو الجدول كبير الأداء ممكن يتأثر. حلول: عمود محسوب (computed column) مفهرَس أو استخدام نطاقات تواريخ لو مناسب.
- **فقدان الاتصال قبل `COMMIT`**: إذا انقطعت الجلسة، غالبًا سيُفترض rollback تلقائيًا (يعتمد على نظام قاعدة البيانات).
- احذر من حذف بيانات مهمة — دائماً احتفظ بنسخة احتياطية إن أمكن قبل تغييرات كبيرة.
    

---

## تمارين سريعة 

1. اعرض الصفوف التي ستمسح قبل تنفيذ `DELETE` على موظفين مولودين في شهر يونيو.  
    **الحل:** `SELECT * FROM Employees WHERE MONTH(BirthDate)=6;`
2. نفّذ حذفًا تجريبيًا داخل معاملة ثم ارجع التغييرات: اكتب خطوات `BEGIN TRANSACTION`, `DELETE`, `SELECT @@ROWCOUNT`, `ROLLBACK`.
3. استخدم `DELETE ... OUTPUT deleted.*` وخزن النتيجة في جدول مؤقت ثم راجعها.
    

---


##  -----------------  **2 — أوامر SELECT مع شروط متقدمة**  -------------

#### **1) استخدام WHERE مع الدوال (MONTH, DAY)**

#### الفكرة

أحيانًا مش بنحتاج نقارن التاريخ كامل (اليوم + الشهر + السنة)، لكن نحتاج نقارن جزء منه، زي الشهر بس أو اليوم بس.  
في الحالة دي، SQL Server بيوفر دوال زي:
- **MONTH(date_column)** → بترجع رقم الشهر من التاريخ.
- **DAY(date_column)** → بترجع رقم اليوم من التاريخ.
    

#### الصيغة:
```sql
SELECT * 
FROM table_name
WHERE MONTH(date_column) = رقم_الشهر;

SELECT * 
FROM table_name
WHERE DAY(date_column) = رقم_اليوم;
```

#### مثال عملي:
```sql
SELECT * 
FROM Employees 
WHERE MONTH(BirthDate) = 5; -- كل اللي اتولدوا في شهر 5 (مايو)

SELECT * 
FROM Employees 
WHERE DAY(BirthDate) = 21; -- كل اللي عيد ميلادهم يوم 21 في أي شهر
```

💡 **فايدتها**: مفيدة لو عايز تعمل فلترة على التواريخ من غير ما تهتم بالسنة.

---

### **2) شرط NOT IN لاستبعاد قيم معينة**

#### الفكرة

أحيانًا عايز تجيب بيانات، لكن تستبعد شوية قيم محددة.  
هنا بيجي دور **NOT IN**، اللي معناها "القيمة مش ضمن القائمة".

#### الصيغة:

```sql
SELECT عمود1, عمود2
FROM table_name
WHERE column_name NOT IN (قيمة1, قيمة2, قيمة3);
```

#### مثال عملي:

```sql
SELECT St_Fname, St_Address
FROM Student 
WHERE St_Address NOT IN ('alex', 'cairo', 'tanta');
```

📌 النتيجة: هتظهر أسماء وعناوين الطلاب اللي **مش** ساكنين في الإسكندرية أو القاهرة أو طنطا.

💡 **فايدتها**: بدل ما تكتب شرط طويل بـ `<>` أكتر من مرة، بتكتبهم مرة واحدة في **NOT IN**.

---

# مراجعه علي (Joins الأساسية، Self Join، DML مع Joins)**

## ** — أنواع الـ Joins**

**INNER JOIN:** يرجع الصفوف اللي لها تطابق في الجدولين.

```sql
SELECT s.St_Fname, d.Dept_Name
FROM Student s
INNER JOIN Department d ON d.Dept_Id = s.Dept_Id;
```

مثال مع فلترة:

```sql
SELECT std.St_Fname, crs.Crs_Name, stdcrs.Grade
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
JOIN Course crs ON crs.Crs_Id = stdcrs.Crs_Id
WHERE stdcrs.Grade > 100;
```

**LEFT JOIN:** كل صفوف الجدول الأيسر + تطابق اليمين أو NULL.

```sql
SELECT s.St_Fname, d.Dept_Name
FROM Department d
LEFT JOIN Student s ON d.Dept_Id = s.Dept_Id;
```

**RIGHT JOIN:** كل صفوف الجدول اليمين + تطابق اليسار أو NULL.

```sql
SELECT s.St_Fname, d.Dept_Name
FROM Student s
RIGHT JOIN Department d ON d.Dept_Id = s.Dept_Id;
```

> فلترة بعد OUTER JOIN: ضع الشرط داخل `ON` للحفاظ على الصفوف الفارغة.

**دمج أكثر من جدول:**

```sql
SELECT std.St_Fname, crs.Crs_Name
FROM Student std
JOIN Stud_Course sc ON std.St_Id = sc.St_Id
JOIN Course crs ON crs.Crs_Id = sc.Crs_Id;
```

---

## **الجزء 4 — Self Join**

**INNER SELF JOIN:** فقط اللي لهم مشرف:

```sql
SELECT stud.St_Id, stud.St_Fname, mgr.St_Fname AS ManagerName
FROM Student stud
JOIN Student mgr ON mgr.St_Id = stud.St_super;
```

**LEFT SELF JOIN:** كل الطلاب حتى بدون مشرف:

```sql
SELECT stud.St_Id, stud.St_Fname, mgr.St_Fname AS ManagerName
FROM Student stud
LEFT JOIN Student mgr ON mgr.St_Id = stud.St_super;
```

---


## الجزء 5 — DML مع Joins (SELECT/UPDATE/DELETE مع الربط)


- **DML** = أوامر التعامل مع البيانات:
    - `SELECT` (عرض)
    - `UPDATE` (تعديل)
    - `DELETE` (حذف)

    
- **الفكرة**: أتعامل مع أكتر من جدول في نفس الوقت بدل ما أشتغل على كل جدول لوحده.
    1. عرض بيانات مرتبطة من جداول مختلفة.
    2. تعديل بيانات في جدول بناءً على شروط من جدول آخر.
    3. حذف بيانات من جدول بناءً على شروط من جدول آخر.
    4. تنفيذ عمليات على جداول مرتبطة معًا في أمر واحد.


### 1) `SELECT` باستخدام JOIN (عرض بيانات على أساس علاقة)

**مثال:** عرض طلاب + كورساتهم ودرجاتهم:

```sql
SELECT std.St_Fname, crs.Crs_Name, stdcrs.Grade
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
JOIN Course crs         ON crs.Crs_Id = stdcrs.Crs_Id
WHERE std.St_Address = 'cairo';
```

> يعرض فقط طلاب من القاهرة مع اسم المادة ودرجتهم.

---

### 2) `UPDATE` باستخدام JOIN (تعديل بيانات اعتمادًا على جدول آخر)

**قواعد:** الصيغة الشائعة في SQL Server:

```sql
UPDATE targetAlias
SET targetAlias.Column = <new value>
FROM TargetTable targetAlias
JOIN OtherTable o ON ...
WHERE ...;
```

**مثال عملي:** إنقاص الدرجة لجميع طلاب من `cairo` بمقدار 10 نقاط:

```sql
UPDATE stdcrs
SET stdcrs.Grade = stdcrs.Grade - 10
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
WHERE std.St_Address = 'cairo';
```

> **ملاحظة:** تجنب استخدام اختصارات غير واضحة مثل `SET Grade -= 10` لأنها قد لا تكون مدعومة في كل بيئة. اتبع `SET Col = Col - 10`.

**نقطة أمان:** نفَّذ SELECT مماثل أولاً لتتأكد من الصفوف المتأثرة:

```sql
SELECT stdcrs.*
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
WHERE std.St_Address = 'cairo';
```

---

### 3) `DELETE` باستخدام JOIN (حذف بناءً على علاقة)

في SQL Server يمكنك حذف من جدول محدد باستخدام JOIN. شكل عام:

```sql
DELETE tAlias
FROM TargetTable tAlias
JOIN OtherTable o ON ...
WHERE ...;
```

**أ) حذف الكورسات لطلاب بعمر 28:**

```sql
DELETE stdcrs
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
WHERE std.St_Age = 28;
```

> هذا يحذف سجلات `Stud_Course` المرتبطة بطلاب عمرهم 28.

**ب) حذف الطلاب وكورساتهم (حذر! الترتيب مهم عند وجود FK):**  
إذا يوجد علاقة مفتاحية (FK) تمنع حذف الطالب قبل حذف الكورسات، يجب حذف الكورسات أولاً ثم حذف الطلاب داخل معاملة:

```sql
BEGIN TRANSACTION;

-- 1) حذف كورسات الطلاب ذوي العمر 28
DELETE stdcrs
FROM Student std
JOIN Stud_Course stdcrs ON std.St_Id = stdcrs.St_Id
WHERE std.St_Age = 28;

-- 2) حذف الطلاب أنفسهم
DELETE std
FROM Student std
WHERE std.St_Age = 28;

COMMIT;
```

> أو إذا الـ FK معرف بـ `ON DELETE CASCADE`، حذف الطالب سيحذف كورساته آليًا.

---

## تمارين للمراجعة 

1. اكتب استعلامًا يجلب كل الأقسام التي ليس لها طلاب.
2. اكتب استعلامًا يعرض أسماء الطلاب الذين درجاتهم > 90 في مادة "Math".
3. نفّذ UPDATE لرفع الدرجة 5 نقاط لكل طلاب الإسكندرية، مع التأكد بعدد الصفوف المتأثرة.
4. احذف طلابًا (وعلى افتراض عدم وجود cascade) ممن ليس لديهم أي كورسات — خطوات: أظهر أولاً، ثم احذف.
