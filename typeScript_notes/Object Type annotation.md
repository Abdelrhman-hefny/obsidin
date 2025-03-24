### 📌 **Type Annotations مع الـ Object في TypeScript**

1️⃣ **تحديد نوع الـ Object عند تعريفه**

```typescript
let user: { name: string; age: number } = { name: "Ahmed", age: 25 }; // ✅ صح
```

2️⃣ **جعل بعض الخصائص اختيارية (`?`)**

```typescript
let user: { name: string; age?: number };
user = { name: "Ali" }; // ✅ صح
user = { name: "Ali", age: 30 }; // ✅ صح
```

3️⃣ **استخدام `readonly` لجعل الخاصية غير قابلة للتعديل**

```typescript
let user: { readonly id: number; name: string };
user = { id: 1, name: "Omar" };
user.id = 2; // ❌ خطأ: لا يمكن تعديل `id`
```

4️⃣ **استخدام `index signature` للسماح بخصائص إضافية غير معرفة مسبقًا**

```typescript
let user: { name: string; [key: string]: any };
user = { name: "Sara", age: 22, country: "Egypt" }; // ✅ صح
```

### 📌 **فهم `Index Signature` في TypeScript**

🚀 **المشكلة:**  
لما بتحدد نوع الـ `object` في TypeScript، بيكون **مقيد بالخصائص اللي عرّفتها فقط**، ولو حاولت تضيف خاصية جديدة، هيحصل **خطأ**.

#### ❌ **مثال بدون `index signature`**

```typescript
let user: { name: string };
user = { name: "Sara", age: 22 }; // ❌ خطأ: `age` غير معرف في الـ type
```

---

### ✅ **الحل: `Index Signature`**

`[key: string]: any;` معناها إن **أي مفتاح إضافي (`key`) من نوع `string` ممكن يكون له أي قيمة (`any`)**.

#### 📝 **مثال عملي**

```typescript
let user: { name: string; [key: string]: any };

user = { name: "Sara", age: 22, country: "Egypt" }; // ✅ صح
user = { name: "Ali", skills: ["JS", "TS"], isAdmin: true }; // ✅ صح
```

🔹 **ليه دا شغال؟**

- `name` مطلوب ويكون `string` ✅
- أي مفتاح (`key`) إضافي **مسموح**، بشرط يكون نوعه `string` ✅

---

### 🎯 **فائدة `Index Signature`؟**

✔ لما يكون الكائن **مفتوح لأي خصائص إضافية** زي **بيانات الـ API**  
✔ لما يكون عندك **خصائص غير معروفة مسبقًا**

---

### ❌ **تحذير:**

`[key: string]: any;` ممكن يخلي الكود **أقل أمانًا** لأنه يسمح بأي قيمة.  
✅ **بديل أكثر أمانًا:** تحديد الأنواع المتوقعة بدل `any`

```typescript
let user: { name: string; [key: string]: string | number };

user = { name: "Ahmed", age: 30 }; // ✅ صح
user = { name: "Ali", country: "Egypt" }; // ✅ صح
user = { name: "Omar", isAdmin: true }; // ❌ خطأ: `boolean` غير مسموح
```

