xml -> طريقه التوصيل بين السرفر والكلاينت 
sql -> بديل للفايل بيز اللي كانت زمان

Database Life Cycle | مراحل الداتا بيس



1. Analysis -> System Analyst
src | system requirement sonification in c# | required document | بتفهم من العميل هو عايز اي زي العقد كدا اي اللي انت عايزه واي اللي هيطلعه ودا شغلانته ال analysis

2. Database Design -> Database Designer
Entity Relationship Diagram 
بترسملي الدااتا بيس علي ورق او برنامج بالجداول 
3. Database Mapping -> Database Designer
Database Schema
بيحول ال داتا بيس مابينج لسكيما
4. Database Implementation -> Database Developer
Using DBMS (SQL Server) To Create The Database
مرحله الكود
5. Application -> Application Programmer
Web - Desktop - Mobile
مرحله تخزين الداتا
6. Client -> End Usireet.google.com is sharing your screen.
مرحله ال end user 

Use Database Indirectly Through The Application


---
Database Users and Roles

Database Administrator (DBA)| اللي بيتعحكمل في الداتا وبيدي الصلاحيات 
System Analysts | 
Database Designer
Database Developer
Application programmers
BI & Big Data Specialist (Data Scientist)
End users

---

The ER Model
Basic Components of the E-R model:
1. Entities: person, place, object, event, concept (often corresponds to a real time object that is distinguishable from any other object)| اي بيانات بخدمها من الداتا انليزسيت
2. Attributes: property or characteristic of an entity type (often corresponds to a field in a table)| وصف للintity
3. Relationships: link between entities (corresponds to primary key - foreign key equivalencies in related tables)
4. ![[Pasted image 20250414215116.png]]
Rectangles: Entity sets| اي حاجه هجمع عنها معلومات
Diamonds: Relationship sets | علاقه
Ellipses: Attributes | المعلومات اللي جمعتها الي بتوصف الانتيتي

Strong Entity Vs Weak Entity
Strong Entity:

An Entity set that has a primary key.
It doesn't depend on any other entity.
Weak Entity:
A Doesn't have any primary key because it doesn't have sufficient attributes to form a primary key but it has partial key.
It depends on another strong entity

Types of Attributes
1. Simple Attribute
2. Composite Attribute
3. Multi-valued Attribute
4. Derived Attribute
5. Complex attribute