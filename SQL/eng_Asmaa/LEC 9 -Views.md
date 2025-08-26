
# :-) ايه هو الـ View وليه نستخدمه؟

- الـ **View** هو **جدول وهمي (Virtual Table)**.
- يعني هو **مش بيخزن بيانات بنفسه**، لكنه بيعرض نتيجة استعلام (Query) محفوظ في قاعدة البيانات.
- كل مرة تنادي على الـ View، السيرفر بيروح يشغل الاستعلام اللي جواه ويرجعلك النتيجة.

---

1. **تبسيط الاستعلامات المعقدة**
    - لو عندك كويري طويل ومعقد بيوصل بيانات من كذا جدول (JOINs)، ممكن تخزنه كـ View وتستخدمه كأنه جدول عادي.
    - كده بدل ما تعيد كتابة الكويري كل مرة، هتنادي على الـ View وخلاص.
        
2. **الفصل بين المستخدم والجداول الأصلية (Security Layer)**
    - المستخدم ممكن يشوف View من غير ما يعرف أسماء أو تفاصيل الجداول الحقيقية.
    - ده بيزود الأمان (تقدر تخلي المستخدم يشوف أعمدة محددة فقط).
        
3. **إعادة الاستخدام (Reusability)**
    - بدل ما تكرر نفس الاستعلام في كذا مكان، تكتبه مرة واحدة وتخزنه كـ View.
        
4. **مرونة التغيير (Flexibility)**
    - لو غيرت الكويري داخل الـ View، التطبيقات اللي بتستخدمه مش هتتأثر، لأنها لسه شايفاه كأنه جدول ثابت.
        
5. **تحسين الأداء 
    - الـ View العادي مش بيخزن بيانات → فكل مرة بيروح يجيبها من الجداول.
    - لكن في حالة خاصة اسمها **Indexed View** (لما تعمل فهرس عليه) البيانات بتتخزن فعلاً جواه، وده ممكن يسرع الأداء في بعض الحالات.
        
---
###  امتى أستخدم الـ View؟

- لما تحب **تخفي التفاصيل** عن المستخدم (زي أسماء الجداول أو الأعمدة).
- لما تحتاج **واجهة مبسطة** للوصول للبيانات بدل كتابة استعلامات طويلة ومعقدة.
- لما عندك **صلاحيات** وعايز المستخدم يشوف بيانات معينة بس (مثلاً الطلاب من القاهرة بس).
- لما التطبيق محتاج **ثبات في البنية** حتى لو غيرت الكويري الداخلي.
- في حالات تحسين الأداء باستخدام **Indexed View** لو عندك استعلام بيتكرر كتير وبيكون مكلف.
    
---
# 2) إنشاء View —

```sql
CREATE VIEW view_name  -- بعمل View جديدة وبدّيها اسم (مثلاً `v_Employees`).

AS -- الكلمة المفتاحية اللي بعدها هنكتب الاستعلام.

SELECT column1, column2, ...

FROM table_name --→ الاستعلام اللي هيشتغل كل مرة نستدعي الـ View.

WHERE condition; --→ (اختياري) لو عايز تعرض داتا معينة فقط.
```

---

##### مثال 1 — View بسيط لطلاب القاهرة
```sql
-- إنشاء أو تعديل فيو يُعيد طلاب القاهرة
CREATE OR ALTER VIEW dbo.VCairoStudents
WITH ENCRYPTION  -- اختياري: يخفي الكود عند استخدام sp_helptext
AS
SELECT
    St_Id      AS [StudentCode],    -- إعادة تسمية العمود لواجهة أبسط
    St_Fname   AS [StudentName],
    St_Address AS [StudentAddress]
FROM dbo.Student
WHERE St_Address = 'Cairo';
GO

-- قراءة البيانات من الـ View
SELECT * FROM dbo.VCairoStudents;
-- ملاحظة: لو مش مشفّر، تقدر تشوف تعريفه:
-- EXEC sp_helptext 'dbo.VCairoStudents';
```

**** هنا الـ View بيبقى مجرد اختصار لاستعلام `SELECT ... FROM Student WHERE St_Address='Cairo'`.  
إضافة `WITH ENCRYPTION` تمنع رؤية النص المصدرى (`sp_helptext`) لكن تفرض قيود (متبوعة لاحقاً).

مثال آخر: الطلاب من **Alex**:

``` SQL
CREATE OR ALTER VIEW dbo.VAlexStudents

 AS SELECT
	 St_Id,
	 St_Fname,
	 St_Address
	 
 FROM Student
 WHERE St_Address = 'Alex'; GO  
 -- استعلام SELECT St_Fname, St_Address FROM dbo.VAlexStudents;
```

---

### مثال 2 — View مع alias وأسماء أعمدة مُخصّصة

```sql
-- إخفاء أسماء الأعمدة الحقيقية عبر تحديد أسماء جديدة في تعريف الـ View
CREATE OR ALTER VIEW dbo.VStudentsInDepartments
(
    StudentIDs,        -- اسم بدل St_Id
    StudentNames,      -- اسم بدل St_Fname
    DepartmentIDs,     -- اسم بدل Dept_Id
    DepartmentNames    -- اسم بدل Dept_Name
)
AS

SELECT
    S.St_Id,
    S.St_Fname,
    D.Dept_Id,
    D.Dept_Name
    
FROM dbo.Student AS S
INNER JOIN dbo.Department AS D
    ON S.Dept_Id = D.Dept_Id;
GO

-- مثال استعلام
SELECT * FROM dbo.VStudentsInDepartments WHERE DepartmentNames = 'SD';
```

