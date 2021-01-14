---
description: Distributed Replay Requirements
title: Требования распределенного воспроизведения
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 6fffee7d-891f-4d9d-b2c3-dd19855a1c2c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: f03a5bd4c35bc071df184b37e48cd358d62660a6
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170676"
---
# <a name="distributed-replay-requirements"></a>Distributed Replay Requirements
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Для использования компонента распределенного воспроизведения [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо обеспечить выполнение требований, указанных в этой статье.  
  
## <a name="input-trace-requirements"></a>Требования к входным данным трассировки  
 Для успешного воспроизведения данных трассировки они должны соответствовать требованиям к версии и формату и содержать необходимые события и столбцы.  
  
### <a name="input-trace-versions"></a>Версии входных данных трассировки  
 Распределенное воспроизведение поддерживает входные данные трассировки, собранные в следующих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [!INCLUDE[ssSQL15](../../includes/sssqlv14-md.md)], накопительное обновление 1 и более поздних версий. Ознакомьтесь со сведениями о [накопительных обновлениях SQL Server 2017](https://aka.ms/sql2017cu).
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]   
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
-   [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>Форматы входных данных трассировки  
 Входные данные трассировки могут предоставляться в любом из следующих форматов:  
  
-   Отдельный файл трассировки с расширением `.trc` .  
  
-   Набор файлов продолжения трассировки, соответствующих соглашению об именовании для переключения на файл продолжения, например: `<TraceFile>.trc`, `<TraceFile>_1.trc`, `<TraceFile>_2.trc`, `<TraceFile>_3.trc`, … `<TraceFile>_n.trc`.  
  
### <a name="input-trace-events-and-columns"></a>События и столбцы входных данных трассировки  
 Входные данные трассировки должны содержать определенные события и столбцы, которые воспроизводятся компонентами распределенного воспроизведения. Шаблон **TSQL_Replay** в приложении [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] содержит все необходимые события и столбцы, помимо дополнительных сведений. Дополнительные сведения об этом шаблоне см. в разделе [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
> [!WARNING]  
>  Если для записи входных данных трассировки шаблон **TSQL_Replay** не используется или если требования к входным данным трассировки не соблюдены, при воспроизведении могут возникнуть непредвиденные результаты.  
  
 Также можно создать пользовательский шаблон трассировки и использовать его для воспроизведения событий в программе распределенного воспроизведения, если он содержит следующие события:  
  
-   Аудит входа в систему  
  
-   Audit Logout  
  
-   ExistingConnection  
  
-   RPC Output Parameter  
  
-   RPC:Completed  
  
-   RPC:Starting  
  
-   SQL:BatchCompleted  
  
-   SQL:BatchStarting  
  
 При воспроизведении серверных курсоров также необходимы следующие события:  
  
-   CursorClose  
  
-   CursorExecute  
  
-   CursorOpen  
  
-   CursorPrepare  
  
-   CursorUnprepare  
  
 При воспроизведении инструкций SQL, подготовленных на сервере, дополнительно необходимы следующие события:  
  
-   Exec Prepared SQL  
  
-   Prepare SQL  
  
 Все входные данные трассировки должны содержать следующие столбцы:  
  
-   Класс событий  
  
-   EventSequence  
  
-   TextData  
  
-   Имя приложения  
  
-   LoginName  
  
-   имя_базы_данных  
  
-   Идентификатор базы данных  
  
-   HostName  
  
-   Binary Data  
  
-   SPID  
  
-   Время начала  
  
-   EndTime  
  
-   IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>Поддерживаемые сочетания входных данных трассировки и целевых серверов  
 В следующей таблице перечислены поддерживаемые версии данных трассировки и для каждой версии указываются поддерживаемые версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для которых могут воспроизводиться данные.  
  
|Версия входных данных трассировки|Поддерживаемые версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для экземпляра целевого сервера|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>Требования к операционной системе  
 Для запуска средства администрирования, контроллера и клиентских служб поддерживаются те же операционные системы, что и для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения об операционных системах, поддерживаемых экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , см. в статье [Требования к оборудованию и программному обеспечению для установки SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
 Компоненты распределенного воспроизведения поддерживаются в операционных системах как для платформы x86, так и для платформы x64. Для операционных систем платформы x64 поддерживается только режим Windows on Windows (WOW).  
  
## <a name="installation-limitations"></a>Ограничения на установку  
 На одном компьютере можно устанавливать только один экземпляр компонента распределенного воспроизведения. В следующей таблице указано, сколько установленных экземпляров каждого компонента допускается в одной среде распределенного воспроизведения.  
  
|Компонент распределенного воспроизведения|Максимальное число установленных экземпляров для среды воспроизведения|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Служба контроллера распределенного воспроизведения|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Служба клиента распределенного воспроизведения|16 (физических или виртуальных компьютеров)|  
|Средство администрирования|Без ограничений|  
  
> [!NOTE]  
>  Хотя на одном компьютере можно устанавливать только один экземпляр программы администрирования, допускается одновременный запуск нескольких экземпляров программы администрирования. Команды, поступающие от нескольких экземпляров программы администрирования, разрешаются в порядке поступления.  
  
## <a name="data-access-provider"></a>Поставщик данных  
 Распределенное воспроизведение поддерживает только поставщик ODBC доступа к данным собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="target-server-preparation-requirements"></a>Требования к подготовке целевого сервера  
 Рекомендуется размещать целевой сервер в среде тестирования. Чтобы воспроизвести данные трассировки на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , отличном от экземпляра, в котором они были изначально записаны, убедитесь, что для целевого сервера выполняются следующие требования.  
  
-   Все имена входа и пользователи, содержащиеся в данных трассировки, должны присутствовать на целевом сервере в той же базе данных.  
  
-   Все имена входа и пользователи на целевом сервере должны обладать теми же разрешениями, которые были у них на исходном сервере.  
  
-   Желательно, чтобы идентификаторы баз данных на целевом и на исходном серверах совпадали. Впрочем, если они не совпадают, соответствие можно установить по параметру **DatabaseName** , если он есть в трассировке.  
  
-   Для каждого имени входа в трассировке должна быть задана база данных по умолчанию, соответствующая целевой базе данных имени входа. Например, на исходном экземпляре **в базе данных** Fred_Db **данные трассировки для воспроизведения содержат операцию для имени входа** Fred [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Поэтому на целевом сервере необходимо задать базу данных по умолчанию для имени входа **Fred**, соответствующую базе данных **Fred_Db** (даже если имена баз данных различаются). Базу данных по умолчанию для имени входа можно задать с помощью хранимой процедуры `sp_defaultdb` .  
  
 В результате воспроизведения событий, связанных с отсутствующими или неверными именами входа, будут возникать ошибки воспроизведения, но сама операция воспроизведения будет продолжена.  
  
## <a name="see-also"></a>См. также:  
 [Распределенное воспроизведение SQL Server](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Безопасность распределенного воспроизведения](../../tools/distributed-replay/distributed-replay-security.md)   
 [Установка распределенного воспроизведения — обзор](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
  
