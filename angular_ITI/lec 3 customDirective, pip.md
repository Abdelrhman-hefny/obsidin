custom directive
host Listener
custom pipe 
components interaction (parent - child)

---

## ✅ Angular Syntax جديد: `@for` و `@empty`

```html
@for(card of Products; track card.id; let i = $index) {
  <!-- محتوى الكرت -->
} @empty {
  <h2 class="text-center fs-2">No product Found</h2>
}
```

- `@for`: بديل جديد لـ `*ngFor`.
    
- `@empty`: بيشتغل زي `else` لو الآراي فاضية.
    

---

## ✅ @if و @else

```html
@if (condition) {
  <p>True</p>
} @else {
  <p>False</p>
}
```

بديل لـ `*ngIf` القديم.

---

## ✅ @switch / @case (بديل لـ `ngSwitch`)

```html
@switch(type) {
  @case('admin') { <p>Admin</p> }
  @case('user') { <p>User</p> }
  @default { <p>Unknown</p> }
}
```

---

## 🟩 Custom Directive

### ✅ إنشاء Directive:

```bash
ng g d directives/green
```

### ✅ مثال بسيط:

```ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appGreen]',
  standalone: true
})
export class GreenDirective {
  constructor(ele: ElementRef) {
    ele.nativeElement.style.color = 'red';
  }
}
```

```html
<!-- الاستخدام -->
<div class="card-title" appGreen>{{ card.name }}</div>
```

✅ تأكد من استيرادها:

```ts
imports: [GreenDirective]
```

---

## 🎯 تطوير الديركتيف باستخدام `@HostListener`

```ts
@Directive({
  selector: '[appGreen]',
  standalone: true
})
export class GreenDirective {
  constructor(private ele: ElementRef) {
    ele.nativeElement.style.color = 'red';
  }

  @HostListener('mouseover')
  over() {
    this.ele.nativeElement.style.color = 'pink';
  }

  @HostListener('mouseout')
  out() {
    this.ele.nativeElement.style.color = 'red';
  }
}
```

### ✅ شرح `@HostListener`:

بيسمحلك تتابع أحداث (events) زي `mouseover`, `click`, `keydown` على نفس العنصر اللي عليه الديركتيف.

---

## ✏️ تخصيص اللون باستخدام `@Input`

### ✅ استخدام قيمة خارجية:

```html
<div class="card-title" appGreen externtalColor="green">Title</div>
```

```ts
@Input() externtalColor: string = 'black';

@HostListener('mouseover') over() {
  this.ele.nativeElement.style.color = this.externtalColor;
}
```

---

### ✅ تمرير قيمة مباشرة داخل الـ Directive:

```html
<div class="card-title" appGreen="blue">Title</div>
```

```ts
@Input('appGreen') DefultColor: string = 'black';

@HostListener('mouseout') out() {
  this.ele.nativeElement.style.color = this.DefultColor;
}
```

---

## 🧪 Pipes في Angular

### ✅ تعريف بسيط:

Pipe = أنبوبة بتعدي فيها بيانات، وانت بتغير شكل البيانات قبل عرضها في الواجهة.

### ✅ أمثلة على Built-in Pipes:

```html
<p>{{ price | currency }}</p>
<p>{{ dateValue | date }}</p>
```

✅ تأكد من استيراد `CommonModule` أو `CurrencyPipe` و `DatePipe` لو بتشتغل بـ standalone components.

---

## 🔨 إنشاء Custom Pipe

```bash
ng g pipe pipes/squer 
```

### ✅ مثال:

```ts
@Pipe({
  name: 'squer',
  standalone: true
})
export class SquerPipe implements PipeTransform {
  transform(value: number, pow: number = 2): number {
    return Math.pow(value, pow);
  }
}
```

```html
<h2>{{ 5 | squer : 3 }}</h2> <!-- تطبع 125 -->
```

---

---

---

## ✅ Life Cycle Component

هي دورة حياة الـ Component من أول ما يتولد لحد ما يموت.

