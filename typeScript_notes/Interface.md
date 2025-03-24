
interface declaration
methods ,   paramitas
Reopen And Use Cases
Extend

### ال **Interfaces في TypeScript**

#### 1️⃣ **تعريف `Interface`**

`interface` بتستخدم لتعريف **هيكل الكائنات (Objects)** والتأكد من أن الكود متناسق.

```typescript
interface User {
  name: string;
  age: number;
}

let user: User = { name: "Ahmed", age: 25 }; // ✅ صح
```

---

#### 2️⃣ **إضافة Methods & Parameters في `Interface`**

✔ يمكن تعريف **دوال داخل الـ `interface`**  
✔ تحدد **اسم الدالة، المعاملات، ونوع القيمة المرجعة**

```typescript
interface Person {
  name: string;
  greet(message: string): void; // دالة تستقبل `message` وترجع `void`
}

let person: Person = {
  name: "Ali",
  greet(msg) {
    console.log(`${this.name} says: ${msg}`);
  },
};

person.greet("Hello!"); // ✅ "Ali says: Hello!"
```

---

#### 3️⃣ **إعادة فتح `Interface` (Reopen & Merge)**

✔ يمكن **إعادة فتح** `interface` وإضافة خصائص جديدة بدون تعديل التعريف الأصلي

```typescript
interface Car {
  brand: string;
}

interface Car { // نفس الاسم يعني إعادة الفتح
  speed: number;
}

let myCar: Car = { brand: "Toyota", speed: 200 }; // ✅ صح
```

🔹 ده بيساعد لو كنت بتضيف ميزات جديدة بدون تعديل الكود الأساسي.

---

#### 4️⃣ **توسيع `Interface` (Extending)**

✔ يمكن **توريث** `interface` من `interface` آخر  
✔ الكائن الناتج لازم يلتزم بكل الخصائص

```typescript
interface Animal {
  name: string;
}

interface Dog extends Animal {
  bark(): void;
}

let myDog: Dog = {
  name: "Rex",
  bark() {
    console.log("Woof Woof!");
  },
};

myDog.bark(); // ✅ "Woof Woof!"
```

🔹 `Dog` ورث **`name` من `Animal`** وأضاف دالة `bark()`.

---

### 🎯 **متى نستخدم `Interface`؟**

✔ لما نحتاج **تعريف شكل البيانات** في المشروع  
✔ لما يكون عندنا **دوال داخل الكائنات**  
✔ عند الحاجة إلى **توسيع الهيكلة بدون تعديل الكود الأصلي**

---
### 📌 **Type Aliases vs Interfaces في TypeScript**

|❓ **الفرق**|🏷 **Type Alias (`type`)**|📄 **Interface (`interface`)**|
|---|---|---|
|**الاستخدام الأساسي**|لإنشاء **أنواع مخصصة** (للكائنات، المصفوفات، الدوال...)|لتعريف **هيكل للكائنات (Objects)**|
|**إعادة الفتح (Reopen)**|❌ **لا يمكن** إعادة فتحه لتحديثه لاحقًا|✅ **يمكن** إعادة فتحه وإضافة خصائص جديدة|
|**التوسيع (Extend)**|✅ **يمكن الدمج مع `&` (Intersection)**|✅ **يمكن التوسيع باستخدام `extends`**|
|**استخدام الدوال**|✅ ممكن تحديد دوال|✅ ممكن تحديد دوال|
|**القابلية للاستخدام مع الأصناف (`class`)**|❌ لا يمكن تطبيقه مباشرة على `class`|✅ يمكن استخدامه مع `class`|

---

### 1️⃣ **مثال `Type Alias`**

✔ `type` يمكنه تخزين أنواع أخرى غير الكائنات  
✔ يستخدم `&` لدمج الأنواع

```typescript
type ID = string | number;

type User = { name: string; age: number };

type Employee = User & { salary: number };

let emp: Employee = { name: "Ahmed", age: 30, salary: 5000 }; // ✅ صح
```

---

### 2️⃣ **مثال `Interface`**

✔ `interface` مناسب أكثر للكائنات  
✔ يمكن **إعادة فتحه وإضافة خصائص** لاحقًا

```typescript
interface User {
  name: string;
  age: number;
}

interface Employee extends User {
  salary: number;
}

let emp: Employee = { name: "Ali", age: 25, salary: 6000 }; // ✅ صح
```

---

