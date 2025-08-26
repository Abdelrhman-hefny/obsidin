# Angular Binding & Structural Directives 

## 1) الأساسيات التي سنبني عليها

- **Binding**: نقل بيانات بين TypeScript (المكوّن) وواجهة العرض (HTML).
- **Structural Directives**: أوامر تغيّر هيكل الـ DOM مثل الإظهار/الإخفاء والتكرار.
- سنستخدم مشروع مصغّر: صفحة تعرض قائمة منتجات، مع حساب إجمالي السعر، وتلوين الـ card حسب حالة المخزون.

> ملحوظة: في Angular 17+ الوضع الافتراضي **Standalone**. يمكنك استخدام Modules في المشاريع القديمة—سأوضح الفروق سريعًا لاحقًا.

---

## 3) إنشاء مشروع وتجهيزه (Standalone)

1. إنشاء مشروع: `ng new e-commerce-app` ثم `cd e-commerce-app` و `ng serve -o`.
2. أضف **Bootstrap** اختياريًا عبر `npm i bootstrap` ثم اربط CSS في `angular.json` أو في `styles.css`.
3. أنشئ مكوّنات: `ng g c header`, `ng g c footer`, `ng g c products`.
4. أنشئ الواجهات (Interfaces) في مجلّد `models/`:
    

```ts
// src/app/models/product.ts
export interface IProduct {
  id: number;
  name: string;
  price: number;
  quantity: number;
  imgUrl: string;
  catId: number;
}

// src/app/models/category.ts
export interface ICategory {
  id: number;
  name: string;
}
```

5. اجعل عرض التطبيق بسيطًا:
    

```html
<!-- src/app/app.component.html -->
<app-header></app-header>
<div class="container my-4">
  <app-products></app-products>
</div>
<app-footer></app-footer>
```

> في Standalone: تأكّد أن `AppComponent` يستورد المكوّنات التي يستخدمها داخل `imports`.

---

## 4) أنواع الـ Binding مع أمثلة دقيقة

### 4.1 Interpolation — من TS إلى HTML

تعرض قيمة مباشرة داخل القالب:

```html
<h3>Total Price: {{ total }}</h3>
```

```ts
export class ProductsComponent {
  total = 0;
}
```

**متى أستخدمها؟** عند عرض نصوص/أرقام/قيم بسيطة أو ناتج تعبير خفيف (بدون عمليات ثقيلة).

 
---

### Property Binding (الربط بالخاصيّة)

هو إنك **تربط خاصيّة من HTML بقيمة من الكومبوننت (TS)**.

مثال:

```ts
// TS
src = 'assets/image.png';
```

```html
<!-- HTML -->
<img [src]="src" alt="404">
```

هنا `[src]` معناها: "خد القيمة من المتغيّر `src` في TS، وحطها في خاصية الصورة".

---

### ليه مش بنكتب `src="src"`؟

لأن دي كده تبقى نص عادي.  
لكن `[src]="src"` بتخليها **ديناميكية**، تتغير مع تغيّر المتغيّر في الكومبوننت.
 
---

### 4.3 Event Binding — من HTML إلى TS

 
أنا بقول للـ HTML → "لما يحصل حدث (زي كليك أو كتابة أو تغيير)… شغّل دالة من عندي في الـ TypeScript".

---

### نطبّق على الكود  

```html
<nav class="navbar navbar-expand-lg bg-body-tertiary" [attr.data-bs-theme]="title">
</nav>

<button (click)="sayHi()">غيّر الثيم</button>

  <input type="text" class="form-control" (keyup)="printInConsole($event)" aria-label="Username" aria-describedby="basic-addon1">
```

```ts
sayHi() {
  this.title = this.any ? 'dark' : 'light';
  this.any = !this.any;
}
   printInConsole(e: KeyboardEvent) {

    console.log(e.key);

   }
```

---

### الفكرة باختصار:

- ا `<nav>` مربوط بالمتغيّر `title` → بيتغير ما بين "dark" و "light".
    
- الزرار عنده **Event Binding** على `click` → أول ما تدوس عليه يشغّل `sayHi()`.
    
- ا `sayHi()` تغيّر قيمة `title` مرّة dark ومرّة light → فيتغير الثيم.
    
