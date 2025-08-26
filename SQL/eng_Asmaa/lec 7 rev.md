# ----- الدوال التجميعية  (Aggregate Functions) -------


## 1) ما هي الدوال التجميعية؟ ولماذا نستخدمها؟

**الدوال التجميعية** بتحسب قيمة واحدة تلخّص مجموعة صفوف.
بنستخدمها للإحصائيات والتقارير: عدد الموظفين، متوسط الرواتب، أعلى راتب، أقل راتب… إلخ.

أشهرها:

- `COUNT()`                    
- `SUM()`                 
- `AVG()`                   
- `MIN()`                          
- `MAX()`

**فكرتها :**

> لو عندك جدول كبير الدالة  "بتجمع" بيانات كتير وتديك "رقم ملخّص" (أو سطر ملخّص لكل مجموعة).

---
## 2) COUNT() — العد

**الصيغة الأساسية:**
```sql
-- يعد كل الصفوف
SELECT COUNT(*)

-- يعد الصفوف التي بها قيمة غير NULL في العمود المحدد
SELECT COUNT(ColName)

-- يعد القيم المميّزة فقط (يلغي التكرارات)
SELECT COUNT(DISTINCT ColName)
```

**أمثلة :**
```sql
-- (MyCompany.Employee)
SELECT COUNT(*), COUNT(ssn), COUNT(dno)
FROM Employee;
```

- ا `COUNT(*)`: إجمالي عدد الموظفين (حتى لو `ssn` أو `dno` NULL).
- ا `COUNT(ssn)`: عدد الموظفين الذين لديهم SSN غير NULL.
- ا `COUNT(dno)`: عدد الموظفين الذين لديهم رقم قسم غير NULL.

**متى أستخدمه؟** لمعرفة الحجم: كام موظف؟ كام فاتورة؟ كام طلب؟

---

## 3) SUM() و AVG() — الجمع والمتوسط

**SUM()** يجمع قيم رقمية.  
**AVG()** يحسب المتوسط الحسابي للقيم غير NULL.

```sql
SELECT SUM(salary)       FROM Employee;       -- إجمالي الرواتب
SELECT AVG(salary)       FROM Employee;       -- متوسط الرواتب
SELECT SUM(salary) / COUNT(salary) FROM Employee; -- زي AVG لكن انتبه لـ NULL
```

> **ملحوظة مهمة:** لو في قيم NULL في `salary`، فـ `AVG(salary)` = `SUM(salary)/COUNT(salary)` (الاتنين يتجاهلوا NULL).  
> **لكن لو استخدمت** `SUM(salary)/COUNT(*)` ممكن يطلع أصغر من الحقيقي لأن `COUNT(*)` بيعد صفوف حتى لو راتبها NULL.

**أخطاء النوع:**

```sql
SELECT SUM(Fname) FROM Employee;
-- خطأ: nvarchar لا ينفع مع SUM
```

**تفادي القسمة الصحيحة:**

```sql
SELECT CAST(SUM(salary) AS decimal(18,2)) / NULLIF(COUNT(salary),0)
FROM Employee;
-- CAST لضمان كسور عشرية، و NULLIF لتجنّب القسمة على صفر لو مفيش رواتب
```

**DISTINCT مع SUM/AVG:**

```sql
SELECT SUM(DISTINCT salary) FROM Employee; -- يجمع القيم المختلفة فقط
SELECT AVG(DISTINCT salary) FROM Employee; -- متوسط القيم المختلفة فقط
```

---

## 4) MIN() و MAX() — الصغرى والعظمى

- تعمل مع الأرقام والتواريخ والنصوص.
- مع **النصوص**، المقارنة **قاموسية/أبجدية** (Lexicographical).

- مثال:
```sql
SELECT MAX(Fname) AS [Max Fname], MIN(Fname) AS [Min Fname]
FROM Employee;
```

**استخدام شائع:** أعلى/أقل راتب، أقدم/أحدث تاريخ تعيين.

---

## 5) GROUP BY — التجميع حسب عمود/أكثر

```sql
SELECT Dno, MIN(salary) AS MinSalary, MAX(salary) AS MaxSalary
FROM Employee
GROUP BY Dno;  -- سطر لكل قسم
```

- كل قسم (**Dno**) هيطلع له سطر فيه أقل وأعلى راتب.
- أي عمود خارج دوال التجميعية **لازم** يكون في `GROUP BY`.
    

**HAVING مثال:**

```sql
-- الأقسام التي متوسط راتبها >= 10000
SELECT Dno, AVG(salary) AS AvgSalary
FROM Employee
GROUP BY Dno
HAVING AVG(salary) >= 10000;
```

