

### 1. View Child
#### ما هو `@ViewChild`؟
- `@ViewChild` هو ديكوريتور في Angular يُستخدم للوصول إلى عنصر في القالب (Template) أو مكون فرعي (Child Component) من داخل الكود البرمجي (TypeScript).
- يسمح لك بالتحكم في العناصر مثل `<input>` أو مكونات أخرى مباشرة من الـ TS.

#### متى نستخدمه؟
- عندما تريد الوصول إلى عنصر في الـ HTML (مثل `<input>`) لتغيير قيمته، إضافة فوكاس، إلخ.
- عندما تريد الوصول إلى مكون فرعي لاستدعاء دواله أو الوصول إلى خصائصه.

#### طرق استخدام `@ViewChild`:
1. **الوصول إلى عنصر في الـ HTML باستخدام Reference**:
   - في الـ HTML، نضع `#` على العنصر لإعطائه اسم مرجعي (Reference).
   - مثال:
     ```html
     <input type="text" #usernameInput />
     <button (click)="change()">Change</button>
     ```
   - في الـ TS:
     ```typescript
     import { Component, ViewChild, ElementRef, AfterViewInit } from '@angular/core';

     @Component({
       selector: 'app-example',
       template: `...`
     })
     export class ExampleComponent implements AfterViewInit {
       @ViewChild('usernameInput') myInput!: ElementRef;

       ngAfterViewInit(): void {
         // التأكد من تحميل العنصر
         this.myInput.nativeElement.value = 'Abderhman';
         this.myInput.nativeElement.focus(); // إضافة فوكاس
       }

       change() {
         this.myInput.nativeElement.value = 'Changed!';
       }
     }
     ```
   - **ملاحظة**: نستخدم `ngAfterViewInit` لأن العناصر في القالب لا تكون جاهزة إلا بعد تحميل العرض.

2. **الوصول إلى مكون فرعي (Child Component)**:
   - إذا كنت تريد الوصول إلى مكون مثل `ProductsComponent`:
     ```html
     <app-products #productsComp></app-products>
     ```
     ```typescript
     import { Component, ViewChild, AfterViewInit } from '@angular/core';
     import { ProductsComponent } from './products.component';

     @Component({
       selector: 'app-parent',
       template: `...`
     })
     export class ParentComponent implements AfterViewInit {
       @ViewChild(ProductsComponent) productsComp!: ProductsComponent;

       ngAfterViewInit(): void {
         console.log(this.productsComp); // الوصول إلى المكون
         console.log(this.productsComp.total); // الوصول إلى خاصية total
       }
     }
     ```

#### الفرق بين `@ViewChild` و`@Input`/`@Output`:
- **`@ViewChild`**:
  - يُستخدم للوصول **مباشرة** إلى عنصر أو مكون في القالب.
  - مثال: تغيير قيمة `<input>` أو استدعاء دالة في مكون فرعي.
  - يعتمد على **الوصول الفيزيائي** للعنصر/المكون في الـ DOM.
  - **الاستخدام**: عندما تحتاج إلى التحكم الكامل (مثل وضع فوكاس، تغيير قيمة، إلخ).

- **`@Input`**:
  - يُستخدم لتمرير **بيانات** من المكون الأب (Parent) إلى المكون الفرعي (Child).
  - مثال: تمرير `receivedCatId` إلى `ProductsComponent`:
    ```html
    <app-products [receivedCatId]="selectedCategory"></app-products>
    ```
    ```typescript
    @Input() receivedCatId: number = 0;
    ```
  - **الاستخدام**: لإرسال بيانات ثابتة أو متغيرة إلى المكون الفرعي.

- **`@Output`**:
  - يُستخدم لإرسال **أحداث** من المكون الفرعي إلى المكون الأب.
  - مثال: إرسال `total` من `ProductsComponent` إلى `OrederComponent`:
    ```html
    <app-products (onTotalPriceChanged)="CalcTotalPrice($event)"></app-products>
    ```
    ```typescript
    @Output() onTotalPriceChanged = new EventEmitter<number>();

    add() {
      this.onTotalPriceChanged.emit(this.total);
    }
    ```
  - **الاستخدام**: عندما يحتاج المكون الفرعي إلى إخبار الأب بتغيير (مثل تحديث إجمالي السعر).

