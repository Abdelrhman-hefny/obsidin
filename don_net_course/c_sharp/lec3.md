### variable
vlaue tube
stract 
enum 
refrance type
class 
interface

---

## 📘 Value Types in C#

في C#، **Value Types** هي الأنواع اللي بتخزن **القيمة نفسها** في الذاكرة (stack)، مش عنوان لمكان تاني زي Reference Types.

---

### 🔹 1. **Primitive Data Types** (الأنواع الأولية)

دي أنواع جاهزة في C#، وكل نوع له حجم معيّن في الذاكرة:

|النوع|الاسم الكامل|الحجم في الذاكرة|المجال (نطاق القيم)|
|---|---|---|---|
|`byte`|Byte|1 Byte|0 → 255|
|`sbyte`|Signed Byte|1 Byte|-128 → 127|
|`short`|Short|2 Byte|-32,768 → 32,767|
|`ushort`|Unsigned Short|2 Byte|0 → 65,535|
|`int`|Integer|4 Byte|-2,147,483,648 → 2,147,483,647|
|`uint`|Unsigned Int|4 Byte|0 → 4,294,967,295|
|`long`|Long|8 Byte|أرقام كبيرة سالبة وموجبة|
|`ulong`|Unsigned Long|8 Byte|0 → قيم موجبة ضخمة جداً|

---

### ✳️ 2. **User-Defined Value Types**

دي أنواع بتنشئها إنت بنفسك، لكنها بتتصرف كـ Value Types:

---

#### ✅ Struct (هيكل)

- بيشبه الكلاس لكنه خفيف ومش بيدعم الوراثة.
    
- بيستخدم لما تحتاج كائن بسيط فيه بيانات بس من غير سلوك معقد.
    

📌 مثال:

```csharp
struct Point
{
    public int X;
    public int Y;
}
```

---

#### ✅ Enum (تعداد)

- مجموعة ثوابت ممكن تختار منها.
    
- بيستخدم لتقليل الخطأ في القيم الثابتة.
    

📌 مثال:

```csharp
enum Gender
{
    Male,
    Female
}
```

---

### 🧠 مثال تطبيقي بسيط:

```csharp
byte b = 100;
int age = 21;

Point p1 = new Point();
p1.X = 5;
p1.Y = 10;

Gender g = Gender.Male;
```

---

## 📊 Table: C# Built-In Data Types

| #   | C# Type   | Size (Bits) | .NET System Type | CLS-Compliant |
| --- | --------- | ----------- | ---------------- | ------------- |
| 1   | `sbyte`   | 8           | `System.SByte`   | ❌ No          |
| 2   | `short`   | 16          | `System.Int16`   | ✅ Yes         |
| 3   | `int`     | 32          | `System.Int32`   | ✅ Yes         |
| 4   | `long`    | 64          | `System.Int64`   | ✅ Yes         |
| 5   | `byte`    | 8           | `System.Byte`    | ✅ Yes         |
| 6   | `ushort`  | 16          | `System.UInt16`  | ❌ No          |
| 7   | `uint`    | 32          | `System.UInt32`  | ❌ No          |
| 8   | `ulong`   | 64          | `System.UInt64`  | ❌ No          |
| 9   | `char`    | 16          | `System.Char`    | ✅ Yes         |
| 10  | `bool`    | 8           | `System.Boolean` | ✅ Yes         |
| 11  | `float`   | 32          | `System.Single`  | ✅ Yes         |
| 12  | `double`  | 64          | `System.Double`  | ✅ Yes         |
| 13  | `decimal` | 128         | `System.Decimal` | ✅ Yes         |
| 14  | `string`  | N/A         | `System.String`  | ✅ Yes         |
| 15  | `object`  | N/A         | `System.Object`  | ✅ Yes         |

---

✅ **ملاحظات سريعة:**

- **CLS-Compliant = Yes** → يعني النوع متوافق مع لغات .NET التانية زي VB وF#.
    
- الأنواع اللي فيها `u` في اسمها (زي `uint`, `ulong`) مش CLS-Compliant.
    
- `string` و `object` أنواع مرجعية (Reference Types).
    
- `float`, `double`, `decimal` بيستخدموا للأرقام العشرية، و`decimal` هو الأدق بينهم.
    

تحب أجهزلك ملخص سريع بالأنواع الأكثر استخدامًا في المشاريع؟


---
instanse = object


## ✅ الفرق بين: `Instance` و `Object` في C#