**WHERE قبل التجميع:**

```sql
-- اعتبر فقط الموظفين الذين لديهم Dno غير NULL
SELECT Dno, COUNT(*) AS Cnt
FROM Employee
WHERE Dno IS NOT NULL
GROUP BY Dno;
```

---

## 7) ا Window Functions مع OVER(PARTITION BY)

**الفكرة:** ترجع قيمة تجميعية **لكل صف** بدون ما تقلّل عدد الصفوف (عكس GROUP BY).

```sql

SELECT *, MIN(salary) OVER (PARTITION BY Dno) AS DeptMinSalary
FROM Employee;

-- نفس القيمة لكن صف واحد لكل قسم باستخدام DISTINCT (أو GROUP BY)
SELECT DISTINCT Dno, MIN(salary) OVER (PARTITION BY Dno) AS DeptMinSalary
FROM Employee;
```

**متى أستخدمها؟** لو عايز كل صف يحتفظ بتفاصيله + تضيف له إحصائية على مستوى القسم/المجموعة.

> **مقارنة سريعة**

- `GROUP BY`: يُرجع **سطر واحد لكل مجموعة**.
    
- `OVER(PARTITION BY)`: يُرجع **قيمة لكل صف** داخل مجموعته.
    

---

## 8) أمثلة عملية

### 8.1 

```sql
USE MyCompany;

-- 1) إحصائيات عامة
SELECT COUNT(*) AS TotalRows,
       COUNT(ssn) AS WithSSN,
       COUNT(dno) AS WithDept
FROM Employee;

-- 2) إجمالي ومتوسط الرواتب
SELECT SUM(salary) AS TotalSalary,
       AVG(salary) AS AvgSalary
FROM Employee;

-- 3) أعلى/أقل راتب على مستوى الشركة
SELECT MAX(salary) AS [Max salary],
       MIN(salary) AS [Min salary]
FROM Employee;

-- 4) أعلى/أقل راتب لكل قسم
SELECT Dno,
       MIN(salary) AS MinSalary,
       MAX(salary) AS MaxSalary
FROM Employee
GROUP BY Dno;

-- 5) أقل راتب لكل موظف حسب قسمه (بدون تقليل الصفوف)
SELECT E.*, MIN(salary) OVER (PARTITION BY Dno) AS DeptMin
FROM Employee AS E;

-- 6) الفرق بين AVG و SUM/COUNT
SELECT AVG(salary) AS Avg1,
       CAST(SUM(salary) AS decimal(18,2))/NULLIF(COUNT(salary),0) AS Avg2
FROM Employee;
```

### 8.2 مثال للبيانات 

|SSN|Fname|Dno|Salary|
|---|---|---|---|
|1|Ali|10|8000|
|2|Sara|10|10000|
|3|Omar|20|NULL|
|4|Mona|20|12000|

- `COUNT(*)` = 4
    
- `COUNT(salary)` = 3 (لأن قيمة واحدة NULL)
    
- `SUM(salary)` = 8000 + 10000 + 12000 = 30000
    
- `AVG(salary)` = 30000 / 3 = 10000
    
- `MIN/MAX` لكل قسم:
    
    - قسم 10: Min=8000, Max=10000
        
    - قسم 20: Min=12000, Max=12000 (لأن NULL مُتجاهَلة)
        

### 8.3 نص + MIN/MAX

```sql
SELECT MAX(Fname) AS MaxFname, MIN(Fname) AS MinFname
FROM Employee; -- مقارنة أبجدية
```

---

## 11) تمرين صغير 🚶 

1. هات عدد الموظفين الكلّي، وعدد من لديهم راتب غير NULL، وعدد من لديهم قسم غير NULL.
2. هات إجمالي الرواتب ومتوسطها للشركة ككل.
3. هات أعلى/أقل راتب لكل قسم.
4. هات الأقسام التي عدد موظفيها ≥ 5.
5. هات الموظفين مع إضافة عمود يوضح **أقل راتب في قسم كل موظف** (باستخدام `OVER`).
6. احسب **متوسط الرواتب المميّزة فقط** (باستخدام `AVG(DISTINCT salary)`).


---


# ----- دوال التعامل مع القيم الفارغة - NULL Functions -----


### 1) ال NULL في قواعد البيانات

- يعني: _"لا توجد قيمة"_ أو _"غير معروف"_ أو _"فارغ"_.
- يختلف عن القيم الفارغة مثل `''` (سلسلة نصية فارغة) أو `0` (صفر عددي).
    

---

## 2) لماذا التعامل مع NULL مهم؟

