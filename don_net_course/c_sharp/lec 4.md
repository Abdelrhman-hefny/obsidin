تمام يا عبدالرحمن، هشرحلك المواضيع دي بأسلوب مبسط وسهل، وبعدها أعملك نوتس منظمة تقدر تراجع منها وقت ما تحب.

---

## 🌟 أولًا: Fractions and Discard (الأنواع العشرية في C#)

### الأنواع الرقمية العشرية في C#:

|النوع|الحجم|الدقة|
|---|---|---|
|`float`|4 bytes (32-bit)|حوالي 7 أرقام عشرية|
|`double`|8 bytes (64-bit)|حوالي 15-16 رقم عشري|
|`decimal`|16 bytes (128-bit)|حوالي 28-29 رقم عشري (أعلى دقة)|

> ✳️ ملاحظات:

- استخدم `float` أو `double` للعمليات الحسابية العادية.
    
- استخدم `decimal` في الحسابات اللي بتحتاج دقة عالية جدًا (زي العمليات المالية).
    

---

## 🎭 ثانيًا: Casting (التحويل بين الأنواع)

الـ **Casting** يعني تحويل قيمة من نوع لنوع تاني.

### أنواع التحويل:

1. **Implicit Casting** (تحويل ضمني):
    
    - بيحصل تلقائيًا لما التحويل آمن ومفيش فقد بيانات.
        
    
    ```csharp
    int x = 10;
    double y = x;  // تم التحويل ضمنيًا من int إلى double
    ```
    
2. **Explicit Casting** (تحويل صريح):
    
    - بيحتاج تكتب التحويل يدويًا علشان ممكن يحصل فقد بيانات.
        
    
    ```csharp
    double a = 10.5;
    int b = (int)a;  // تم التحويل صريحًا من double إلى int
    ```
    

---

## ⚙️ ثالثًا: Operators (العوامل)

### بعض أهم العوامل في C#:

|النوع|المثال|الشرح|
|---|---|---|
|الجمع|`+`|جمع رقمين أو ضم نصين|
|الطرح|`-`|طرح رقمين|
|الضرب|`*`|ضرب رقمين|
|القسمة|`/`|قسمة رقمين|
|باقي القسمة|`%`|باقي قسمة رقمين|
|المقارنة|`==`, `!=`, `<`, `>`|للمقارنة بين القيم|
|منطقية|`&&`, `||

---

## 🧵 رابعًا: String Formatting (تنسيق النصوص)

### أمثلة على تنسيق النصوص:

```csharp
int age = 21;
string name = "Abdelrahman";

// الطريقة العادية
Console.WriteLine("My name is " + name + " and I am " + age + " years old.");

// string interpolation
Console.WriteLine($"My name is {name} and I am {age} years old.");

// Format method
Console.WriteLine(string.Format("My name is {0} and I am {1} years old.", name, age));
```

---

## 🔀 خامسًا: Conditional Statements (الشروط)

### 1. if - else:

```csharp
int num = 10;

if (num > 0)
{
    Console.WriteLine("Positive number");
}
else if (num < 0)
{
    Console.WriteLine("Negative number");
}
else
{
    Console.WriteLine("Zero");
}
```

### 2. switch:

```csharp
int day = 2;

switch (day)
{
    case 1:
        Console.WriteLine("Saturday");
        break;
    case 2:
        Console.WriteLine("Sunday");
        break;
    default:
        Console.WriteLine("Unknown day");
        break;
}
```

---