- **الخلاصة**:
  - `@ViewChild`: للتحكم المباشر في العناصر/المكونات (مثل تغيير قيمة أو استدعاء دالة).
  - `@Input`: لتمرير بيانات إلى الفرعي.
  - `@Output`: لإرسال أحداث من الفرعي إلى الأب.

#### ملاحظات على الكود المقدم:
- في الكود:
  ```typescript
  @ViewChild(ProductsComponent) ProjecrObj!: ElementRef; // خطأ
  ```
  - هنا خطأ لأن `ProjecrObj` يجب أن يكون من نوع `ProductsComponent` وليس `ElementRef`. `ElementRef` تُستخدم لعناصر HTML فقط (مثل `<input>`). التصحيح:
    ```typescript
    @ViewChild(ProductsComponent) ProjecrObj!: ProductsComponent;
    ```
- أيضًا، استخدام `nativeElement` مع مكون فرعي غير صحيح. بدلاً من ذلك، يمكنك الوصول إلى خصائص المكون مباشرة:
  ```typescript
  ngAfterViewInit(): void {
    console.log(this.ProjecrObj.total); // الوصول إلى total
  }
  ```

---

### 2. Services
#### ما هي الـ Service؟
- الـ Service هي **كلاس** في Angular تُستخدم لمشاركة البيانات أو الوظائف بين المكونات (Components) غير المرتبطة مباشرة.
- تُستخدم لتخزين البيانات (مثل قائمة المنتجات) أو تنفيذ عمليات مشتركة (مثل استدعاء API).

#### لماذا نستخدم الـ Services؟
- لتجنب تكرار الكود.
- لمشاركة البيانات بين مكونات مختلفة.
- لفصل المنطق (Logic) عن المكونات.

#### أنماط التصميم المستخدمة:
1. **Singleton Design Pattern**:
   - الـ Service تكون **نسخة واحدة** (Instance) في التطبيق بأكمله.
   - عند استخدام `providedIn: 'root'`، يتم إنشاء نسخة واحدة فقط تُشاركها جميع المكونات.
2. **Dependency Injection (DI)**:
   - Angular يقوم بحقن (Inject) الـ Service تلقائيًا في المكونات عبر الـ Constructor.
   - مثال:
     ```typescript
     constructor(private _ProductsService: ProductsService) {}
     ```

#### مثال من الكود:
```typescript
import { Injectable } from '@angular/core';
import { Iproducts } from '../iproducts';

@Injectable({
  providedIn: 'root' // Singleton: نسخة واحدة لكل التطبيق
})
export class ProductsService {
  Products: Iproducts[];

  constructor() {
    this.Products = [
      { id: 1, name: 'laptop', catid: 1, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 2, name: 'laptop', catid: 1, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 3, name: 'iphone', catid: 2, price: 5000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 4, name: 'Oppo', catid: 2, price: 6000, quantity: 2, imgUrl: 'assets/image.png' },
      { id: 5, name: 'Linobo- tablet', catid: 3, price: 2000, quantity: 0, imgUrl: 'assets/image.png' },
      { id: 6, name: 'samsung', catid: 2, price: 20000, quantity: 0, imgUrl: 'assets/image.png' },
    ];
  }

  GetAllProducts(): Iproducts[] {
    return this.Products;
  }

  GetProductById(id: number): Iproducts | null {
    let prod = this.Products.find((e) => e.id === id); // تصحيح: == → ===
    return prod ? prod : null;
  }

  GetProductByCatgId(catId: number): Iproducts[] {
    if (catId === 0) {
      return this.Products;
    }
    return this.Products.filter((e) => e.catid === catId); // تصحيح: = → ===
  }
}
```

#### كيف يتم استخدام الـ Service في المكون؟
- يتم حقن الـ Service في الـ Constructor:
  ```typescript
  constructor(private _ProductsService: ProductsService) {
    this.Products = this._ProductsService.GetAllProducts();
  }
  ```
- يمكن استدعاء دوال الـ Service مثل:
  ```typescript
  ngOnChanges(changes: SimpleChanges) {
    if (changes['receveCatId']) {
      this.FilterProductsList = this._ProductsService.GetProductByCatgId(this.receveCatId);
    }
  }
  ```

