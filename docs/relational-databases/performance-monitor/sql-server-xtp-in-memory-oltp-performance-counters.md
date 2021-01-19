---
title: Счетчики производительности XTP (In-Memory OLTP)
description: SQL Server предоставляет объекты и счетчики, которые могут использоваться системным монитором для отслеживания активности In-Memory OLTP.
ms.custom: seo-dt-2019
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: fe3cbaf4-65f4-44c5-acc6-7b735cda0c5d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38d44cc0c916c7e40e26b89196b21012f7b72499
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170716"
---
# <a name="sql-server-xtp-in-memory-oltp-performance-counters"></a>Счетчики производительности XTP (выполняющаяся в памяти OLTP) для SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляет объекты и счетчики, которые могут использоваться системным монитором для отслеживания активности In-Memory OLTP. Объекты и счетчики являются общими для всех экземпляров данной версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на компьютере, начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 В прошлом имена объектов и счетчиков содержали *XTP*, как в **Курсоры XTP**. Начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]имена составляются по следующей схеме:  
  
-   **Курсоры XTP** *\<version>* **в SQL Server**  
  
 Где *\<version>* имеет значение вида "2016".  
  
##  <a name="sql-server-xtp-performance-objects"></a><a name="SQLServerPOs"></a> Объекты производительности XTP в SQL Server  
 В следующей таблице приводятся описания счетчиков производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Объект производительности|Описание|  
|------------------------|-----------------|  
|[Курсоры XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-cursors.md)|Объект производительности XTP Cursors в SQL Server содержит счетчики, относящиеся к внутренним курсорам механизма In-Memory OLTP. Курсоры — это низкоуровневые строительные блоки, используемые механизмом In-Memory OLTP для обработки запросов [!INCLUDE[tsql](../../includes/tsql-md.md)] . Обычно вы не имеете прямого контроля над ними как таковыми.|  
|[Базы данных XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-databases.md)|Объект производительности баз данных XTP в SQL Server содержит счетчики, относящиеся к базе данных для выполняющейся в памяти OLTP.|  
|[Сборка мусора XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-garbage-collection.md)|Объект производительности XTP Garbage Collection в SQL Server содержит счетчики, относящиеся к механизму сборщика мусора XTP в In-Memory OLTP.|  
|[Регулятор ввода-вывода XTP в SQL Server 2016](../../relational-databases/performance-monitor/sql-server-xtp-io-governor.md)|Объект производительности регулятора ввода-вывода XTP в SQL Server содержит счетчики, относящиеся к регулятору скорости ввода-вывода для выполняющейся в памяти OLTP.|
|[Обработчик фантомных строк XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-phantom-processor.md)|Объект производительности SQL Server XTP Phantom Processor содержит счетчики, относящиеся к подсистеме обработки фантомов механизма In-Memory OLTP. Этот компонент отвечает за обнаружение фантомных строк в транзакциях, выполняемых на уровне изоляции SERIALIZABLE.|  
|[Хранилище XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-storage.md)|Объект производительности хранилища XTP в SQL Server содержит счетчики, относящиеся к хранилищу In-Memory OLTP в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Журнал транзакций XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transaction-log.md)|Объект производительности транзакций XTP SQL Server содержит счетчики, связанные с журналом транзакций In-Memory OLTP в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Транзакции XTP в SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transactions.md)|Объект производительности транзакций XTP в SQL Server содержит счетчики, относящиеся к транзакциям модуля In-Memory OLTP в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
  
