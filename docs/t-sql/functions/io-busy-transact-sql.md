---
description: '&#x40;&#x40;IO_BUSY (Transact-SQL)'
title: '@@IO_BUSY (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@IO_BUSY'
- '@@IO_BUSY_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- ticks [SQL Server]
- I/O [SQL Server], time spent performing operations
- '@@IO_BUSY function'
- output operations [SQL Server]
- input operations [SQL Server]
- time [SQL Server], I/O operations
ms.assetid: 3c26770c-41ae-4e34-8c82-7bef920ffbca
author: cawrites
ms.author: chadam
ms.openlocfilehash: dbcd33b3ccc8079a8bf6c23aab582d38b3bf66e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102578"
---
# <a name="x40x40io_busy-transact-sql"></a>&#x40;&#x40;IO_BUSY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает время, затраченное [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на выполнение операций ввода-вывода с момента последнего запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Результат представляется в интервалах ЦП (тактах) и объединяет информацию обо всех ЦП, поэтому может превышать фактическое время выполнения операций. Умножьте его на значение @@TIMETICKS, чтобы преобразовать в микросекунды.  
  
> [!NOTE]  
>  Если время, возвращенное @@CPU_BUSY или @@IO_BUSY, превышает приблизительно 49 дней совокупного времени ЦП, выдается предупреждение об арифметическом переполнении. В этом случае значения переменных @@CPU_BUSY, @@IO_BUSY и @@IDLE являются неточными.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
@@IO_BUSY  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **integer**  
  
## <a name="remarks"></a>Комментарии  
 Чтобы отобразить отчет, содержащий несколько статистик [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните хранимую процедуру sp_monitor.  
  
## <a name="examples"></a>Примеры  
 Следующий код возвращает число миллисекунд, которые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потратил на выполнение операций ввода-вывода с момента запуска до текущего момента. Во избежание арифметического переполнения при преобразовании значения в микросекунды в этом примере одно из значений преобразуется в тип данных **float**.  
  
```sql  
SELECT @@IO_BUSY*@@TIMETICKS AS 'IO microseconds',   
   GETDATE() AS 'as of';  
```  
  
 Типичный результирующий набор:  
  
```  
  
IO microseconds as of                   
--------------- ----------------------  
4552312500      12/5/2006 10:23:00 AM   
```  
  
## <a name="see-also"></a>См. также  
 [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [@@CPU_BUSY (Transact-SQL)](../../t-sql/functions/cpu-busy-transact-sql.md)   
 [sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Системные статистические функции (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
