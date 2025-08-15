ERD => Relational Mapping(Schema)
Step 0: Mapping 1-1 relationship mandatory from 2 sides
Step 1: Mapping of Regular (Strong) Entity Types
Step 2: Mapping of Weak Entity Types
Step 3: Mapping of Binary 1:1 Relation Types
Step 4: Mapping of Binary 1:N Relationship Types.
Step 5: Mapping of Binary M:N Relationship Types.
Step 6: Mapping of N-ary Relationship Types.
Step 7: Mapping of Unary Relationship.

---

Step O: Mapping 1-1 relationship (Both sides are Mandatory

Step 3: Mapping 1-1 relationship
(Mandatory - Optional)
Name
CID
Model
Employee
1
1
Has
EID
Car
A Mandatory - Optional 1-1 Relationship Between Employee and Car

---

### Step 1: Mapping of Regular Entity Types


Mapping Composite Attribute:

---

Mapping Multivalued Attribute:
PRIMARY KEY ، FORNKEY 

---
المفتاح الرئيس هيتحط ك مفتاح اجنبي في الجدول التاني عشان اعمل جدول تاني ل employee_skills
بنعبر عنه ب نقاط تحت الfornkey 
composed key 
![[Z_IMAGES/Pasted image 20250421111149.png]]

---
In the most cases Derived attribute not be stored in DB

Step 2: Mapping of Weak Entity Types
Create table for each weak entity.
Add foreign key that correspond to the owner entity type.
➤ Primary key composed of:
o Partial identifier of weak entity
Primary key of identifying relation (strong entity)
![[Pasted image 20250421140956.png]]

Step 3: Mapping of Binary 1:1 Relation Types
We Have 3 Cases :
1. Both sides are Mandatory.
2. ![[Pasted image 20250421141101.png]]
3. Mandatory - Optional.
4. ![[Pasted image 20250421141219.png]]
5. Both sides are Optional.

![[Pasted image 20250421141257.png]]

---



----
Step 3: Mapping 1-1 relationship
(Both sides are optional)
Name
CID
Model
Employee
1
1
Has
EID
Car
An Optional 1-1 Relationship Between Employee and Car

ملاحظه مهممه :
بنعبر عن القيمه الالزاميه بخطين في رسم السكيما
وبنعبر عن الاوبشنال بخط واحد في الرسم

نكمل 
في الحاله التاليه ف انا بعمل جدول للموظف جدول للكار وعمل جدول تالت يضم الاتنين والاتنين فيهم fornkey وخلي البرامير كاي هو رقم الموظف
يما يبق عندك اوبشنال في النحيتين علطول نعمل جدول تالت

---
Step 4: Mapping of Binary 1:M Relationship Types
الموظفين والاقسام

We Have 2 Cases :
1. M-Side Mandatory
2. ![[Pasted image 20250421141513.png]]
3. M-Side Optional
![[Pasted image 20250421141554.png]]
. M-Side Mandatory
الموظف والاقسام

لما يبق عندك جدولين واحد ميني وواحد وان ركز علي الميني وخلي الفورن كاي يتاخد من الوان
مينفعش اني اكرر الداتا بمعني ممكن موظف يكون شغال في قسم وموظف تاني شغال في نفس القسم ف لو عملت جدول بالاقسام وخليه الرئيس كدا يبق في تكرار ف الاصح ان الجدول الابشن اخد منه فورن كي للجدول الاساسي


---
 M-Side Optional
الموظف والشماريع
جدول تالت البريمير اي دي هناخده من الميني







---
Step 5: Mapping of Binary M:N Relationship Types
SID
SName
CID
CName
M
M
Student
Takes
Course
M-M Relationship Between Student And Course
![[Pasted image 20250421141622.png]]

---
Step 6: Mapping of N-ary Relationship Types
Vendor
Vid
VName
Warehouse
Whld
WhName
Product
Code
PName
t Vendor Warehouse
ستنتهي مكالمتك بعد 8 دقائق.
يبلغ الحد الزمني المسموح به للمكالمات الجماعية المجانية ساعة واحدة.
Vid
PCode
Whld
Unit_Cost
Ship_Mode
Ship_Time
![[Pasted image 20250421141650.png]]

---
Step 7: Mapping Unary Relationship
علاقه مع نفسها يعني مثلا الموظف بيدير موظف تاني 

Id
Name
Employee
1
M
Manages
Unary Relationship Between 2 Instances From Employee Entite
aployees
Id
Name

ID NAME MANAGE_ID
   
![[Pasted image 20250421141711.png]]

---
Relational Mapping (Schema) Summary
1-1=> One Table, Choose PK of any entity to this table.
1-1=> 2 Tables, Take PK Of Optional As FK In Mandatory Table.
1-1=> 3 Tables, Choose PK of any entity to this table.
1-M => 2 Tables, Take PK Of One As FK in Many
1-M => 3 Tables, PK of Third Table is PK of Many
M-M => 3 Tables, PK of Third Table is Composite PK (PK1 + PK2)
Ternary => Table for Each Entity + Table for Relation Contains PKs of table as FK (PK is Composite PK).