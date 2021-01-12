---
title: Извлечение и изменение данных
description: В .NET поставщик данных Microsoft SqlClient для SQL Server выступает в качестве моста между приложением и источником данных для считывания и обновления данных.
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771560"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Извлечение и изменение данных в ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Основной функцией любого приложения базы данных является соединение с источником данных и извлечение данных, которые он содержит. Поставщик данных SqlClient обеспечивает взаимодействие между приложением и источником данных, позволяя выполнять команды и получать данные с помощью **DataReader** или **DataAdapter**. Ключевой функцией любого приложения базы данных является возможность обновления данных, хранимых в базе данных. В поставщике данных Microsoft SqlClient для SQL Server обновление данных включает использование **DataAdapter** и <xref:System.Data.DataSet>, а также объектов **Command**. Обновление может включать использование транзакций.

## <a name="in-this-section"></a>В этом разделе

[подключение к источнику данных](connecting-to-data-source.md);  
Описывается установка подключения к источнику данных и работа с событиями подключения.

[Строки подключения](connection-strings.md)  
Содержит разделы, в которых описываются различные аспекты использования строк подключения, в том числе ключевых слов строки подключения, сведения о безопасности, их хранение и извлечение.

[Организация пулов соединений](connection-pooling.md)  
Описание объединения подключений в пулы для поставщика данных Microsoft SqlClient для SQL Server.

[Команды и параметры](commands-parameters.md)  
Содержит разделы, в которых описывается создание команд и построителей команд, настройка параметров и выполнение команд для извлечения и изменения данных.

[Объекты DataAdapter и DataReader](dataadapters-datareaders.md)  
Содержит разделы, в которых описываются объекты DataReader, DataAdapter, параметры, обработка событий объекта и выполнение пакетных операций.

[Транзакции и параллельность](transactions-and-concurrency.md)  
Содержит разделы, в которых описывается выполнение локальных транзакций, распределенных транзакций и работа с оптимистичным параллелизмом.

[Извлечение сведений о схеме базы данных](retrieving-database-schema-information.md)  
Описывает получение доступных баз данных или каталогов, таблиц и представлений базы данных, существующих ограничений для таблиц и других сведений о схеме из источника данных.

[DbProviderFactories](dbproviderfactories.md)  
Описывается модель фабрики поставщика и демонстрируется использование базовых классов в пространстве имен `System.Data.Common`.  

[Извлечение значений идентификаторов или автонумерации](retrieve-identity-or-autonumber-values.md)  
Содержит пример сопоставления значений, созданных для столбца **identity** таблицы SQL Server, для столбца строки, вставленной в таблицу. Рассматривается слияние значений идентификаторов в объекте `DataTable`.  
  
[Извлечение двоичных данных](retrieve-binary-data.md)  
Описание извлечения двоичных данных или крупных структур данных с помощью метода `CommandBehavior`.`SequentialAccess` для изменения поведения по умолчанию объекта `DataReader`.  
  
[Изменение данных с помощью хранимых процедур](modify-data-with-stored-procedures.md)  
Описывается использование входных и выходных параметров хранимой процедуры для вставки строки в базу данных с возвратом нового значения идентификатора.  

[Трассировка данных в SqlClient](data-tracing.md)  
Описание того, как поставщик данных Microsoft SqlClient для SQL Server предоставляет встроенные функции трассировки данных.  
  
[Счетчики производительности в SqlClient](performance-counters.md)  
Описание счетчиков производительности, доступных для поставщика данных Microsoft SqlClient для SQL Server.  
  
[Асинхронное программирование](asynchronous-programming.md)  
Описание поддержки поставщика данных Microsoft SqlClient для SQL Server для асинхронного программирования.  
  
[Поддержка потоковой передачи в SqlClient](sqlclient-streaming-support.md)  
Описывает, как создавать приложения, которые выполняют потоковую передачу данных из SQL Server без их полной загрузки в память.  

## <a name="see-also"></a>См. также

- [Сопоставления типов данных в ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server и ADO.NET](./sql/index.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
