SSMS -> 
الطريقه الاولي لانشاء داتا بيز كليك يمين و new database

الطريقه التانيه باستخدام ال query
```mySQL
used master;
create database salse;
```
هيعمل داتا بيز باستخدام الdeafult medule الموجود في medule

لو عايز تخصص كل حاجه 
```sql 
use master;

create DATABASE sales2

ON

(

    NAME = saledata,

    FILENAME = "C:/Program Files/Microsoft SQL Server/MSSQL16.MSSQLSERVER/MSSQL/DATA/saledata.mdf",

    SIZE =10,

    MAXSIZE = 50,

    FILEGROWTH =5

)

LOG ON

(    NAME = salelog,

FILENAME =  "C:/Program Files/Microsoft SQL Server/MSSQL16.MSSQLSERVER/MSSQL/DATA/salelog.ldf",

SIZE =5MB,

MAXSIZE = 25MB,

FILEGROWTH = 5MB

  

)

GO
```
