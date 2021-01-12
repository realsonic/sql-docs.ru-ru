---
title: Жизненный цикл поддержки драйвера SqlClient
description: Страница с информацией о жизненном цикле поддержки продукта.
ms.date: 01/04/2020
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 200e600448f1f93cca3d566be10d6065ffd537e7
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878587"
---
# <a name="sqlclient-driver-support-lifecycle"></a>Жизненный цикл поддержки драйвера SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Библиотека Microsoft.Data.SqlClient соответствует последней версии политики поддержки .NET Core для всех выпусков.

[Просмотреть политику поддержки .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="microsoftdatasqlclient-release-cadence"></a>Частота выпусков Microsoft.Data.SqlClient

Новые стабильные (общедоступные) выпуски публикуются каждые шесть месяцев с обычной частотой, начиная с версии 1.2, а между ними выходят по 2–3 предварительных выпуска. Выпуски долгосрочной поддержки (LTS) будут отбираться заинтересованными лицами и специалистами по поддержке на основе нескольких характеристик и отзывов клиентов.

### <a name="actively-supported-releases"></a>Поддерживаемые выпуски

| Версия | Дата официального выпуска | Последняя версия исправления | Дата выпуска исправления | Уровень поддержки  | Окончание поддержки |
| -- | -- | -- | -- | -- | -- |
| 2.1 | 19 ноября 2020 г. | 2.1.1 | 18 декабря 2020 г. | Текущий | |
| 2.0 | 16 июня 2020 г. | 2.0.1 | 25 августа 2020 г. | Текущий | 19 февраля 2021 г. |
| 1.1 | 20 ноября 2019 г. | 1.1.3 | 15 мая 2020 г. | LTS | 21 ноября 2022 г. |

### <a name="out-of-support-releases"></a>Неподдерживаемые выпуски

| Версия | Дата выпуска последнего исправления | Последняя версия исправления | Дата прекращения поддержки |
| -- | -- | -- | -- |
| 1.0 | 26 сентября 2019 г. | 1.0.19269.1 | 20 февраля 2020 г. |

### <a name="long-term-support-lts-releases"></a>Выпуски с долгосрочной поддержкой (LTS)

Выпуски долгосрочной поддержки поддерживаются в течение трех лет после первоначального выпуска.

### <a name="current-releases"></a>Текущие выпуски

Текущие выпуски поддерживаются в течение трех месяцев после следующего текущего выпуска или выпуска долгосрочной поддержки.

## <a name="sql-version-compatibility-with-microsoftdatasqlclient"></a>Совместимость версий SQL с Microsoft.Data.SqlClient

|Версия базы данных&nbsp;&#8594;<br />&#8595; Версия драйвера|База данных SQL Azure|Azure Synapse Analytics|Управляемый экземпляр SQL Azure|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|
|---|---|---|---|---|---|---|---|---|
|2.1|Да|Да|Да|Да|Да|Да|Да|Да|
|2.0|Да|Да|Да|Да|Да|Да|Да|Да|
|1,1|Да|Да|Да|Да|Да|Да|Да|Да|
|1.0|Да|Да|Да|Да|Да|Да|Да|Да|

## <a name="supported-os-versions"></a>Поддерживаемые версии ОС

### <a name="support-for-net-framework-applications"></a>Поддержка приложений .NET Framework

Microsoft.Data.SqlClient поддерживает все операционные системы, поддерживаемые .NET Framework версии 4.6 и выше.

[.NET Framework, требования к системе](/dotnet/framework/get-started/system-requirements)

### <a name="support-for-net-core-applications"></a>Поддержка приложений .NET Core

Microsoft.Data.SqlClient поддерживает все операционные системы, поддерживаемые .NET Core версии 2.1 и выше.

[Политика жизненного цикла ОС, поддерживаемая .NET Core](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md)

> [!NOTE]
> В настоящее время инвариантный режим глобализации не поддерживается.