- NULL ممكن يغيّر نتائج الاستعلامات الحسابية.
    
- ا NULL لا يساوي أي شيء، حتى NULL آخر (يعني `NULL = NULL` → النتيجة `UNKNOWN` وليس TRUE).
- لو ما تعاملتش مع NULL، ممكن تطلع نتائج ناقصة أو غير دقيقة.
    

مثال:

```sql
SELECT salary + 1000 FROM Employee;
-- if salary = NULL → النتيجة NULL، مش هتحصل على 1000
```

---

## 3) الدوال الأساسية: ISNULL() و COALESCE()

### 3.1 ISNULL()

**الصيغة:**
```sql
ISNULL(expression, replacement_value)
```

- لو `expression` = NULL → تستبدلها بالـ `replacement_value`.
- لو مش NULL → ترجع القيمة الأصلية.
    

**مثال :**
```sql
SELECT ISNULL(Lname, 'Not found')
FROM Employee;
```

- لو `Lname` = NULL → تظهر `'Not found'`.
    
- لو `Lname` فيها قيمة → تظهر كما هي.
    

---

### 3.2 COALESCE()
فكرتها: 
دالة في SQL بتدور على **أول قيمة مش NULL** في قائمة قيم، وترجعها.  
لو كل القيم NULL، هترجع القيمة الافتراضية اللي انت حاططها في الآخر.

- بنحط بديل أو قيمة احتياطية تظهر بدل الـNULL للمستخدم.
- بتسهل كتابة الكود بدل ما نعمل `CASE` طويل.

**الصيغة:**

```sql
COALESCE(expr1, expr2, expr3, ..., default_value)
```

- يبدأ من الشمال → أول قيمة مش NULL هيرجعها.
- لو كل القيم NULL → يرجع `default_value`.
    
**مثال :**

```sql
SELECT COALESCE(Lname, Address, Fname, 'Not found')
FROM Employee;
```

1. شوف `Lname` → لو موجود ارجعه.
2. لو NULL → جرب `Address`.
3. لو NULL → جرب `Fname`.
4. لو NULL → ارجع `'Not found'`.
    

---

## 4) دمج النصوص مع NULL

### 4.1 ISNULL داخل النصوص

```sql
SELECT Lname + ' ' + ISNULL(Fname, ' ')
FROM Employee;
```

- لو `Fname` NULL → تحط مسافة بدلها.
    

### 4.2 CONCAT() — أسهل للتعامل مع NULL

```sql
SELECT CONCAT(Lname, ' ', Fname)
FROM Employee;
```

- `CONCAT` تعتبر NULL = سلسلة فارغة.
    

### 4.3 CONCAT_WS() — مع فاصل Separator

```sql
SELECT CONCAT_WS('-', Fname, Lname, Address, SSN)
FROM Employee;
```

- لو قيمة NULL → تتجاهلها وتحافظ على الفاصل.
    
- مثال: لو Address = NULL → الناتج ممكن يكون: `Ali-Ahmed-12345` (بدون فاصل مزدوج).
    

---

## 5) الفروق بين ISNULL و COALESCE

| الخاصية                 | ISNULL        | COALESCE                  |
| ----------------------- | ------------- | ------------------------- |
| عدد المعاملات           | 2 فقط         | 2 أو أكثر                 |
| معيار ANSI SQL          | لا            | نعم                       |
| التوافق مع أنواع مختلفة | يحاول التحويل | يختار نوع البيانات الأعلى |
| الأداء                  | أسرع قليلاً   | مرن أكثر                  |

---

## 6) أمثلة 

```sql
-- 1) استبدال القيم الفارغة في Lname بكلمة "Not found"
SELECT ISNULL(Lname, 'Not found')
FROM Employee;

-- 2) أول قيمة غير NULL من Lname أو Address أو SSN
SELECT COALESCE(Lname, Address, SSN, 'Not found')
FROM Employee;

-- 3) دمج الاسم الكامل حتى لو Fname أو Lname NULL
SELECT CONCAT(Lname, ' ', Fname)
FROM Employee;

-- 4) دمج البيانات مع فاصل - وتجاهل القيم NULL
SELECT CONCAT_WS('-', Fname, Lname, Address, SSN)
FROM Employee;
```

---


##  تمرين صغير 🚶 

1. استبدل `Address` الفارغ بكلمة `'Unknown'`.
2. اكتب استعلام يرجع أول قيمة غير NULL من `Email` أو `Phone` أو كلمة `'No Contact'`.
3. ادمج `Fname` و `Lname` في عمود واحد حتى لو إحداهما NULL.
4. جرّب الفرق بين `ISNULL` و `COALESCE` إذا أعطيت أنواع بيانات مختلفة.
    

