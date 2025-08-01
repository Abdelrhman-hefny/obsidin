هشرحلك الكود ده كأنك قاعد قدامي في محاضرة SQL، كل أمر لوحده، وبأسلوب أكاديمي مبسّط:

---

## 🧠 أولًا: اختيار قاعدة البيانات

```sql
Use MyCompany
```

- **الشرح**: بنقوله "استخدم قاعدة البيانات اللي اسمها `MyCompany`".
    
- **ليه؟** عشان أي استعلام بعد كده يكون على جداول قاعدة البيانات دي، زي جدول `Employee`.
    

---

## 🧮 Aggregate Functions (الدوال التجميعية)

### 1. `COUNT`

```sql
select COUNT(*), Count([SSN]), Count([Dno]) from Employee
```

- **`COUNT(*)`**: بيعد كل الصفوف حتى لو فيها قيم NULL.
    
- **`Count([SSN])`**: بيعد الصفوف اللي فيها SSN **مش NULL**.
    
- **`Count([Dno])`**: نفس الكلام، بيعد اللي فيهم قيمة فعلية لـ `Dno`.
    

---

### 2. `SUM`

```sql
select SUM(Salary) from Employee
```

- **الشرح**: بيجمع كل الرواتب الموجودة في عمود `Salary`.
    
- **ملاحظة**: الصفوف اللي فيها `NULL` في الراتب مش بيحسبها.
    

---

### 3. `AVG`

```sql
Select AVG(Salary) from Employee
```

- **الشرح**: بيحسب متوسط الرواتب = (مجموع الرواتب ÷ عددهم).
    

---

### 4. `MAX` و `MIN`

```sql
select MAX(Salary) [Max Salary] from Employee
select MIN(Salary) [minmuin salary] from Employee
```

- **الشرح**: بيعرض أعلى وأقل راتب.
    
- **`[Max Salary]` و `[minmuin salary]`**: أسماء مخصصة للأعمدة اللي هتظهر في النتيجة.
    

---

### 5. أسماء الموظفين

```sql
select MIN([Fname]), MAX([Fname]) from Employee
```

- **الشرح**: أقل وأعلى اسم موظف حسب الترتيب الأبجدي.
    

---

### 6. تمييز القيم المختلفة

```sql
select distinct [Dno] from Employee
```

- **الشرح**: بيعرض كل أرقام الأقسام (Dno) بدون تكرار.
    

---

### 7. التجميع باستخدام `GROUP BY`

```sql
select [Dno],[Address], MIN(Salary) [Min Salary], MAX(Salary) [Max salay]
from [Employee]
group by [Dno],[Address]
```

- **الشرح**: بنقسم الموظفين حسب القسم والعنوان، وبنحسب أقل وأعلى راتب لكل مجموعة.
    

---

## 🚫 Null Functions

### 1. `ISNULL`

```sql
select ISNULL([Lname],'Not Fount') from [Employee]
```

- **الشرح**: لو `Lname` = NULL، يعرض "Not Fount" بدلها.
    

---

```sql
select ISNULL(Lname,Fname) from [Employee]
```

- **الشرح**: لو `Lname` = NULL، يعرض `Fname` بدلها.
    

---

```sql
select ISNULL(Salary,Fname) --invalid
```

- **الشرح**: خطأ ❌ لأن `Salary` عدد و `Fname` نص. لازم النوعين يكونوا نفس النوع.
    

---

```sql
select ISNULL([Lname],IsNull([Address],Fname)) from Employee
```

- **الشرح**: سلسلة بديلة:
    
    - لو `Lname` = NULL → جرب `Address`
        
    - لو `Address` كمان = NULL → جرب `Fname`
        

---

### 2. `COALESCE`

```sql
select COALESCE([Lname],[Fname],[Address],'not fount') from Employee
```

- **الشرح**: بتعرض أول قيمة **مش NULL** من اليسار لليمين.
    

---

## 📅 Date & Time Functions

```sql
select SYSDATETIME()
```

- **الشرح**: يعرض التاريخ والوقت بالتفصيل (بما فيهم الثواني الجزئية).
    

```sql
select SYSUTCDATETIME()
```

- **الشرح**: نفس اللي فوق بس بالتوقيت العالمي (UTC).
    

```sql
select GETDATE()
```

- **الشرح**: يعرض التاريخ والوقت في لحظة التنفيذ، لكن بدقة أقل من `SYSDATETIME()`.
    

```sql
select DATENAME(WEEKDAY,'06-12-2025')
```

- **الشرح**: بيعرض اسم اليوم (Saturday مثلاً) للتاريخ المحدد.
    

```sql
select Year('06-12-2025')
```

- **الشرح**: بيستخرج السنة بس = 2025.
    

---

## 🔄 Conversion Functions (التحويل بين الأنواع)

### 1. `CONVERT`

```sql
select fname +'_'+ ISNULL(convert(varchar(20),Salary),'') from Employee
```

- **الشرح**: بنحوّل `Salary` لنص عشان نقدر ندمجه مع الاسم.
    

---

### 2. `CAST`

```sql
select fname + ' ' + CAST(Salary as varchar(20)) from Employee
```

- **الشرح**: نفس فكرة `CONVERT` لكن باستخدام `CAST`.
    

---

### 3. استخدام CAST و CONVERT مع تواريخ

```sql
declare @today date = GetDate()
select CAST(@today as varchar(max))
select CONVERT(varchar(max),@today)
```

- **الشرح**:
    
    - عرفنا متغير `@today` يحتوي على تاريخ اليوم.
        
    - بعد كده، حولنا التاريخ إلى نص باستخدام `CAST` و `CONVERT`.
        

---

لو حابب أعملك ملخص أو إنفوجرافيك صغير بالشرح ده، قولي بس 💡  
تحب أديك تدريب عملي عليهم؟