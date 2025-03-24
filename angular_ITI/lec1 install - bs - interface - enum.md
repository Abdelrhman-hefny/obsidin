
---

## Angular قبل الإصدار 17

- **اعتماد الموديولز:**  
    في الإصدارات السابقة لـ Angular، كان التطبيق يعتمد على نظام الـ Modules لتقسيم الوظائف والأجزاء، حيث يتم تجميع المكونات والخدمات في ملفات مستقلة.
    
- **تثبيت نسخة أقل من 17:**  
    لإنشاء مشروع Angular باستخدام النظام القائم على الموديولز، يتم استخدام الأمر التالي:
    
    ```bash
    ng new lec1 --no-standalone
    ```
    
    هذا يضمن أن المشروع يتم إعداده باستخدام أسلوب الإصدارات القديمة.
    

---

## مكونات الكومبوننت

- **الملفات الأساسية لكل كومبوننت:**  
    يتكون كل كومبوننت من 3 ملفات رئيسية:
    
    - **HTML:** يحتوي على التصميم والعرض.
    - **TypeScript:** يحتوي على المنطق البرمجي والتعامل مع البيانات.
    - **CSS:** مسؤول عن تنسيق وتصميم الواجهة.
- **الفرق بين الكلاس والكومبوننت:**
    
    - **الكومبوننت:** يحتوي على ديكوراتور `@Component` الذي يحدد خصائص المكون مثل الـ selector والقوالب والستايلز.
    - **الكلاس:** هو عبارة عن كود TypeScript عادي بدون ديكوراتور خاص بالمكون.
    
    **مثال توضيحي:**
    
    ```ts
    import { Component } from '@angular/core';
    
    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']  // تأكد من استخدام styleUrls وليس styleUrl
    })
    export class AppComponent {
      title = 'lec1';
    }
    ```
    

---

## تثبيت Bootstrap في Angular

- **أمر التثبيت:**
    
    لتثبيت مكتبة ng-bootstrap، يتم تنفيذ الأمر التالي:
    
    ```bash
    ng add @ng-bootstrap/ng-bootstrap
    ```
    
- **إضافة Bootstrap إلى المشروع:**  
    بعد التثبيت، يجب تعديل ملف `angular.json` في جزء `styles` ليحتوي على مسار ملف CSS الخاص بـ Bootstrap:
    
    ```json
    "node_modules/bootstrap/dist/css/bootstrap.min.css"
    ```
    
- **ملاحظات إضافية:**
    
    - مكتبة Bootstrap العادية قد لا تعمل مباشرةً مع Angular بسبب اعتماده على TypeScript.
    - يُفضل استخدام مكتبة `ng-bootstrap` التي تم تعديلها لتناسب Angular.
