---
title: Программа ssbdiagnose (компонент Service Broker)
description: Программа ssbdiagnose сообщает о проблемах в диалогах Service Broker или в конфигурации служб Service Broker.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 8590f3677d27c367782999877cb6892251785278
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641309"
---
# <a name="ssbdiagnose-utility-service-broker"></a>Программа ssbdiagnose (компонент Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Программа **ssbdiagnose** сообщает о проблемах в диалогах [!INCLUDE[ssSB](../../includes/sssb-md.md)] или в конфигурации службы [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Проверка конфигурации может быть выполнена для одной или для двух служб. Сведения о неполадках могут выводиться в окне командной строки в виде удобочитаемого текста или в формате XML, который может быть перенаправлен в файл или в другую программу.

## <a name="syntax"></a>Синтаксис  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>Параметры командной строки  
 **-XML**  
 Указывает, что вывод **ssbdiagnose** будет создан как форматированный XML, который может быть перенаправлен в файл или в другое приложение. Если параметр **-XML** не указан, вывод **ssbdiagnose** форматируется в виде удобочитаемого (для человека) текста.  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 Определяет, какие типы сообщений включаются в отчет.  
  
 **ERROR**: только сообщения об ошибках.  
  
 **WARNING**: сообщения об ошибках и предупреждения.  
  
 **INFO**: ошибки, предупреждения и информационные сообщения.  
  
 Значение по умолчанию — **WARNING**.  
  
 **-IGNORE** _идентификатор_ошибки_  
 Указывает, что ошибки и сообщения, имеющие заданное значение *error_id* , не будут включены в отчет. Параметр **-IGNORE** можно указать несколько раз, чтобы запретить вывод сообщений с различными идентификаторами.  
  
 **\<baseconnectionoptions>**  
 Задает основные сведения о соединении, которые используются программой **ssbdiagnose** , если параметры соединения не включены в определенное предложение. Сведения о соединении, заданные в специальном предложении, переопределяют сведения **baseconnectionoption** . Приоритеты учитываются по каждому параметру отдельно. Например, если в **baseconnetionoptions** указываются оба параметра **-S** и **-d**, а в **toconnetionoptions** указывается только **-d**, то **ssbdiagnose** использует -S из **baseconnetionoptions** и -d из **toconnetionoptions**.  
  
 **CONFIGURATION**  
 Запрашивает вывод отчета об ошибках конфигурации для одной службы или для пары служб компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
 **FROM SERVICE** _имя_службы_  
 Указывает службу, которая является инициатором диалога.  
  
 **\<fromconnectionoptions>**  
 Задает сведения, необходимые для соединения с базой данных, в которой находится вызывающая служба. Если параметры **fromconnectionoptions** не указаны, то **ssbdiagnose** использует для подключения к базе данных инициатора сведения о соединении из **baseconnectionoptions** . Если параметры **fromconnectionoptions** задаются, то в них должна быть указана база данных, содержащая службу инициатора. Если параметры **fromconnectionoptions** не указываются, в **baseconnectionoptions** необходимо указать базу данных инициатора.  
  
 **TO SERVICE** _имя_службы_[, *идентификатор\_брокера* ]  
 Указывает целевую службу для диалогов.  
  
 *service_name*: задает имя целевой службы.  
  
 *broker_id*: задает идентификатор [!INCLUDE[ssSB](../../includes/sssb-md.md)] , определяющий целевую базу данных. *broker_id* — идентификатор GUID. Чтобы выяснить этот идентификатор, можно выполнить в целевой базе данных следующий запрос.  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 Задает сведения, необходимые для соединения с базой данных, где размещается целевая служба. Если параметры **toconnectionoptions** не указаны, то **ssbdiagnose** использует для подключения к целевой базе данных сведения о соединении из **baseconnectionoptions** .  
  
 **MIRROR**  
 Указывает, что связанная служба [!INCLUDE[ssSB](../../includes/sssb-md.md)] размещается в зеркальной базе данных. Программа **ssbdiagnose** проверяет, является ли маршрут к службе зеркальным маршрутом, где в инструкции CREATE ROUTE был указан параметр MIRROR_ADDRESS.  
  
 **\<mirrorconnectionoptions>**  
 Позволяет задать сведения, необходимые для соединения с зеркальной базой данных. Если параметры **mirrorconnectionoptions** не указаны, то **ssbdiagnose** использует для подключения к зеркальной базе данных сведения о соединении из **baseconnectionoptions** .  
  
 **ON CONTRACT** _имя_контракта_  
 Требует, чтобы программа **ssbdiagnose** проверила только те конфигурации, в которых используется указанный контракт. Если параметр ON CONTRACT не указан, то программа **ssbdiagnose** работает с контрактом DEFAULT.  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 Запрашивает проверку правильности настройки диалога для заданного уровня шифрования.  
  
 **ON**: Параметр по умолчанию. Настраивается полная защита диалога. На обеих сторонах диалога производится развертывание сертификатов, присутствует привязка удаленной службы, а в инструкции GRANT SEND для целевой службы указывается вызывающий пользователь.  
  
 **OFF**: защита диалога не настраивается. Развертывание сертификатов не выполняется, привязка удаленной службы не была создана, и в инструкции GRANT SEND для службы инициатора была указана роль **public** .  
  
 **ANONYMOUS**: защита диалога для анонимной работы настраивается. Один сертификат развернут, привязка удаленной службы указала анонимное предложение, а в инструкции GRANT SEND для целевой службы была указана роль **public** .  
  
 **RUNTIME**  
 Запрашивает отчет о проблемах, вызывающих ошибки времени выполнения в диалоге компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Если не указан ни параметр **-NEW** , ни параметр **-ID** , то программа **ssbdiagnose** наблюдает за всеми диалогами во всех базах данных, указанных в параметрах соединения. Если указан параметр **-NEW** или **-ID** , то программа **ssbdiagnose** создает список идентификаторов, указанных в параметрах.  
  
 Во время работы программа **ssbdiagnose** записывает все события [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , которые указывают на ошибки времени выполнения. Регистрируются события, происходящие для заданных идентификаторов, а также события системного уровня. При обнаружении ошибки времени выполнения программа **ssbdiagnose** запускает отчет по связанной конфигурации.  
  
 По умолчанию в ошибки времени выполнения в выходной отчет не включаются. Отчет содержит только результаты анализа конфигурации. Чтобы включить в отчет ошибки времени выполнения, укажите параметр **-SHOWEVENTS** .  
  
 **-SHOWEVENTS**  
 Указывает, что при создании отчета RUNTIME программа **ssbdiagnose** включает в него события [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . В отчет включаются только те события, которые считаются ошибками. По умолчанию программа **ssbdiagnose** только наблюдает за событиями ошибок, но не включает их в выходной отчет.  
  
 **-NEW**  
 Запрашивает наблюдение за первым диалогом, который запускается после начала работы программы **ssbdiagnose** .  
  
 **-ID**  
 Запрашивает мониторинг выполнения указанных элементов диалога. Параметр **-ID** можно указывать несколько раз.  
  
 Если указан дескриптор диалога, то в отчет будут включены только те события, которые связаны с соответствующей конечной точкой диалога. Если указан идентификатор диалога, то в отчет будут включены все события для этого диалога и его вызывающей и целевой конечных точек. Если задан идентификатор группы сообщений, то в отчет будут включены все события для всех диалогов и конечных точек в этой группе сообщений.  
  
 *conversation_handle*  
 Уникальный идентификатор, определяющий конечную точку диалога в приложении. Дескрипторы диалога уникальны для одной конечной точки диалога. Вызывающая и целевая конечные точки имеют различные дескрипторы диалога.  
  
 Дескрипторы диалога возвращаются в приложения с помощью параметра *\@dialog_handle* инструкции **BEGIN DIALOG**, а также в столбце **conversation_handle** результирующего набора инструкции **RECEIVE**.  
  
 Дескрипторы диалогов отображаются в столбце **conversation_handle** представлений каталога **sys.transmission_queue** и **sys.conversation_endpoints** .  
  
 *conversation_group_id*  
 Уникальный идентификатор, определяющий группу сообщений.  
  
 Идентификаторы групп диалогов возвращаются в приложения с помощью параметра *\@conversation_group_id* инструкции **GET CONVERSATION GROUP**, а также в столбце **conversation_group_id** результирующего набора инструкции **RECEIVE**.  
  
 Идентификаторы групп диалогов отображаются в столбцах **conversation_group_id** представлений каталога **sys.conversation_groups** и **sys.conversation_endpoints** .  
  
 *conversation_id*  
 Уникальный идентификатор, определяющий диалог. Для вызывающей и целевой конечных точек диалога идентификаторы диалога совпадают.  
  
 Идентификаторы диалогов отображаются в столбце **conversation_id** представления каталога **sys.conversation_endpoints** .  
  
 **-TIMEOUT** _интервал_времени_ожидания_  
 Задает время выполнения отчета **RUNTIME** в секундах. Если параметр **-TIMEOUT** не задан, то отчет может выполняться бесконечно долго. Параметр **-TIMEOUT** используется только для отчетов **RUNTIME** , а не для отчетов **CONFIGURATION** . Работу программы **ssbdiagnose** можно завершить нажатием клавиш CTRL+C, если параметр **-TIMEOUT** не указан, а также если нужно завершить отчет до истечения времени ожидания **-** . Параметр *timeout_interval* должен быть числом от 1 до 2 147 483 647.  
  
 **\<runtimeconnectionoptions>**  
 Задает сведения для соединения с базой данных, где содержатся службы, связанные с отслеживаемыми элементами диалога. Если все службы расположены в одной базе данных, нужно указать только одно предложение **CONNECT TO** . Если службы находятся в разных базах данных, то предложение **CONNECT TO** необходимо указать для каждой базы данных. Если параметры **runtimeconnectionoptions** не указаны, то **ssbdiagnose** использует сведения о подключении из **baseconnectionoptions**.  
  
 **-E**  
 Откройте соединение с экземпляром компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , использующее проверку подлинности Windows, указав текущую учетную запись Windows в качестве идентификатора входа. Имя входа должно быть членом предопределенной роли сервера **sysadmin** .  
  
 Параметр -E не учитывает имя пользователя и пароль, заданные в переменных среды SQLCMDUSER и SQLCMDPASSWORD.  
  
 Если не указан ни параметр **-E** , ни параметр **-U** , то программа **ssbdiagnose** использует значение из переменной среды SQLCMDUSER. Если переменная SQLCMDUSER также не задана, то программа **ssbdiagnose** использует проверку подлинности Windows.  
  
 Если параметр **-E** используется в сочетании с параметром **-U** или **-P** , выдается сообщение об ошибке.  
  
 **-U** _идентификатор_входа_  
 Откройте соединение, использующее проверку подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , с помощью указанного идентификатора входа. Имя входа должно быть членом предопределенной роли сервера **sysadmin** .  
  
 Если не указан ни параметр **-E** , ни параметр **-U** , то программа **ssbdiagnose** использует значение из переменной среды SQLCMDUSER. Если переменная SQLCMDUSER также не задана, то программа **ssbdiagnose** пытается установить соединение в режиме проверки подлинности Windows на основании учетных данных Windows пользователя, запустившего программу **ssbdiagnose**.  
  
 Если параметр **-U** указан одновременно с параметром **-E** , то выдается сообщение об ошибке. Если после параметра **-U** указано более одного аргумента, выдается сообщение об ошибке и программа завершает работу.  
  
 **-P** _пароль_  
 Указывает пароль для идентификатора имени входа **-U** . Пароли учитывают регистр. Если параметр **-U** указан, а параметр **-P** не указан, то программа **ssbdiagnose** использует значение из переменной среды SQLCMDPASSWORD. Если переменная SQLCMDPASSWORD также не задана, то программа **ssbdiagnose** запросит пароль у пользователя.  
  
> [!IMPORTANT]  
>  Пароль, набираемый при вводе команды SET SQLCMDPASSWORD, будет отображаться на экране в открытом виде.  
  
 Если параметр **-P** задан, а пароль не указан, то программа **ssbdiagnose** использует пароль по умолчанию (NULL).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Дополнительные сведения см. в разделе [Надежные пароли](../../relational-databases/security/strong-passwords.md).  
  
 Запрос на ввод пароля выводится на консоль следующим образом: `Password:`.  
  
 Вводимые пользователем данные на экране не отображаются, то есть символы не выводятся и курсор остается на месте.  
  
 Если параметр **-P** указан одновременно с параметром **-E** , выдается сообщение об ошибке.  
  
 Если после параметра **-P** указано более одного аргумента, то выдается сообщение об ошибке.  
  
 **-S** _имя_сервера_[\\*имя\_экземпляра*]  
 Задает экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , где размещаются службы компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] для анализа.  
  
 Укажите значение *server_name* , чтобы подключиться к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] по умолчанию на этом сервере. Укажите _server\_name_ **\\** _instance\_name_, чтобы подключиться к именованному экземпляру [!INCLUDE[ssDE](../../includes/ssde-md.md)] на этом сервере. Если параметр **-S** не указан, то программа **ssbdiagnose** использует значение переменной среды SQLCMDSERVER. Если переменная SQLCMDSERVER также не задана, то программа **ssbdiagnose** соединяется с экземпляром компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] по умолчанию на локальном компьютере.  
  
 **-d** _имя_базы_данных_  
 Задает базу данных, где размещаются службы компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] для анализа. Если такой базы данных не существует, то выдается ошибка. Если параметр **-d** не задан, то по умолчанию используется база данных, указанная в свойстве default-database текущего имени входа.  
  
 **-l** _время_ожидания_входа_  
 Указывает время ожидания соединения с сервером (в секундах). Если параметр **-l** не задан, то программа **ssbdiagnose** использует значение из переменной среды SQLCMDLOGINTIMEOUT. Если переменная SQLCMDLOGINTIMEOUT также не задана, то время ожидания по умолчанию составляет тридцать секунд. Время ожидания входа должно быть числом в диапазоне от 0 до 65 534. Если указанное значение не является числом или выходит за пределы указанного диапазона, то программа **ssbdiagnose** выдаст сообщение об ошибке. Значение 0 задает неограниченное время ожидания.  
  
 **-?**  
 Отображает справку командной строки.  
  
