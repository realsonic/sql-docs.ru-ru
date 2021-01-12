---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fa09a4ac7fb59ec39524a709387ee4ff91aed63
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083104"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет значение параметра конфигурации (столбец **config_value** в результирующем наборе данных процедуры **sp_configure**) с помощью хранимой системной процедуры **sp_configure**. Поскольку для изменения некоторых параметров конфигурации требуется остановка и перезапуск сервера, инструкция RECONFIGURE не всегда обновляет текущее значение (столбец **run_value** в результирующем наборе данных процедуры **sp_configure**) для измененного параметра конфигурации.    
    
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Синтаксис    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 RECONFIGURE    
 Указывает, что если для изменения параметра конфигурации не требуется остановка и перезапуск сервера, то значение будет изменено. Инструкция RECONFIGURE также проверяет новые конфигурационные значения на соответствие спецификации (например, значение порядка сортировки, которое не существует в таблице **syscharsets**) или выявляет нежелательные значения. Для параметров конфигурации, которые можно изменять без остановки или перезапуска сервера, текущие значения параметров должны совпасть с новыми после вызова RECONFIGURE.    
    
 WITH OVERRIDE    
 Отключает проверку значений параметров конфигурации (на недопустимые или нежелательные значения) для расширенного параметра настройки **интервала восстановления**.    
    
 Практически любой параметр конфигурации можно перенастроить с помощью параметра WITH OVERRIDE, однако некоторые неустранимые ошибки по-прежнему блокируются. Например, параметр **min server memory** может принимать значения большие, чем предусмотрено параметром **max server memory**.
      
## <a name="remarks"></a>Примечания    
 При использовании процедуры **sp_configure** недопустимо, чтобы новые значения параметров конфигурации выходили за пределы установленных в документации диапазонов для каждого параметра.    
    
 Недопустимо использование RECONFIGURE в явной или неявной транзакции. При одновременной перенастройке нескольких параметров в случае сбоя какой-либо из операций перенастройки ни одна из этих операций не вступит в силу.    
    
 При перенастройке регулятора ресурсов см. описание параметра RECONFIGURE в разделе [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md).    
    
## <a name="permissions"></a>Разрешения    
 Разрешения для RECONFIGURE по умолчанию предоставляются участникам, которым предоставлено разрешение ALTER SETTINGS. Этим разрешением неявно обладают встроенные роли сервера **sysadmin** и **serveradmin**.    
    
## <a name="examples"></a>Примеры    
 Следующий пример устанавливает верхний предел для параметра `recovery interval` в `75` минут с помощью разрешения `RECONFIGURE WITH OVERRIDE`. Интервалы восстановления более 60 минут нежелательны и по умолчанию запрещены. Однако из-за параметра `WITH OVERRIDE`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не проверяет, является ли указанное значение (`75`) параметра `recovery interval` допустимым.    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>См. также:    
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  