---

# -------دوال التاريخ والوقت Date & Time Functions ----

1 نسخدمها في: - تقارير يومية/شهرية، حساب مدد، استخراج اليوم/الشهر/السنة، ومعالجة مواعيد (Deadlines).
    

### 2 جلب التاريخ/الوقت الحالي

```sql
SELECT SYSDATETIME();      -- دقة عالية (datetime2) بالتوقيت المحلي للسيرفر
SELECT SYSUTCDATETIME();   -- نفس الدقة لكن بتوقيت UTC
SELECT GETDATE();          -- تاريخ/وقت محلي (datetime)
SELECT GETUTCDATE();       -- تاريخ/وقت UTC (datetime)
```


- لو محتاج دقة نانو ثانية تقريبًا → `SYSDATETIME()` / `SYSUTCDATETIME()`.
- لو كافي ثواني/مللي ثانية → `GETDATE()` / `GETUTCDATE()`.
- لو بتسجّل أحداث عالمية (Logs) الأفضل تخزن UTC لتجنّب مشاكل المناطق الزمنية.

---

### 3.3 استخراج أجزاء من التاريخ

```sql
SELECT DATENAME(day, GETDATE()); -- اسم الجزء كنص (مثلاً "14")
SELECT DAY('2024-11-14');        -- 14
SELECT MONTH('2024-11-14');      -- 11
SELECT YEAR('2024-11-14');       -- 2024
```

> **ملحوظة:** استخدم صيغة تاريخ آمنة وغير ملتبسة مثل `YYYY-MM-DD` أو الأفضل `YYYYMMDD`.

---

### 3.4 الفرق بين تاريخين: `DATEDIFF`

```sql
-- كام يوم من النهاردة لغاية 10-10-2025 (انتبه للصيغة)
SELECT DATEDIFF(day, GETDATE(), '2025-10-10');
```

- `DATEDIFF` يحسب **عدد حدود الوحدات** التي تم تجاوزها (Days/Months/Years). في الغالب ده يساوي الفرق الفعلي في الأيام عندما تتعامل مع تواريخ بدون ساعات.
    

---

### 3.5 نهاية الشهر: `EOMONTH`

```sql
SELECT EOMONTH('2024-11-14'); -- 2024-11-30
SELECT EOMONTH('1950-02-14'); -- 1950-02-28
SELECT EOMONTH('2018-02-14'); -- 2018-02-28
SELECT EOMONTH('2019-02-14'); -- 2019-02-28
SELECT EOMONTH('2020-02-14'); -- 2020-02-29 (سنة كبيسة)
SELECT EOMONTH('2021-02-14'); -- 2021-02-28
```

**فايدة مهمة:** بتحدد آخر يوم في الشهر بسهولة (للتقارير الشهرية، إقفال الحسابات، إلخ).

---

### 3.6 التحقق من صلاحية التاريخ: `ISDATE`

```sql
SELECT ISDATE('2021-02-14'); -- 1 لو تاريخ صالح، 0 لو غير صالح
SELECT ISDATE('Minya');      -- 0
```

> **ملاحظة:** `ISDATE` ممكن يتأثر بإعدادات اللغة/التنسيق. البديل الأحدث والأدق غالبًا هو `TRY_CONVERT(date, ... )` أو `TRY_CAST(... AS date)` (تشوفها في فصل التحويل).

---

### 3.7 صيغ التاريخ وأفضل الممارسات

- **تجنّب** تواريخ بصيغة ملتبسة زي `'10-11-2025'` (دي يوم 10 ولا شهر 10؟).
- استخدم **`YYYYMMDD`** (مثال: `20251011`) أو **`YYYY-MM-DD`** لتقليل اللبس.
- عند العرض للمستخدمين، استخدم `FORMAT` أو `CONVERT` بـ **Style** مناسب.
    

---

### 3.8 أمثلة عملية 

```sql
SELECT SYSDATETIME();       -- 2025-08-14 12:30:41.3592251 (مثال)
SELECT SYSUTCDATETIME();    -- 2025-08-14 19:31:12.1258336
SELECT GETDATE();           -- 2025-08-14 12:33:10.760
SELECT GETUTCDATE();        -- 2025-08-14 19:33:41.627

SELECT DATENAME(day, GETDATE());             -- مثلاً 14
SELECT DATEDIFF(day, GETDATE(), '2025-10-10');

SELECT DAY('2024-11-14');  -- 14
SELECT MONTH('2024-11-14');-- 11
SELECT YEAR('2024-11-14'); -- 2024

SELECT EOMONTH('2024-11-14');
```