## <a name="remarks"></a>Remarks  
 Программа **ssbdiagnose** предназначена для выполнения следующих задач.  
  
-   Проверка отсутствия ошибок конфигурации во вновь настроенном приложении компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
-   Проверка отсутствия ошибок конфигурации после настройки существующего приложения компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
-   Проверка отсутствия ошибок конфигурации после отсоединения базы данных компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] и дальнейшего присоединения к новому экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Выявление наличия ошибок конфигурации при возникновении неполадок при передаче сообщений между службами.  
  
-   Выдача сообщений об ошибках, возникающих в наборе элементов диалогов компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
## <a name="configuration-reporting"></a>Отчеты о конфигурации  
 Чтобы правильно проанализировать конфигурацию, используемую в диалоге, отчет о конфигурации в программе **ssbdiagnose** следует запускать с теми же параметрами, которые используются в диалоге. Если в программе **ssbdiagnose** заданы параметры более низкого уровня, чем те, которые используются в диалоге, то программа **ssbdiagnose** может не зарегистрировать ошибки, которые могут возникнуть в диалоге. Если же задать в программе **ssbdiagnose** параметры более высокого уровня, то она может обнаружить проблемы в ситуациях, которые никогда в диалоге не возникнут. Например, диалог между двумя службами, расположенными в одной базе данных, можно запустить с указанием параметра ENCRYPTION OFF. Если запустить программу **ssbdiagnose** для проверки конфигурации диалога между этими двумя службами и указать значение по умолчанию ENCRYPTION ON, то программа **ssbdiagnose** сообщит об отсутствии главного ключа в базе данных, хотя для этого диалога главный ключ не нужен.  
  
 При каждом запуске программы **ssbdiagnose** для составления отчетов о конфигурации выполняется анализ либо одной службы, либо одной пары служб компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Чтобы создать отчет для нескольких пар служб компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] , создайте командный CMD-файл, который будет вызывать программу **ssbdiagnose** несколько раз.  
  
