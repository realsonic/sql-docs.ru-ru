---
title: Изменение целевого времени восстановления базы данных
description: Узнайте, как задать или изменить целевое время восстановления базы данных SQL Server в SQL Server, с помощью SQL Server Management Studio или Transact-SQL.
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 00c7778583545c56b886bfc30c4763bacd1404b3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171566"
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>Изменение целевого времени восстановления базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается изменение целевого времени восстановления базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. По умолчанию целевое время восстановления — 60, а база данных использует *косвенные контрольные точки*. Значение целевого времени восстановления устанавливает верхнюю границу времени восстановления для этой базы данных.  
  
> [!NOTE]  
>  Верхняя граница, указываемая для отдельной базы данных посредством настройки целевого времени восстановления, может быть превышена из-за долгой транзакции, которая может вызвать чрезмерное время для отмены действий.  
  
-   **Перед началом работы**  [Ограничения](#Restrictions), [Безопасность](#Security)  
  
-   **Изменение целевого времени восстановления с помощью**   [SQL Server Management Studio](#SSMSProcedure) или [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения 
  
> [!CAUTION]  
>  В базе данных, которая настроена на использование косвенных контрольных точек, может снизиться производительность обработки транзакционной нагрузки в режиме «в сети». Косвенные конечные точки сохраняют количество «грязных» страниц ниже определенного порогового значения, чтобы восстановление базы данных выполнялось в течение заданного времени восстановления. Параметр конфигурации интервал восстановления использует количество транзакций для определения времени восстановления вместо косвенных контрольных точек, которые основываются на количестве "грязных" страниц. Если косвенные конечные точки включены в базе данных, получающей большое число операций DML, средство фоновой записи может начать агрессивно сбрасывать «грязные» буферы обмена на диск, чтобы гарантировать, что время, необходимое для выполнения восстановления, находится в пределах целевого периода восстановления базы данных. Это может вызвать дополнительную активность операций ввода-вывода в определенных системах, что способно привести к созданию узких мест с точки зрения производительности, если подсистема диска превысила пороговое значение операций ввода-вывода или приближается к нему.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Изменение целевого времени восстановления**  
  
1.  В **обозревателе объектов** подключитесь к экземпляру [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]и разверните его.  
  
2.  Раскройте контейнер **Базы данных**, щелкните правой кнопкой мыши базу данных, которую необходимо изменить, и выберите пункт **Свойства**.  
  
3.  В диалоговом окне **Свойства базы данных** выберите страницу **Параметры** .  
  
4.  На панели **Восстановление** в поле **Целевое время восстановления (секунды)** укажите количество секунд в качестве верхней границы времени восстановления этой базы данных.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Изменение целевого времени восстановления**  
  
1.  Установите подключение к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , на котором размещена база данных.  
  
2.  Используйте инструкцию [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) следующим образом:  
  
     TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     Начиная с версии [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], значение по умолчанию равно 1 минуте. Если значение больше 0 (значение по умолчанию для более старых версий), то оно указывает значение верхней границы времени восстановления для заданной базы данных в случае сбоя.  
  
     SECONDS  
     Указывает, что значение *target_recovery_time* выражается в количестве секунд.  
  
     MINUTES  
     Указывает, что значение *target_recovery_time* выражается в количестве минут.  
  
     В следующем примере устанавливается время восстановления базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] в `60` секунд.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a>См. также:  
 [Контрольные точки базы данных (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Параметры ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
