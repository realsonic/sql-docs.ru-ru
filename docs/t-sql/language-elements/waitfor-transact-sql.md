---
description: WAITFOR (Transact-SQL)
title: WAITFOR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0933c90f711f5c86425c7e2c6b4b350c47612ddc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095878"
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Блокирует выполнение пакета, хранимой процедуры или транзакции, пока не наступит указанное время, не истечет заданный интервал времени либо заданная инструкция не изменит или не возвратит по крайней мере одну строку.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 DELAY  
 Заданный период времени (не более 24 часов), который должен пройти до выполнения пакета, хранимой процедуры или продолжения транзакции.  
  
 '*time_to_pass*'  
 Период времени ожидания. Аргумент *time_to_pass* может быть задан в формате данных типа **datetime** либо в виде локальной переменной. Указать дату невозможно, поэтому элемент даты в значении **datetime** недопустим. Аргумент *time_to_pass* имеет формат чч:мм[[:сс].мсс].
  
 TIME  
 Заданное время выполнения пакета, хранимой процедуры или транзакции.  
  
 '*time_to_execute*'  
 Время, в которое инструкция WAITFOR завершает работу. Аргумент *time_to_execute* может быть задан в формате данных типа **datetime** или в виде локальной переменной. Указать дату невозможно, поэтому элемент даты в значении **datetime** недопустим. Аргумент *time_to_execute* имеет формат чч:мм[[:сс].мсс] и при необходимости может включать дату 1900-01-01.
  
 *receive_statement*  
 Допустимая инструкция RECEIVE.  
  
> [!IMPORTANT]  
>  Инструкция WAITFOR с аргументом *receive_statement* применима только к сообщениям компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Дополнительные сведения см. в разделе [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md).  
  
 *get_conversation_group_statement*  
 Допустимая инструкция GET CONVERSATION GROUP.  
  
> [!IMPORTANT]  
>  Инструкция WAITFOR с аргументом *get_conversation_group_statement* применима только к сообщениям компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Дополнительные сведения см. в разделе [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
 TIMEOUT *timeout*  
 Указывает период времени ожидания очередного сообщения (в миллисекундах).  
  
> [!IMPORTANT]  
>  Инструкция WAITFOR с аргументом TIMEOUT применима только к сообщениям компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Дополнительные сведения см. в разделах [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md) и [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
 Во время выполнения инструкции WAITFOR выполняется транзакция, и другие запросы не могут быть выполнены в рамках этой транзакции.  
  
 Фактическая временная задержка может отличаться от времени, указанного в аргументах *time_to_pass*, *time_to_execute* или *timeout*, и зависит от уровня активности сервера. Счетчик времени запускается, когда запланирован поток инструкции WAITFOR. Если сервер занят, запланировать поток сразу может быть невозможно, поэтому время задержки может оказаться больше заданного.  
  
 Инструкция WAITFOR не изменяет семантику запроса. Если запрос не может возвратить строки, инструкция WAITFOR будет ждать неограниченное время или до достижения значения TIMEOUT, если оно задано.  
  
 Для инструкций WAITFOR невозможно открыть курсоры.  
  
 Для инструкций WAITFOR невозможно указать представления.  
  
 Если запрос превышает значение, заданное аргументом query  wait, параметр инструкции WAITFOR может завершиться без выполнения. Дополнительные сведения см. в статье [Настройка параметра конфигурации сервера query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Чтобы просмотреть активные и ожидающие процессы, используйте процедуру [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md).  
  
 Каждая инструкция WAITFOR имеет связанный с ней поток. Если на одном сервере задано несколько инструкций WAITFOR, то можно объединить несколько потоков, ожидающих выполнения этих инструкций. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отслеживает число потоков инструкций WAITFOR и случайным образом завершает работу некоторых из этих потоков, если на сервере начинает не хватать для них ресурсов.  
  
 Если запустить запрос с инструкцией WAITFOR внутри транзакции, которая сама блокирует изменение того же набора строк, к которому обращается инструкция, может возникнуть взаимоблокировка. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] идентифицирует эти сценарии и возвращает пустой результирующий набор, если есть вероятность такой взаимоблокировки.  
  
> [!CAUTION]  
>  Использование указания WAITFOR приведет к замедлению выполнения процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], в результате чего в приложении может открыться сообщение об истечении времени ожидания. При необходимости отрегулируйте значение времени ожидания для соединения на уровне приложения.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-waitfor-time"></a>A. Использование инструкции WAITFOR TIME  
 В следующем примере выполняется хранимая процедура `sp_update_job` в базе данных msdb в 22:20. (`22:20`).  
  
```sql  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>Б. Использование WAITFOR DELAY  
 В следующем примере хранимая процедура выполняется после 2-часовой задержки.  
  
```sql  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>В. Использование WAITFOR DELAY с локальной переменной  
 Следующий пример показывает, как можно использовать локальную переменную с параметром `WAITFOR DELAY`. Хранимая процедура ожидает в течение периода времени, заданного переменной, а затем возвращает пользователю данные о том, сколько прошло часов, минут и секунд.  
  
```sql  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo VARCHAR(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a>См. также  
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
