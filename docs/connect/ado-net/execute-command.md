---
title: Выполнение команды
description: Описание объекта `Command` поставщика данных Microsoft SqlClient для SQL Server и способа его использования для выполнения запросов и команд в отношении источника данных.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 772ecef470b69a34e4bd4b0eba2ca60255657eaf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771427"
---
# <a name="executing-a-command"></a>Выполнение команды

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Поставщик данных Microsoft SqlClient для SQL Server содержит объект <xref:Microsoft.Data.SqlClient.SqlCommand>, наследуемый от <xref:System.Data.Common.DbCommand>. Этот объект предоставляет методы для выполнения команд на основе типа команды и требуемого возвращаемого значения, как описано в следующей таблице.

|Команда|Возвращаемое значение|  
|-------------|------------------|  
|`ExecuteReader`|Возвращает объект `DataReader`.|  
|`ExecuteScalar`|Возвращает одно скалярное значение.|  
|`ExecuteNonQuery`|Выполняет команду, которая не возвращает строк.|  
|`ExecuteXMLReader`|Возвращает значение типа <xref:System.Xml.XmlReader>. Этот метод предусмотрен только для объекта `SqlCommand`.|

Каждый строго типизированный объект команды поддерживает также перечисление <xref:System.Data.CommandType>, которое указывает способ интерпретации строки команды, как описано в следующей таблице.

|CommandType|Описание|
|-----------------|-----------------|  
|`Text`|Команда SQL, определяющая инструкции, которые выполняются применительно к источнику данных.|  
|`StoredProcedure`|Имя хранимой процедуры. Свойство команды `Parameters` может использоваться для доступа к входным и выходным параметрам и возвращаемым значениям независимо от вызываемого метода `Execute`.|  
|`TableDirect`|Имя таблицы.|

> [!IMPORTANT]
> При использовании `ExecuteReader` возвращаемые значения и выходные параметры будут недоступными, пока не будет закрыт `DataReader`.

## <a name="example"></a>Пример

Следующий пример кода демонстрирует способ создания объекта <xref:Microsoft.Data.SqlClient.SqlCommand> для выполнения хранимой процедуры путем установки его свойств. Объект <xref:Microsoft.Data.SqlClient.SqlParameter> используется для задания входных параметров хранимой процедуры. Команда выполняется с помощью метода <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>, и выходное значение из <xref:Microsoft.Data.SqlClient.SqlDataReader> отображается в окне консоли.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>Устранение неполадок с командами

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Поставщик данных Microsoft SqlClient для SQL Server добавляет **счетчики производительности**, позволяющие обнаружить временные проблемы, связанные с невыполненными командами. Дополнительные сведения см. в статье [Счетчики производительности в SqlClient](performance-counters.md).

## <a name="see-also"></a>См. также

- [Команды и параметры](commands-parameters.md)
- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
