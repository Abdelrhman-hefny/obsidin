## **ما هي Literal Types؟**

هي نوع بيانات يسمح لك بتحديد قيمة **ثابتة** لمتغير بدلًا من نوع عام مثل `string` أو `number`.

### **🔹 مثال على String Literal Type**

```ts
let direction: "left" | "right" | "up" | "down";

direction = "left";  // ✅ مسموح
direction = "right"; // ✅ مسموح
// direction = "forward"; // ❌ خطأ، لأن "forward" غير مسموح به
```

🔹 `direction` يمكنه فقط تخزين `"left" | "right" | "up" | "down"`.

---
```ts 
type nums = 0 | 1 | -1;
function compare(num1: number, num2: number): nums {
  if (num1 == num2) {
    return 0;
  } else if (num1 > num2) {
    return 1;
  } else {
    return -1;
  }
}
console.log(compare(20, 20));
console.log(compare(20, 30));
console.log(compare(20, 10));
```

---
