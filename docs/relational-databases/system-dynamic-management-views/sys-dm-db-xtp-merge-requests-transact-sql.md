---
description: sys.dm_db_xtp_merge_requests (Transact-SQL)
title: sys.dm_db_xtp_merge_requests (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c1224e88-af74-4c99-ae32-d5d2c552a1f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a7f8f4a38bffb8abc6be1380a2225077130f143
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099872"
---
# <a name="sysdm_db_xtp_merge_requests-transact-sql"></a>sys.dm_db_xtp_merge_requests (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Отслеживает запросы на слияние базы данных. Запрос на слияние может быть создан SQL Server или запрос был выполнен пользователем с помощью [sys.sp_xtp_merge_checkpoint_files (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-merge-checkpoint-files-transact-sql.md).

> [!NOTE]
> Это динамическое административное представление (DMV) sys.dm_db_xtp_merge_requests существует до Microsoft SQL Server 2014.
> Но начиная с SQL Server 2016 это динамическое административное представление больше не применяется.

## <a name="columns-in-the-report"></a>Столбцы в отчете

| Имя столбца | Тип данных | Описание |
| :-- | :-- | :-- |
| request_state | tinyint | Состояние запроса на слияние:<br/>0 = запрошен<br/>1 = ожидание<br/>2 = установлено<br/>3 = оставлен |
| request_state_desc | nvarchar(60) | Значения для текущего состояния запроса:<br/><br/>Запрошено — существует запрос на слияние.<br/>Ожидание — выполняется обработка слияния.<br/>Установлено — слияние завершено.<br/>Брошено — слияние не удалось завершить, возможно, из-за отсутствия хранилища. |
| destination_file_id | Код GUID | Уникальный идентификатор целевого файла для слияния исходных файлов. |
| lower_bound_tsn | BIGINT | Минимальная метка времени для целевого файла объединения. Наименьшая метка времени транзакции всех исходных файлов, которые необходимо объединить. |
| upper_bound_tsn | BIGINT | Максимальная метка времени для целевого файла объединения. Наивысшая метка времени транзакции всех исходных файлов, которые необходимо объединить. |
| collection_tsn | BIGINT | Метка времени, в которое можно собрать текущую строку.<br/><br/>Строка в состоянии «Установлено» удаляется при обретении параметром checkpoint_tsn значения, численно превышающего значение collection_tsn.<br/><br/>Строка в состоянии «Оставлено» удаляется при обретении параметром checkpoint_tsn значения, которое численно меньше значения collection_tsn. |
| checkpoint_tsn | BIGINT | Время запуска контрольной точки.<br/><br/>Все операции удаления, выполняемые транзакциями, с меткой времени ниже этой учитываются в новом файле данных. Остальные операции удаления перемещаются в целевой разностный файл. |
| sourcenumber_file_id | Код GUID | До 16 внутренних идентификаторов файлов, которые уникально идентифицируют исходные файлы в слиянии. |

## <a name="permissions"></a>Разрешения

Необходимо разрешение VIEW DATABASE STATE на текущую базу данных.

## <a name="see-also"></a>См. также статью

[Оптимизированные для памяти динамические административные представления таблиц (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)