- الـ input عنده **Event Binding** على `keyup` → كل ما تكتب حرف ويترفع زرار الكيبورد، الدالة `printInConsole()` تشتغل وتطبع الحرف في الـ console.

---

## 1) فكرة الـ Two-Way Binding

الـ **Two-Way Binding** يعني إن القيمة اللي بكتبها في الـ input تتخزن أوتوماتيك في الـ component، والعكس صحيح: لو غيرت القيمة في الـ component تتحدث في الـ input برضه.  
في Angular ده بيتعمل باستخدام الـ **`[(ngModel)]`**.

---

## 2) تهيئة FormsModule

عشان تقدر تستخدم **`ngModel`** لازم تـimport `FormsModule`.

```ts
@NgModule({
  imports: [BrowserModule, AppRoutingModule, FormsModule],
})
export class AppModule {}
```

---
## 3) مشكلة الـ Zone.js

ممكن يطلعلك error بالشكل ده:
```
at new _NgZone (debug_node.mjs:16466:19)
```
المشكلة إن Angular معتمد على **Zone.js** لإدارة التغييرات.  

الحل:  تثبّت المكتبة:
```bash
npm i zone.js
```

2. تعمل import في `main.ts`:
```ts
import 'zone.js';
```

---

## 4) الكود في الـ HTML

```html
<p>{{ searchTirm }}</p>

<input
  type="email"
  class="form-control"
  id="exampleInputEmail1"
  aria-describedby="emailHelp"
  placeholder="Enter email"
  [(ngModel)]="searchTirm"
/>
```

# Ts
```ts
export class ProductsComponent {
  searchTirm: string = '';
}

```


### شرح :

- `<p>{{ searchTirm }}</p>`  
    بيعرض النص اللي متخزن في المتغير `searchTirm`.
    
- `[(ngModel)]="searchTirm"`  
    دي أهم حاجة: **ربط اتجاهين** بين الـ input والـ variable `searchTirm`.  
    يعني لو كتبت "[test@gmail.com](mailto:test@gmail.com)" في الـ input → تتخزن أوتوماتيك في `searchTirm`.  
    ولو غيرت `searchTirm` من الكود → يظهر التغيير في الـ input.
    
---

لو المشروع كبير وفيه **تحقق قوي (Validation) أو Inputs معقدة** → هتحتاج تسيب الـ `ngModel` وتستخدم **Reactive Forms**. لكن في البدايات، `ngModel` هو الأسهل والأوضح.

---
## template reference 

## الفكرة ببساطة

تخيل إن عندك قهوة صغيرة (المشروع بتاعك)، وجواها كباية شاي على الترابيزة (input).  
إنت عايز توصل للكباية دي بسرعة من غير لف ودوران.  
في Angular بنحط "اسم مستعار" للكباية دي عشان نعرف نمسكها بسهولة → ده هو **Template Reference Variable**.

---
## إزاي نعمله؟

بنستخدم **`#`** قدام أي عنصر في الـ HTML.

مثال:

```html
<input #myInput type="text" placeholder="اكتب اسمك" />

<button (click)="sayHi(myInput.value)">قول مرحبا</button>
```

---

## الكود في الـ Component:

```ts
export class AppComponent {
  sayHi(name: string) {
    alert('أهلا يا ' + name);
  }
}
```

---
### الاستخدام الاشهر في التحكم في عناصر ال html btn - input

زي تعمل focus أو reset للـ input.
```html
`<input #emailInput type="email" placeholder="ادخل الايميل" /> <button (click)="emailInput.focus()">ركّز على الإيميل</button> <button (click)="emailInput.value = ''">امسح</button>`
```
- هنا بستخدم الـ reference كأني ماسك العنصر بالـ DOM نفسه.
---- 
## 🔹 1) ngIf

تخيلك داخل كافيه، والويتر بيقولك:
- "لو في فلوس في جيبك هتشرب قهوة ☕"
- "لو مفيش فلوس مش هتشرب"

ده بالظبط فكرة **ngIf**: بيعرض حاجة **لو الشرط صح**، ويخفيها لو الشرط غلط.
### مثال عملي:

```html
<p *ngIf="isLoggedIn">أهلا بيك يا مستخدم</p>
<p *ngIf="!isLoggedIn">من فضلك سجل الدخول</p>
```

```ts
export class AppComponent {
  isLoggedIn = false;
}
```

- لو `isLoggedIn = true` → يظهر "أهلا بيك"     |         - لو `false` → يظهر "سجل الدخول"

---
### Use Case تاني (مع else):

```html
<p *ngIf="products.length > 0; else noProducts">
  عندنا منتجات متاحة ✅
