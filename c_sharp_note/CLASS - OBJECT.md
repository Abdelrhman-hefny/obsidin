### شرح **الفئة (Class)** و **الكائن (Object)** 
#### 1. **الفئة (Class)**:

الفئة `student` هي **قالب** أو **تصميم** لتعريف كائنات (Objects) تحتوي على خصائص معينة. في الكود، تم تعريف الفئة `student` التي تحتوي على ثلاث خصائص:

- `Name`: لاسم الطالب.
    
- `Phone`: لرقم هاتف الطالب.
    
- `Adress`: لعنوان الطالب.
    

```csharp
public class student
{
    public string Name { get; set; }
    public string Phone { get; set; }
    public string Adress { get; set; }
}
```

- **شرح**: الفئة `student` هي تصميم يحتوي على الخصائص الثلاث (الاسم، الهاتف، والعنوان). يتم استخدام الـ `get` و `set` لجعل هذه الخصائص قابلة للوصول والتعديل.
    

#### 2. **الكائن (Object)**:

الكائن هو **مثال حي** يتم إنشاؤه من الفئة. في الكود، تم إنشاء الكائن `Ahmed` من الفئة `student` لتمثيل طالب معين.

```csharp
student Ahmed = new student();
```

- **شرح**: الكائن `Ahmed` هو نسخة من الفئة `student` ويمكن تخصيص خصائصه (مثل الاسم، الهاتف، والعنوان) بالقيم المدخلة بواسطة المستخدم.
    

#### 3. **إدخال البيانات إلى الكائن**:

بعد إنشاء الكائن، يمكن تخصيص قيم الخصائص باستخدام القيم المدخلة من المستخدم:

```csharp
Ahmed.Name = Console.ReadLine();
Ahmed.Phone = Console.ReadLine();
Ahmed.Adress = Console.ReadLine();
```

- **شرح**: هنا، يتم ملء الخصائص الخاصة بالكائن `Ahmed` بالقيم المدخلة من المستخدم (اسم الطالب، الهاتف، والعنوان).
    

### **الفرق بين الفئة والكائن**:

- **الفئة (Class)**: هي التصميم أو القالب الذي يحدد الخصائص والسلوكيات (الوظائف) التي ستكون للكائنات.
    
- **الكائن (Object)**: هو مثال حي تم إنشاؤه من الفئة ويحتوي على قيم حقيقية للخصائص المحددة في الفئة.
    

---









### شرح الـ **Constructor** باختصار:

- **الـ Constructor** هو دالة تُنفَّذ تلقائيًا عند إنشاء كائن جديد من الفئة.
    
- يتيح لك الـ Constructor تهيئة الكائن بتعيين قيم للخصائص عند إنشائه.
    

**مثال من الكود:**

```csharp
public class student
{
    public student(string Name)  // Constructor
    {
        Console.WriteLine("the constructor has been started");
        this.Name = Name;  // تعيين قيمة Name
    }

    public string Name { get; set; }
    public string Phone { get; set; }
    public string Adress { get; set; }
}

// عند إنشاء الكائن:
student Ahmed = new student("ahmed");
```

- عند إنشاء كائن `Ahmed`، يتم استدعاء الـ Constructor، ويتم تعيين `"ahmed"` للخاصية `Name` وطباعة رسالة "the constructor has been started".
    

**الفائدة**: يتم استخدام الـ Constructor لتهيئة الكائنات بشكل مناسب عند إنشائها.

---





### ما هي الخصائص؟

الخصائص هي عبارة عن آلية تسهل قراءة وتعديل البيانات داخل الكائن أو الكلاس. على سبيل المثال، إذا كان لديك كائن من نوع "طالب"، فقد يكون له خصائص مثل الاسم، والطول، والعمر، والوزن. هذه الخصائص تعتبر بمثابة **البيانات** المرتبطة بالكائن.

### كيف يمكن أن نستخدم الخصائص؟

في لغات البرمجة التي لا تدعم الخصائص بشكل مباشر (مثل Java)، نحتاج إلى استخدام **getter** و **setter**، وهما دالتان لقراءة وكتابة القيم. وفي Java، قد تكتب كود مثل هذا:

```java
private String name;

public String getName() {
    return name;
}

public void setName(String name) {
    this.name = name;
}
```

لكن في C#، يمكنك استخدام الخصائص لتبسيط هذا الكود. إليك كيفية تعريف خاصية في C#:

```csharp
public string Name { get; set; }
```

هذه الخاصية تقوم بدور **getter** و **setter** بشكل تلقائي، مما يسهل عليك عملية التعامل مع البيانات داخل الكائنات.

### ما هي الفائدة من استخدام الخصائص؟

الخصائص في C# تمنحك طريقة مريحة للتفاعل مع البيانات دون الحاجة لكتابة الكثير من الكود. مثلاً، إذا كنت تريد قراءة أو تعديل قيمة خاصية، يمكنك القيام بذلك مباشرة باستخدام:

```csharp
student.Name = "محمد"; // تعيين قيمة
Console.WriteLine(student.Name); // قراءة القيمة
```

### حالات استخدام الخصائص:

1. **خاصية تلقائية (Auto Property)**: عندما لا تحتاج إلى تنفيذ منطق إضافي في getter أو setter، يمكنك استخدام **خاصية تلقائية** لتسهيل الأمور:
    
    ```csharp
    public string Name { get; set; }
    ```
    
2. **مع منطق إضافي (Custom Getter/Setter)**: أحيانًا، قد تحتاج إلى إضافة منطق خاص لقراءة أو كتابة القيم. على سبيل المثال، إذا كان لديك خاصية `Age` وكنت تريد حساب العمر استنادًا إلى تاريخ الميلاد، يمكنك إضافة منطق في الـ getter أو setter:
    
    ```csharp
    private DateTime birthDate;
    
    public int Age
    {
        get
        {
            return DateTime.Now.Year - birthDate.Year;
        }
        set
        {
            birthDate = DateTime.Now.AddYears(-value); // حساب تاريخ الميلاد استنادًا إلى العمر
        }
    }
    ```
    
3. **متحولات فقط للقراءة (Read-Only Properties)**: إذا كنت لا تريد السماح بتغيير قيمة الخاصية بعد تعيينها، يمكنك استخدام الخاصية كـ read-only باستخدام فقط `get`:
    
    ```csharp
    public string Name { get; }
    ```
    

### ماذا يحدث خلف الكواليس؟

عندما تستخدم الخصائص، يتم التعامل مع الكود بشكل داخلي كما لو كنت قد كتبت getter و setter بشكل يدوي، ولكن يتم تبسيط الأمر. على سبيل المثال، خاصية `Name { get; set; }` يتم تحويلها في الكود الداخلي إلى `get` و `set` تلقائيًا.

### مثال تطبيقي:

```csharp
class Student
{
    private string name;
    
    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

في هذا المثال، تم تعريف خاصية `Name` لقراءة وكتابة القيمة الخاصة بها.

### الخلاصة:

- **الخصائص** توفر طريقة مريحة وآمنة للوصول إلى البيانات داخل الكائنات.
    
- يمكن استخدام **الخصائص التلقائية** لتبسيط الكود إذا لم تكن بحاجة إلى منطق خاص.
    
- عند الحاجة إلى **منطق خاص** في عملية القراءة أو الكتابة، يمكن تعريف getter و setter مخصصين.
- 