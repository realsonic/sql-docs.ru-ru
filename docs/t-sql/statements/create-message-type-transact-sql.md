---
description: CREATE MESSAGE TYPE (Transact-SQL)
title: CREATE MESSAGE TYPE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_MESSAGE_TSQL
- MESSAGE_TSQL
- MESSAGE
- CREATE MESSAGE
- CREATE_MESSAGE_TYPE_TSQL
- MESSAGE TYPE
- MESSAGE_TYPE_TSQL
- CREATE MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- XML [Service Broker]
- validation [Service Broker]
- message types [Service Broker], creating
- empty messages [SQL Server]
- binary [SQL Server], message types
- CREATE MESSAGE TYPE statement
ms.assetid: 98fe0fff-1a2e-4ca2-b37f-83a06fdf098e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b32fe2417a7a9d9d290b72cf3de2dfc325fc0eaf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093459"
---
# <a name="create-message-type-transact-sql"></a>CREATE MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Создает новый тип сообщений. Тип сообщений определяет имя сообщения и проверку, выполняемую компонентом [!INCLUDE[ssSB](../../includes/sssb-md.md)] для сообщений с этим именем. Обе стороны диалога должны определить одинаковые типы сообщений.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE MESSAGE TYPE message_type_name  
    [ AUTHORIZATION owner_name ]  
    [ VALIDATION = {  NONE  
                    | EMPTY   
                    | WELL_FORMED_XML  
                    | VALID_XML WITH SCHEMA COLLECTION schema_collection_name  
                   } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *message_type_name*  
 Имя создаваемого типа сообщений. Создается новый тип сообщений в текущей базе данных, которой владеет участник, указанный в предложении AUTHORIZATION. Не могут быть указаны имена сервера, базы данных и схемы. Имя *Message_type_name* может содержать не более 128 символов.  
  
 AUTHORIZATION *owner_name*  
 Устанавливает указанного пользователя или роль базы данных в качестве владельца типа сообщений. Если текущим пользователем является **dbo** или **sa**, то аргумент *owner_name* может быть именем любого допустимого пользователя или роли. В противном случае аргумент *owner_name* должен быть именем текущего пользователя, именем пользователя, для которого у текущего пользователя есть разрешение IMPERSONATE, или именем роли, которой принадлежит текущий пользователь. Если это предложение опущено, тип сообщений будет принадлежать текущему пользователю.  
  
 VALIDATION  
 Указывает, как компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит проверку текста сообщения этого типа. Если это предложение не указано, то по умолчанию проверке присваивается значение NONE.  
  
 None  
 Указывает, что проверка не выполняется. Текст сообщения может содержать любые данные или иметь значение NULL.  
  
 EMPTY  
 Указывает, что текст сообщения должен быть NULL.  
  
 WELL_FORMED_XML  
 Указывает, что текст сообщения должен содержать корректные XML-данные.  
  
 VALID_XML WITH SCHEMA COLLECTION *schema_collection_name*  
 Указывает, что текст сообщения должен содержать XML-данные, которые соответствуют схеме в указанной коллекции схем. Аргумент *schema_collection_name* должен быть именем существующей коллекции XML-схем.  
  
## <a name="remarks"></a>Remarks  
 Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] проверяет входящие сообщения. Если сообщение содержит текст, который не соответствует указанному типу проверки, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] удаляет неправильное сообщение и возвращает службе, которая его отправила, сообщение об ошибке.  
  
 Обе стороны диалога должны задать одинаковые имена типа сообщений. В целях оказания помощи при поиске и устранении неполадок, обе стороны диалога обычно указывают одинаковые проверки для типа сообщений, хотя компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не требует использования обеими сторонами одинаковой проверки.  
  
 Тип сообщений не может быть временным объектом. Имена типов сообщений могут начинаться с символа **#** , но они должны быть постоянными объектами.  
  
## <a name="permissions"></a>Разрешения  
 Разрешения на создание типа сообщений по умолчанию есть у предопределенных ролей базы данных **db_ddladmin** или **db_owner** и у предопределенной роли сервера **sysadmin**.  
  
 По умолчанию разрешение на REFERENCES для типов сообщений принадлежит владельцу типа сообщений, членам предопределенной роли базы данных **db_owner** и членам предопределенной роли сервера **sysadmin**.  
  
 Если инструкция CREATE MESSAGE TYPE задает коллекцию схемы, выполняющий инструкцию пользователь должен иметь разрешения REFERENCES для указанной коллекции схем.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-message-type-containing-well-formed-xml"></a>A. Создание типа сообщений, содержащего XML-документ правильного формата  
 Этот пример создает новый тип сообщений, который содержит корректный XML.  
  
```sql  
CREATE MESSAGE TYPE  
  [//Adventure-Works.com/Expenses/SubmitExpense]  
  VALIDATION = WELL_FORMED_XML ;     
```  
  
### <a name="b-creating-a-message-type-containing-typed-xml"></a>Б. Создание типа сообщений, содержащего типовой XML  
 Этот пример создает тип сообщений для отчета о затратах, закодированного в XML. Пример создает коллекцию XML-схем, которая содержит схему для простого отчета о затратах. Затем пример создает новый тип сообщений, который сверяет сообщения со схемой.  
  
```sql  
CREATE XML SCHEMA COLLECTION ExpenseReportSchema AS  
N'<?xml version="1.0" encoding="UTF-16" ?>  
  <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
     targetNamespace="https://Adventure-Works.com/schemas/expenseReport"  
     xmlns:expense="https://Adventure-Works.com/schemas/expenseReport"  
     elementFormDefault="qualified"  
   >   
    <xsd:complexType name="expenseReportType">  
       <xsd:sequence>  
         <xsd:element name="EmployeeName" type="xsd:string"/>  
         <xsd:element name="EmployeeID" type="xsd:string"/>  
         <xsd:element name="ItemDetail"  
           type="expense:ItemDetailType" maxOccurs="unbounded"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:complexType name="ItemDetailType">  
      <xsd:sequence>  
        <xsd:element name="Date" type="xsd:date"/>  
        <xsd:element name="CostCenter" type="xsd:string"/>  
        <xsd:element name="Total" type="xsd:decimal"/>  
        <xsd:element name="Currency" type="xsd:string"/>  
        <xsd:element name="Description" type="xsd:string"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:element name="ExpenseReport" type="expense:expenseReportType"/>  
  
  </xsd:schema>' ;  
  
  CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = VALID_XML WITH SCHEMA COLLECTION ExpenseReportSchema ;  
```  
  
### <a name="c-creating-a-message-type-for-an-empty-message"></a>В. Создание типа сообщений для пустого сообщения  
 Этот пример создает новый тип сообщений с пустой кодировкой.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = EMPTY ;  
```  
  
### <a name="d-creating-a-message-type-containing-binary-data"></a>Г. Создание типа сообщений, содержащего двоичные данные  
 Этот пример создает новый тип сообщений, который может содержать двоичные данные. Так как сообщение будет хранить данные, отличные от XML, в типе сообщений задается тип проверки `NONE`. Обратите внимание, что в таком случае приложение, которое принимает сообщение этого типа, должно проверить, имеются ли в этом сообщении данные, и относятся ли эти данные к ожидаемому типу.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ReceiptImage]  
    VALIDATION = NONE ;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/alter-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
