تمام 👌 خليني أرتبلك النص بشكل أنيق ومرتب من غير مسافات كتيرة، بحيث يبقى جاهز تحطه في الكتاب على طول:

---

# 🔹 مقدمة عن **Angular** (من كورس ITI - Angular 2024)

## 1) تقسيم أي بروجيكت Web

أي **ويب أبليكيشن** بيتقسم لحاجتين:

- **Client Side (الـ Frontend):** الجزء اللي بيشوفه اليوزر في المتصفح (Browser). والـ Browser مش بيفهم غير 3 حاجات بس: HTML, CSS, JavaScript. 👉 أي لغة زيادة (زي TypeScript أو SASS) لازم تتحول (Compile) للحاجات اللي فوق.
    
- **Server Side (الـ Backend):** الجزء اللي بيهندل الداتا والـ Logic. ممكن يتعمل بلغات مختلفة: Node.js / PHP / Java / Python … إلخ.
    

---

## 2) طريقة التواصل بين Frontend و Backend

- **الـ Client (المتصفح)** → يبعت **Request** (طلب).
    
- **الـ Server** → يرد بـ **Response** (استجابة).
    

🔹 الـ Request بيتكون من: URL, Method (GET/POST/PUT/DELETE), Body (لو فيه داتا مبعوتة), Headers.  
🔹 الـ Response بيرجع فيه: Status Code (200, 404, 500 ...), Body (البيانات نفسها), Headers.

---

## 3) زمان كان البروجيكت عامل إزاي؟

- **زمان:** بيعملوا بروجيكت واحد فيه Frontend + Backend مع بعض. الباك اند ديفيلوبر هو اللي بيهندل كل حاجة (حتى الصفحات).
    
- **دلوقتي:** بقينا نفصلهم:
    
    - Frontend Project لوحده (باستخدام Angular / React / Vue).
        
    - Backend Project لوحده (بيقدّم API).
        
    - الاتنين بيتواصلوا عن طريق **Request & Response**.
        

---

## 4) نوع ثالث: Serverless

لو الديفيلوبر عايز يعمل Frontend بس ومش عايز يكتب كود Backend → يستخدم خدمات زي **Firebase**: Database جاهزة, Authentication جاهز, API جاهزة. 🔥 كده يبقى اسمه **Serverless App**.

---

## 5) ليه نستخدم Framework زي Angular؟

سؤال مهم: ليه ما نشتغلش HTML + CSS + JS وخلاص؟  
لأن Angular (زي React و Vue) بتسهل علينا نعمل **Single Page Application (SPA)**.

---

## 6) يعني إيه **Single Page Application (SPA)**؟

- الطبيعي: كل ما تضغط على لينك → المتصفح يعمل **Refresh** ويجيب صفحة جديدة من السيرفر.
    
- في Angular: أول مرة تدخل الموقع بيرجعلك كل الـ Components. بعد كده لما تتحرك بين الصفحات → Angular بيبدّل الـ Components جوه نفس الصفحة **من غير Refresh**.
    

📌 مثال: Home = Component, Login = Component, Navbar = Component, Footer = Component.  
👉 لما أضغط "Login" → Angular يخفي الـ Home ويعرض الـ Login، من غير ما يجيب صفحة جديدة من السيرفر.

---

## 7) مشكلة حجم الصفحات وحلها

لو البروجيكت كبير جدًا وفيه Components كتيرة، تحميلهم كلهم مرة واحدة هيبطأ الموقع. الحل: **Lazy Loading** → بعض الـ Components تتحمل بس عند الحاجة.

---

## 8) Angular vs React (Framework ولا Library؟)

- **Angular = Framework:** بيجي معاه Modules جاهزة (Forms, HTTP, Routing …)، بيديك Structure كامل.
    
- **React = Library:** مش بيجي معاه حاجات كتير جاهزة، إنت اللي تبني الـ Structure بنفسك وتضيف أي مكتبة محتاجها.
    

---

# 🚀 إنشاء مشروع Angular

### الأمر:

```bash
ng new lec1 --no-standalone
```

- `ng new lec1` 👉 يعمل مشروع جديد اسمه **lec1**.
    
