---
description: CREATE AGGREGATE (Transact-SQL)
title: CREATE AGGREGATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_AGGREGATE_TSQL
- CREATE AGGREGATE
- AGGREGATE_TSQL
- AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE AGGREGATE statement
- aggregate functions [SQL Server], user-defined
- user-defined functions [CLR integration]
ms.assetid: 62eebc19-9f15-4245-94fa-b3fcd64a9d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 773c2010cdefa2455e0800ed0370cbc118c2dab9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098434"
---
# <a name="create-aggregate-transact-sql"></a>CREATE AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает определяемую пользователем агрегатную функцию, реализация которой определена в классе сборки платформы [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Чтобы компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] привязал агрегатную функцию к ее реализации, содержащая реализацию сборка платформы [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] должна быть сначала передана в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью инструкции CREATE ASSEMBLY.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
CREATE AGGREGATE [ schema_name . ] aggregate_name  
        (@param_name <input_sqltype>   
        [ ,...n ] )  
RETURNS <return_sqltype>  
EXTERNAL NAME assembly_name [ .class_name ]  
  
<input_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
<return_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы, которой принадлежит определяемая пользователем агрегатная функция.  
  
 *aggregate_name*  
 Имя создаваемой агрегатной функции.  
  
 **@** _param_name_  
 Один или несколько параметров в определяемой пользователем статистической функции. Значение параметра должно быть задано пользователем при выполнении агрегатной функции. Определяет имя параметра, используя знак **@** как первый символ. Имя параметра должно соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Параметры являются локальными для функции.  
  
 *system_scalar_type*  
 Любой из системных скалярных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для хранения значения входного параметра или возвращаемого значения. Для определяемого пользователем статистического выражения в качестве параметра могут использоваться все скалярные типы данных, кроме **text**, **ntext** и **image**. Нельзя использовать нескалярные типы, такие как **cursor** и **table**.  
  
 *udt_schema_name*  
 Имя схемы, к которой относится пользовательский тип данных среды CLR. Если не указано иное, [!INCLUDE[ssDE](../../includes/ssde-md.md)] ссылается на аргумент *udt_type_name* в следующем порядке:  
  
-   собственное пространство имен типов SQL;  
  
-   в установленной по умолчанию для текущего пользователя схеме в текущей базе данных;  
  
-   Схема **dbo** в текущей базе данных.  
  
 *udt_type_name*  
 Название пользовательского типа среды CLR, созданного в текущей базе данных. Если аргумент *udt_schema_name* не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предполагает, что тип принадлежит к схеме текущего пользователя.  
  
 *assembly_name* [ **.** _class_name_ ]  
 Указывает сборку, с которой связывается определяемая пользователем агрегатная функция и, при необходимости, имя схемы, к которой принадлежит сборка, и имя класса в сборке, реализующего определяемую пользователем статистическую функцию. Сборка уже должна быть создана в базе данных с помощью инструкции CREATE ASSEMBLY. Параметр *class_name* должен быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и совпадать с именем класса, существующим в сборке. Аргумент *class_name* может быть именем, указанным в пространстве имен, если в языке программирования, использовавшемся для написания класса, применяются пространства имен, например C#. Если аргумент *class_name* не задан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] считает, что его значение равно значению аргумента *aggregate_name*.  
  
## <a name="remarks"></a>Комментарии  
 По умолчанию возможность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускать код CLR отключена. Можно создавать, изменять и удалять объекты базы данных, которые ссылаются на модули управляемого кода, но код в этих модулях не будет выполняться в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], пока параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) не будет включен с помощью хранимой процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 Класс сборки, указанной в аргументе *assembly_name*, и его методы должны соответствовать всем требованиям к реализации определяемых пользователем агрегатных функций в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Пользовательские агрегатные функции среды CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md).  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CREATE AGGREGATE и разрешения REFERENCES для сборки, указанной в предложении EXTERNAL NAME.  
  
## <a name="examples"></a>Примеры  
 В следующем примере предполагается, что образец приложения StringUtilities.csproj скомпилирован. Дополнительные сведения см. в разделе [Пример функций программы работы со строками](/previous-versions/sql/sql-server-2016/ff878119(v=sql.130)).  
  
 Пример создает статистическое выражение `Concatenate`. Перед созданием статистического выражения в локальной базе данных регистрируется сборка `StringUtilities.dll`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SamplesPath nvarchar(1024)  
-- You may have to modify the value of the this variable if you have  
--installed the sample some location other than the default location.  
  
SELECT @SamplesPath = REPLACE(physical_name, 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf', 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\')   
     FROM master.sys.database_files   
     WHERE name = 'master';  
  
CREATE ASSEMBLY StringUtilities FROM @SamplesPath + 'StringUtilities\CS\StringUtilities\bin\debug\StringUtilities.dll'  
WITH PERMISSION_SET=SAFE;  
GO  
  
CREATE AGGREGATE Concatenate(@input nvarchar(4000))  
RETURNS nvarchar(4000)  
EXTERNAL NAME [StringUtilities].[Microsoft.Samples.SqlServer.Concatenate];  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [DROP AGGREGATE (Transact-SQL)](../../t-sql/statements/drop-aggregate-transact-sql.md)  
  