</p>

<ng-template #noProducts>
  <p>مفيش منتجات دلوقتي ❌</p>
</ng-template>
```

- لو فيه منتجات → يطلع الرسالة الأولى.       |              لو مفيش → يطلع الرسالة التانية.
---
## 🔹 ng-template مع ngIf

الـ **ngIf** لو الشرط اتحقق بيعرض الكود اللي جواه، لكن ساعات بنحتاج نعرض **بديل (else)** لو الشرط ما تحققش.  -البديل ده بيتكتب جوه **`ng-template`**.

---
### 📌 الصيغة:

```html
<div *ngIf="شرط; else اسمTemplate">
  اللي هيظهر لو الشرط صح ✅
</div>

<ng-template #اسمTemplate>
  اللي هيظهر لو الشرط غلط ❌
</ng-template>
```

### 📌 مثال عملي:
تخيلك فاتح صفحة منتجات:
 لو فيه منتجات → يطلعلك "فيه منتجات".
- لو مفيش → يطلعلك رسالة "مفيش منتجات متاحة".
 
    ```html
<div *ngIf="products.length > 0; else noProducts">
  <p>🎉 عندنا منتجات متاحة!</p>
</div>

<ng-template #noProducts>
  <p>❌ مفيش منتجات دلوقتي</p>
</ng-template>
```

```ts
export class AppComponent {
  products: string[] = []; // جرب تخليها ['Laptop', 'Phone'] وشوف الفرق
}
```

- لو `products` فيها بيانات → هيظهر "🎉 عندنا منتجات متاحة!"
 لو فاضية → هيظهر محتوى `ng-template` → "❌ مفيش منتجات دلوقتي".
    
---
## 🔹 2) ngSwitch

تخيلك راكب أوبر 🚗، والسواق بيسألك:

- "رايح فين؟"
- لو قلت "وسط البلد" → ياخد طريق معين.
- لو قلت "المطار" → ياخد طريق تاني.
- لو قلت حاجة تالتة → يختار طريق افتراضي.

ده هو **ngSwitch**: بيختار حاجة واحدة من كذا اختيار حسب القيمة.
### مثال عملي:

```html
<div [ngSwitch]="role">
  <p *ngSwitchCase="'admin'">أهلا يا مدير 👨‍💻</p>
  <p *ngSwitchCase="'user'">أهلا يا مستخدم 👤</p>
  <p *ngSwitchDefault>أهلا بالزائر 🙋</p>
</div>
```

```ts
export class AppComponent {
  role = 'user';
}
```

- لو `role = 'admin'` → يطلع "أهلا يا مدير"   
- لو `role = 'user'` → "أهلا يا مستخدم"    
- غير كده → "أهلا بالزائر"

---
### باختصار:

- **ngIf** = لو الشرط صح → اعرض، لو غلط → متعرضش (زي قرار القهوة حسب الفلوس).
- **ngSwitch** = عندك كذا اختيار، تختار واحد بس حسب القيمة (زي السواق والوجهة).

---
تمام ✅

##   **`*ngFor`**

بنستخدمه عشان نكرّر عنصر معين في الـ HTML على حسب عدد العناصر في **Array** أو **List**.
### ✨ مثال عملي:

```html
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

📌 لو `items = ['Angular', 'React', 'Vue']`  
النتيجة هتكون:  - Angular    - React     - Vue
    

---

## ⚠️ **تحذير: بعض التوجيهات (Directives) في Angular أصبحت مهملة (Deprecated) وقد تُحذف في الإصدارات القادمة، لذلك لا يُنصح باستخدامها في المشاريع الجديدة.**

---
#  في المشاريع الجديدة. 

