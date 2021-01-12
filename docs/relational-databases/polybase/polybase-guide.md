---
title: Что такое PolyBase? | Документы Майкрософт
description: PolyBase позволяет экземпляру SQL Server обрабатывать запросы Transact-SQL, которые считывают данные из внешних источников, таких как Hadoop и хранилище BLOB-объектов Azure.
ms.date: 12/14/2019
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
ms.custom: contperf-fy21q2
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: afcc848a169ec6a9c9dd02ecee50718d7e1508c1
ms.sourcegitcommit: 81f06a754fcaf4b72136859ccb18c82693f24780
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811539"
---
# <a name="what-is-polybase"></a>Что такое PolyBase?

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

PolyBase позволяет экземпляру SQL Server обрабатывать запросы Transact-SQL, которые считывают данные из внешних источников. Этот запрос можно также использовать для доступа к реляционным таблицам в экземпляре SQL Server. PolyBase также позволяет применять этот запрос для объединения данных из внешних источников и SQL Server.

Чтобы использовать PolyBase, в экземпляре SQL Server сделайте следующее:

1. [Установка PolyBase на компьютере по управлением Windows](polybase-installation.md)
1. создайте [внешний источник данных](../../t-sql/statements/create-external-data-source-transact-sql.md);
1. создайте [внешнюю таблицу](../../t-sql/statements/create-external-table-transact-sql.md).

Все это позволит подключиться к внешнему источнику данных.

В SQL Server 2016 представлена функция PolyBase с поддержкой подключений к Hadoop и Хранилищу BLOB-объектов Azure.

В SQL Server 2019 доступны дополнительные соединители, в том числе для SQL Server, Oracle, Teradata и MongoDB.

![Логика PolyBase](../../relational-databases/polybase/media/polybase-logical.png "Логика PolyBase")

PolyBase отправляет некоторые вычисления во внешний источник, чтобы оптимизировать запрос в целом. PolyBase обеспечивает внешний доступ не только к Hadoop. Поддерживаются и другие неструктурированные нереляционные таблицы, например текстовые файлы с разделителями.

В числе внешних соединителей PolyBase:

- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)

### <a name="supported-sql-products-and-services"></a>Поддерживаемые продукты и службы SQL

PolyBase предоставляет одинаковые функции для следующих продуктов SQL от корпорации Майкрософт:

- SQL Server 2016 и более поздние версии (только Windows);
- Analytics Platform System (прежнее название — Parallel Data Warehouse);
- Azure Synapse Analytics

### <a name="azure-integration"></a>Интеграция с Azure

Запросы T-SQL на основе PolyBase также можно использовать для импорта данных из хранилища BLOB-объектов Azure и экспорта данных в него. Кроме того, PolyBase позволяет Azure Synapse Analytics импортировать данные из Хранилища BLOB-объектов Azure и Azure Data Lake Store, а также экспортировать в них данные.

## <a name="why-use-polybase"></a>Зачем нужна технология PolyBase

PolyBase позволяет объединять данные из экземпляра SQL Server с внешними данными. Прежде чем PolyBase объединит данные во внешних источниках, можно выполнить одно из следующих действий:

- передать часть данных, чтобы все они находились в одном месте;
- запросить данные из двух источников, а затем написать пользовательскую логику запроса для объединения и интеграции данных на уровне клиента.

PolyBase позволяет легко объединять данные, используя Transact-SQL.

PolyBase не требует установки дополнительного программного обеспечения в среде Hadoop. При запросе внешних данных используется такой же синтаксис T-SQL, как и при запросе таблицы базы данных. Все вспомогательные действия, реализуемые PolyBase, выполняются прозрачно. Автору запроса не требуется знать, как работает внешний источник.

### <a name="polybase-uses"></a>Варианты использования PolyBase

PolyBase поддерживает перечисленные ниже сценарии в SQL Server.

- **Запрашивание данных, хранящихся в Hadoop, из экземпляра SQL Server или PDW.** Пользователи хранят данные в более экономичных распределенных и масштабируемых системах, таких как Hadoop. PolyBase позволяет легко запрашивать данные с помощью T-SQL.

- **Запрашивать данные, хранящиеся в хранилище BLOB-объектов Azure.** Данные для использования службами Azure удобно хранить в хранилище BLOB-объектов Azure.  PolyBase позволяет легко обращаться к данным с помощью T-SQL.

- **Импорт данных из Hadoop, хранилища BLOB-объектов Azure или Azure Data Lake Store.** Используйте скорость технологии Microsoft SQL columnstore и возможности анализа, импортируя данные из Hadoop, хранилища BLOB-объектов Azure или Azure Data Lake Store в реляционные таблицы. Нет необходимости в отдельном средстве ETL или импорта.

- **Экспортировать данные в Hadoop, хранилище BLOB-объектов Azure или Azure Data Lake Store.** Архивация данных в Hadoop, хранилище BLOB-объектов Azure или Azure Data Lake Store позволяет создать экономичное хранилище и обеспечить его подключение к сети для удобного доступа к данным.

- **Интегрироваться со средствами бизнес-аналитики.** Можно использовать PolyBase со средствами бизнес-аналитики и стеком технологий анализа Майкрософт, а также применять любые сторонние средства, совместимые с SQL Server.

## <a name="performance"></a>Производительность

- **Принудительная отправка вычислений в Hadoop.** Оптимизатор запросов принимает решение принудительно отправить вычисления в Hadoop, если это улучшит производительность запросов.  Для принятия такого решения оптимизатор запросов использует статистику из внешних таблиц. При включении вычислений создаются задания MapReduce и применяются распределенные вычислительные ресурсы Hadoop.

- **Масштабирование вычислительных ресурсов.** Для повышения производительности запросов можно использовать [группы горизонтального масштабирования PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)в SQL Server. Это обеспечивает параллельную передачу данных между экземплярами SQL Server и узлами Hadoop, а также добавляет вычислительные ресурсы для работы с внешними данными.

## <a name="next-steps"></a>Дальнейшие действия

Прежде чем использовать PolyBase, необходимо [установить компонент PolyBase](polybase-installation.md). Затем ознакомьтесь со следующими руководствами по настройке в зависимости от источника данных:

- [Hadoop](polybase-configure-hadoop.md)
- [Хранилище BLOB-объектов Azure](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Универсальные типы ODBC](polybase-configure-odbc-generic.md)