---

### 3.9 أخطاء شائعة ونصائح

1. إدخال تاريخ بصيغة ملتبسة → نتائج غير متوقعة. استخدم `YYYYMMDD`.
2. الخلط بين **التخزين** و**العرض**: خزّن كـ `date/datetime2`، ونسّق عند العرض فقط.
3. تجاهل المنطقة الزمنية: سجّل UTC لو السيستم متعدد المناطق.

---

## # -------- دوال التحويل — Conversion Functions --------------

### **أولًا: ليه بنحوّل أنواع البيانات؟**
في SQL Server، البيانات ممكن تيجي بأشكال مختلفة:
* نص (string)
* أرقام (int, decimal)
* تواريخ (date, datetime)
* عملات (money)

لكن أحيانًا بنحتاج نغير النوع علشان:
1. **نعرضها بشكل معيّن** (مثلاً التاريخ بصيغة 14/08/2025 بدل 2025-08-14).
2. **نجمع بين أنواع مختلفة** (مثلاً نضم الاسم + الراتب في نص واحد).
3. **نحوّل النص لرقم أو تاريخ** قبل نعمل عليه عمليات حسابية أو مقارنة.


---

### 1️⃣ **CAST**

الصيغة:
```sql
CAST(القيمة AS النوع)
```

* طريقة قياسية (standard) موجودة في SQL وأنظمة أخرى.
* سهلة وبسيطة.
* مفيهاش اختيار style للتواريخ.

مثال:
```sql
SELECT CAST(123.45 AS int); -- النتيجة: 123
```

---

### 2️⃣ **CONVERT**

الصيغة:
```MYsql
CONVERT(النوع, القيمة [, style])
```

* خاصة بـ SQL Server.
* تقدر تحدد **style** لتنسيق التاريخ أو العملة.

مثال:
```sql
DECLARE @Today datetime = GETDATE();
SELECT CONVERT(varchar, @Today, 103); -- 14/08/2025
```

💡 **الخلاصة**:

* لو عايز تحويل بسيط → استخدم CAST.
* لو محتاج تتحكم في شكل التاريخ/الرقم → استخدم CONVERT.
>ال** `FORMAT` (المبني على .NET) أنيق لكن أبطأ من `CONVERT`/`CAST`—في التقارير الكبيرة استخدم `CONVERT`.

---
### 4.3 أنماط/Styles شائعة للتواريخ والاكثر استخداما

- **101**: `mm/dd/yyyy`
- **103**: `dd/mm/yyyy`
- **104**: `dd.mm.yyyy`
- **105**: `dd-mm-yyyy`
- **106**: `dd Mon yyyy`
- **126**: `yyyy-mm-ddThh:mi:ss` (ISO 8601 — مفيد للتبادل)
- **130/131**: تنسيقات هجري عربية

---

## **TRY\_CAST و TRY\_CONVERT**

* نفس فكرة CAST و CONVERT لكن:
* **لو التحويل فشل، يرجع NULL بدل ما يرمي Error.**

مثال:
```sql

SELECT TRY_CAST('Ali' AS int); -- NULL (لأن النص مش رقم)

SELECT TRY_CONVERT(date, '14/08/2025', 103); -- يحولها لتاريخ

```

💡 مهم لما تتعامل مع بيانات جاية من مستخدم أو ملفات خارجية.

---

## **PARSE و TRY\_PARSE**

* أقوى في قراءة النصوص بصيغ مختلفة مع تحديد الثقافة (Culture) مثل en-US أو fr-FR.
* أبطأ من CAST/CONVERT.
* مفيد لو عندك نصوص جاية من دول مختلفة.

مثال:
```sql
SELECT PARSE('Thursday, 14 November 2024' AS date USING 'en-US');
SELECT PARSE('$300.89' AS money USING 'en-US');
SELECT TRY_PARSE('Ali' AS money USING 'en-US'); -- NULL
```

---

## **أمثلة عملية**

```sql
-- دمج الاسم + الراتب كنص واحد
SELECT Fname + '_' + ISNULL(CONVERT(varchar(20), salary), ' ')
FROM Employee;

-- عرض التاريخ بأكثر من شكل
DECLARE @Today datetime = GETDATE();
SELECT CONVERT(varchar, @Today, 101); -- 08/14/2025
SELECT CONVERT(varchar, @Today, 103); -- 14/08/2025
SELECT CONVERT(varchar, @Today, 105); -- 14-08-2025

-- التحويل الآمن
SELECT TRY_CAST('2025-08-14' AS date); -- تاريخ صحيح
SELECT TRY_CAST('14/08/2025' AS date); -- NULL حسب الإعدادات
```
---