- `--no-standalone` 👉 يخلي المشروع بالنظام القديم (AppModule) بدل Standalone Components (الجديدة من Angular 17).
    

---

# 📂 فايلات المشروع بعد الإنشاء

### 1) **package.json**

- فيه الـ **dependencies** و **devDependencies** + سكريبتات.
    
- زي البطاقة التعريفية للمشروع.
    

🔹 الفرق:

- **dependencies:** الحاجات اللي المشروع محتاجها يشتغل فعليًا (زي `@angular/core`, `rxjs`).
    
- **devDependencies:** أدوات تطوير بس (زي `typescript`, `karma`, `jasmine`).
    

---

### 2) **angular.json**

ملف إعدادات المشروع:

- بيحدد بداية الكود (**sourceRoot = src**).
    
- يحدد ملف **index.html** الرئيسي.
    
- يحدد أماكن: `assets` (صور/ملفات ثابتة), `styles` (CSS عام), `scripts` (JS إضافية).
    

---

### 3) **src folder**

جوه الكود الأساسي:

- **index.html:** الصفحة الرئيسية.
    
- **main.ts:** أول ملف بيبدأ منه المشروع.
    
- **app folder:** فيه الموديول الرئيسي والكومبوننتات.
    

---

# 🧩 AppModule (الموديول الرئيسي)

