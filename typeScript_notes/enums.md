
1️⃣ **تعريف Enum:**

- `enum` بيستخدم لتعريف مجموعة من القيم الثابتة بأسماء معبرة.

```typescript
enum Kids {
    Five = 15, 
    Seven = 20, 
    Ten = 15,
}
console.log(Kids.Five); // 15
```

2️⃣ **القيم الافتراضية للأعداد:**

- إذا لم تحدد قيمة، يبدأ من `0` ويزيد تلقائيًا.

```typescript
enum Color {
    Red,   // 0
    Green, // 1
    Blue,  // 2
}
```

3️⃣ **استخدام Enum داخل Enum:**

```typescript
enum Level {
    Kid = Kids.Ten, // 15
    easy = 9,
    medum = 6,
}
console.log(Level.Kid); // 15
```

4️⃣ **عدم استخدام دوال داخل Enum مباشرة** ❌

```typescript
function test(): number {
    return 3;
}

const HardValue = test(); // ✅ حل المشكلة

enum Level {
    Hard = HardValue, // ✅ استخدام متغير
}
```

5️⃣ **المقارنة الصحيحة بين القيم:**

```typescript
let lvl: string = "easy";
if (lvl === "easy") { // ✅ استخدم === وليس =
    console.log(`The level is ${lvl}`);
}
```

📌 **فايدة الـ Enums؟**

- تحسين قراءة الكود
- تقليل الأخطاء
- استخدام القيم الثابتة بطريقة منظمة

🔥 **Shortcut:** `const enum` لتوفير الأداء عن طريق استبدال القيم مباشرة في الكود.

```typescript
const enum Speed {
    Slow = 5,
    Fast = 10
}
console.log(Speed.Fast); // 10
```

عند استخدام كونست ف مش هيظهر في ملف الجافا سكريبت غير المخرجات ف لو عايز تظهر الenum
ولكن يفضل عدم استخدامها عشان توفر ف يالكود وتخليه اسرع
``` ts
    "preserveConstEnums": true,
    /* Disable erasing 'const enum' declarations in generated code. */
```

