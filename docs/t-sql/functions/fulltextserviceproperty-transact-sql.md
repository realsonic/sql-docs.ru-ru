---
description: FULLTEXTSERVICEPROPERTY (Transact-SQL)
title: FULLTEXTSERVICEPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTSERVICEPROPERTY_TSQL
- FULLTEXTSERVICEPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], properties
- FULLTEXTSERVICEPROPERTY function
- services [SQL Server], full-text search properties
- test
ms.assetid: b7dcacb0-af83-4807-9d1e-49148b56b59c
author: cawrites
ms.author: chadam
ms.openlocfilehash: b5d14ac6097fb6b1199124046c5780139e644949
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087325"
---
# <a name="fulltextserviceproperty-transact-sql"></a>FULLTEXTSERVICEPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает информацию, связанную со свойствами механизма полнотекстового поиска. Значения этих свойств можно задавать и получать с помощью процедуры **sp_fulltext_service**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FULLTEXTSERVICEPROPERTY ('property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *property*  
 Выражение, содержащее имя свойства уровня полнотекстовой службы. В таблице перечислены свойства и описания возвращаемых сведений.  
  
> [!NOTE]
>  Следующие свойства будут удалены в последующей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **ConnectTimeout**, **DataTimeout** и **ResourceUsage**. Избегайте использовать эти свойства в новых разработках и запланируйте изменение приложений, где они используются в настоящий момент.  
  
|Property (Свойство)|Значение|  
|--------------|-----------|  
|**ResourceUsage**|Возвращает 0. Поддерживается только для обеспечения обратной совместимости.|  
|**ConnectTimeout**|Возвращает 0. Поддерживается только для обеспечения обратной совместимости.|  
|**IsFulltextInstalled**|Установлен ли полнотекстовый компонент с текущим экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = полнотекстовый компонент не установлен.<br /><br /> 1 = полнотекстовый компонент установлен.<br /><br /> NULL = недопустимое входное значение или ошибка.|  
|**DataTimeout**|Возвращает 0. Поддерживается только для обеспечения обратной совместимости.|  
|**LoadOSResources**|Указывает, зарегистрированы ли средства разбиения по словам и фильтры операционной системы и используются ли они с этим экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. По умолчанию это свойство отключено, чтобы предотвратить случайные изменения поведения в результате применения обновлений к операционной системе (ОС). Разрешение использования ресурсов ОС обеспечивает доступ к ресурсам для языков и типов документов, зарегистрированных со службой [!INCLUDE[msCoName](../../includes/msconame-md.md)] Indexing Service, но ресурс специально для данного экземпляра не установлен. Если разрешена загрузка ресурсов ОС, следует убедиться в том, что ресурсы ОС представляют собой надежные подписанные двоичные файлы; в противном случае, если параметру **VerifySignature** присвоено значение 1, они не могут быть загружены.<br /><br /> 0 = использовать только фильтры и средства разбиения по словам, характерные для этого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = загрузить фильтры и средства разбиения по словам из операционной системы.|  
|**VerifySignature**|Указывает, только ли подписанные двоичные файлы загружаются службой поиска [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search Service.  По умолчанию загружаются только доверенные, подписанные двоичные файлы.<br /><br /> 0 = не проверять наличие подписи у двоичных файлов.<br /><br /> 1 = убедиться, что загружаются только доверенные, подписанные двоичные файлы.|  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="examples"></a>Примеры  
 В следующем примере проверяется, разрешена ли загрузка только подписанных бинарных файлов. Возвращаемое значение указывает, что проверка загружаемых файлов не производится.  
  
```sql  
SELECT fulltextserviceproperty('VerifySignature');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------   
0  
```  
  
 Следует заметить, что для возвращения параметру проверки подписи его значение по умолчанию 1 можно использовать следующую инструкцию `sp_fulltext_service`:  
  
```sql  
EXEC sp_fulltext_service @action='verify_signature', @value=1;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [FULLTEXTCATALOGPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_fulltext_service (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
  
  
