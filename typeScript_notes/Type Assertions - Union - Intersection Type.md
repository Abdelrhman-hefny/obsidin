### 📌 **Type Assertions في TypeScript**

`Type Assertions` بتسمح لك تقول للمترجم إنك متأكد من نوع المتغير، حتى لو TypeScript مش قادر يكتشفه تلقائيًا. لكنها **ما بتحول** البيانات فعليًا، بس بتساعد المترجم في فهم الكود.

---

## ✅ **طريقتين لكتابة Type Assertions**

1️⃣ **استخدام `as` (المفضلة)**

```typescript
let myImg = document.getElementById("img") as HTMLImageElement;
console.log(myImg.src);
```

2️⃣ **استخدام `<Type>` (أقل شيوعًا)**

```typescript
let myImg = <HTMLImageElement>document.getElementById("img");
console.log(myImg.src);
```

**⚠ ملاحظة:** الطريقة دي **ما تشتغلش** في ملفات **`.tsx`** الخاصة بـ React، استخدم `as` بدلها.

---

## ✅ **استخدام Type Assertions مع `any`**

بما إن `any` ممكن يكون أي حاجة، أحيانًا تحتاج تقول للمترجم النوع الفعلي:

```typescript
let data: any = "1000";
console.log((data as string).repeat(3)); // 100010001000
```

⚠ **لو النوع مش متوافق، هيحصل خطأ وقت التشغيل!**  
مثال خطأ:

```typescript
let num: any = 1000;
console.log((num as string).repeat(3)); // ❌ خطأ وقت التشغيل
```

---

## ✅ **Type Assertions مع DOM**

TypeScript ممكن **ما يكونش متأكد** من عناصر الـ DOM، فلازم نحدد النوع:

```typescript
let input = document.getElementById("username") as HTMLInputElement;
console.log(input.value);
```

بدون `as HTMLInputElement`، TypeScript هيعتبر `input` من نوع `HTMLElement` اللي **ما عندوش** `value`.

---

### 🎯 **متى نستخدم Type Assertions؟**

✔ لما TypeScript مش قادر يحدد النوع تلقائيًا  
✔ عند التعامل مع `any`  
✔ مع عناصر الـ DOM  
✔ في البيانات القادمة من API

❌ **ما تستخدمهاش إلا لو متأكد 100% من النوع!** لأنها **ما بتحولش** البيانات فعليًا، بس بتجبر TypeScript يعاملها كنوع معين. 🚀

---
---
# Union And Intersection Types


### 📌 **Union & Intersection Types في TypeScript**

1️⃣ **Union (`|`)** → المتغير يقبل **واحد من الأنواع**

```typescript
let value: string | number;
value = "Hello"; // ✅ صح
value = 100;     // ✅ صح
```

2️⃣ **Intersection (`&`)** → المتغير لازم يحتوي على **كل الأنواع معًا**

```typescript
type Person = { name: string };
type Employee = { employeeId: number };
type Worker = Person & Employee;

let worker: Worker = { name: "Ahmed", employeeId: 1234 }; // ✅ صح
```

3️⃣ **استخدام `typeof` مع `Union` للتعامل مع الأنواع**

```typescript
function formatData(data: string | number) {
    return typeof data === "string" ? data.toUpperCase() : data.toFixed(2);
}
```

🎯 **متى تستخدم كل واحد؟**  
✔ **Union (`|`)** → لو المتغير ممكن يكون **نوع واحد فقط** من عدة أنواع.  
✔ **Intersection (`&`)** → لو المتغير لازم يكون **كل الأنواع معًا**.

