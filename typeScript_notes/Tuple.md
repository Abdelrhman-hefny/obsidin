### **📌 ملخص Tuple في TypeScript**

🟢 **Tuple** هو نوع بيانات يسمح بتخزين قيم **بأنواع وترتيب محدد مسبقًا** داخل مصفوفة.
عارفين كل عنصر مكانه ونوعه =, مثال موقع مقال = id , title , bool 

---

### **✅ تعريف Tuple:**

```ts
let person: [string, number] = ["Ahmed", 25];
```

🔹 العنصر الأول **string** والثاني **number**.  
🔹 **لا يمكن تغيير الترتيب أو الأنواع**.

---

### **📌 الوصول إلى العناصر:**

```ts
console.log(person[0]); // "Ahmed"
console.log(person[1]); // 25
```

---

### **✍️ تعديل القيم داخل Tuple:**

```ts
person[0] = "Ali"; // ✅ مسموح
person[1] = 30;    // ✅ مسموح
```

---

### **⚠️ استخدام `push()` مع Tuple:**

```ts
person.push("extra"); // ✅ مسموح لكن غير موصى به
console.log(person); // ["Ahmed", 25, "extra"]
```

🔹 **TypeScript لا يمنع الإضافة، لكن هذا يتعارض مع الهدف الأساسي من Tuple.**

---

### **🚀 جعل Tuple غير قابلة للتعديل (`readonly`)**

```ts
const car: readonly [string, number] = ["Toyota", 2023];
// car[0] = "Honda"; // ❌ خطأ: لا يمكن التعديل
```

---

### **📝 Named Tuple لتحسين الوضوح**

```ts
type User = [id: number, name: string, isActive: boolean];

const user: User = [1, "Aly", true];
console.log(user[1]); // "Aly"
```

🔹 يساعد على **فهم العناصر بسهولة** عند القراءة أو التعامل مع البيانات.

---

### **💡 الخلاصة**

✔ **Tuple = بيانات مرتبة + أنواع ثابتة**  
✔ تمنع **الأخطاء العشوائية** في القيم  
✔ يمكن جعلها **غير قابلة للتعديل** باستخدام `readonly`

🚀 **استخدمها عندما تحتاج إلى ترتيب معين للقيم مع أنواع مختلفة!**