### ملف: `app.module.ts`

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],   // الكومبوننتات/دايركتيف/بايب
  imports: [BrowserModule],       // الموديولات اللي محتاجها
  providers: [],                  // السيرفس
  bootstrap: [AppComponent]       // أول كومبوننت يبدأ
})
export class AppModule {}
```

📌 شرح:

- **declarations:** الكومبوننتات اللي أنا عاملها.
    
- **imports:** الموديولات الجاهزة.
    
- **providers:** السيرفس.
    
- **bootstrap:** الكومبوننت الأساسية (عادةً `AppComponent`).
    

---

# 🖼️ AppComponent (أول كومبوننت)

### ملف: `app.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'مرحبا بك في Angular 🚀';
}
```

### ملف: `app.component.html`

```html
<h1>{{ title }}</h1>
<p>أول مشروع Angular بسيط.</p>
```

📌 يعني: أول ما أفتح المشروع هيعرض الكلام ده.

---

# ▶️ تشغيل المشروع

### الأمر:

```bash
ng serve -o
```

يشغل المشروع على: [http://localhost:4200](http://localhost:4200/).  
أي تغيير في الكود يبان تلقائي.

---

# 🧪 أوامر مهمة

- `ng test` 👉 يشغل الـ Unit Tests.
    
- `ng build` 👉 يبني نسخة للنشر (في فولدر `dist`).


---

## ---------------------------------part 2-----------------------------




# Angular —  (Navbar/Home/Footer + Bootstrap)

 
---

## 1) الكومبوننت Component ببساطة

الكومبوننت = جزء UI مستقل. ملفاته:

- `*.component.ts` → المنطق والكلاس و`@Component`.
    
- `*.component.html` → القالب.
    
- `*.component.css/scss` → ستايلات خاصة به.
    
- `*.component.spec.ts` → اختبارات (يمكن تجاهلها الآن).  
    معلومة سريعة: TS بيتنفّذ أولًا ويربط HTML/CSS.
    

---

## 2) Inline Template/Styles vs External Files

### الشكل الافتراضي (External)

```ts
@Component({
  selector: 'app-sample',
  templateUrl: './sample.component.html',
  styleUrls: ['./sample.component.css']
})
```

مناسب للمشاريع لأن القالب والستايلات عادةً طويلة.

### الشكل البديل (Inline)

```ts
@Component({
  selector: 'app-inline-demo',
  template: `<h1>من داخل TS</h1>`,
  styles: [`h1{color:green}`]
})
```

مفيد للتجارب السريعة فقط.

---

## 3) نكوّن صفحة فيها Navbar/Home/Footer

هنعمل 3 كومبوننتس:

```bash
ng g c components/navbar
ng g c components/home
ng g c components/footer
```

كل كومبوننت بياخد `selector` تلقائيًا: `app-<name>`.  
للعرض داخل `app.component.html`:

```html
<app-navbar></app-navbar>
<app-home></app-home>
<app-footer></app-footer>
```

---

## 4) دور `index.html` vs `app.component.html`

- `index.html` فيه الجذر `<app-root></app-root>` وبعض الأكواد العامة فقط.
    
- التركيب الفعلي للصفحة يتم في `app.component.html`.
    

---

## 5) إضافة Bootstrap (CDN/NPM) + ترتيب `angular.json`

### A) CDN (سريع للتجربة)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"/>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

أضفهما في `index.html` (الأول داخل `<head>`، الثاني قبل `</body>`).

### B) NPM (أفضل للمشاريع)

```bash
npm i bootstrap
```

ثم داخل `angular.json` (رتّب الـ styles هكذا: Bootstrap ثم `src/styles.css` كي تُغطي تعديلاتك على Bootstrap):

```jsonc
{
  "projects": {"your-app-name": {"architect": {"build": {"options": {
    "styles": [
      "node_modules/bootstrap/dist/css/bootstrap.min.css",
      "src/styles.css"
    ],
    "scripts": [
      "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
    ]
  }}}}}
}
```

ملاحظة: أي تعديل في `angular.json`/`index.html` يحتاج إيقاف `serve` وتشغيله مجددًا.

---

## 6) JavaScript بتاع Bootstrap داخل Angular

لو استخدمت عناصر فيها سلوك JS (Dropdown/Modal/Tooltip…):

- لا تكتب JS خام داخل Angular.
    
- استخدم مكتبة متوافقة:
    
    - `@ng-bootstrap/ng-bootstrap`
        
        ```bash
        ng add @ng-bootstrap/ng-bootstrap
        ```
        
        ثم أضِف `NgbModule` في `app.module.ts`.
        
    - أو `ngx-bootstrap` (بديل مشابه).  
        الكلاسات الـ CSS العادية تكفيها Bootstrap وحدها.
        

---

## 7) الموديل/الإنترفيس: `IStore`

إنشاء:

```bash
mkdir src/app/models
ng g i models/store
```

المحتوى:

```ts
export interface IStore {
  name: string;
  imageUrl: string;
  branches: string[];
}
```

الهدف: توحيد شكل البيانات ومساعدة الـ TypeScript في الفحص أثناء التطوير.

---

## 8) ربط البيانات بسرعة

- Interpolation: `{{ value }}`.
    
- Property Binding: `[prop]="value"` مثل `[src]`/`[disabled]`.
    
- تكرار: `*ngFor="let item of items"`.
    
- (Angular 17) شكل بديل: `@for (...) { ... }`، استخدم `*ngFor` إن كنت مبتدئًا.
    

---

## 9) المثال العملي الكامل

### 9.1 `app.component.html`

```html
<app-navbar></app-navbar>
<div class="container my-4">
  <div class="row">
    <div class="col-12">
      <app-home></app-home>
    </div>
  </div>
</div>
<app-footer></app-footer>
```

### 9.2 `components/navbar/navbar.component.html`

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">MyStore</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#nav" aria-controls="nav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="nav">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item"><a class="nav-link active" aria-current="page" href="#">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Products</a></li>
        <li class="nav-item"><a class="nav-link" href="#">About</a></li>
      </ul>
    </div>
  </div>
</nav>
```

### 9.3 `components/footer/footer.component.html`

```html
<footer class="bg-light border-top py-3">
  <div class="container d-flex justify-content-between align-items-center">
    <span>© 2025 MyStore</span>
    <a href="https://iti.gov.eg" target="_blank" rel="noopener">ITI.gov.eg</a>
  </div>
</footer>
```

### 9.4 منطق الـ Home: `components/home/home.component.ts`

```ts
import { Component } from '@angular/core';
import { IStore } from '../../models/store';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {
  myStore: IStore = {
    name: 'H&M',
    imageUrl: 'https://picsum.photos/seed/store/900/350',
    branches: ['Cairo','Assiut','Sohag','Alexandria']
  };

  trackByIndex(i: number){ return i; }
}
```

### 9.5 واجهة الـ Home: `components/home/home.component.html`