ال Angular قدمت بنية جديدة للـ control flow: `@if`, `@for`, `@switch` بدل `*ngIf`, `*ngFor`, `ngSwitch` — الأسلوب الجديد أوضح وكتابي أكتر، وفيه أدوات مساعدة (ونصّحوا بالترحيل باستخدام الـ schematic). ([Angular](https://v17.angular.io/guide/control_flow?utm_source=chatgpt.com "Built-in control flow - Angular"), [Angular](https://angular.dev/reference/migrations/control-flow?utm_source=chatgpt.com "Migration to Control Flow syntax - Angular"))

---

## 1) `@if` — الشرط (Condition)

**الفكرة:** تستعملها عشان تعرض HTML فقط لو الشرط صحيح. بدل `*ngIf` لكن بشكل بلوك واضح وسهل القراءة.

**الـ Syntax:**

```ts
@if (isLoggedIn) {
  <p>أهلا {{ user.name }}</p>
} @else if (isPending) {
  <p>جاري المعالجة...</p>
} @else {
  <p>من فضلك سجل الدخول</p>
}
```

**خليها في كتابك — مثال واقعى (صحبك):**  
تخيّل صاحبي داخل موقع — لو مسجّل يدخل لصفحته، لو مش مسجّل يشوف زر تسجيل. سهلة زي ما بتقول للويتر: "لو الزبون معاه تذكرة دخله، ديله مكان".

**ميزة عملية مهمة:** تقدر تخزن نتيجة تعبير طويل باسم داخل البلوك وتستخدمه (زي `as` في docs). مثال:

```ts
@if (user$ | async as user) {
  <p>أهلا {{ user.name }}</p>
} @else {
  <p>تحميل...</p>
}
```

(الميزة دي مذكورة في دليل التحكم بالتدفق). ([Angular](https://angular.dev/guide/templates/control-flow?utm_source=chatgpt.com "Control flow - Angular"))

**Use-cases شائعة:**

- عرض لوحة المستخدم لو هو مسجّل، وإظهار صفحة تسجيل لو لا.
    
- عرض حالة تحميل/خطأ عند جلب بيانات من API.
    
- إظهار أجزاء من الواجهة حسب صلاحيات (admin/user).
    

---

## 2) `@for` — التكرار (Loop)

**الفكرة:** تكرّر بلوك لكل عنصر في مصفوفة، مع ضرورة تحديد **طريقة التتبع (track)** عشان يحافظ Angular على DOM بكفاءة. ده بديل لـ `*ngFor` لكن مع صيغة أوضح، وفيه جزء `@empty` للحالة الفاضية. ([Angular](https://angular.dev/api/core/%40for?utm_source=chatgpt.com "@for • Angular"))

**الـ Syntax:**

```ts
@for (let task of tasks; track task.id) {
  <li>{{ task.title }}</li>
} @empty {
  <li>مفيش مهام دلوقتي</li>
}
```


**ملاحظة مهمّة:** الـ `track` مطلوب في الصيغة الجديدة عشان الأداء؛ لازم تختار حقل ثابت (مثل `id`) أو تعبير يميّز العنصر. ([Angular](https://angular.dev/api/core/%40for?utm_source=chatgpt.com "@for • Angular"))

- عرض مجموعة ثمّ عرض رسالة “لا توجد عناصر” باستخدام `@empty`.
    

---

## 3) `@switch` — اختيار حالة من عدة حالات

**الفكرة:** لما يكون عندك قيمة بتاخد منها قرار بين كذا حالة، استخدم `@switch` بدل `ngSwitch` — بنية بلوكية ونظيفة.

**الـ Syntax:**

```ts
@switch (orderStatus) {
  @case ('pending') {
    <p>طلبك تحت المعالجة ⏳</p>
  }
  @case ('shipped') {
    <p>طلبك في الطريق 🚚</p>
  }
  @default {
    <p>حالة غير معروفة</p>
  }
}
```

**ملاحظات عملية:** `@switch` يسهل قراءة القوالب لما الحالات كتيرة، ومناسب لعرض badges أو رسائل حالة. ([Angular University](https://blog.angular-university.io/angular-switch/?utm_source=chatgpt.com "Angular @switch: Complete Guide"), [Angular](https://v17.angular.io/guide/control_flow?utm_source=chatgpt.com "Built-in control flow - Angular"))

**Use-cases شائعة:**

- حالة الطلب في e-commerce (pending/shipped/delivered).
- اختيار Template بناءً على نوع عنصر (image/video/text).

---