#### تصحيحات على الكود:
1. في `GetProductById` و`GetProductByCatgId`:
   - استخدمت `=` بدلاً من `===` في الفلترة، وهذا خطأ لأن `=` للتعيين وليس للمقارنة. التصحيح:
     ```typescript
     GetProductById(id: number): Iproducts | null {
       let prod = this.Products.find((e) => e.id === id);
       return prod ? prod : null;
     }

     GetProductByCatgId(catId: number): Iproducts[] {
       if (catId === 0) {
         return this.Products;
       }
       return this.Products.filter((e) => e.catid === catId);
     }
     ```
2. في المكون:
   - لاحظت أنك كتبت `receveCatId` بدلاً من `receivedCatId`. تأكد من استخدام الاسم الصحيح:
     ```typescript
     @Input() receivedCatId: number = 0; // وليس receveCatId
     ```

#### نصائح:
- إذا كنت تخطط لجلب البيانات من API، استخدم `HttpClient` داخل الـ Service بدلاً من تخزين البيانات يدويًا.
- احرص على فصل المنطق (مثل تصفية المنتجات) إلى الـ Service قدر الإمكان لجعل المكونات أخف.

---

# Routing في Angular


Routing هو النظام اللي بيسمحلك تتنقل بين الصفحات (components) في تطبيق الـ Single Page Application (SPA) من غير ما تعمل Reload للصفحة.

---

### 🧩 1. إعداد مسارات (Routes)

```ts
const routes: Routes = [
  { path: '', redirectTo: 'version', pathMatch: 'full' },
  { path: 'version', component: VersionComponent },
  { path: 'order', component: OrderComponent },
];
```

- `path`: هو رابط الصفحة.
    
- `component`: الكومبوننت اللي هيتعرض لما المستخدم يدخل على المسار ده.
    
- `redirectTo`: تحويل تلقائي لمسار معين.
    
- `pathMatch: 'full'`: معناها لازم يطابق المسار بالكامل عشان يتحول.
    

---

### 🧩 2. استخدام `<router-outlet>`

```html
<router-outlet></router-outlet>
```

- المكان اللي بيتعرض فيه الكومبوننت اللي الراوتر وصّل ليه.
    
- بنكتبه في الكومبوننت الرئيسي (غالبًا `app.component.html`).
    

---

### 🧩 3. Children Routes (المسارات الفرعية)

```ts
{
  path: 'dashboard',
  component: DashboardComponent,
  children: [
    { path: 'settings', component: SettingsComponent },
    { path: 'profile', component: ProfileComponent },
  ]
}
```

- مفيدة في لو عندك layout ثابت وبتغير محتوى فرعي حسب الرابط.
    

---

### 🧩 4. RouterLink

```html
<a routerLink="/order">Order</a>
<button [routerLink]="['/version']">Go to Version</button>
```

- زي `<a href>` لكن بدون ما تعمل reload للصفحة.

---

## إعادة التوجيه (Redirect)

```typescript
{ 
  path: '', 
  redirectTo: 'vershuin',
  pathMatch: 'full'
}
```
- `redirectTo`: يحدد المسار الجديد لإعادة التوجيه إليه
- `pathMatch: 'full'` يعني أن المسار يجب أن يتطابق تمامًا مع URL

## أفضل الممارسات الإضافية

1. **تجميع المسارات (Route Groups)**:
   - تنظيم المسارات ذات الصلة معًا في ملفات منفصلة

2. **الحماية (Guards)**:
   ```typescript
   { 
     path: 'admin', 
     component: AdminComponent,
     canActivate: [AuthGuard] 
   }
   ```
   - التحكم في الوصول إلى المسارات

3. **تحميل الكسول (Lazy Loading)**:
   ```typescript
   { 
     path: 'dashboard', 
     loadChildren: () => import('./dashboard/dashboard.module').then(m => m.DashboardModule) 
   }
   ```
   - تحسين الأداء عن طريق تحميل الميزات عند الحاجة فقط

4. **إدارة الحالة مع المسارات**:
   - يمكن تمرير البيانات عبر `state` في `router.navigate`

5. **أحداث الروتر**:
   ```typescript
   import { Router, NavigationEnd } from '@angular/router';

   constructor(private router: Router) {
     router.events.subscribe(event => {
       if (event instanceof NavigationEnd) {
         // تنفيذ إجراءات عند تغيير المسار
       }
     });
   }
   ```

## نصائح عملية

- استخدم أسماء وصيغ متسقة للمسارات
- تجنب المسارات المتداخلة بشكل مفرط
- استفد من المعلمات في المسارات (`/user/:id`)
- ضع في اعتبارك تجربة المستخدم عند تصميم تدفق التنقل  