### 🔸 أول حاجة: يعني إيه **Object**؟

**Object** هو كائن (نسخة حقيقية) بيتكوّن في الذاكرة من كلاس (Class) معين.  
هو اللي بتتعامل معاه وقت التشغيل (runtime) وبتقدر توصله للخصائص (properties) والدوال (methods) اللي فيه.

📌 مثال:

```csharp
Car myCar = new Car();
```

- `myCar` هنا هو **object** من الكلاس `Car`.
    

---

### 🔸 طيب يعني إيه **Instance**؟

كلمة **Instance** معناها "مثيل" أو "نسخة" من كلاس معين. يعني لما تعمل Object من كلاس، فأنت كده "أنشأت Instance" من الكلاس ده.

✅ ببساطة:

> كل **Instance** هو **Object**،  
> لكن مش كل Object لازم تقول عليه "Instance" في الكلام العادي، ده حسب السياق.

---

### 🧠 توضيح عملي:

```csharp
class Person
{
    public string name;
}

// هنا بننشئ instance (نسخة) من الكلاس Person
Person p1 = new Person();
p1.name = "Abdelrahman";
```

- `p1` هو **object**.
    
- وده كمان اسمه **instance** من الكلاس `Person`.
    

---

### 📌 جملة "instance = object" معناها:

- الكلمتين بيشيروا لنفس الحاجة تقريبًا.
    
- بس "instance" دايمًا بتستخدم علشان نقول "نسخة من كلاس".
    
- و"object" بتستخدم بمعناها الأوسع اللي هو "كائن في الذاكرة".
    

---
Reference Type Data Types
 الكود اللي معاك بيوضح **كيفية عمل الـ Reference Types في C#**، خصوصًا لما بنستخدم الكلاسات، وازاي الذاكرة بتتقسم ما بين **Stack** و **Heap**.  



---

## ✅ شرح الكود:

```csharp
class Point
{
    Point P1;
    int y;
    int x;

    P1 = new Point();
}
```

### 🔸 إيه اللي بيحصل هنا؟

#### 1. `Point P1;`

- ده **تعريف متغير من نوع مرجعي (Reference Type)**.
    
- `P1` بيتخزن في **Stack** وبيكون في الأول بيساوي `null`.
    
- اتحجز له 4 بايت في الـ Stack (لأنه pointer للإشارة إلى كائن في الـ Heap).
    

#### 2. `int y;` و `int x;`

- دول **Value Types**.
    
- لكن لأنهم جوا كلاس (Reference Type)، فبيتخزنوا في **Heap** لما نعمل كائن (Object).
    

#### 3. `P1 = new Point();`

- الخطوة دي مهمة جدًا:
    
    - 🧠 أولاً: **يتم حجز مساحة للـ Object الجديد في Heap**.
        
    - ✨ بعدين: **يتم تهيئة القيم الافتراضية للخصائص** (`x=0`, `y=0`, `P1=null`).
        
    - 🔧 لو فيه **Constructor مخصص**، بيتنفذ.
        
    - وأخيرًا: يتم **تخزين مرجع (Reference)** لهذا الكائن في متغير `P1` الموجود في الـ Stack.
        

---

### 🔄 الجزء التاني من الكود:

```csharp
Point P2 = new Point(); // إنشاء كائن جديد
P2 = P1; // P2 بيشير لنفس الكائن اللي P1 بيشاور عليه
P1.x = 9; // تعديل قيمة x من خلال P1
Console.WriteLine(P2.x); // النتيجة هتكون 9
```

#### ✅ ليه النتيجة 9؟

- لأن `P1` و `P2` **بيشاوروا على نفس الكائن** في الـ Heap.
    
- فلما تغير `x` من خلال `P1`، التغيير بيبان برضو لما تطبع من خلال `P2`.
    

---

## 📝 نوتس مختصرة:

### 🟦 Reference Type (مثل: Class)

- بيتخزن في **Heap**.
    
- المتغير اللي بيشاور عليه بيتخزن في **Stack** (كـ pointer).
    
- لو عملت `A = B`، الاتنين هيشاوروا على **نفس الكائن**.
    
- أي تعديل من خلال واحد منهم، هينعكس على التاني.
    

### 📌 عند إنشاء Object باستخدام `new`:

1. يتم **حجز ذاكرة في Heap**.
    
2. تهيئة القيم الافتراضية.
    
3. تنفيذ أي Constructor.
    
