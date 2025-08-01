console.

            Console.WriteLine("Hello World!");
            Console.BackgroundColor = ConsoleColor.Blue;
            Console.ForegroundColor = ConsoleColor.White;
            Console.WriteLine("Hello World!");
            Console.ResetColor();
            Console.Title = "My App";
            Console.WriteLine("Hello World!");
            Console.ReadKey();

   التحكم في الكونسول ، هناك الكثير من الابناء داخل الاب كونسول او يمكن اعتباره فولدر وداخله ابناء ،
   ___
   `WriteLine()` → طباعة النص مع سطر جديد  
✅ `Write()` → طباعة النص بدون سطر جديد  
✅ `ReadLine()` → إدخال نص من المستخدم  
✅ `ReadKey()` → انتظار إدخال مفتاح  
✅ `Clear()` → مسح الشاشة  
✅ `ForegroundColor` / `BackgroundColor` → تغيير الألوان  
✅ `Beep()` → تشغيل صوت

---
## variable
لتعريف متغير في C#، نستخدم الصيغة التالية:

`DataType variableName = value;`
## **🔹 2. أنواع البيانات (Data Types) في C#**

### **📌 أنواع البيانات الأساسية:**

| النوع     | الحجم   | المجال                                          |
| --------- | ------- | ----------------------------------------------- |
| `int`     | 4 بايت  | من -2,147,483,648 إلى 2,147,483,647             |
| `double`  | 8 بايت  | للأرقام العشرية ذات الدقة العالية               |
| `float`   | 4 بايت  | للأرقام العشرية (أقل دقة من `double`)           |
| `decimal` | 16 بايت | للأرقام العشرية عالية الدقة (للمعاملات المالية) |
| `char`    | 2 بايت  | حرف واحد فقط بين `' '`                          |
| `string`  | متغير   | سلسلة نصية بين `"`                              |
| `bool`    | 1 بايت  | يأخذ `true` أو `false`                          |

---
✅ يتم تعريف المتغيرات باستخدام `DataType variableName = value;`.  
✅ يوجد عدة أنواع بيانات مثل `int`, `double`, `string`, `bool`.  
✅ يمكن استخدام `var` لتحديد النوع تلقائيًا، و`dynamic` لتغيير النوع أثناء التشغيل.  
✅ القيم الافتراضية للمتغيرات تعتمد على نوعها.  
✅ يمكن استخدام `const` لإنشاء متغيرات ثابتة لا تتغير.  
✅ `int?` يسمح بتخزين `null` في الأنواع القيمية.

using System;
class Program
{
    static void Main()
    {
        int age = 30;
        double height = 5.9;
        string name = "Ali";
        bool isStudent = false;

        Console.WriteLine($"Name: {name}, Age: {age}, Height: {height}, Student: {isStudent}");
    }
}


---

