---
description: EXISTS (Transact-SQL)
title: EXISTS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXISTS_TSQL
- EXISTS
dev_langs:
- TSQL
helpviewer_keywords:
- existence testing [SQL Server]
- testing existence
- EXISTS keyword
- subqueries [SQL Server], EXISTS keyword
- queries [SQL Server], comparing
- comparing queries
- NOT EXISTS keyword
- row existence testing [SQL Server]
ms.assetid: b6510a65-ac38-4296-a3d5-640db0c27631
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e6da0d8fae0f521fde7e101005259037ace2d032
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100408"
---
# <a name="exists-transact-sql"></a>EXISTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Указывает вложенный запрос для проверки существования строк.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
EXISTS ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *subquery*  
 Ограниченная инструкция SELECT. Ключевое слово INTO не допускается. Дополнительные сведения о вложенных запросах см. в разделе [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-values"></a>Результирующие значения  
 Возвращает значение TRUE, если вложенный запрос содержит хотя бы одну строку.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-null-in-a-subquery-to-still-return-a-result-set"></a>A. Использование значения NULL во вложенном запросе для возвращения результирующего набора  
 В следующем примере возвращается результирующий набор со значением `NULL`, указанным во вложенном запросе, и устанавливается значение TRUE с помощью ключевого слова `EXISTS`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name   
FROM HumanResources.Department   
WHERE EXISTS (SELECT NULL)  
ORDER BY Name ASC ;  
```  
  
### <a name="b-comparing-queries-by-using-exists-and-in"></a>Б. Сравнение запросов с помощью ключевых слов EXISTS и IN  
 В следующем примере сравниваются два семантически эквивалентных запроса. В первом запросе используется ключевое слово `EXISTS`, а во втором — ключевое слово `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE EXISTS  
(SELECT *   
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 В следующем запросе используется ключевое слово `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE a.LastName IN  
(SELECT a.LastName  
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 Здесь приведен результирующий набор для каждого запроса.  
  
 ```
FirstName                                          LastName
-------------------------------------------------- ----------
Barry                                              Johnson
David                                              Johnson
Willis                                             Johnson
  
(3 row(s) affected)
 ```  
  
### <a name="c-comparing-queries-by-using-exists-and--any"></a>В. Сравнение запросов с помощью ключевых слов EXISTS и = ANY  
 В следующем примере показаны два запроса для поиска магазинов, названия которых совпадают с названием поставщика. В первом запросе используется ключевое слово `EXISTS`, а во втором — ключевое слово `=``ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE EXISTS  
(SELECT *  
    FROM Purchasing.Vendor AS v  
    WHERE s.Name = v.Name) ;  
GO  
```  
  
 В следующем запросе используется ключевое слово `= ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE s.Name = ANY  
(SELECT v.Name  
    FROM Purchasing.Vendor AS v ) ;  
GO  
```  
  
### <a name="d-comparing-queries-by-using-exists-and-in"></a>Г. Сравнение запросов с помощью ключевых слов EXISTS и IN  
 В следующем примере показаны запросы для поиска сотрудников подразделений, имена которых начинаются на `P`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE EXISTS  
(SELECT *  
    FROM HumanResources.Department AS d  
    JOIN HumanResources.EmployeeDepartmentHistory AS edh  
       ON d.DepartmentID = edh.DepartmentID  
    WHERE e.BusinessEntityID = edh.BusinessEntityID  
    AND d.Name LIKE 'P%') ;  
GO  
```  
  
 В следующем запросе используется ключевое слово `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
   ON e.BusinessEntityID = edh.BusinessEntityID   
WHERE edh.DepartmentID IN  
(SELECT DepartmentID  
   FROM HumanResources.Department  
   WHERE Name LIKE 'P%') ;  
GO  
```  
  
### <a name="e-using-not-exists"></a>Д. Использование ключевого слова NOT EXISTS  
 Работа ключевого слова NOT EXISTS противоположна работе ключевого слова EXISTS. Предложение WHERE в ключевом слове NOT EXISTS удовлетворяется, если вложенный запрос не возвратил никаких строк. В следующем примере выполняется поиск сотрудников, не входящих в состав подразделений, имена которых начинаются на `P`.  
  
```sql  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE NOT EXISTS  
(SELECT *  
   FROM HumanResources.Department AS d  
   JOIN HumanResources.EmployeeDepartmentHistory AS edh  
      ON d.DepartmentID = edh.DepartmentID  
   WHERE e.BusinessEntityID = edh.BusinessEntityID  
   AND d.Name LIKE 'P%')  
ORDER BY LastName, FirstName  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstName                      LastName                       Title
------------------------------ ------------------------------ ------------
Syed                           Abbas                          Pacific Sales Manager
Hazem                          Abolrous                       Quality Assurance Manager
Humberto                       Acevedo                        Application Specialist
Pilar                          Ackerman                       Shipping & Receiving Superviso
François                       Ajenstat                       Database Administrator
Amy                            Alberts                        European Sales Manager
Sean                           Alexander                      Quality Assurance Technician
Pamela                         Ansman-Wolfe                   Sales Representative
Zainal                         Arifin                         Document Control Manager
David                          Barber                         Assistant to CFO
Paula                          Barreto de Mattos              Human Resources Manager
Shai                           Bassli                         Facilities Manager
Wanida                         Benshoof                       Marketing Assistant
Karen                          Berg                           Application Specialist
Karen                          Berge                          Document Control Assistant
Andreas                        Berglund                       Quality Assurance Technician
Matthias                       Berndt                         Shipping & Receiving Clerk
Jo                             Berry                          Janitor
Jimmy                          Bischoff                       Stocker
Michael                        Blythe                         Sales Representative
David                          Bradley                        Marketing Manager
Kevin                          Brown                          Marketing Assistant
David                          Campbell                       Sales Representative
Jason                          Carlson                        Information Services Manager
Fernando                       Caro                           Sales Representative
Sean                           Chai                           Document Control Assistant
Sootha                         Charncherngkha                 Quality Assurance Technician
Hao                            Chen                           HR Administrative Assistant
Kevin                          Chrisulis                      Network Administrator
Pat                            Coleman                        Janitor
Stephanie                      Conroy                         Network Manager
Debra                          Core                           Application Specialist
Ovidiu                         Crãcium                        Sr. Tool Designer
Grant                          Culbertson                     HR Administrative Assistant
Mary                           Dempsey                        Marketing Assistant
Thierry                        D'Hers                         Tool Designer
Terri                          Duffy                          VP Engineering
Susan                          Eaton                          Stocker
Terry                          Eminhizer                      Marketing Specialist
Gail                           Erickson                       Design Engineer
Janice                         Galvin                         Tool Designer
Mary                           Gibson                         Marketing Specialist
Jossef                         Goldberg                       Design Engineer
Sariya                         Harnpadoungsataya              Marketing Specialist
Mark                           Harrington                     Quality Assurance Technician
Magnus                         Hedlund                        Facilities Assistant
Shu                            Ito                            Sales Representative
Stephen                        Jiang                          North American Sales Manager
Willis                         Johnson                        Recruiter
Brannon                        Jones                          Finance Manager
Tengiz                         Kharatishvili                  Control Specialist
Christian                      Kleinerman                     Maintenance Supervisor
Vamsi                          Kuppa                          Shipping & Receiving Clerk
David                          Liu                            Accounts Manager
Vidur                          Luthra                         Recruiter
Stuart                         Macrae                         Janitor
Diane                          Margheim                       Research & Development Enginee
Mindy                          Martin                         Benefits Specialist
Gigi                           Matthew                        Research & Development Enginee
Tete                           Mensa-Annan                    Sales Representative
Ramesh                         Meyyappan                      Application Specialist
Dylan                          Miller                         Research & Development Manager
Linda                          Mitchell                       Sales Representative
Barbara                        Moreland                       Accountant
Laura                          Norman                         Chief Financial Officer
Chris                          Norred                         Control Specialist
Jae                            Pak                            Sales Representative
Wanda                          Parks                          Janitor
Deborah                        Poe                            Accounts Receivable Specialist
Kim                            Ralls                          Stocker
Tsvi                           Reiter                         Sales Representative
Sharon                         Salavaria                      Design Engineer
Ken                            Sanchez                        Chief Executive Officer
José                           Saraiva                        Sales Representative
Mike                           Seamans                        Accountant
Ashvini                        Sharma                         Network Administrator
Janet                          Sheperdigian                   Accounts Payable Specialist
Candy                          Spoon                          Accounts Receivable Specialist
Michael                        Sullivan                       Sr. Design Engineer
Dragan                         Tomic                          Accounts Payable Specialist
Lynn                           Tsoflias                       Sales Representative
Rachel                         Valdez                         Sales Representative
Garrett                        Vargar                         Sales Representative
Ranjit                         Varkey Chudukatil              Sales Representative
Bryan                          Walton                         Accounts Receivable Specialist
Jian Shuo                      Wang                           Engineering Manager
Brian                          Welcker                        VP Sales
Jill                           Williams                       Marketing Specialist
Dan                            Wilson                         Database Administrator
John                           Wood                           Marketing Specialist
Peng                           Wu                             Quality Assurance Supervisor
  
(91 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-using-exists"></a>Е. Использование ключевого слова EXISTS  
 В следующем примере определяется, могут ли строки в таблице `ProspectiveBuyer` соответствовать строкам в таблице `DimCustomer`. Запрос будет возвращать строки, только при совпадении значений `LastName` и `BirthDate` в двух таблицах.  
  
```sql
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
### <a name="g-using-not-exists"></a>Ж. Использование ключевого слова NOT EXISTS  
 Работа ключевого слова NOT EXISTS противоположна работе ключевого слова EXISTS. Предложение WHERE в ключевом слове NOT EXISTS удовлетворяется, если вложенный запрос не возвратил никаких строк. В следующем примере показан поиск всех строк в таблице `DimCustomer`, где `LastName` и `BirthDate` не соответствуют записям в таблице `ProspectiveBuyers`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE NOT EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
## <a name="see-also"></a>См. также:  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  