## <a name="runtime-reporting"></a>Отчеты времени выполнения  
 Если указан параметр -RUNTIME, программа **ssbdiagnose** выполняет поиск всех баз данных, указанных в **runtimeconnectionoptions** и **baseconnectionoptions** , для построения списка идентификаторов [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Полное содержимое списка зависит от значений, заданных для параметров -NEW и -ID.  
  
-   Если не указан ни параметр **-NEW** , ни параметр **-ID** , то в список будут включены все диалоги во всех базах данных, указанных в параметрах соединения.  
  
-   Если указан параметр **-NEW** , то программа **ssbdiagnose** включает в список элементы для первого диалога, который начинается после запуска программы **ssbdiagnose** . К таким элементам относятся идентификатор диалога и дескрипторы диалога для вызывающей и целевой конечных точек диалога.  
  
-   Если параметр **-ID** указан с дескриптором диалога, то в список будет включен только этот дескриптор.  
  
-   Если параметр **-ID** указан с идентификатором диалога, то в список будет добавлен этот идентификатор и дескрипторы для обеих конечных точек соответствующего диалога.  
  
-   Если параметр **-ID** указан с идентификатором группы диалогов, то в список будут добавлены все идентификаторы диалогов и все дескрипторы диалогов, входящих в эту группу.  
  
 В список не включаются элементы, находящиеся в базах данных, которые не указаны в параметрах соединения. Например, предположим, что в параметре **-ID** указан идентификатор диалога, а предложение **runtimeconnectionoptions** задано для базы данных инициатора и не задано для целевой базы данных. Программа **ssbdiagnose** не включит дескриптор целевого диалога в свой список идентификаторов. В список будет включен только идентификатор диалога и дескриптор инициатора диалога.  
  
 Программа **ssbdiagnose** наблюдает за событиями [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] в базах данных, охватываемых **runtimeconnectionoptions** и **baseconnectionoptions**. Она выполняет поиск событий [!INCLUDE[ssSB](../../includes/sssb-md.md)] , указывающих на ошибку, обнаруженную одним или несколькими идентификаторами [!INCLUDE[ssSB](../../includes/sssb-md.md)] в списке времени выполнения. Программа **ssbdiagnose**, кроме того, выполняет поиск событий ошибок [!INCLUDE[ssSB](../../includes/sssb-md.md)] системного уровня, не связанных ни с одной из групп диалогов.  
  
 Если программа **ssbdiagnose** обнаружит ошибки диалога, то предпримет попытку выяснить их первопричину, запустив отчет о конфигурации. Программа **ssbdiagnose** на основе метаданных баз данных определяет, какие экземпляры, идентификаторы [!INCLUDE[ssSB](../../includes/sssb-md.md)] , базы данных, службы и контракты используются в диалоге. Затем запускается отчет о конфигурации, учитывающий все доступные сведения.  
  
 По умолчанию программа **ssbdiagnose** не сообщает о событиях ошибок. Она сообщает только об исходных проблемах, выявленных в процессе проверки конфигурации. Это сокращает объем включаемых в отчет сведений и позволяет сосредоточить внимание на первопричинах возникновения проблем конфигурации. Можно задать параметр **-SHOWEVENTS** , чтобы вывести обнаруженные программой **ssbdiagnose** события ошибок.  
  
## <a name="issues-reported-by-ssbdiagnose"></a>Неполадки, о которых сообщает программа ssbdiagnose  
 Программа **ssbdiagnose** включает в отчет неполадки трех типов. Каждый класс неполадок включается в выходной XML-файл в виде отдельного элемента Issue. Далее перечислены три типа неполадок, о которых сообщает программа **ssbdiagnose** .  
  
 **Diagnosis**  
 Указывает на проблемы конфигурации. К ним относятся проблемы, обнаруженные во время выполнения отчета **CONFIGURATION** или во время этапа конфигурации отчета **RUNTIME** . Программа **ssbdiagnose** сообщает о каждой проблеме конфигурации один раз.  
  
 **Событие**  
 Сообщает о событии [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , сигнализирующем о проблеме, обнаруженной в диалоге, который отслеживается отчетом **RUNTIME** . Программа **ssbdiagnose** сообщает о каждом случае создания таких событий. Если проблема обнаружена в нескольких диалогах, то событие может быть включено в отчет многократно.  
  
 **Проблема.**  
 Сообщает о проблеме, из-за которой программа **ssbdiagnose** не имеет возможности выполнять анализ конфигурации или мониторинг диалогов.  
  
## <a name="sqlcmd-environment-variables"></a>Переменные среды sqlcmd  
 Программа **ssbdiagnose** поддерживает переменные среды SQLCMDSERVER, SQLCMDUSER, SQLCMDPASSWORD и SQLCMDLOGINTIMEOUT, которые также используются программой **sqlcmd** . Они могут быть заданы при помощи команды командной строки SET или команды **setvar** в скриптах [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые выполняются программой **sqlcmd**. Дополнительные сведения об использовании **setvar** в **sqlcmd** см. в разделе [Использование программы sqlcmd с переменными скрипта](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
## <a name="permissions"></a>Разрешения  
 В каждом предложении **connectionoptions** имя входа, указанное в параметре **-E** или **-U** , должно быть членом предопределенной роли сервера **sysadmin** для экземпляра, указанного в параметре **-S**.  
  
## <a name="examples"></a>Примеры  
 В этом разделе приведены примеры использования программы **ssbdiagnose** в командной строке.  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>A. Проверка конфигурации двух служб, расположенных в одной базе данных  
 В этом примере показано, как запросить отчет о конфигурации, если выполняются следующие условия.  
  
-   Вызывающая и целевая службы размещаются в одной базе данных.  
  
-   База данных размещена в экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]по умолчанию.  
  
-   Экземпляры расположены на том же компьютере, на котором работает программа **ssbdiagnose** .  
  
 Программа **ssbdiagnose** строит отчет по конфигурации, использующей контракт DEFAULT, поскольку не указано предложение ON CONTRACT.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>Б. Проверка конфигурации двух служб, расположенных на разных компьютерах, использующих одно имя входа  
 В следующем примере показано, как запросить отчет о конфигурации в том случае, если вызывающая и целевая службы расположены на разных компьютерах, но к ним возможен доступ с помощью одного имени входа, проходящего проверку подлинности Windows.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>В. Проверка конфигурации двух служб, расположенных на разных компьютерах, использующих различные имена входа  
 В следующем примере показано, как запросить отчет о конфигурации в том случае, когда вызывающая и целевая службы расположены на разных компьютерах и каждому из экземпляров компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо собственное имя входа для проверки подлинности [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>Г. Проверка конфигурации для нескольких компьютеров, в которой участвует зеркальная служба и используется анонимное шифрование  
 В следующем примере показано, как запросить отчет о конфигурации в том случае, когда вызывающая и целевая службы расположены на разных компьютерах, а для вызывающей службы существует зеркальная копия на именованном экземпляре. В ходе отчета также проверяется, что службы настроены для использования анонимного шифрования.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>Д. Проверка конфигурации двух контрактов  
 В этом примере показано, как построить командный файл, запрашивающий отчет о конфигурации, если выполняются следующие условия.  
  
-   Вызывающая и целевая службы размещаются в одной базе данных.  
  
-   База данных размещена в экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]по умолчанию.  
  
-   Экземпляр расположен на том же компьютере, на котором работает программа **ssbdiagnose** .  
  
 При каждом запуске программы **ssbdiagnose** она формирует отчет по конфигурации отдельного контракта, действующего между одними и теми же службами.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>Е. Наблюдение за состоянием отдельного диалога на локальном компьютере с ограниченным временем ожидания  
 В следующем примере показано, как запустить наблюдение за отдельным диалогом, где вызывающая и целевая службы расположены в одной базе данных на экземпляре по умолчанию, который работает на том же компьютере, где запускается программа **ssbdiagnose**. Время ожидания устанавливается в значение 20 секунд.  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>Ж. Наблюдение за состоянием диалога, в котором участвуют два компьютера  
 В следующем примере показано, как запустить наблюдение за отдельным диалогом, где вызывающая и целевая службы расположены на разных компьютерах.  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>З. Наблюдение за состоянием диалога, в котором участвуют две базы данных, расположенные в одном экземпляре  
 В следующем примере показано, как запустить наблюдение за отдельным диалогом, где вызывающая и целевая службы находятся в разных базах данных, размещенных в одном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. В этом примере параметр **baseconnectionoptions** указывает сведения об экземпляре и данные входа, а два предложения CONNECT TO указывают базы данных. Кроме того, указан параметр -SHOWEVENTS, согласно которому в отчет включаются все события времени выполнения.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>И. Наблюдение за состоянием двух диалогов между двумя базами данных  
 В следующем примере показано, как запустить наблюдение за двумя диалогами, где вызывающая и целевая службы находятся в разных базах данных, размещенных в одном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. В этом примере параметр **baseconnectionoptions** указывает сведения об экземпляре и данные входа, а два предложения CONNECT TO указывают базы данных.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>К. Наблюдение за состоянием всех диалогов между двумя базами данных  
 В следующем примере показано, как запустить наблюдение за всеми диалогами между двумя базами данных, расположенными в одном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. В этом примере параметр **baseconnectionoptions** указывает сведения об экземпляре и данные входа, а два предложения CONNECT TO указывают базы данных.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>Л. Пропуск отдельных ошибок  
 В следующем примере показано, как пропустить обработку известных ошибок (303 и 304) в конфигурации активации, используемой в тестовой системе.  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>М. Перенаправление вывода XML программы ssbdiagnose  
 В следующем примере показано, как запросить формирование программой **ssbdiagnose** выходного XML-файла, который перенаправляется в файл. Далее файл TestDiag.xml можно открыть в приложении, предназначенном для анализа XML-файлов программы **ssbdiagnose** или создания по ним отчетов. Кроме того, этот файл можно просмотреть в редакторе XML общего назначения, например в XML-блокноте.  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>Н. Использование переменной среды  
 В приведенном ниже примере сначала устанавливается переменная среды SQLCMDSERVER, в которой хранится имя сервера, а затем запускается программа **ssbdiagnose** без указания параметра **-S**.  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE (Transact-SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE (Transact-SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE (Transact-SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue (Transact-SQL)](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups (Transact-SQ)](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
