الانجلولار بيتكون من كومبونت 
- كل كمبونت بتحتوي علي 3 فايلات 
- ( home.conponent.html
- home.component.scss
- home.component.ts)
https://angular.dev/installation
npm install -g @angular/cli    => او ممكن تحدد الاصدار اللي انت عايزه من npm
npm i @angular/cli@17.3.12 

---
iti => modules :
#### ng new lec1 --no-standalone

---
ng new <project-name>
عشان تبدا مشروع جديد 
ng new <project-name> --help 
عشان تعرف كل الاوامر اللي ممكن تستخدمها،

بعد التثبيت هتلاقي ملفات كتيره منها الاعدادات لو بتشتغل مع فريق وهكذا

- عشان تبدا تشغل المشروع بتاعك علي السرف 
- ng serve --open
- ng serve --open --hmr  => استبدال الجزء المحدد فقط في عمليه التطوير بدل عمل ريلود للمشروع كله
- 