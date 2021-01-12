---
description: Инструкция ALTER BROKER PRIORITY (Transact-SQL)
title: ALTER BROKER PRIORITY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_BROKER_TSQL
- ALTER BROKER PRIORITY
- ALTER BROKER
- ALTER_BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER BROKER PRIORITY statement
- ssbdiagnose
ms.assetid: 15fda1b2-e4dd-4f9d-935a-2e38926075b2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 463c68ab8dd5abf6c65f52fe30eefbbed23ae912
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100272"
---
# <a name="alter-broker-priority-transact-sql"></a>Инструкция ALTER BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет свойства приоритета диалога компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
ALTER BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
{ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = { PriorityValue | DEFAULT } ]  
              )  
}  
[;]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *ConversationPriorityName*  
 Имя изменяемого приоритета диалога. Это имя должно ссылаться на приоритет диалога в текущей базе данных.  
  
 SET  
 Задает критерий для определения применимости приоритета к заданному диалогу. Аргумент SET обязателен и должен содержать хотя бы один критерий: CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME или PRIORITY_LEVEL.  
  
 CONTRACT_NAME = {*ContractName* | **ANY**}  
 Указывает имя контракта, который будет использоваться в качестве критерия, определяющего применимость приоритета к диалогу. Аргумент *ContractName* — это идентификатор компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], который должен указывать имя контракта в текущей базе данных.  
  
 *ContractName*  
 Указывает, что приоритет может применяться только к диалогам, в которых инструкция BEGIN DIALOG, начинающая диалог, содержит параметр ON CONTRACT *ContractName*.  
  
 ANY  
 Указывает, что приоритет может применяться к любому диалогу, независимо от используемого контракта.  
  
 Если аргумент CONTRACT_NAME не был указан, то свойство контракта приоритета диалога не изменяется.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Указывает имя службы, которая будет использоваться в качестве критерия для определения применимости приоритета к конечной точке диалога.  
  
 Аргумент *LocalServiceName* — это идентификатор [!INCLUDE[ssDE](../../includes/ssde-md.md)]; он должен указывать имя службы в текущей базе данных.  
  
 *LocalServiceName*  
 Указывает, что объектом применения приоритета диалога может быть:  
  
-   любая конечная точка-инициатор диалога, имя вызывающей службы которой соответствует аргументу *LocalServiceName*;  
  
-   любая целевая конечная точка диалога, имя целевой службы которой соответствует аргументу *LocalServiceName*.  
  
 ANY  
 -   Указывает, что приоритет может применяться к любой конечной точке диалога, независимо от имени локальной службы, используемой точкой.  
  
 Если аргумент LOCAL_SERVICE_NAME не был указан, то свойство локальной службы приоритета диалога не изменяется.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Указывает имя службы, которая будет использоваться в качестве критерия для определения применимости приоритета к конечной точке диалога.  
  
 *RemoteServiceName* — это литерал типа **nvarchar(256)**. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит побайтовое сравнение при поиске соответствия строке *RemoteServiceName*. При сравнении учитывается регистр и не применяются текущие параметры сортировки. Целевая служба может располагаться в текущем экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] или в удаленном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 '*RemoteServiceName*'  
 Задает приоритет диалога, который будет иметь:  
  
-   любая конечная точка-инициатор диалога, для которой имя целевой службы совпадает с параметром *RemoteServiceName*;  
  
-   любая целевая конечная точка диалога, для которой имя вызывающей службы совпадает с параметром *RemoteServiceName*.  
  
 ANY  
 Указывает, что приоритет диалога будет применяться ко всем конечным точкам диалога, вне зависимости от имен удаленных служб, связанных с ними.  
  
 Если аргумент REMOTE_SERVICE_NAME не был указан, то свойство удаленной службы приоритета диалога не изменяется.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Задает уровень приоритета, который будет присвоен любой конечной точке диалога, использующей контракты и службы, указанные в приоритете диалога. Аргумент *PriorityValue* должен быть целочисленным литералом в диапазоне от 1 (наименьший приоритет) до 10 (наибольший приоритет).  
  
 Если аргумент PRIORITY_LEVEL не был указан, то свойство уровня приоритета для приоритета диалога не изменяется.  
  
## <a name="remarks"></a>Комментарии  
 Свойства, измененные с помощью инструкции ALTER BROKER PRIORITY, не применяются к существующим диалогам. Существующие диалоги будут использовать приоритет, который был задан при их запуске.  
  
 Дополнительные сведения см. в статье [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на создание приоритета диалога по умолчанию имеют члены предопределенных ролей базы данных **db_ddladmin** и **db_owner** и члены предопределенной роли сервера **sysadmin**. Необходимо разрешение ALTER на базу данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-changing-only-the-priority-level-of-an-existing-conversation-priority"></a>A. Изменение только уровня приоритета существующего диалога.  
 Изменяет уровень приоритета, но не изменяет свойства контракта, локальной службы и удаленной службы.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-changing-all-of-the-properties-of-an-existing-conversation-priority"></a>Б. Изменение всех свойств приоритета существующего диалога.  
 Изменяет свойства уровня приоритета, контракта, локальной и удаленной служб.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContractB,  
         LOCAL_SERVICE_NAME = TargetServiceB,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceB',  
         PRIORITY_LEVEL = 8);  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys.conversation_priorities (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