## **أخطاء شائعة**

1. **نسيان تحديد style للتواريخ** → ممكن يعرضها بصيغة غير متوقعة.
2. **استخدام PARSE في كل حاجة** → هي أبطأ، استخدمها بس وقت الحاجة.
3. **عدم استخدام TRY\_* في البيانات غير المضمونة*\* → ممكن الاستعلام يقف بسبب خطأ تحويل.
4. **استخدام FORMAT في تقارير ضخمة** → بطيء جدًا مقارنة بـ CONVERT.


---
## تمرين صغير 🚶 

1. احسب عدد الأيام المتبقية حتى نهاية الشهر الحالي باستخدام `EOMONTH` + `DATEDIFF`.
2. خزّن التاريخ كـ `date` ثم اعرضه بصيغ: 101 و103 و106.
3. حوّل السلسلة `'14/08/2025'` إلى `date` بأمان (بدون أخطاء) مهما كانت اللغة.
4. حوّل نص مبلغ `'300,89'` إلى `money` مرّة بالثقافة الفرنسية وأخرى بالإنجليزية—واشرح الفرق.


---

# ---------- دوال النصوص  (String Functions) ------

## 1) لماذا نستخدم دوال النصوص؟

- تجهيز عرض البيانات في التقارير (اسم كامل، رقم به فواصل، تاريخ منسّق…).
- تنظيف وتجهيز البيانات قبل المقارنة أو البحث.
- تحويل مخرجات رقمية/تاريخية إلى نصوص مفهومة للمستخدم النهائي.

---

## 2) ---- دمج النصوص: ` (+) `  `CONCAT` vs `CONCAT_WS` VS ---

### 2.1 عامل الجمع النصّي( `+` )

```sql
SELECT Lname + ' ' + ISNULL(Fname, ' ') AS FullName
FROM Employee;
```

- **المشكلة:** لو أي جزء `NULL` النتيجة كلها **NULL**. لذلك استخدم `ISNULL` لتفاديه.
    

### 2.2 ا`CONCAT()` — الأسهل والأكثر أمانًا مع NULL

```sql
SELECT CONCAT(Lname, ' ', Fname) AS FullName
FROM Employee;
```

- يعامل `NULL` كسلسلة فارغة "" (لا يُسقط النتيجة).
    

### 2.3 ا `CONCAT_WS(sep, ...)` — دمج مع فاصل ثابت

```sql
SELECT CONCAT_WS('-', Fname, Lname, Address, SSN) AS ContactLine
FROM Employee;
```

- ا **WS = With Separator** (بفاصل ثابت).
    - بيتجاهل القيم الفاضية أو `NULL` وما يحطش فواصل زيادة.

>📌 **متى تستخدم إيه؟**
- عندك احتمال وجود NULL → استخدم `CONCAT` أو `CONCAT_WS`.
- عايز تحكم يدوي → استخدم `+` مع `ISNULL`.

---

## 3) تغيير حالة الحروف: `LOWER` / `UPPER`

```sql
SELECT LOWER(Fname) AS FirstLower, UPPER(Lname) AS LastUpper
FROM Employee;
```

- مفيد لتوحيد الإدخال قبل المقارنة/الفرز.
    

> **ملاحظة Collation:** المقارنة قد تكون حساسة لحالة الحروف أو لا حسب **Collation** قاعدة البيانات، لكن `LOWER/UPPER` دائمًا تغيّر الشكل المعروض.

---

## 4) طول النص: `LEN`

```sql
SELECT LEN(Lname) AS NameLength
FROM Employee;
```

- بيعطيك عدد الحروف.
- **مهم:** ما بيحسبش المسافات في آخر النص. لو مهم تحسبها، استخدم `DATALENGTH`.

---

## 5) استخراج جزء من النص: `SUBSTRING`

- **SUBSTRING(النص, البداية, الطول)**
```sql
SELECT Lname,
       SUBSTRING(Lname, 2, 3) AS Mid3
FROM Employee;
```

- يبدأ العد من **1** (وليس 0).
- المثال يستخرج 3 حروف ابتداءً من الحرف الثاني.

---

## 6) تنسيق العرض: `FORMAT`

### 6.1 أرقام

```sql
SELECT FORMAT(Salary, '###,###') AS SalaryWithCommas
FROM Employee;

SELECT FORMAT(764245354523, '###,###') AS BigNumber; --> 764,245,354,523
```