```html
<h1 class="h3 mb-3">{{ myStore.name }}</h1>
<img class="img-fluid rounded mb-4" [src]="myStore.imageUrl" alt="Store image"/>
<h2 class="h5">الفروع</h2>
<ul class="list-group mb-4">
  <li class="list-group-item" *ngFor="let b of myStore.branches; index as i; trackBy: trackByIndex">{{ i+1 }}- {{ b }}</li>
</ul>
<div class="row g-3">
  <div class="col-md-6"><div class="p-3 border rounded">عمود 1</div></div>
  <div class="col-md-6"><div class="p-3 border rounded">عمود 2</div></div>
</div>
```

---

## 10) توسيع عملي مهم

### A) Events (أزرار بتشغّل دوال)

أضِف في `home.component.ts`:

```ts
newBranch = '';
addBranch(){ if(this.newBranch.trim()) { this.myStore.branches.push(this.newBranch.trim()); this.newBranch=''; } }
removeBranch(i:number){ this.myStore.branches.splice(i,1); }
```

أضِف في `home.component.html` (حقول + أزرار):

```html
<div class="input-group mb-3">
  <input class="form-control" [(ngModel)]="newBranch" placeholder="فرع جديد"/>
  <button class="btn btn-primary" (click)="addBranch()">إضافة</button>
</div>
<ul class="list-group mb-4">
  <li class="list-group-item d-flex justify-content-between align-items-center" *ngFor="let b of myStore.branches; index as i">
    <span>{{ i+1 }}- {{ b }}</span>
    <button class="btn btn-sm btn-outline-danger" (click)="removeBranch(i)">حذف</button>
  </li>
</ul>
```

> ملاحظة: لو استخدمت `[(ngModel)]` لازم تضيف `FormsModule` كما بالأسفل.

### B) Two-way Binding (`[(ngModel)]`)

في `app.module.ts`:

```ts
import { FormsModule } from '@angular/forms';
@NgModule({ imports: [BrowserModule, FormsModule], ... }) export class AppModule {}
```

### C) شرطية بـ `*ngIf` + Empty State

```html
<h2 class="h5">الفروع</h2>
<p class="text-muted" *ngIf="!myStore.branches.length">لا توجد فروع بعد.</p>
<ul class="list-group" *ngIf="myStore.branches.length">
  <li class="list-group-item" *ngFor="let b of myStore.branches">{{ b | uppercase }}</li>
</ul>
```

### D) Pipes سريعة

- `uppercase/lowercase/date/currency`… مثال: `{{ price | currency:'EGP' }}`.
    

---

## 11) Service + Dependency Injection (مختصر عملي)

إنشاء خدمة تُرجع بيانات المتجر (مؤقتًا ثابتة):

```bash
ng g s services/store
```

`store.service.ts`:

```ts
import { Injectable } from '@angular/core';
import { IStore } from '../models/store';

@Injectable({ providedIn: 'root' })
export class StoreService {
  getStore(): IStore {
    return { name:'H&M', imageUrl:'https://picsum.photos/seed/store/900/350', branches:['Cairo','Assiut','Sohag','Alexandria'] };
  }
}
```

استخدمها داخل `home.component.ts`:

```ts
constructor(private storeSvc: StoreService){}
ngOnInit(){ this.myStore = this.storeSvc.getStore(); }
```

> المعنى: بدال ما الكومبوننت يكوّن الداتا، يطلبها من Service. لاحقًا نبدّلها بنداء API.

---

## 12) (اختياري) تفعيل الـ Router وروابط الـ Navbar

```bash
ng g m app-routing --flat --module=app
```

