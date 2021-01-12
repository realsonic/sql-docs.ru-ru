---
description: CREATE XML SCHEMA COLLECTION (Transact-SQL)
title: CREATE XML SCHEMA COLLECTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE XML SCHEMA COLLECTION
- CREATE_XML_SCHEMA_COLLECTION_TSQL
- CREATE XML SCHEMA
- CREATE_XML_SCHEMA_TSQL
- COLLECTION
- COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML SCHEMA COLLECTION statement
- importing schema components
- schema collections [SQL Server], creating
- multiple schema namespaces
- XML schema collections [SQL Server], creating
ms.assetid: 350684e8-b3f6-4b58-9dbc-0f05cc776ebb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 44cfb98daf253a1229d8fd4510b22e8d765c5db4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099461"
---
# <a name="create-xml-schema-collection-transact-sql"></a>CREATE XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Импортирует компоненты схемы в базу данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE XML SCHEMA COLLECTION [ <relational_schema>. ]sql_identifier AS Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *relational_schema*  
 Определяет имя реляционной схемы. Если значение не указано, то применяется реляционная схема по умолчанию.  
  
 *sql_identifier*  
 Идентификатор SQL для коллекции XML-схем.  
  
 *Выражение*  
 Строковая константа или скалярная переменная. Имеет тип **varchar**, **varbinary**, **nvarchar** или **xml**.  
  
## <a name="remarks"></a>Remarks  
 С помощью инструкции [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md) можно добавлять новые пространства имен к коллекции или новые компоненты к существующим пространствам имен в коллекции.  
  
 Чтобы удалить коллекции, используйте [DROP XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Для создания коллекции XML SCHEMA COLLECTION требуется, по крайней мере, один из следующих наборов разрешений:  
  
-   разрешение CONTROL на сервере;  
  
-   разрешение ALTER ANY DATABASE на сервере;  
  
-   разрешение ALTER в базе данных;  
  
-   разрешение CONTROL в базе данных;  
  
-   разрешения ALTER ANY SCHEMA и CREATE XML SCHEMA COLLECTION в базе данных;  
  
-   разрешение ALTER или CONTROL в реляционной схеме и разрешение CREATE XML SCHEMA COLLECTION в базе данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>A. Создание коллекции схем XML в базе данных  
 Следующие примеры создают коллекцию XML-схем `ManuInstructionsSchemaCollection`. Коллекция имеет только одно пространство имен схемы.  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note collection name specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 В качестве альтернативы можно связать коллекцию схем с переменной и указать переменную в инструкции `CREATE XML SCHEMA COLLECTION` следующим образом:  
  
```  
DECLARE @MySchemaCollection nvarchar(max)  
Set @MySchemaCollection  = N' copy the schema collection here'  
CREATE XML SCHEMA COLLECTION MyCollection AS @MySchemaCollection   
```  
  
 Переменная в примере принадлежит к типу `nvarchar(max)`. Переменная также может принадлежать к типу данных **xml**, и в таком случае она неявно преобразуется в строку.  
  
 Дополнительные сведения см. в разделе [Просмотр хранимой коллекции схем XML](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Коллекции схем можно хранить в столбце типа **xml**. В этом случае для создания коллекции XML-схем необходимо выполнить следующее:  
  
1.  Получите коллекцию схем из столбца с помощью инструкции SELECT и свяжите ее с переменной типа **xml** или **varchar**.  
  
2.  Укажите имя переменной в инструкции CREATE XML SCHEMA COLLECTION.  
  
 Инструкция CREATE XML SCHEMA COLLECTION сохраняет только такие компоненты схемы, которые понятны для SQL Server; все остальные элементы XML-схемы в базе данных не сохраняются. Таким образом, если нужно получить коллекцию XML-схем в том виде, в котором она была предоставлена, рекомендуется сохранять XML-схемы в столбце базы данных или в другой папке компьютера.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>Б. Указание нескольких пространств имен схем в коллекции схем  
 Можно указать несколько XML-схем при создании коллекции XML-схем. Пример:  
  
```sql  
CREATE XML SCHEMA COLLECTION MyCollection AS N'  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->    
</xsd:schema>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->  
</xsd:schema>';  
```  
  
 В следующем примере создается коллекция XML-схем `ProductDescriptionSchemaCollection`, которая включает два пространства имен XML-схем.  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO -- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>В. Импорт схемы, не указывающей целевое пространство имен  
 Если схема, не содержащая атрибут **targetNamespace**, импортируется в коллекцию, ее компоненты связываются с пустой строкой в качестве целевого пространства имен, как показано в следующем примере. Обратите внимание на то, что отсутствие связывания одной или нескольких схем, импортированных в коллекцию, приводит к тому, что некоторые компоненты схемы (потенциально несвязанные) будут связаны с целевым пространством имен по умолчанию в виде пустой строки.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
go  
-- Query will return the names of all the collections that   
--contain a schema with no target namespace.  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
### <a name="d-using-an-xml-schema-collection-and-batches"></a>Г. Использование коллекций и пакетов схем XML  
 На коллекцию схем нельзя ссылаться в том же пакете, в котором эта коллекция была создана. При попытке сослаться на коллекцию в том же пакете, где коллекция была создана, возникнет ошибка, сообщающая, что коллекция не существует. Выполнение следующего примера возможно; однако если удалить строку кода `GO` и таким образом попытаться заставить коллекцию XML-схем напечатать имя переменной `xml` в том же пакете, то будет возвращена ошибка.  
  
```sql  
CREATE XML SCHEMA COLLECTION mySC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="string"/>  
</schema>  
';  
GO  
CREATE TABLE T (Col1 xml (mySC));  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [DROP XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