- يعرض فواصل الآلاف بسهولة.
    

### 6.2 تواريخ

```sql
DECLARE @today date = GETDATE();
SELECT FORMAT(@today, 'd', 'en-US');  -- 8/14/2025
SELECT FORMAT(@today, 'd', 'ar-sa');  -- 20/02/47 (هجري)
SELECT FORMAT(@today, 'D', 'en-US');  -- Thursday, August 14, 2025
SELECT FORMAT(@today, 'D', 'ar-sa');  -- 20/صفر/1447

SELECT FORMAT(@today, 'dd MM yy');            -- 14 08 25
SELECT FORMAT(@today, 'ddd MMM yyy');         -- Thu Aug 2025
SELECT FORMAT(@today, 'dddd MMMM yyyy');      -- Thursday August 2025
SELECT FORMAT(@today, 'dddd MMMM yyyy hh:mm:ss tt', 'en-US');
SELECT FORMAT(@today, 'dddd MMMM yyyy hh:mm:ss tt', 'ar-sa');
```

> **تنبيه أداء:** `FORMAT` أنيق لكن أبطأ من `CONVERT`. استخدمه للعرض النهائي، وليس داخل استعلامات ضخمة ثقيلة.

---

## 7) مثال شامل 

```sql
-- دمج اسم العائلة مع الاسم الأول مع معالجة NULL
SELECT Lname + ' ' + ISNULL(Fname, ' ') AS FullName
FROM Employee;

-- نفس الفكرة لكن بأسلمية أعلى مع NULL
SELECT CONCAT(Lname, ' ', Fname) AS FullName
FROM Employee;

-- دمج بقواسم ثابتة مع تجاهل NULL
SELECT CONCAT_WS('-', Fname, Lname, Address, SSN) AS Line
FROM Employee;

-- تغيير حالة الحروف وطول الاسم
SELECT LOWER(Fname) AS FLower, UPPER(Lname) AS LUpper, LEN(Lname) AS LLen
FROM Employee;

-- جزء من الاسم
SELECT Lname, SUBSTRING(Lname, 2, 3) AS Mid3
FROM Employee;

-- تنسيق الأرقام والتواريخ
SELECT FORMAT(Salary, '###,###') AS SalaryWithCommas FROM Employee;
DECLARE @t date = GETDATE();
SELECT FORMAT(@t, 'D', 'en-US') AS UsLong, FORMAT(@t, 'D', 'ar-sa') AS ArLong;
```

---

**تمرين صغير 🚶 :**

1. كوّن عمود `FullName` يدمج `Fname` و`Lname` بأمان لو في `NULL`.
2. أنشئ `UserLine` بصيغة: `Fname-Lname-SSN` مع تجاهل أي قيمة NULL تلقائيًا.
3. استخرج أول 2 حرف وآخر 2 حرف من `Lname` (تلميح: `SUBSTRING` و `LEN`).
4. اعرض `Salary` بفواصل آلاف، ثم جرّب نفس الفكرة بـ `CONVERT` لمقارنة الأداء.

---


# ----------------------- GROUP BY (التجميع) ------------


## 1) الفكرة ببساطة

`GROUP BY` في SQL بتخلّي قاعدة البيانات **تجمع الصفوف اللي ليها نفس القيمة في عمود معيّن أو أكتر**، وبعدها نقدر نستخدم دوال تجميعية زي:

* **COUNT** → عدد الصفوف
* **SUM** → مجموع القيم
* **AVG** → متوسط القيم
* **MIN** → أقل قيمة
* **MAX** → أكبر قيمة

💡 **مثال :**

تخيل عندك ملف فيه بيانات موظفين. لو عايز تعرف **إجمالي المرتبات في كل قسم**، هتجمّع الموظفين حسب رقم القسم (`GROUP BY Dno`) وبعدين تحسب المجموع.

## 2) إمتى بستخدمها؟

* عدد الطلاب في كل قسم.
* أكبر وأصغر راتب في كل فرع.
* متوسط المبيعات لكل شهر.
* عدد الطلبات لكل عميل.

## 3) القواعد الأساسية

1. أي عمود في `SELECT` مش داخل دالة تجميعية **لازم** يكون في `GROUP BY`.
2. الترتيب في `GROUP BY` مش بيأثر على الحساب، بس ممكن يغيّر ترتيب العرض.
3. ا **WHERE** بيصفّي الصفوف قبل التجميع، و **HAVING** بيصفّي المجموعات بعد التجميع.
4. الناتج من `GROUP BY` بيكون **سطر واحد لكل مجموعة**.

---

## 4) أمثلة عملية