`app-routing.module.ts`:

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './components/home/home.component';
const routes: Routes = [ { path:'', component: HomeComponent }, { path:'products', component: HomeComponent }, { path:'about', component: HomeComponent } ];
@NgModule({ imports:[RouterModule.forRoot(routes)], exports:[RouterModule] }) export class AppRoutingModule{}
```

حدّث الـ Navbar ليستعمل `routerLink`:

```html
<li class="nav-item"><a class="nav-link" routerLink="/">Home</a></li>
<li class="nav-item"><a class="nav-link" routerLink="/products">Products</a></li>
<li class="nav-item"><a class="nav-link" routerLink="/about">About</a></li>
```

وفي `app.component.html` بدّل `<app-home>` بـ:

```html
<router-outlet></router-outlet>
```

---

## 13) أخطاء شائعة + حلول

- عدّلت `angular.json`/`index.html` → أعد تشغيل السيرفر.
    
- عدّلت HTML/CSS/TS وملقتش تحديث → احفظ أي سطر في TS أو أعد التشغيل.
    
- عناصر JS لا تعمل (Dropdown/Modal…) → ثبّت `@ng-bootstrap/ng-bootstrap` أو `ngx-bootstrap`.
    
- ستايلاتك لا تُطبق بسبب Bootstrap → ضع `src/styles.css` بعد Bootstrap في `angular.json`.
    
- صور لا تظهر مع `[src]` → تأكد من `http/https` الصحيح أو استخدم مسارًا محليًا تحت `assets`.
    

---


--------------------- create new project 
أبسط طريقة لبدء مشروع Angular جديد:

```bash
# تثبيت Angular CLI (مرة واحدة على الجهاز)
npm install -g @angular/cli

# إنشاء مشروع جديد باسم example-store
ng new example-store

# الانتقال للمجلد وتشغيل المشروع
cd example-store
ng serve --open
```

**شرح:**

- `ng new example-store` ينشئ مشروع Angular مع إعدادات افتراضية.
    
- `ng serve --open` يبني المشروع ويشغل خادم تطوير ويفتح المتصفح.
    

---

# 2. إضافة Bootstrap في Angular (CDN أو Local)

**باستخدام CDN**: أضف روابط CSS وJS داخل `src/index.html` داخل `<head>` (CSS) ونهاية `<body>` (JS).

```html
<!-- index.html (داخل <head>) -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">

<!-- index.html (قبل إغلاق </body>) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
```

**باستخدام local (npm)**: ثبت Bootstrap عبر `npm` ثم ربط الملفات في `angular.json` (مفسَّل لاحقاً).

---

# 3. مشكلة JavaScript في Bootstrap العادي

- مشكلة شائعة: بعد تثبيت Bootstrap عبر `npm` وربط الملفات، بعض عناصر Bootstrap التي تعتمد على JS (مثل dropdown, modal, tooltips) لا تعمل عند استخدام الأكواد الجاڤاسكربتية غير المتوافقة مع Angular.
    
- السبب: Bootstrap **الأساسي** يعطيك كود JavaScript يعتمد على التلاعب المباشر بالـ DOM بطرق قد لا تكون متوافقة مع Angular (أو تحتاج تضمين bundle الصحيح). كما أن استخدام JS خارجي قد يتعارض مع آليات Angular (zone.js، التغييرات في lifecycle).
    

حلول:

- استخدام `bootstrap.bundle.min.js` (يحتوي على Popper) واضافته إلى `angular.json` → قد يحل معظم الحالات.
    
- الأفضل: استخدم مكتبات مخصصة لـ Angular مثل `ng-bootstrap` أو `ngx-bootstrap`، لأنهما يقدّمان نفس الـ UI لكن بواجهات TypeScript / Angular-friendly.
    

---

# 4. استخدام ng-bootstrap و ngx-bootstrap

**ng-bootstrap** (مبنية من فريق ng-bootstrap):

```bash
npm install @ng-bootstrap/ng-bootstrap
```

في `app.module.ts`:

```ts
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

@NgModule({
  imports: [NgbModule, /* ... */]
})
export class AppModule {}
```

ثم استخدم الكمبوننتات حسب التوثيق (مثال: modal, accordion, alert) عبر سينتكس Angular (بدون JS مباشر).

**ngx-bootstrap** (مكتبة أخرى شائعة):

```bash
npm install ngx-bootstrap --save
```

مثال استيراد موديول:

```ts
import { CollapseModule } from 'ngx-bootstrap/collapse';

