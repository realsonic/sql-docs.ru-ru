---
title: Объекты DataAdapter и DataReader
description: Сведения о поставщике данных Microsoft SqlClient для SQL Server DataReader, который извлекает данные из базы данных, и DataAdapter, который извлекает данные из источника данных и заполняет DataSet.
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3a252ca44683c062e531081f0b625a5e2359d236
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771396"
---
# <a name="dataadapters-and-datareaders"></a>Объекты DataAdapter и DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

С помощью поставщика данных Microsoft SqlClient для SQL Server **DataReader** можно получить из базы данных поток данных, доступный только для чтения и допускающий перемещение только в прямом направлении. Результаты возвращаются после выполнения запроса и хранятся в сетевом буфере на клиенте до тех пор, пока не будут запрошены с помощью метода **Read** класса **DataReader**. Класс **DataReader** позволяет увеличить производительность приложения как путем получения данных, как только они становятся доступны, так и (по умолчанию) путем сохранения в памяти только одной строки за один раз, что снижает нагрузку на системные ресурсы.

Класс <xref:System.Data.Common.DataAdapter> используется для получения данных из источника данных и заполнения таблиц в <xref:System.Data.DataSet>. Класс `DataAdapter` позволяет также решить задачу по возврату изменений, сделанных в объекте `DataSet`, обратно в источник данных. `DataAdapter` использует объект `Connection` поставщика данных Microsoft SqlClient для SQL Server для подключения к источнику данных и использует объекты `Command` для получения данных из источника данных и разрешения изменений в нем.

В .NET есть объекты <xref:System.Data.Common.DbDataReader> и <xref:System.Data.Common.DbDataAdapter>, поставщик данных Microsoft SqlClient для SQL Server включает объекты <xref:Microsoft.Data.SqlClient.SqlDataReader> и <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="in-this-section"></a>В этом разделе

[Извлечение данных с помощью DataReader](retrieve-data-by-datareader.md)  
Содержит описание объекта ADO.NET **DataReader** и способы возвращения потока результатов из источника данных с его помощью.

[Заполнение набора данных DataSet с помощью DataAdapter](populate-dataset-from-dataadapter.md)  
Содержит описание того, как заполнить `DataSet` таблицами, столбцами и строками с использованием `DataAdapter`.

[Параметры DataAdapter](dataadapter-parameters.md)  
Показывает, как использовать параметры со свойствами команды `DataAdapter`, включая то, как сопоставить содержимое столбца в `DataSet` с параметром команды.

[Добавление существующих ограничений к набору данных DataSet](add-existing-constraints-to-dataset.md)  
Показывает, как добавить существующие ограничения к `DataSet`.

[Сопоставления DataAdapter, DataTable и DataColumn](dataadapter-datatable-datacolumn-mappings.md)  
Описывает, как задать `DataTableMappings` и `ColumnMappings` для `DataAdapter`.

[Разбивка на страницы результатов запроса](paging-through-query-result.md)  
Предоставляет пример просмотра результатов запроса в виде страниц данных.

[Обновление источников данных с помощью DataAdapter](update-data-sources-with-dataadapters.md)  
Описывает, как использовать `DataAdapter` для решения задачи записи изменений в `DataSet` обратно в базу данных.

[Обработка событий DataAdapter](handle-dataadapter-events.md)  
Описывает события `DataAdapter` и способы их использования.

[Пакетные операции с использованием объектов DataAdapter](batch-operations-using-dataadapters.md)  
Показывает, как повысить производительность приложения путем уменьшения количества циклов обмена данными с SQL Server в ходе применения обновлений из `DataSet`.

## <a name="see-also"></a>См. также

- [подключение к источнику данных](connecting-to-data-source.md);
- [Команды и параметры](commands-parameters.md)
- [Транзакции и параллельность](transactions-and-concurrency.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
