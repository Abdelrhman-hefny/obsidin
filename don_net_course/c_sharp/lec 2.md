Compiler And Interpreter
Error Types
Variable Declaration
Comments and Regions
CTS and CLS
Datatypes [ Value - Reference]
Object

---

## 🛠️ Compiler and Interpreter

**Compiler** و **Interpreter** هما نوعين من برامج الترجمة (Language Processors) اللي بيحوّلوا الكود المكتوب بلغة برمجة عالية المستوى (زي C، Java، Python) إلى لغة الآلة (Machine Code) اللي الكمبيوتر يقدر ينفذها.

---

### 🧾 Compiler

- هو برنامج بيترجم **الكود بالكامل مرة واحدة** إلى ملف تنفيذي مستقل.
    
- بيخزن الـ Machine Code الناتج في ملف منفصل (زي `.exe`)، والكمبيوتر بيشغله بعدين.
    
- بيدي أداء عالي وقت التشغيل (لأن الترجمة بتكون مسبقة).
    
- بيعرض كل الأخطاء مرة واحدة **بعد ما يكمّل الترجمة**.
    

🧪 **أمثلة للغات بتستخدم Compiler:**  
`C`, `C++`, `Rust`, `Go`

---

### 🧾 Interpreter

- هو مترجم فوري بيقرأ ويترجم **سطر بسطر** وقت التنفيذ.
    
- مابيعملش ملف تنفيذي، بيترجم ويشغل الكود مباشرة.
    
- أبطأ من الـ Compiler في وقت التشغيل.
    
- بيكتشف الأخطاء أثناء التنفيذ، يعني ممكن البرنامج يقف في أول خطأ.
    

🧪 **أمثلة للغات بتستخدم Interpreter:**  
`Python`, `JavaScript`, `PHP`, `Ruby`

---

## 📊 مقارنة بين Compiler و Interpreter

|Feature|Compiler|Interpreter|
|---|---|---|
|**نوع الترجمة**|يترجم الكود بالكامل دفعة واحدة|يترجم الكود سطر بسطر|
|**وقت التنفيذ**|أسرع (بعد ما يتم الترجمة)|أبطأ (لأنه بيترجم كل سطر لحظيًا)|
|**التعامل مع الأخطاء**|يعرض كل الأخطاء بعد الترجمة|يكتشف الأخطاء أثناء التشغيل|
|**المخرجات**|بينتج ملف تنفيذي مستقل (.exe مثلاً)|لا يُنتج ملف تنفيذي|
|**استخدام الذاكرة**|يحتاج ذاكرة أكبر لتخزين ملف exe، لكن أقل أثناء التشغيل|يحتاج ذاكرة أقل (لأنه مفيش ملف) لكن أكتر وقت التشغيل|
|**أمثلة للغات**|C, C++|JavaScript, Python, PHP|

---

## 🚨 أنواع الأخطاء  (Error Types)

في لغة C#، الأخطاء بتنقسم لأكثر من نوع حسب توقيت حدوثها وطبيعتها. فهم كل نوع بيساعدك تكتب كود أنظف وأسهل في التصحيح.

---

### 1️⃣ **Compile-Time Errors (Syntax Errors)**

🔧 **تعريف:**  
أخطاء بتحصل أثناء كتابة الكود، لما يكون فيه كسر لقواعد اللغة (Syntax Rules).  
المترجم (Compiler) بيكتشفها ومش بيسمح بتشغيل البرنامج إلا بعد تصحيحها.

🧠 **أمثلة:**

- نسيان الفاصلة المنقوطة `;`
    
- استخدام متغير بدون ما تعرّفه
    
- توقيع دالة غلط (Method Signature)
    
- أقواس مش متطابقة `{}` أو `()`
    

```csharp
int x = 5  // خطأ: نسيان ;
Console.WriteLine("Hello World"  // خطأ: قوس ناقص
```

---

### 2️⃣ **Runtime Errors (Exceptions)**