@NgModule({
  imports: [CollapseModule.forRoot(), /* ... */]
})
export class AppModule { }
```

**ملاحظة:** كلتا المكتبتين تَستخدمان Bootstrap CSS لكن توفران _وظائف_ مضمَّنة بنمط Angular (event bindings, inputs, outputs)، لذلك تمنع مشاكل التوافق مع DOM.

---

# 5. إنشاء Interface للبيانات (Istore)

نحن نريد وصف شكل البيانات (type) قبل الاستخدام. أنشئ مجلداً `src/app/models` ثم:

```bash
ng g interface models/istore
```

ينتج ملف `src/app/models/istore.ts` مثال:

```ts
export interface Istore {
  name: string;
  imgUrl: string;
  branches: string[];
}
```

**شرح:**

- `Istore` يصف شكل بيانات المتجر: `name` اسم المتجر، `imgUrl` رابط الصورة، `branches` مصفوفة عناوين الفروع.
    
- استخدام interface يساعد TypeScript ويدل المحرر (Editor) على الأخطاء في وقت التطوير.
    

---

# 6. HomeComponent Example مع Interface

**إنشاء الكومبوننت:**

```bash
ng g c components/home
```

**home.component.ts** (مكتوب مع شرح):

```ts
import { Component } from '@angular/core';
import { Istore } from 'src/app/models/istore';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {
  // متغير من النوع Istore
  myStore: Istore;

  constructor() {
    // نعطي قيمة ابتدائية (هيئة البيانات التي سنعرضها)
    this.myStore = {
      name: 'HNM Store',
      imgUrl: 'https://via.placeholder.com/300x150.png?text=Store+Image',
      branches: ['Cairo', 'Asyut', 'Sohag']
    };
  }
}
```

**شرح:**

- `myStore` متغير يحمل بيانات النوع `Istore`.
    
- نعطيه قيمة في `constructor` لضمان أنه معرف قبل العرض.
    

---

# 7. عرض البيانات باستخدام Interpolation و Property Binding

**home.component.html** مثال:

```html
<div class="card" style="width: 18rem;">
  <!-- Property binding لعرض الصورة -->
  <img [src]="myStore.imgUrl" class="card-img-top" alt="{{ myStore.name }}">
  <div class="card-body">
    <!-- Interpolation لعرض الاسم -->
    <h5 class="card-title">{{ myStore.name }}</h5>
    <!-- ngFor لعرض الفروع -->
    <ul class="list-group list-group-flush">
      <li *ngFor="let branch of myStore.branches">{{ branch }}</li>
    </ul>
  </div>
</div>
```

**شرح:**

- `{{ myStore.name }}` = interpolation — يستخدم لعرض قيم نصية داخل عناصر HTML.
    
- `[src]="myStore.imgUrl"` = property binding — يربط خاصية عنصر DOM (src) بقيمة من TS (مهم للـ attributes).
    
- `*ngFor="let branch of myStore.branches"` = تكرار (loop) عبر مصفوفة `branches`.
    

---

# 8. الفرق بين External HTML/CSS و Inline Template/Styles في Angular

**External (مفصول):** في ملف `component.ts` ستجد `templateUrl` و `styleUrls`.

- مناسب عندما يكون الـ HTML/CSS طويل.
    
- أسهل للتنظيم.
    

**Inline (مضمّن داخل TypeScript):** استخدم `template` و `styles` بدلاً من `templateUrl` و `styleUrls`:

```ts
@Component({
  selector: 'app-inline',
  template: `<h1>From TS template</h1>`,
  styles: [`h1 { color: green; }`]
})
export class InlineComponent {}
```

**متى تستخدم كل منهما؟**

- Inline مناسب لقوالب صغيرة أو أمثلة سريعة.
    
- External مفضل في المشاريع الحقيقية للحفاظ على قابلية الصيانة.
    

---

# 9. بنية المشروع (Navbar – Header – Main – Footer – Home)

هيكلية نموذجية داخل `src/app`:

```
app/
  app.component.ts
  components/
    navbar/
      navbar.component.ts
      navbar.component.html
    header/ (اختياري)
    home/
      home.component.ts
      home.component.html
    footer/
      footer.component.ts
      footer.component.html
  models/
    istore.ts
```

---

# 10. إنشاء Components باستخدام Angular CLI

أوامر مفيدة:

```bash
# إنشاء navbar
ng g c components/navbar

