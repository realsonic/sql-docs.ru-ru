---
description: MSSQLSERVER_3988
title: MSSQLSERVER_3988
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3988 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 46070023b397f949971cf35a7c6ced936fc91e6d
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797809"
---
# <a name="mssqlserver_3988"></a>MSSQLSERVER_3988
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|3988|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|XACT_UNSUPPORT_PARALLEL_TRAN2|
|Текст сообщения|Новая транзакция не допускается, так как в данном сеансе запущены другие потоки.|
||

## <a name="explanation"></a>Пояснение

Эта ошибка возникает при выполнении распределенного запроса, который соединяет несколько таблиц, размещенных на удаленных экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и если параметр сеанса `XACT_ABORT` имеет значение **ON**. Пользователю выводится сообщение об ошибке наподобие следующего:

> Сообщение 3988, уровень 16, состояние 1, строка  
Новая транзакция не допускается, так как в данном сеансе запущены другие потоки.

## <a name="cause"></a>Причина

При следующих условиях проявляются некоторые ограничения того, как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обрабатывает распределенные запросы:

- SQL Server соединяет несколько таблиц одного удаленного источника данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Сеанс, выдающий запрос, не указан в распределенной транзакции.

В этом случае попытка выполнить запрос может вызвать одну из двух ошибок, упомянутых в разделе **Пояснения**.

## <a name="user-action"></a>Рекомендуемые действия

Чтобы устранить эту ошибку, заключите распределенный запрос в инструкцию начала распределенной транзакции:

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