⚠️ **تعريف:**  
أخطاء بتحصل **أثناء تشغيل البرنامج**، وبتكون بسبب عمليات غير قانونية زي:  
القسمة على صفر، الوصول لمكان غير موجود في المصفوفة، أو محاولة استخدام كائن فاضي (null).

📌 **أشهر أنواع الـ Runtime Exceptions:**

- `NullReferenceException`: لما تحاول تستخدم كائن قيمته `null`.
    
- `IndexOutOfRangeException`: لما تدخل على Index مش موجود في المصفوفة أو القائمة.
    
- `DivideByZeroException`: لما تقسم على صفر.
    
- `FileNotFoundException`: لما البرنامج يدور على ملف مش موجود أثناء عملية قراءة أو كتابة.
    

🧠 **أمثلة:**

```csharp
int[] arr = new int[3];
Console.WriteLine(arr[5]);  // IndexOutOfRangeException

string text = null;
Console.WriteLine(text.Length);  // NullReferenceException
```

---

### 3️⃣ **Logical Errors**

🧠 **تعريف:**  
البرنامج بيشتغل وبيتنفذ، لكن الناتج **مش صحيح** بسبب خطأ في التفكير أو المنطق.  
الـ Compiler مش بيكتشفه لأنه كود "صحيح شكليًا".

🎯 **أسباب شائعة:**

- استخدام معادلة أو خوارزمية غلط.
    
- شرط `if` مكتوب بشكل خاطئ.
    
- استخدام نوع بيانات غير مناسب.
    

🧠 **أمثلة:**

```csharp
int a = 10, b = 5;
int result = a - b;  // كنت تقصد a + b
```

---

### 4️⃣ **File I/O Errors**

📁 **تعريف:**  
أخطاء بتحصل أثناء التعامل مع الملفات، زي القراءة من ملف مش موجود، أو الكتابة في ملف محمي.

🧠 **أمثلة:**

- محاولة قراءة ملف مش موجود.
    
- الكتابة في ملف للقراءة فقط.
    

```csharp
string content = File.ReadAllText("data.txt");  // FileNotFoundException لو الملف مش موجود
File.WriteAllText("readonly.txt", "data");      // UnauthorizedAccessException لو الملف محمي
```

---

## 📝 Comments in C[#]


---
### 1️⃣ **Single-line Comment**

- بيبدأ بـ `//`
    
- يستخدم لسطر واحد فقط.
    

```csharp
// ده تعليق سطر واحد
int x = 5;  // ممكن ييجي بعد كود
```

---

### 2️⃣ **Multi-line Comment**

- بيبدأ بـ `/*` وينتهي بـ `*/`
    
- يستخدم لأكثر من سطر.
    

```csharp
/*
ده تعليق
على أكتر من سطر
*/
int x = 10;
```

---

### 3️⃣ **XML Comment**

- بيبدأ بـ `///`
    
- يُستخدم لتوثيق الكود (Documentation Comments)
    
- مفيد مع الـ IntelliSense في Visual Studio.
    

```csharp
/// <summary>
/// دي دالة بتجمع رقمين
/// </summary>
int Sum(int a, int b)
{
    return a + b;
}
```

---

### 4️⃣ **#region / #endregion**

- لتجميع أجزاء الكود داخل بلوك قابل للطي/الإخفاء.
    
- مفيد لتنظيم الكود في الملفات الطويلة.
    

```csharp
#region الحسابات
int Add(int a, int b) => a + b;
int Sub(int a, int b) => a - b;
#endregion
```

---
common type system
Common Type System (CTS)
VB
CTS Divides Data Types into two Categories:
CLS
1. Value Types: These include primitive types like int, char, float, and user-defined types like struct.
F#
C#
15
A
A
2. Reference Types: These include Non-Primitive types like class, interface, delegate, and array.
CLS: Common Language Specification
meet.google.com

CTS Divides Data Types into two Categories:
1. Value Types: These include primitive types like int, char, float, and user-defined types
like struct.
They hold the actual data. When you assign a primitive type variable to another, it creates a copy of the value.
Memory Allocation: Primitive types are stored in the stack, making them faster to access compared to reference types
2. Reference Types: These include Non-Primitive types like class, interface, delegate, and array.