# إنشاء footer
ng g c components/footer

# إنشاء home
ng g c components/home
```

كل أمر ينشئ مجلد للكومبوننت بداخله الملفات: `.ts`, `.html`, `.css`, `.spec.ts`.

---

# 11. كيفية عرض Components داخل الـ App Component

افتح `src/app/app.component.html` وضع سلكتور الكومبوننتات:

```html
<app-navbar></app-navbar>
<main class="container mt-4">
  <app-home></app-home>
</main>
<app-footer></app-footer>
```

**شرح:** كل كومبوننت له `selector` (مذكور في ملف `.ts`) يمثل الوسم الذي تدرجه في HTML.

---

# 12. Selectors لكل Component (Navbar, Home, Footer)

في كل ملف `*.component.ts` ستجد `selector`. أمثلة افتراضية:

```ts
// navbar.component.ts
@Component({ selector: 'app-navbar', templateUrl: './navbar.component.html' })

// home.component.ts
@Component({ selector: 'app-home', templateUrl: './home.component.html' })

// footer.component.ts
@Component({ selector: 'app-footer', templateUrl: './footer.component.html' })
```

---

# 13. إضافة Navbar باستخدام Bootstrap

**مثال navbar بسيط في `navbar.component.html`:**

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">MyStore</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link active" aria-current="page" href="#">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Shop</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Contact</a></li>
      </ul>
    </div>
  </div>
</nav>
```

**ملاحظة:** إذا لم يعمل `navbar-toggler` (الـ collapse) فذلك يعود لـ JS (انظر بند 3 و 4).

---

# 14. إضافة Footer باستخدام Bootstrap

**مثال footer في `footer.component.html`:**

```html
<footer class="bg-light text-center text-lg-start mt-4">
  <div class="text-center p-3">
    © 2025 MyStore - All rights reserved
  </div>
</footer>
```

---

# 15. تصميم Home Component باستخدام Grid System (12 Columns)

**مثال بسيط في `app.component.html` أو `home.component.html`:**

```html
<div class="container">
  <div class="row">
    <!-- Main content: يأخذ 8 أعمدة على الشاشات الكبيرة -->
    <div class="col-12 col-md-8">
      <app-home></app-home>
    </div>

    <!-- Sidebar: يأخذ 4 أعمدة -->
    <div class="col-12 col-md-4">
      <div class="card">
        <div class="card-body">Sidebar content</div>
      </div>
    </div>
  </div>
</div>
```

**شرح:** نظام Grid في Bootstrap مبني على صفوف (`row`) وعمود (`col-*`) داخل `container`. المجموع في كل صف يجب أن يكون 12 عمود.

---

# 16. مشكلة التحديث في HTML وعدم ظهور التغييرات (Watch Issue)

**المشكلة:** أحياناً التغييرات في `index.html` أو `angular.json` لا تُطبق تلقائياً على `ng serve` (watch لا يلتقطها).

**أسباب وحلول:**

- تغييرات في `angular.json` تتطلب إعادة تشغيل `ng serve`. الحل: أوقف السيرفر (`Ctrl+C`) ثم `ng serve` مرة أخرى.
    
- تغييرات في `index.html` أو ملفات تم تحميلها مرة على الخادم قد تتطلب إعادة تحميل الصفحة أو مسح الكاش في المتصفح.
    
- إذا كان التغيير في TS/HTML/CSS للكومبوننتات العادية ولم يحدث تحديث: تأكد من حفظ الملف، وتحقق من وجود أخطاء في الـ console في الطرفية أو المتصفح.
    

نصيحة: إن حدث تعليق (hanging) في الـ watcher فعلّ فتح الطرفية مجدداً أو حذف مجلد `node_modules` وإعادة `npm install` (مجرّد حل أخير).

---

# 17. الفرق بين Bootstrap CDN و Install Bootstrap

- **CDN**:
    
    - سريع وسهل: لا تحتاج لتثبيت.
        
    - يعتمد على خوادم خارجية؛ قد يتوقف أو يتغير الإصدار.
        
    - ملفات لا تكون داخل مشروعك.
        