### مثال 1: أقل وأعلى راتب لكل قسم

```sql
SELECT Dno, MIN(salary) AS MinSalary, MAX(salary) AS MaxSalary
FROM Employee
GROUP BY Dno;
```

> الناتج: سطر واحد لكل `Dno` فيه أقل وأعلى راتب في القسم.

### مثال 2: عدد الموظفين في كل قسم مع استبعاد الأقسام الفارغة

```sql
SELECT Dno, COUNT(*) AS EmpCount
FROM Employee
WHERE Dno IS NOT NULL
GROUP BY Dno;
```

### مثال 3: الأقسام التي متوسط راتبها >= 10000 (استخدم HAVING)

```sql
SELECT Dno, AVG(salary) AS AvgSalary
FROM Employee
GROUP BY Dno
HAVING AVG(salary) >= 10000;
```

> هنا `HAVING` طلّع فقط المجموعات (الأقسام) اللي تحقق الشرط بعد الحساب.

### مثال 4: تجميع على أكثر من عمود 

```sql
USE iti;
SELECT Dept_Id, St_Address, COUNT(*) AS [Number of students per Department]
FROM student
WHERE Dept_Id IS NOT NULL
GROUP BY Dept_Id, St_Address;
```

> سيعطي سطرًا لكل مجموعة فريدة من `Dept_Id` و `St_Address`.

### مثال 5: استخدام `COUNT(DISTINCT ...)`

```sql
SELECT Dept_Id, COUNT(DISTINCT StudentID) AS UniqueStudents
FROM student
GROUP BY Dept_Id;
```

> `COUNT(DISTINCT ...)` يحسب القيم المميّزة فقط داخل كل مجموعة.

---

## 5) مقارنة سريعة: `GROUP BY` vs `OVER(PARTITION BY)`

- `GROUP BY` يُقلّل عدد الصفوف ويرجع سطر لكل مجموعة.
    
- `OVER (PARTITION BY ...)` (دوال النافذة) لا تقلّل الصفوف؛ تعطي قيمة تجميعية لكل صف داخل مجموعته.
    

**مثال توضيحي :**

```sql
-- كل صف سيحمل قيمة أقل راتب في قسمه ولكن الصفوف كلها تبقى
SELECT E.*, MIN(salary) OVER (PARTITION BY Dno) AS DeptMinSalary
FROM Employee E;
```

---

## 6) سلوك خاص: NULLs في التجميع

- القيم `NULL` في عمود التجميع تُعتبر قيمة واحدة — يعني كل الصفوف التي فيها `Dno = NULL` تتجمع معًا في مجموعة واحدة.
    

---

## 7) أخطاء شائعة وكيف تتجنبها

- **نسيان إضافة عمود غير مجمع في `GROUP BY`** → خطأ في الاستعلام.
- **استخدام `WHERE` بدل `HAVING`** عندما تريد تصفية بناءً على aggregate → لا يعمل.
- **الخلط بين `COUNT(*)` و `COUNT(col)`**: الأولى تعد كل الصفوف، الثانية تعد فقط الصفوف التي القيمة فيها ليست NULL.
- **قسمة على صفر** عند حساب المتوسط من `SUM/COUNT` — استخدم `NULLIF(COUNT(...),0)` لتفادي القسمة على صفر.

---

## 8)   مثال: 

جدول `Employee`:

|SSN|Fname|Dno|Salary|
|---|---|---|---|
|1|Ali|10|8000|
|2|Sara|10|10000|
|3|Omar|20|NULL|
|4|Mona|20|12000|
|5|John|NULL|7000|

استعلام:

```sql
SELECT Dno, COUNT(*) AS Cnt, AVG(salary) AS AvgSal
FROM Employee
GROUP BY Dno;
```

الناتج المتوقّع:

- Dno = 10 → Cnt = 2, AvgSal = (8000+10000)/2 = 9000
- Dno = 20 → Cnt = 2, AvgSal = 12000 (NULL متجاهلة في AVG)
- Dno = NULL → Cnt = 1, AvgSal = 7000


---

## 9) تمرين صغير 🚶  

1. اكتب استعلامًا يجيب `Dno` وإجمالي الرواتب لكل قسم، مع استبعاد الأقسام التي إجمالي رواتبها أقل من 20000.
2. اكتب استعلامًا يجيب `Dno` وعدد الموظفين فيه، ورتب الناتج تنازليًا حسب عدد الموظفين.
3. اكتب استعلامًا يجيب `Dept_Id` و`St_Address` وعدد الطلاب، ثم اختَر فقط الصفوف التي عدد طلابها >= 5.
