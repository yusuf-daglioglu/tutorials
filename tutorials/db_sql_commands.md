# DB SQL COMMANDS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 DISTINCT

City ve Country sütunlarının her farklı kombinasyonunu döndürür. Duplicate olan sütunları dönmez. Örnek: Türkiye-İstanbul, Türkiye-Ankara'dan 1'er adet döner. Oysa customer tablomuzda Türkiye-Ankara'dan 1'den fazla customer olabilir.

Yani DISTINCT; select yapılan tüm sütunlara bakar. Başka bir değiş ile; DISTINCT ile sorgu çalıştırırken, sadece DISTINCT keyword'ünün ilk sağındaki sütuna bakarak hareket etmez.

```sql
SELECT DISTINCT City, Country FROM Customers;
```

## 📌 ORDER

```sql
SELECT * FROM Customers
ORDER BY Country DESC, CustomerName ASC;

-- DESC -> descending
-- ASC -> ascending
```

## 📌 top vs limit vs rownum

Some keywords supported only by some servers:

- Top --> `MSSQL`
- LIMIT --> `MySQL`
- rownum -> `Oracle DB`
- FETCH FIRST 100 ROWS ONLY; -> `Oracle DB`

Examples:

```sql
SELECT TOP 3 * FROM Customers
WHERE Country='Germany';
```

```sql
SELECT * FROM Customers
WHERE Country='Germany'
LIMIT 3;
```

```sql
SELECT * FROM Customers
WHERE Country='Germany' AND ROWNUM <= 3;
```

## 📌 IN

```sql
SELECT column_name(s)
 FROM table_name
 WHERE column_name IN (value1,value2,...);
```

## 📌 Between

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

## 📌 AS

şunun için kullanılır:

- result'taki sutun ismi değiştirme
- table'ların SQL'deki alias'ı

```sql
SELECT CustomerName, Address+', '+City+', '+PostalCode+', '+Country AS Address
 FROM Customers;
```

yukarıdakini fonksiyon ile yapma:

```sql
SELECT CustomerName, CONCAT(Address,', ',City,', ',PostalCode,', ',Country)  AS Address
 FROM Customers;
```

table alias'ı:

```sql
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName="Around the Horn" AND  c.CustomerID=o.CustomerID;
```

## 📌 table merges

- __JOIN__

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM  Orders
INNER JOIN Customers
ON Orders.CustomerID=Customers.CustomerID;
```

"ON" condition'ındaki eşitlik koşulunu sağlamayan satırlar olabilir. İşte bu durumlar için:

- INNER JOIN: sadece condition'ı sağlayan satırlar döner.
- LEFT JOIN: sol tarafta (orders tablosu) olan tüm satırlar kesin döner.
- RIGHT JOIN: sağ tarafta (customers) olan tüm satırlar kesin döner.
- FULL JOIN: her iki tablodaki satırlar mutlaka döner.

Notes:

- In some DB also "OUTER" keyword is using. example: "LEFT OUTER JOIN", "FULL OUTER JOIN".
- Her zaman (daha kolay olur düşüncesiyle) LEFT JOIN yerine FULL JOIN kullanmamalıyız. Burada performans farkı karşımıza çıkabilir. Tabi performanstan ziyade şöyle de bir durum var: Left join, yerine full join kullanırsak, sol taraftaki data'larda fazlalık var ise o zaman tüm SQL sorgumuzda "is not null" gibi ek koşullar eklememiz şart olacaktır. Dolayısı ile bir ekstra condition çalıştırmış olacağız.

- __UNION__

```sql
SELECT CityName AS City FROM Customers
UNION
SELECT City FROM Suppliers
```

"UNION ALL" kullanılırsa; duplicate value'larda gelecektir.

## 📌 group by, having by, where

- __Hazır fonksiyonlar__

bazı hazır fonksiyonlar "__aggregation__" yaparlar, bu sebeple __aggregate function__ olarak da adlandırılırlar.

fakat her fonksiyon aggregate yapmayabilir. örnek: "Date" fonksiyonları.

Örnekte count (aggregate) fonksiyonunun kullanımı mevcuttur:

```sql
SELECT COUNT(*) AS NumberOfOrders FROM Orders;
```

`MSSQL`'da kullanılan "TOP" özel bir syntax'tır. Bir fonksiyon değildir. Kaynaklar:

- 1- Burada görüldüğü gibi listede TOP yoktur: <https://docs.microsoft.com/en-us/sql/t-sql/functions/aggregate-functions-transact-sql?view=sql-server-ver16>
- 2- <https://docs.microsoft.com/en-us/sql/t-sql/queries/top-transact-sql?view=sql-server-ver16#interoperability> "interoperability" başlığındaki ilk cümlede expression olduğu yazmaktadır.

- __Group By__ and __having__

```sql
-- AVG ve COUNT array alır, 1 value döner.
-- product_tag zaten 1 value'dur.
-- AVG ve COUNT'a giden array, 1 adet product_tag'e denk gelen bütün price ve product_id'lerdir.
-- "price" değerini fonksiyonsuz (tek başına) kullanamazdık. çünkü o bir array.

