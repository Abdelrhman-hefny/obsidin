## Boolean Data Type

✅ **استخدم `&&` عندما تتعامل مع القيم المنطقية (`bool`) لتجنب العمليات غير الضرورية وتحسين الأداء.**  
✅ **استخدم `&` عندما تتعامل مع الأعداد (`int`, `byte`...) لإجراء عمليات على مستوى البِت.**

✅ **استخدم `||` عندما تتعامل مع القيم المنطقية (`bool`) لتجنب العمليات غير الضرورية وتحسين الأداء.**

الـ `!` في C# هو **عامل النفي المنطقي** (Logical NOT Operator).  
يُستخدم لعكس قيمة **Boolean**، أي أنه يُحوّل `true` إلى `false` والعكس صحيح.

### عامل **XOR** هو **Exclusive OR**، ويمثله الرمز `^` في C#.  

يقوم هذا العامل بإرجاع `true` فقط إذا كانت القيمتين **مختلفتين**، وإذا كانتا متطابقتين فإنه يرجع `false`.

bool a = true;
bool b = false;

Console.WriteLine(a ^ b); // النتيجة: True
Console.WriteLine(a ^ a); // النتيجة: False
Console.WriteLine(b ^ b); // النتيجة: False

### استخدام XOR لتبديل القيم بدون متغير إضافي**


`int a = 10, b = 20;  a = a ^ b; b = a ^ b; a = a ^ b;  Console.WriteLine($"a = {a}, b = {b}"); // النتيجة: a = 20, b = 10`

🔹 **كيف يعمل هذا؟**

1. `a = a ^ b` → `a` يخزن `a XOR b`
2. `b = a ^ b` → `b` يصبح `a`
3. `a = a ^ b` → `a` يصبح `b`

---
cheat? 
write ascii from a to z  , 1-9
is the enter digit  
is the enter number;

---
## string
- `string` يُستخدم لتخزين النصوص.
- يمكن تعديل النصوص باستخدام **الوظائف المدمجة** مثل:
    - `ToUpper()`, `ToLower()`, `Replace()`, `Substring()`, `Split()`, `Trim()`, `Contains()`.
- `string` **غير قابل للتغيير (`Immutable`)**، لذا يجب استخدام `StringBuilder` عند الحاجة

- `@` يسمح باستخدام **الكلمات المحجوزة كمتغيرات**.
- `@` يجعل النصوص **حرفية (Verbatim)** لتسهيل كتابة المسارات والنصوص متعددة الأسطر.
- `@` يُستخدم مع **LINQ/SQL** لتعريف **متغيرات داخل الاستعلامات**.
## **استخدام `@` مع النصوص الحرفية (Verbatim String)**

🔹 عندما نضع `@` قبل النص (`string`) فإننا نمنع استخدام **الأحرف الخاصة (Escape Characters)** مثل `\n` و `\t`، ويصبح النص يُكتب كما هو.
`string message = @"Hello,`
`This is a multi-line`
`string!";`
`Console.WriteLine(message);`

---
بدلًا من استخدام **concatenation (`+`)**، يمكنك استخدام `$"{ }"` لإدراج القيم مباشرة داخل النص.


---

## string method:
    - `ToUpper()`, `ToLower()`, `Replace()`, `Substring()`, `Split()`, `Trim()`, `Contains()`.
    ___
    startWith();
- `string` **غير قابل للتغيير (`Immutable`)**، لذا يجب استخدام `StringBuilder` عند الحاجة
`Console.WriteLine(name1.StartsWith("mo", StringComparison.OrdinalIgnoreCase));`

---
task:
## write a program to replace a word from user;
    Console.WriteLine("enter your name: ");
string name1= Console.ReadLine() ?? "";

    Console.WriteLine("enter the target word: ");
    string name2 = Console.ReadLine() ?? "";

    Console.WriteLine("Enter what you want to replace: ");
    string name3 = Console.ReadLine() ?? "";
    Console.WriteLine(name1.Replace(name2,name3));

## ___
---
num data

### **📌 أنواع البيانات الرقمية في C# ببساطة**

🔹 **الأعداد الصحيحة (بدون فواصل عشرية)**

- `byte` → من **0** إلى **255** (1 بايت)
- `int` → من **-2 مليار** إلى **+2 مليار** (4 بايت) (عنده ابنين بيدي كل واحد 2 مليار قيمه)
- `uint` → **+4 مليار** (4 بايت) (عنده ابن واحد موجب بياخد كل ال 4 مليون)
- `long` → من **-9 كوينتيليون** إلى **+9 كوينتيليون** (8 بايت)

🔹 **الأعداد العشرية (بفواصل عشرية)**

- `float` → دقة **7 أرقام** (4 بايت)
- `double` → دقة **15 رقم** (8 بايت)
- `decimal` → دقة **28 رقم** (16 بايت) – الأفضل للعمليات المالية