بتمر على 4 مراحل رئيسية:

1. **Creation**
    
2. **Change Detection**
    
3. **Rendering**
    
4. **On Destroy**
    

---

### 🛠️ Creation

#### 🔹 `constructor()`

- أول حاجة بتتنفذ.
    
- بنستخدمه علشان نحط **initial values** أو نحقن services.
    

🔸 مثال:

```ts
constructor() {
  this.title = 'Hello Angular';
}
```

---

### 🔄 Change Detection

#### 🔹 `ngOnInit()`

- بتشتغل بعد الـ constructor.
    
- مثالية لجلب البيانات من API.
    
- بتحصل **مرة واحدة بس**.
    

🔸 مثال:

```ts
ngOnInit() {
  this.getDataFromApi();
}
```

---

#### 🔹 `ngOnChanges()`

- بتشتغل كل مرة يحصل تغيير في الـ @Input.
    
- مثالية لو الكومبوننت بتستقبل بيانات من أب.
    

🔸 مثال:

```ts
ngOnChanges() {
  this.filterDataByCategory();
}
```

---

#### 🔹 `ngAfterViewInit()`

- بتحصل بعد ما يحصل render لكل عناصر الـ View.
    
- بنستخدمها لما نحب نتحكم في DOM Elements.
    

🔸 مثال:

```ts
ngAfterViewInit() {
  console.log(this.elementRef.nativeElement);
}
```

---

### 🧹 On Destroy

#### 🔹 `ngOnDestroy()`

- بتتفعل قبل ما الكومبوننت يتشال من الشاشة.
    
- بنستخدمها لمسح البيانات المؤقتة (زي cache أو unsubscribe من Observables).
    

🔸 مثال:

```ts
ngOnDestroy() {
  localStorage.removeItem('cart');
}
```

---

### 🔍 مثال بسيط يجمع كذا مرحلة:

```ts
@Input('appGreen') defult: string = 'blue';

constructor(private ele: ElementRef) { }

ngOnInit(): void {
  this.ele.nativeElement.style.color = this.defult;
}
```

---

## 💬 Components Interaction

### الأب والابن (Parent ↔️ Child)

#### ✅ لو الأب عايز يبعث حاجة للابن:

بنستخدم `@Input`

🔸 مثال:

```ts
@Input() receveCatId: number = 0;
```

```html
<app-products [receveCatId]="selectedCategory"></app-products>
```

---

#### ✅ لو الابن عايز يبعت حاجة للأب:

بنستخدم `@Output` + `EventEmitter`

### 📦 إزاي بنبعت event؟

#### 1. نعرف الـ Event داخل الكلاس:

```ts
@Output() onTotalPriceChaged = new EventEmitter<number>();
```

#### 2. نفاير الـ Event في موقف معين:

```ts
add() {
  if (this.btnNumber == 3) return;
  this.btnNumber++;
  this.onTotalPriceChaged.emit(this.total); // بيبعت total للأب
}
```

---

#### 3. في الأب نعمل **listener** للـ Event:

```html
<app-products
  [receveCatId]="selectedCategory"
  (onTotalPriceChaged)="CalcTotalPrice($event)">
</app-products>
```

💡 `$event` هنا هو القيمة اللي بعتها الابن عن طريق `emit()`.

---

### 📍 مثال كامل على Interactions:

#### ✅ Order Component (الأب):

```ts
selectedCategory = 0;
receveTotalPrice = 0;

CalcTotalPrice(top: number) {
  this.receveTotalPrice = top;
}
```

#### ✅ Products Component (الابن):

```ts
@Input() receveCatId: number = 0;
@Output() onTotalPriceChaged = new EventEmitter<number>();

add() {
  this.onTotalPriceChaged.emit(this.total);
}
```

#### ✅ في Template:

```html
<app-products
  [receveCatId]="selectedCategory"
  (onTotalPriceChaged)="CalcTotalPrice($event)">
</app-products>
```

---

