---
title: Сохранение результатов трассировки в таблице
titleSuffix: SQL Server Profiler
description: Узнайте, как использовать SQL Server Profiler для сохранения результатов трассировки в таблицу в базе данных SQL Server. Узнайте, как можно указать максимальное число строк для сохранения.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: edbecf74-683b-4e43-a1ef-7a3d5f5e27f6
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 9e1dbe5def1065874cf840245bfaa3f2676af042
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171596"
---
# <a name="save-trace-results-to-a-table-sql-server-profiler"></a>сохранять результаты трассировки в таблицу (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этом подразделе описывается, как сохранять результаты трассировок в таблицу базы данных с помощью приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
## <a name="to-save-trace-results-to-a-table"></a>Сохранение результатов трассировки в таблицу
  
1.  В меню **Файл** выберите пункт **Создать трассировку**, а затем подключитесь к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Отображается диалоговое окно **Свойства трассировки**.  
  
    > [!NOTE]  
    >  Если установлен параметр **Начать трассировку немедленно после установления соединения**, диалоговое окно **Свойства трассировки** не отображается, а вместо этого начинается трассировка. Чтобы отключить этот параметр, в меню **Сервис** выберите пункт **Параметры** и снимите флажок **Начать трассировку немедленно после установления соединения** .  
  
2.  В поле **Имя трассировки** введите имя трассировки и нажмите кнопку **Сохранить в таблицу**.  
  
3.  В диалоговом окне **Соединение с сервером** подключитесь к содержащей таблицу трассировок базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
4.  В диалоговом окне **Целевая таблица** выберите базу данных из списка **База данных**.  
  
5.  В списке **Владелец** выберите владельца трассировки.  
  
6.  В списке **Таблица** введите или выберите имя таблицы для сохранения результатов трассировки. Нажмите кнопку **ОК.**  
  
7.  В диалоговом окне **Свойства трассировки** установите флажок **Set maximum rows (in thousands)** (Максимальное число строк (тыс.)), чтобы задать максимальное число строк для сохранения.  
  
## <a name="see-also"></a>См. также:  
 [Приложение SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