SELECT AVG(price), COUNT(product_id), product_tag
FROM products
WHERE price > 9 --gruplamadan önce işletiliyor.
GROUP BY product_tag;
HAVING product_tag = "meyve" and COUNT(product_id) > 7 --gruplamadan sonra işletiliyor.
ORDER BY product_tag; -- en sonda işletilir.

-- HAVING'in icindeki condition'da SELECT'te yazacağımız alias'ı kullanamıyoruz. Çünkü henüz result oluşturulmadı, yani "select" kısmı işletilmemiş oluyor.
```

## 📌 Scheme changing

- create table

```sql
CREATE TABLE Persons
(
PersonID int,
LastName varchar(255) NOT NULL,
FirstName varchar(255) UNIQUE,
Address varchar(255),
City varchar(255)
);
```

- tablo şeması değiştirme

```sql
ALTER TABLE Persons
ADD DateOfBirth date
```

- View yaratma

```sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

## 📌 data manipulations

- Truncate VS delete

|                             | truncate   | delete          |
|-----------------------------|------------|-----------------|
| rollback inside transaction | impossible | can be rollback |
| locks                       | table      | rows            |
| command type                | DDL        | DML             |
| where clause                | not exist  | optional        |
| triggers                    | not fired  | fired           |

Farklar yukarıdakinden daha detaylı ve bazı DB'lerde istisna durumlar olabiliyor. Bu sebeple hangi DB'ye olduğumuza göre çok ince ayrıntılar karşımıza çıkabilir.

- tabloya ekleme ya sütun isimleri belirtilerek, yada belirtilmezse kolon sırasına göre yapılır.

```sql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```

yada

```sql
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

birden fazla satır eklemek gerekirse:

```sql
INSERT INTO table_name
VALUES (value1,value2),
       (value3,value4),;
```

insert'tin eklediği yeni satırın oto-generate edilen primary key'ini görebilmek için birden fazla yöntem var:

<https://stackoverflow.com/questions/42648/how-to-get-the-identity-of-an-inserted-row>

<https://dba.stackexchange.com/questions/124847/best-way-to-get-last-identity-inserted-in-a-table>

- update

```sql
UPDATE Customers
SET ContactName='Alfred Schmidt', City='Hamburg'
WHERE CustomerName='Alfreds Futterkiste';
```

- delete

```sql
DELETE FROM Customers
 WHERE CustomerName='Alfreds Futterkiste' AND ContactName='Maria Anders';
```

## 📌 Others

- GO

GO, bir SQL standart keyword'ü değildir. Sadece client SQL editor'ler tarafından kullanılır. Hatta editor'ler tarafından değiştirilebilirler (yani; editörler, go yerine farklı (istediğimiz) bir keyword'ü kullanmamıza olanak sağlarlar). Go keyword'ü sunucuda çalıştırılamaz (sunucuya yollanmaz).

"go" bir group işlemin (örnek batch) sonuna konulur. bu şekilde onun aşağısında olan başka SQL kodları var ise onlar başka bir "batch" işlemi olarak uzak sunucuda işletilir. Örneğin; 2 adet bağımsız script'imiz olsun. ikisinde de local variable'larımız olsun. eğer 2 sinin arasına GO keyword'ünü koymazsak ve hepsini tek bir execution işlemi olarak uzak DB server'ına yollarsa, o zaman local variable'lar 2 script'te de ortak variable olarak algılanacak ve hatalara sebep olacaktır.

Go komutunun yanında yorum satırı hariç hiçbir şey olmamalıdır (noktalı virgülde olmamalıdır). Tek istisna şudur: Go yanında bir integer alabilir. Bu sayı GOnun üstündeki batch'in tümüyle kaç kere üst üste işletileceğini belirler.

- EXEC

İlgili string'i execute eder. JS'teki "eval" fonksiyonu gibi çalışır.

```sql
EXEC ('USE my_db; SELECT * FROM person;');
```

- BEGIN ... END

arasına statement veya statement'ler bloğu alır. BEGIN - END; IF veya WHILE gibi terimlerden sonra kullanılmak için gerekli keyword'lerdir.

- comments

```sql
/*
 block comment
*/
SELECT * FROM PERSON
-- line comment
```

- Backslash (Line Continuation)

example of splitting a character string:

```sql
SELECT 'person_\
ID' AS person_number
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 merge

