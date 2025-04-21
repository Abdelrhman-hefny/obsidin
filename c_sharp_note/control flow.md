
🎯 التحكم في تدفق الكود في C#:

1️⃣ اتخاذ القرارات:
   - if (شرط) { كود } else { كود }
   - switch (حالة) { case 1: كود; break; default: كود }

2️⃣ الحلقات التكرارية:
   - for (بداية; شرط; تحديث) { كود }
   - while (شرط) { كود }
   - do { كود } while (شرط)
   - foreach (var item in collection) { كود }

3️⃣ التحكم في الحلقات:
   - break → يخرج من الحلقة
   - continue → يتخطى التكرار الحالي
   - return → ينهي تنفيذ الدالة فورًا

---
- التيرنري أوبريتر (?:) هو اختصار لجملة if-else.
- الصيغة: condition ? expression1 : expression2;
- مناسب للقرارات البسيطة مثل التحقق من الأرقام الزوجية، العمر، الدرجات، إلخ.
- لا يُنصح باستخدامه في الحالات المعقدة لتجنب صعوبة القراءة.

---
1. `for`:
   - يُستخدم عند الحاجة إلى عداد (Counter).
   - مفيد للوصول إلى عناصر باستخدام `index`.
   - الصيغة: for(initialization; condition; increment) { ... }
     ```c#
int[] numbers = { 10, 20, 30, 40, 50 };
for (int i = 0; i < numbers.Length; i++){
    Console.WriteLine(numbers[i]);}
// Output: 10 20 30 40 50
```

2. `foreach`:
   - يُستخدم لتكرار العناصر في المصفوفات والقوائم بسهولة.
   - لا يحتاج إلى `index` لكنه لا يسمح بتعديل القيم.
   - الصيغة: foreach (type variable in collection) { ... }
```c#
int[] numbers = { 10, 20, 30, 40, 50 };
foreach (int num in numbers){
    Console.WriteLine(num);}
// Output: 10 20 30 40 50
```

---


task for loop:
![[Z_IMAGES/Pasted image 20250316082906.png]]
```c#

        int[] arry = { 1, 2, 3, 4, 5, 6 };
        int[] arr2 = new int[arry.Length];

        for (int i = 0; i < arry.Length; i++)
        {
            arr2[arry.Length - 1 - i] = arry[i];
        }
        
        Console.WriteLine("المصفوفة المعكوسة:");
        foreach (int num in arr2)
        {
            Console.Write(num + " ");
        }


```

```c#
            Console.WriteLine("enter the number of array: ");
            int numberSize = int.Parse(Console.ReadLine() ?? "0");
            int[] arrayNumber = new int[numberSize];
            for (int i = 0; i < numberSize; i++)
            {
                Console.WriteLine($"enter the item {i + 1}: ");
                arrayNumber[i] = int.Parse(Console.ReadLine() ?? "0");

            }
            int[] reverseArrNum = new int[numberSize];
            for (int i = numberSize - 1; i >= 0; i--)
            {
                reverseArrNum[numberSize - i - 1] = arrayNumber[i];
            }
            Console.WriteLine(string.Join(",", reverseArrNum));

```