في الدرس ده، بنتعلم إزاي نربط البيانات بين الكود البرمجي (Component) والـ HTML باستخدام **الربط (Binding)**، وكمان إزاي نتحكم في هيكل الصفحة باستخدام **التوجيهات الهيكلية (Structural Directives)**.

---

### **1. الربط في Angular (Binding)**

الربط هو الطريقة اللي بننقل بيها البيانات بين الكومبوننت والـ View. ينقسم لنوعين رئيسيين:

#### **1.1 ربط البيانات باتجاه واحد (One-Way Data Binding)**  
يعني البيانات بتتحرك في اتجاه واحد بس، يا من الكومبوننت للـ View أو العكس.

- **Interpolation ({{ }})**: بتعرض قيم المتغيرات في الـ HTML.  
  مثال من كودك:  
  ```html
  <h3 class="container m-auto text-center w-100 my-3">
    Total Price: {{ total }}
  </h3>
  ```
  ```typescript
  export class ProductsComponent {
    total = 0; // القيمة بتتعرض في الـ HTML
  }
  ```

- **Property Binding ([ ])**: بتربط خاصية في الـ HTML بقيمة من الكومبوننت.  
  مثال من كودك:  
  ```html
  <div
    class="card cursor-pointer"
    [style.border-color]="card.quantity == 0 ? 'red' : 'white'"
  >
  ```
  هنا لون الحدود بيتغير ديناميكيًا بناءً على قيمة `quantity`.

- **Event Binding (( ))**: بتربط حدث (زي النقر) بدالة في الكومبوننت.  
  مثال من كودك:  
  ```html
  <button class="btn" (click)="add()">add +</button>
  ```
  ```typescript
  add() {
    if (this.btnNumber == 3) {
      return;
    }
    this.btnNumber++;
  }
  ```

#### **1.2 ربط البيانات باتجاهين (Two-Way Data Binding)**  
بيسمح بتحديث البيانات في الاتجاهين باستخدام `[(ngModel)]`.  
مثال من كودك:  
```html
<select name="" id="" [(ngModel)]="btnNumber">
  <option *ngFor="let btn of Categories" [value]="btn.id">
    {{ btn.name }}
  </option>
</select>
<h3>{{ btnNumber }}</h3>
```
```typescript
export class ProductsComponent {
  btnNumber = 0;
  Categories: Icatigory[] = [
    { id: 1, name: 'laptop' },
    { id: 2, name: 'phone' },
    { id: 3, name: 'tablet' },
  ];
}
```
هنا لما تختار قيمة من القائمة، `btnNumber` بتتغير، والعكس صحيح.

---

### **2. المتغيرات المرجعية (Template Variables)**

- **Template Variable (#var)**: بتخليك توصل لعنصر في الـ HTML مباشرة.  
  مثال من كودك:  
  ```html
  <input
    #InputPrice
    type="text"
    class="form-control input"
    placeholder="enter count"
  />
  <button
    (click)="TotalPrice(+InputPrice.value, card.price)"
    class="btn btn-primary m-auto"
  >
    show total price
  </button>
  ```
  ```typescript
  TotalPrice(inputPrice: number, price: number) {
    this.total += inputPrice * price;
  }
  ```
  هنا `#InputPrice` بياخد قيمة الإدخال ويمررها للدالة `TotalPrice`.

---

### **3. التوجيهات الهيكلية (Structural Directives)**

بتستخدم لتعديل هيكل الـ DOM.

- **`*ngFor`**: بتكرر عنصر لكل حاجة في قايمة.  
  مثال من كودك:  
  ```html
  <div class="col container" *ngFor="let card of Products">
    <div
      class="card cursor-pointer"
      [style.border-color]="card.quantity == 0 ? 'red' : 'white'"
    >
      <img class="card-img-top" src="{{ card.imgUrl }}" />
      <div class="card-body">
        <div class="card-title">{{ card.name }}</div>
        <p class="card-text">Price: {{ card.price }}$</p>
        <p class="card-text">quantity: {{ card.quantity }}</p>
        <input
          #InputPrice
          type="text"
          class="form-control input"
          placeholder="enter count"
        />
        <button
          (click)="TotalPrice(+InputPrice.value, card.price)"
          class="btn btn-primary m-auto"
        >
          show total price
        </button>
      </div>
    </div>
  </div>

```

  ```typescript
  export class ProductsComponent {
    Products: Iproducts[] = [
      { id: 1, name: 'laptop', catid: 1, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 2, name: 'laptop', catid: 1, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 3, name: 'iphone', catid: 2, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 4, name: 'Oppo', catid: 2, price: 6000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 4, name: 'samsung', catid: 2, price: 20000, quantity: 0, imgUrl: 'assets/image.png' },
      { id: 5, name: 'Linobo- tablet', catid: 3, price: 2000, quantity: 0, imgUrl: 'assets/image.png' },
    ];
  }
  ```

- **`*ngIf`**: بتظهر أو تخفي عنصر بناءً على شرط.  
  مش موجودة في كودك بشكل مباشر، لكن ممكن نضيفها لو عايز تتحكم في ظهور حاجة معينة.

---

### **4. مثال عملي: حساب السعر الإجمالي**

مثال من كودك:  
```typescript
export class ProductsComponent {
  total = 0;
  TotalPrice(inputPrice: number, price: number) {
    this.total += inputPrice * price;
  }
}
```

```html
<input
  #InputPrice
  type="text"
  class="form-control input"
  placeholder="enter count"
/>
<button
  (click)="TotalPrice(+InputPrice.value, card.price)"
  class="btn btn-primary m-auto"
>
  show total price
</button>
<h3 class="container m-auto text-center w-100 my-3">
  Total Price: {{ total }}
</h3>
```

---

### **5. ملاحظات إضافية**

- **استخدام `[style.property]`**: لتغيير خاصية CSS ديناميكيًا.  
  مثال من كودك:  
  ```html
  [style.border-color]="card.quantity == 0 ? 'red' : 'white'"
  ```

- **استخدام `ngModel`**: لازم تستورد `FormsModule` في الكومبوننت:  
  ```typescript
  imports: [NgFor, FormsModule, NgIf],
  ```

---