- **مصادر للتصميم:**
    
    - 🔗 [ng-bootstrap](https://ng-bootstrap.github.io/#/home)
    - 🔹 [Angular Material](https://material.angular.io/)
    - 🔹 [CoreUI for Angular](https://coreui.io/angular/docs/components/accordion/)

---

## التعامل مع البيانات (API) والـ Interface

- **أهمية الـ Interface:**  
    عند جلب البيانات من API، من الأفضل تعريف شكل البيانات باستخدام `Interface` بدلاً من عرضها مباشرة. هذا يساهم في التحقق من نوع البيانات ويساعد في كتابة كود نظيف.
    
- **إنشاء Interface باستخدام CLI:**  
    يتم إنشاء ملف Interface باستخدام الأمر:
    
    ```bash
    ng g i Istrore
    ```
    
- **مثال على تعريف Interface:**
    
    ```ts
    export interface Istrore {
      name: string;
      imgUrl: string;
      branches: string[];
    }
    ```
    
- **استخدام Interface في الكومبوننت:**
    
    ```ts
    export class HomeComponent {
      myStore: Istrore;
    
      constructor() {
        this.myStore = {
          name: '$M',
          imgUrl: 'https://picsum.photos/200/300',
          branches: ['minya', 'asyute', 'alx'],
        };
      }
    }
    ```
    
- **بديل آخر:**  
    يمكن أيضًا تعريف مصفوفة داخل الكلاس:
    
    ```ts
    export class ProductsComponent {
      Products: Iproducts[] = [];
    }
    ```
    
    لكن الطريقة الأولى تعتبر من **Best Practice**.
    

---

## عرض المتغيرات في HTML

- **Interpolation (الإنتربوليشن):**  
    طريقة لعرض المتغيرات داخل القالب باستخدام الأقواس المزدوجة:
    
    ```html
    <h1>{{ myStore.name }}</h1>
    ```
    
- **استخدام `*ngFor` للتكرار:**  
    لتكرار عناصر المصفوفة وعرضها:
    
    ```html
    <div *ngFor="let item of myStore.branches">
      <h1>{{ item }}</h1>
    </div>
    ```
    
- **Property Binding (ربط الخصائص):**  
    لربط خصائص HTML بالمتغيرات في الكود:
    
    ```html
    <img [src]="myStore.imgUrl" />
    ```
    
- **تحسين الأداء باستخدام `trackBy`:**  
    في Angular 17 يمكن استخدام خاصية `trackBy` مع `*ngFor` لتحسين الأداء عند التعامل مع قوائم كبيرة:
    
    ```html
    <h3 *ngFor="let branch of myStore.branches; trackBy: trackByFn">
      {{ branch }}
    </h3>
    ```
    

---

## استخدام Enums

- **تحويل الـ enum إلى مصفوفة:**  
    يمكن تحويل الـ enum إلى Array باستخدام `Object.values` وعرضه باستخدام `*ngFor`.
    
    **مثال:**
    
    ```ts
    import { Component } from '@angular/core';
    import { FeatureDescriptions, Features } from '../../enums/cards';
    import { CommonModule } from '@angular/common';
    
    @Component({
      selector: 'app-why-ch-eb',
      standalone: true,
      imports: [CommonModule],
      templateUrl: './why-ch-eb.component.html',
      styleUrl: './why-ch-eb.component.scss',
    })
    export class WhyChEBComponent {
      FeaturesList = Object.values(Features); // تحويل الـ enum إلى Array
      FeatureDescriptions = FeatureDescriptions;
    }
    ```
    
    وفي القالب:
    
    ```html
    <div *ngFor="let feature of FeaturesList; let i = index">
      <h3>{{ feature }}</h3>
    </div>
    ```
    

---

## المفاهيم الثابتة والمتغيرة

- **Interface:**  
    مثل `myStore: Istrore;`، تُستخدم لتحديد نوع البيانات حتى وإن لم تكن القيم معروفة مسبقاً.
    
- **Enum:**  
    مثل `companyType = CompanyType;`، يُستخدم لتعريف بيانات ثابتة يمكن استخدامها مباشرةً داخل التطبيق دون الحاجة لتهيئة.
    

---

## الـ Class في Angular

- **إنشاء Class باستخدام CLI:**
    
    لإنشاء كلاس جديد يمثل كائنًا أو كيانًا، يمكن استخدام الأمر:
    
    ```bash
    ng g class product
    ```
    
- **مثال على تعريف Class يمثل منتجًا:**
    
    ```ts
    export class Product {
      constructor(
        public name: string,
        public price: number,
        public category: string
      ) {}
    
      getDiscountedPrice(discount: number): number {
        return this.price - (this.price * discount) / 100;
      }
    }
    ```
    
- **استخدام الـ Class داخل الكومبوننت:**  
    يمكن إنشاء كائن من الـ Class وعرض خصائصه في القالب:
    
    ```ts
    this.product = new Product('laptop', 4000, 'electronics');
    ```
    
    وعرضه في HTML:
    
    ```html
    <h1>{{ product.price }}</h1>
    ```
    