4. تخزين المرجع (Reference) في المتغير في Stack.
    

### 🔁 المثال يوضح:

```csharp
P2 = P1;  // P2 و P1 بيشاوروا على نفس الكائن
P1.x = 9; // تعديل من خلال P1
P2.x     // نفس القيمة لأنهم نفس الكائن
```


---

## 🔹 1. `class` → **الكائنات اللي ليها حالة وسلوك**

### ✅ تعريف:

الكلاس هو قالب (Template) بتستخدمه لإنشاء كائنات (Objects) ممكن تحتوي على **بيانات (Fields/Properties)** و**وظائف (Methods)**.

### ✅ مثال:

```csharp
public class Person
{
    public string Name;
    public int Age;

    public void SayHello()
    {
        Console.WriteLine($"Hello, my name is {Name}");
    }
}
```

### ✅ استخداماته المشهورة:

- لما يكون عندك كائن ليه حالة وسلوك معقد (state + behavior).
    
- لما تحتاج ترث منه (Inheritance) أو تطبق OOP بشكل كامل.
    

---

## 🔸 2. `struct` → **بيانات خفيفة وبسيطة**

### ✅ تعريف:

الـ `struct` شبه الـ `class` بس أخف، وبيتم تخزينه كـ **Value Type** في الذاكرة (Stack غالبًا).

### ✅ مثال:

```csharp
public struct Point
{
    public int X;
    public int Y;

    public void Print()
    {
        Console.WriteLine($"Point at ({X}, {Y})");
    }
}
```

### ✅ استخداماته المشهورة:

- لما يكون عندك بيانات بسيطة زي الإحداثيات، التاريخ، الألوان، وهكذا.
    
- لما تحتاج سرعة أعلى ومافيش حاجة معقدة (زي التوريث).
    

> ❌ مفيش توريث في الـ struct، بس ممكن يطبق interface.

---

## 🔹 3. `enum` → **قائمة من القيم الثابتة**

### ✅ تعريف:

الـ `enum` بيستخدم علشان تعمل مجموعة من القيم المنطقية المحددة مسبقًا، وكل قيمة ليها رقم داخلي (int).

### ✅ مثال:

```csharp
public enum OrderStatus
{
    Pending,    // 0
    Processing, // 1
    Shipped,    // 2
    Delivered   // 3
}
```

### ✅ استخداماته المشهورة:

- الحالات المنطقية (مثل: حالة الطلب، نوع المستخدم، لون الإشارة).
    
- بيخلي الكود واضح وأسهل في القراءة.
    

---

## 🔸 4. `interface` → **عقد – contract**

### ✅ تعريف:

الـ `interface` هو عقد بيحدد مجموعة من الدوال (الوظائف) اللي الكلاس أو الستركت لازم يطبقها.

### ✅ مثال:

```csharp
public interface IAnimal
{
    void Speak();
}

public class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Woof!");
    }
}
```

### ✅ استخداماته المشهورة:

- لما يكون عندك أنواع مختلفة بس ليهم وظائف مشتركة.
    
- في الـ Dependency Injection والـ SOLID principles.
    
- بيعزز مبدأ الفصل والمرونة في الكود.
    

> ✅ ممكن struct أو class يطبقوا interface، بس **الوراثة الحقيقية (inheritance)** بتكون من كلاس فقط.



---

## 🔹 `object` في C#

### ✅ تعريف:

- `object` هو **النوع الأساسي (base type)** لكل أنواع البيانات في C#.
    
- ممكن تخزن فيه أي حاجة: رقم، نص، كائن... إلخ.
    

### ✅ مثال:

```csharp
object x = 5;
object y = "Hello";
```

### ❌ العيب:

- كل مرة تستخدم `object` مع نوع تاني (زي int)، بيتم **Boxing** و**Unboxing**، وده بيأثر على الأداء.
    

---

## 🔸 `generics` في C#

### ✅ تعريف:

- `Generics` بتخليك تكتب كود مرن يشتغل مع أي نوع **من غير ما تفقد الأداء**.
    

### ✅ مثال:

```csharp
public class Box<T>  
{
    public T Value;
}

var intBox = new Box<int> { Value = 10 };
var stringBox = new Box<string> { Value = "Test" };
```

### ✅ الميزة:

- أسرع: مفيش Boxing/Unboxing.
    
- آمن: بيمنع الأخطاء في وقت الترجمة (compile-time).
    
- مرن: نفس الكود يشتغل لأي نوع.
    

---
