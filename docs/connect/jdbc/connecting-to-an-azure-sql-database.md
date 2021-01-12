---
title: Подключение к базе данных SQL Azure
description: В этой статье обсуждаются проблемы, связанные с использованием драйвера Microsoft JDBC Driver for SQL Server для подключения к Базе данных SQL Azure.
ms.custom: ''
ms.date: 12/18/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03768a309ac10fc16fd1a743660df6fe74b088e7
ms.sourcegitcommit: bc8474fa200ef0de7498dbb103bc76e3e3a4def4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709673"
---
# <a name="connecting-to-an-azure-sql-database"></a>Подключение к базе данных SQL Azure

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В этой статье обсуждаются проблемы, которые могут возникнуть при использовании [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] для соединения с [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]. Дополнительные сведения о соединении с [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] см. в следующих разделах.  
  
- [База данных SQL Azure](/azure/sql-database/sql-database-technical-overview)  
  
- [Руководство. Подключение к SQL Azure с помощью JDBC](/azure/sql-database/sql-database-connect-query-java)  

- [Соединение с использованием проверки подлинности Azure Active Directory](connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>Сведения

При подключении к [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] следует установить соединение с базой данных master, чтобы вызвать **SQLServerDatabaseMetaData.getCatalogs**.  
[!INCLUDE[ssAzure](../../includes/ssazure_md.md)] не поддерживает возврат всего набора каталогов из пользовательских баз данных. **SQLServerDatabaseMetaData.catalog** использует представление sys.databases для получения каталогов. Обсуждение разрешений в статье [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) поможет вам понять поведение **SQLServerDatabaseMetaData.getCatalogs** в [!INCLUDE[ssAzure](../../includes/ssazure_md.md)].  
  
## <a name="connections-dropped"></a>Прерванные подключения

При установлении соединения с [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] простаивающие соединения по истечении определенного времени бездействия могут быть разорваны сетевым компонентом (например, брандмауэром). В данном контексте существует два типа бездействующих соединений.  

- Бездействующие на уровне TCP, которые сбрасываются многими сетевыми устройствами.  

- Бездействующие на уровне шлюза SQL Azure, где, вероятно, отправляются TCP-сообщения **keepalive** (что делает соединение небездействующим на уровне TCP), не выполняющие активных запросов в течение 30 минут. В этом случае шлюз определит, что соединение TDS бездействует в течение 30 минут, и прервет его.  
  
Во втором случае, чтобы предотвратить прекращение шлюзом бездействующих подключений, можно сделать следующее:

* При настройке источника данных Azure SQL используйте [политику подключения](/azure/azure-sql/database/connectivity-architecture#connection-policy) **Redirect**.

* Обеспечьте активность подключения за счет выполнения простых действий. Этот метод не рекомендуется, поэтому используйте его только при отсутствии альтернатив.

В первом случае, чтобы избежать разрыва связи простаивающих подключений сетевыми компонентами, необходимо задать следующие параметры в реестре (или их аналоги для систем, отличных от Windows) той операционной системы, где загружается драйвер:  
  
|Параметр реестра|Рекомендуемое значение|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveTime|30 000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveInterval|1000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ TcpMaxDataRetransmissions|10|  
  
Перезагрузите компьютер, чтобы новые параметры вступили в силу.  

Значения KeepAliveTime и KeepAliveInterval задаются в миллисекундах. Применение этих параметров позволяет установить период в 10–40 секунд, по истечении которого бездействующее подключение разрывается. Если после отправки пакета проверки активности ответ не получен, отправка будет повторятся каждую секунду до 10 раз. Если в течение этого времени ответ не получен, сокет на стороне клиента отключается. Вам может потребоваться увеличить значение KeepAliveInterval в соответствии с прерываниями, характерными для вашей среды (например, миграциями виртуальных машин), которые могут привести к тому, что сервер не будет отвечать на запросы дольше 10 секунд.

> [!NOTE]
> Параметр TcpMaxDataRetransmissions нельзя задать в Windows Vista, Windows 2008 и более поздних версиях.

Чтобы настроить его при работе в Azure, создайте выполняемую при запуске задачу, которая будет добавлять значения в реестр.  Например, в файл определения службы добавьте следующую задачу, выполняемую при запуске.  


```xml
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```

Затем добавьте файл AddKeepAlive.cmd в проект. Выберите для параметра "Копировать в выходной каталог" значение "Всегда копировать". Следующий скрипт представляет собой пример файла AddKeepAlive.cmd.  

```bat
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on Azure SQL  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```

## <a name="appending-the-server-name-to-the-userid-in-the-connection-string"></a>Добавление имени сервера к идентификатору пользователя UserId в строке подключения  

До [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] версии 4.0 при установлении соединения с базой данных [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] требовалось добавление имени сервера к параметру UserId в строке подключения. Например, user@servername. Начиная с версии 4.0 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] добавление компонента @servername к параметру UserId в строке подключения не требуется.  

## <a name="using-encryption-requires-setting-hostnameincertificate"></a>Для использования шифрования необходимо задать hostNameInCertificate

До версии [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 7.2 при подключении к [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] нужно указывать **hostNameInCertificate**, если указано значение **encrypt=true** (если в строке подключения указано имя сервера *shortName*.*domainName*, установите для свойства **hostNameInCertificate** значение \*.*domainName*.). Это свойство является необязательным, начиная с версии драйвера 7.2.

Пример:

```java
jdbc:sqlserver://abcd.int.mscds.com;databaseName=myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate=*.int.mscds.com;
```

## <a name="see-also"></a>См. также раздел

[Подключение к SQL Server с помощью JDBC Driver](connecting-to-sql-server-with-the-jdbc-driver.md)