```sql
MERGE INTO tablename USING table_reference ON (condition)
  WHEN MATCHED THEN
    UPDATE SET column1 = value1 [, column2 = value2 ...]
  WHEN NOT MATCHED THEN
    INSERT (column1 [, column2 ...]) VALUES (value1 [, value2 ...]);
```

Yukarıdaki ifade şuna denk gelmektedir:

```sql
-- UPSERT => update ve insert'in kısaltmasından gelmektedir.
UPSERT INTO Users VALUES (10, "John", "Smith", 27, 60000);
```

Bazı DB sistemlerinde ise buna denk gelmektedir:

```sql
INSERT OR REPLACE INTO
```

Bazı keyword'ler SQL standartlarında yok. Yukarıdakilere denk gelen diğer keyword'ler: https://en.wikipedia.org/wiki/Merge_(SQL) title: "Other non-standard implementations".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Cursor

Cursor ile DB'deki bir alanı işaretliyoruz. Bu belirlediğimiz alanı dolaşmak için `for loop` kullanıp fetch ile gezmemiz gerekiyor. örnek kod:

```sql

declare @DocEntry int -- burada integer tipinde, @DocEntry isminde, bir değişken tanımlıyoruz.

declare CRS_basla cursor for -- cursor'umuzun ismi: CRS_basla
  select DocEntry from OITM -- cursor'umuz OITM tablosundaki DocEntry sütununa işaret ediyor.

open  CRS_basla -- cursor'u her zaman kullanmadan önce açmak zorundayız.

fetch next from CRS_basla into @DocEntry -- ilk satırımızı çekiyoruz.
  
while @@FETCH_STATUS=0
begin
  if (@DocEntry%2=0)
    select @DocEntry
  fetch next from CRS_basla into @DocEntry -- her "for loop" ta bir kere çekme işlemini tekrarlıyoruz.
end

close CRS_basla -- cursor'umuzu kapatmak zorundayız.
DEALLOCATE CRS_basla
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 pagination

pagination eski DB sürümlerinde mevcut değildir ve her server'da farklı keyword'lerle support'u vardır.

- `MSSQL` 2012 ile `OFFSET` and `FETCH` Özelliğinin kullanımı:

ilk n sütun hariç sonraki tüm sütunları döndürür:

```sql
SELECT sutun-adlari
FROM tabloadi
ORDER BY sütun-adlari
OFFSET n ROWS
```

(n + 1) ile (n + 1 + m) arasında olan değerleri döndürür:

```sql
SELECT sutun-adlari
FROM tabloadi
ORDER BY sutun-adlari 
OFFSET n ROWS
FETCH NEXT m ROWS ONLY
```

- ROW_NUMBER destekleyen DB'lerde aşağıdaki gibi bir kulanım ile pagination yapılabilir:

```sql
WITH OrderedOrders AS
(
    SELECT
        ROW_NUMBER() OVER(ORDER BY FirstName DESC) AS RowNumber, 
        FirstName, LastName, ROUND(SalesYTD,2,1) AS "Sales YTD"
    FROM [dbo].[vSalesPerson]
) 
SELECT RowNumber, 
    FirstName, LastName, Sales YTD 
FROM OrderedOrders 
WHERE RowNumber > 50 AND RowNumber < 60;
```

- PostgreSQL ile:

```sql
SELECT * FROM table_name 
LIMIT 10 
OFFSET 10;
```

`PostgreSQL`'deki `LIMIT` keyword'ü, `MSSQL`'deki `TOP` keyword'üne tekabül ediyor. `MSSQL`'de `OFFSET` ile `TOP` keyword'ü aynı `SQL` sorgusunda kullanılamazken, `PostgreSQL`'de yukarıdaki örnekte olduğu gibi kullanılabiliyor. (`MSSQL`'in 2012 versiyonundan sonra artık aynı sorguda kullanılabiliyorlar)

Hem `PostgreSQL` hem de `MSSQL`'deki `OFFSET` çözümünün dezavantajı bir sayfalar arası gezerken, arada data silinmesi veya eklenmesi durumunda tutarsızlıklar yaşayacak olmamızdır.

- JPA ile birden fazla yöntem ile pagination yapılabilir. Fakat hangi çözümü seçersek seçelim, JPA arka planda, DB'ninimplementasyonuna (driver'ına) göre SQL oluşturacaktır.

- SQL'deki "Cursor" kullanımı ile de pagination yapılabilir. Fakat bunun dezavantajı, cursor sorgusunda 50'inci sayfaya gitmek istediğimizde ilk 50 sayfayı gezmek zorunda kalacak olmamızdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
