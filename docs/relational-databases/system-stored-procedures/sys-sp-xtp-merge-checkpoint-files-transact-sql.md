---
description: sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
title: sys.sp_xtp_merge_checkpoint_files (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sp_xtp_merge_checkpoint_files_TSQL
- sys.sp_xtp_merge_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_merge_checkpoint_files
ms.assetid: da04df2a-f7a1-41e7-a1ef-2d5d68919892
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 16cf2326ad9b732d2d01f75a14ccb9026111803f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168152"
---
# <a name="syssp_xtp_merge_checkpoint_files-transact-sql"></a>sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  **sys.sp_xtp_merge_checkpoint_files** выполняет слияние всех данных и разностных файлов в указанном диапазоне транзакций.  
  
 Дополнительные сведения см. в разделе [Создание и управление хранилищем для Memory-Optimizedных объектов](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
||  
|-|  
|**Примечание**. Эта хранимая процедура является устаревшей в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] . Он больше не нужен, и его нельзя использовать, начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .|  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.sp_xtp_merge_checkpoint_files database_name, @transaction_lower_bound, @transaction_upper_bound  
[;]  
```  
  
## <a name="arguments"></a>Аргументы  
 *database_name*  
 Имя базы данных, для которой следует вызвать слияние. Если база данных не содержит таблицы в памяти, эта процедура возвращает ошибку пользователя. Если база данных находится в автономном режиме, возвращается ошибка.  
  
 *lower_bound_Tid*  
 Нижняя граница (BIGINT) транзакций для файла данных, как показано в [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) соответствующий файлу начальной контрольной точки слияния. Для недопустимых значений transactonId возникает ошибка.  
  
 *upper_bound_Tid*  
 Верхняя граница (BIGINT) транзакций для файла данных, как показано в [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md). Для недопустимых значений transactonId возникает ошибка.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Отсутствуют  
  
## <a name="cursors-returned"></a>Возвращенные курсоры  
 None  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли сервера sysadmin и в предопределенной роли базы данных db_owner.  
  
## <a name="remarks"></a>Примечания  
 Объединяет все файлы данных и разностные файлы в допустимом диапазоне для получения одного файла данных и одного разностного файла. Эта процедура не учитывает политику слияния.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