**:** هنا بنحدد أسماء الأعمدة في قوسين بعد اسم الـ View — دي طريقة عملية لإخفاء أسماء الأعمدة الحقيقية عن المستخدم.

---

### مثال 3 — View بضم (JOIN) لعرض درجات الطلبة في كورسات

```sql
--  يعرض اسم الطالب + اسم الكورس + الدرجة + معرف القسم
CREATE OR ALTER VIEW dbo.VStudentCourseGrades
AS

SELECT 
    Std.St_Fname   AS StudentName,
    Crs.Crs_Name   AS CourseName,
    StdCrs.Grade   AS StudentGrade,
    Std.Dept_Id    AS DeptID
    
FROM dbo.Student Std
INNER JOIN dbo.Stud_Course StdCrs ON Std.St_Id = StdCrs.St_Id
INNER JOIN dbo.Course Crs       ON Crs.Crs_Id = StdCrs.Crs_Id;
GO

```

---

# 3) مشاكل وحدود الـ View — 

- ال **ORDER BY** داخل View مش مسموح **إلا** لو استخدمنا : `SELECT TOP (10)  ... ORDER BY ...`
 يعني لازم تحدد عدد الصفوف باستخدام `TOP` مع `ORDER BY` داخل View
 
 - الـ View **مش** معزول (non-materialized)؛ يعني لو الاستعلام الأساسي بطيء، الـ View هيبقى بطيء برضه.
- لو الـ View معرف بـ `WITH ENCRYPTION` مش هتقدر تشوف تعريفه ولا تعدّله بسهولة.
- ال**`ALTER SCHEMA`**: بيستخدم علشان تنقل  View من **Schema لآخر جوه نفس الـ Database**.

---

# 5) التشفير (`WITH ENCRYPTION`) و`sp_helptext`

- `WITH ENCRYPTION` يمنع عرض تعريف الـ View عبر `sp_helptext` أو الاستعراض عبر SSMS. الغرض: حماية Intellectual Property أو إخفاء التفاصيل.
    
- **عيوب/قيود**:
    
    - بعد التشفير مش هتقدر تشوف الكود أو تعدّله بسهولة عبر الأدوات العادية.
    - لو فقدت الكود الأصلي، إزالة التشفير صعبة جداً (مش موجودة أداة رسمية لفك التشفير).
    - التشفير لا يمنع الوصول إلى البيانات نفسها — فقط يخفي الـ definition.

---

# 6) نقل View بين Schemas وDatabases — 

- **نقل داخل نفس قاعدة البيانات بين Schemas**:
    

```sql
-- نقل بين schemas (نفس db)
ALTER SCHEMA HR TRANSFER dbo.VStudentsInDepartments;
-- أو بالعكس
ALTER SCHEMA dbo TRANSFER HR.VStudentsInDepartments;
```

###  7) نقل الـ View بين قواعد بيانات مختلفة:

- **بين قواعد بيانات مختلفة**: لازم تعيد إنشاء الـ View في الداتابيز التانية:
    
    ```sql
CREATE VIEW VStudentsInDepartments AS SELECT * FROM MainDB.dbo.Student;
```
    
> 💡 **ملاحظة:**  
> `ال dbo` معناه **Database Owner** → الـ Schema الافتراضي لأي كائن جديد في SQL Server.
    
---
## 8) التعامل مع DML عبر الـ View

* ممكن تعمل `INSERT`, `UPDATE`, `DELETE` على View لو كان بسيط (مبني على جدول واحد).

* لو فيه Joins أو Aggregate Functions → مش هينفع.

مثال:

```sql

INSERT INTO dbo.VCairoStudents (StudentCode, StudentName, StudentAddress)

VALUES (101, 'Ali', 'Cairo');

-- تحديث اسم طالب من خلال الـ View
UPDATE dbo.VCairoStudents
SET StudentName = 'Ahmed'
WHERE StudentCode = 101;

-- حذف طالب من خلال الـ View
DELETE FROM dbo.VCairoStudents
WHERE StudentCode = 101;

```

---

# 9) الفرق بين `DELETE`, `TRUNCATE`, `DROP` 

| الأمر                              |                               ماذا يفعل |      هل يمسح البنية؟       |  هل يسجل في الـ Transaction Log لكل صف؟  | ملاحظات                                                     |
| ---------------------------------- | --------------------------------------: | :------------------------: | :--------------------------------------: | ----------------------------------------------------------- |
| `DELETE FROM اسم الجدول WHERE ...` |                    يمسح الصفوف بحسب شرط |             لا             |       **نعم** (يسجل لكل صف، أبطأ)        | يمكن استرجاعه بواسطة ROLLBACK داخل Transaction              |
| `TRUNCATE TABLE اسم الجدول`        |                    يمسح كل الصفوف بسرعة | لا (لكن يمسح البيانات فقط) | لا يسجل لكل صف (يُعيد تعيين صفحات), أسرع | لا يعمل إذا في FK يعتمد على الجدول، يعيد تعيين الـ identity |
| `DROP TABLE اسم الجدول`            | يحذف الجدول بالكامل (البنية + البيانات) |            نعم             |          يسجل العملية البنيوية           | لا يمكن التراجع بسهولة إلا باستعادة من Backup               |