- **Install (npm)**:
    
    - ملفات موجودة داخل `node_modules` لديك (مستقرة للإصدار الذي ثبتته).
        
    - تحتاج لإضافتها إلى `angular.json` أو استيرادها في styles.
        
    - يتيح لك تعديل/override الستايلات بسهولة.
        

**الاختيار:** للمشاريع الحقيقية يُفضّل تثبيت عبر npm للحفاظ على الاستقرار والتحكم بالإصدار.

---

# 18. خطوات تثبيت Bootstrap باستخدام npm

```bash
npm install bootstrap
# اذا احتجت popper عادة bundle مضمن
```

ثم تأكد من إضافة المسارات في `angular.json` (السطر التالي).

---

# 19. إضافة Bootstrap CSS وJS داخل angular.json (Styles & Scripts Arrays)

افتح `angular.json` وابحث عن `build > options > styles` و `scripts` ثم أضف المسارات:

```json
"styles": [
  "node_modules/bootstrap/dist/css/bootstrap.min.css",
  "src/styles.css"
],
"scripts": [
  "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
]
```

**شرح:**

- `styles` تُحمّل قبل الستايلات المحلية (`src/styles.css`) حتى تتمكن من الكتابة فوق قواعد Bootstrap إن احتجت.
    
- `scripts` يحمّل JS اللازم (bundle يحتوي على Popper)، لكن تذكّر: التلاعب المباشر بالـ DOM عبر JS قد يسبب مشاكل لذا إن أردت وظائف متقدمة استخدم `ng-bootstrap` أو `ngx-bootstrap`.
    
- بعد تعديل `angular.json` **يجب إعادة تشغيل** `ng serve`.
    

---

## ملحق: أمثلة إضافية وشرح مبسط لكل كود

### إنشاء Interface:

```ts
// src/app/models/istore.ts
export interface Istore {
  name: string;
  imgUrl: string;
  branches: string[];
}
```

**لماذا؟**: لضمان أن أي كائن يمثل متجرًا له الخصائص المتوقعة.

### HomeComponent (TS كامل مع تعليقات):

```ts
import { Component } from '@angular/core';
import { Istore } from 'src/app/models/istore';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {
  myStore: Istore;

  constructor() {
    // اختبر البيانات محليًا (ستبدل فيما بعد بمكالمة API مثلاً)
    this.myStore = {
      name: 'HNM Store',
      imgUrl: 'https://via.placeholder.com/300x150.png?text=Store+Image',
      branches: ['Cairo', 'Asyut', 'Sohag']
    };
  }
}
```

### HomeComponent HTML (عرض البيانات):

```html
<div class="card mb-3">
  <img [src]="myStore.imgUrl" class="card-img-top" alt="{{ myStore.name }}">
  <div class="card-body">
    <h5 class="card-title">{{ myStore.name }}</h5>
    <ul class="list-group list-group-flush">
      <li *ngFor="let branch of myStore.branches" class="list-group-item">{{ branch }}</li>
    </ul>
  </div>
</div>
```

### شرح لكل سطر مهم:

- `[src]="myStore.imgUrl"`: property binding. مفيد للسمات التي تحتاج إلى قيمة ديناميكية (مثل src).
    
- `alt="{{ myStore.name }}"`: interpolation داخل attribute عادي — يعمل لكن حينما تكون سمة DOM يفضّل استخدام property binding.
    
- `*ngFor="let branch of myStore.branches"`: يكرر التاغ لكل عنصر في المصفوفة.
    

---

## نصائح عملية وخاتمة

- استخدم `ng-bootstrap`/`ngx-bootstrap` عندما تحتاج لfunctional components من Bootstrap داخل Angular.
    
- ضع Bootstrap CSS في `angular.json` ومن ثم ملفات JS إذا لزم.
    
- بعد أي تغيير في `angular.json` أعد تشغيل `ng serve`.
    
- أفضل ممارسة: استخدم `Interface` أو `Class` لوصف الداتا القادمة من backend (Type safety).
    
- نظّم كومبوننتاتك داخل مجلد `components/` و models داخل `models/` — هذا يسهل صيانة المشروع.
    

---
 



