---
title: Создание и настройка группы доступности для SQL Server на Linux
description: В этом руководстве описывается, как создать и настроить группы доступности для SQL Server на Linux, а также создать конечные точки и сертификаты группы доступности.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172006"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Создание и настройка группы доступности для SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этом руководстве рассматривается создание и настройка группы доступности для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] на Linux. В отличие от [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] и более ранних версий на платформе Windows, для включения групп доступности необязательно предварительно создавать базовый кластер Pacemaker. Интеграция с кластером, если она требуется, осуществляется позднее.

В руководстве рассматриваются следующие задачи:
 
> [!div class="checklist"]
> * включение групп доступности;
> * создание конечных точек и сертификатов для групп доступности;
> * создание группы доступности с помощью [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) или Transact-SQL;
> * создание имени входа [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] и разрешений для Pacemaker;
> * создание ресурсов групп доступности в кластере Pacemaker (только внешний тип).

## <a name="prerequisite"></a>Предварительные требования
- Разверните кластер Pacemaker с высоким уровнем доступности, как описано в статье [Развертывание кластера Pacemaker для SQL Server на Linux](sql-server-linux-deploy-pacemaker-cluster.md).


## <a name="enable-the-availability-groups-feature"></a>Включение компонента "Группы доступности"

В отличие от Windows, включить компонент "Группы доступности" с помощью PowerShell или диспетчера конфигурации [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] в Linux невозможно. Для этого необходимо использовать служебную программу `mssql-conf`. Включить компонент "Группы доступности" можно двумя способами: с помощью программы `mssql-conf` или путем изменения файла `mssql.conf` вручную.

> [!IMPORTANT]
> Функция "Группы доступности" должна быть включена для реплик, поддерживающих только конфигурацию, даже в [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)].

### <a name="use-the-mssql-conf-utility"></a>Использование служебной программы mssql-conf

В командной строке выполните следующую команду:

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Изменение файла mssql.conf

Вы также можете изменить файл `mssql.conf` в папке `/var/opt/mssql`, добавив следующие строки:

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>Перезапуск [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Так же как и в Windows, после включения групп доступности необходимо перезапустить [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Это можно сделать так:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Создание конечных точек и сертификатов для групп доступности

Для взаимодействия группа доступности использует конечные точки TCP. В Linux конечные точки группы доступности поддерживаются, только если для проверки подлинности используются сертификаты. Это означает, что сертификат из одного экземпляра должен быть восстановлен во всех остальных экземплярах, которые будут репликами в составе одной группы доступности. Этот процесс необходим даже для реплики, поддерживающей только конфигурацию. 

Создавать конечные точки и восстанавливать сертификаты можно только с помощью Transact-SQL. Можно также использовать сертификаты, которые были созданы не [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Кроме того, необходим процесс для управления сертификатами и их замены при истечении срока действия.

> [!IMPORTANT]
> Даже если вы планируете создать группу доступности с помощью мастера [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)], для создания и восстановления сертификатов в Linux необходимо будет использовать Transact-SQL.

Полное описание синтаксиса различных команд и их параметров (например, для повышения уровня безопасности) см. в следующих статьях:

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Хотя вы создаете группу доступности, используется тип конечной точки *FOR DATABASE_MIRRORING*, так как некоторые базовые принципы были унаследованы от этой функции, которая теперь является нерекомендуемой.

В этом примере создаются сертификаты для конфигурации с тремя узлами. Имена экземпляров — LinAGN1, LinAGN2 и LinAGN3.

1.  Выполните приведенные ниже команды в экземпляре LinAGN1, чтобы создать главный ключ, сертификат и конечную точку, а также резервную копию сертификата. В этом примере для конечной точки используется стандартный TCP-порт 5022.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Сделайте то же самое в экземпляре LinAGN2:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Наконец, выполните ту же последовательность команд в LinAGN3:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  Используя `scp` или другую служебную программу, скопируйте резервные копии сертификатов в каждый узел, который будет входить в группу доступности.
    
    В этом примере:
    
    - скопируйте файл LinAGN1_Cert.cer в узлы LinAGN2 и LinAGN3;
    - скопируйте файл LinAGN2_Cert.cer в узлы LinAGN1 и LinAGN3;
    - скопируйте файл LinAGN3_Cert.cer в узлы LinAGN1 и LinAGN2.
    
5.  Измените владельца и группу для скопированных файлов сертификатов на `mssql`.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Создайте в узле LinAGN1 имена входа на уровне экземпляра и пользователей, связанных с LinAGN2 и LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Восстановите сертификаты LinAGN2_Cert и LinAGN3_Cert в узле LinAGN1. Наличие сертификатов других реплик необходимо для обмена данными и безопасности группы доступности.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Создайте в узле LinAGN2 имена входа на уровне экземпляра и пользователей, связанных с LinAGN1 и LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. Восстановите сертификаты LinAGN1_Cert и LinAGN3_Cert в узле LinAGN2.
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. Предоставьте именам входа, связанным с узлами LinAG1 и LinAGN3, разрешение на подключение к конечной точке в узле LinAGN2.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. Создайте в узле LinAGN3 имена входа на уровне экземпляра и пользователей, связанных с LinAGN1 и LinAGN2.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. Восстановите сертификаты LinAGN1_Cert и LinAGN2_Cert в узле LinAGN3. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. Предоставьте именам входа, связанным с узлами LinAG1 и LinAGN2, разрешение на подключение к конечной точке в узле LinAGN3.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Создание группы доступности

В этом разделе описывается, как создать группу доступности для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] с помощью [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) или Transact-SQL.

### <a name="use-ssmanstudiofull-md"></a>Используйте [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)].

В этом разделе показано, как создать группу доступности с типом кластера "Внешний" с помощью мастера создания групп доступности в SSMS.

1.  В SSMS разверните узел **Высокий уровень доступности Always On**, щелкните правой кнопкой мыши узел **Группы доступности** и выберите пункт **Мастер создания групп доступности**.

2.  В диалоговом окне "Введение" нажмите кнопку **Далее**.

3.  В диалоговом окне "Указание параметров группы доступности" введите имя группы доступности и выберите в раскрывающемся списке тип кластера "Внешний" или "Нет". Внешний тип следует использовать, если будет развертываться кластер Pacemaker. Тип "Нет" предназначен для особых случаев, например горизонтального увеличения масштаба для чтения. Параметр "Определение уровня работоспособности баз данных" выбирать необязательно. Дополнительные сведения об этом параметре см. в статье [Параметр определения уровня работоспособности базы данных группы доступности](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Щелкните **Далее**.

    ![Создание группы доступности 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  В диалоговом окне "Выбор баз данных" выберите базы данных, которые будут входить в группу доступности. Прежде чем добавлять базу данных в группу доступности, необходимо создать ее полную резервную копию. Щелкните **Далее**.

5.  В диалоговом окне "Указание реплик" щелкните **Добавить реплику**.

6.  В диалоговом окне "Подключение к серверу" введите экземпляр [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] в Linux, который будет вторичной репликой, и учетные данные для подключения. Нажмите кнопку **Соединить**.

7.  Повторно выполните два предыдущих действия применительно к экземпляру, который будет содержать реплику, поддерживающую только конфигурацию, или еще одну вторичную реплику.

8.  Все три экземпляра теперь должны отображаться в диалоговом окне "Указание реплик". Если используется тип кластера "Внешний", режим доступности для вторичной реплики, которая будет истинной вторичной репликой, должен быть тем же, что и для первичной, а режим отработки отказа должен быть "Внешний". Для реплики, поддерживающей только конфигурацию, выберите режим доступности "Только конфигурация".

    В приведенном ниже примере показана группа доступности с двумя репликами, типом кластера "Внешний" и репликой, поддерживающей только конфигурацию.

    ![Создание группы доступности 04](./media/sql-server-linux-create-availability-group/image4.png)

    В приведенном ниже примере показана группа доступности с двумя репликами, типом кластера "Нет" и репликой, поддерживающей только конфигурацию.

    ![Создание группы доступности 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  Чтобы изменить параметры резервного копирования, выберите вкладку "Параметры резервного копирования". Дополнительные сведения о параметрах резервного копирования для групп доступности см. в статье [Настройка резервного копирования реплик доступности](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Если вы используете вторичные реплики для чтения или создаете группу доступности с типом кластера "Нет" для масштабирования для чтения, можно создать прослушиватель, перейдя на вкладку "Прослушиватель". Прослушиватель можно также добавить позднее. Чтобы создать прослушиватель, установите переключатель в положение **Создать прослушиватель группы доступности** и введите имя, порт TCP/IP, а также укажите, следует ли использовать статический IP-адрес или адрес, назначаемый службой DHCP автоматически. Учтите, что для группы доступности с типом кластера "Нет" IP-адрес должен быть статическим. Им должен быть IP-адрес первичной реплики.

    ![Создание группы доступности 06](./media/sql-server-linux-create-availability-group/image6.png)

11. Если прослушиватель создается для сценариев доступа для чтения, SSMS 17.3 и более поздних версий позволяет создать маршрутизацию с доступом только для чтения в мастере. Ее можно также добавить позднее с помощью SSMS или Transact-SQL. Чтобы добавить маршрутизацию только для чтения сразу, выполните указанные ниже действия.

    а.  Перейдите на вкладку "Маршрутизация только для чтения".

    b.  Введите URL-адреса реплик только для чтения. Эти URL-адреса аналогичны адресам конечных точек за тем исключением, что используется порт экземпляра, а не конечной точки.

    c.  Выберите каждый URL-адрес, а затем в области ниже выберите реплики, доступные для чтения. Чтобы выбрать сразу несколько элементов, нажмите и удерживайте клавишу SHIFT или используйте перетаскивание.

12. Щелкните **Далее**.

13. Выберите способ инициализации вторичных реплик. Способ по умолчанию — [автоматическое заполнение](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md), для чего требуется одинаковый путь на всех серверах, входящих в группу доступности. Можно также выбрать резервное копирование, копирование и восстановление (второй вариант), присоединение, если резервное копирование, копирование и восстановление базы данных в репликах было выполнено вручную (третий вариант), или добавление базы данных позднее (последний вариант). Так же как и в случае с сертификатами, если вы создаете резервные копии и копируете их вручную, в остальных репликах необходимо настроить разрешения на доступ к файлам резервных копий. Щелкните **Далее**.

14. Если в диалоговом окне "Проверка" есть предупреждения о неудачных проверках, определите причину. Некоторые предупреждения не критичны, например предупреждение о том, что прослушиватель не создан. Щелкните **Далее**.

15. В диалоговом окне "Сводка" нажмите кнопку **Готово**. Начнется процесс создания группы доступности.

16. Когда создание группы доступности завершится, в окне "Результаты" нажмите кнопку **Закрыть**. Теперь группа доступности будет отображаться для реплик в динамических административных представлениях, а также в папке "Высокий уровень доступности Always On" в SSMS.

### <a name="use-transact-sql"></a>Использование Transact-SQL

В этом разделе приводятся примеры создания группы доступности с помощью Transact-SQL. Прослушиватель и маршрутизацию только для чтения можно настроить после создания группы доступности. Саму группу доступности можно изменить с помощью инструкции `ALTER AVAILABILITY GROUP`, однако изменить тип кластера в [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] нельзя. Если вы по ошибке создали группу доступности с типом кластера "Внешний", ее следует удалить и создать повторно с типом кластера "Нет". Дополнительные сведения и описание других параметров см. в следующих статьях:

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Настройка маршрутизации только для чтения в группе доступности (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Пример 1. Две реплики с репликой, поддерживающей только конфигурацию (внешний тип кластера)

В этом примере показано, как создать группу доступности с двумя репликами, включающую реплику, поддерживающую только конфигурацию.

1.  Выполните приведенные ниже команды в узле, который будет первичной репликой, содержащей полную копию базы данных для чтения и записи. В этом примере используется автоматическое заполнение.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  В окне запроса, подключенном к другой реплике, выполните приведенные ниже команды, чтобы присоединить реплику к группе доступности и инициировать процесс заполнения из первичной во вторичную реплику.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. В окне запроса, подключенном к реплике, поддерживающей только конфигурацию, присоедините эту реплику к группе доступности.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>Пример 2. Три реплики с маршрутизацией только для чтения (внешний тип кластера)

В этом примере демонстрируется конфигурация с тремя полными репликами и настройка маршрутизации только для чтения в рамках создания группы доступности.

1.  Выполните приведенные ниже команды в узле, который будет первичной репликой, содержащей полную копию базы данных для чтения и записи. В этом примере используется автоматическое заполнение.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Несколько замечаний касательно этой конфигурации:
    
    - *AGName* — это имя группы доступности.
    - *DBName* — это имя базы данных, которая будет использоваться с группой доступности. Это также может быть список имен через запятую.
    - *ListenerName* — это имя, отличающееся от имен любых базовых серверов или узлов. Оно будет зарегистрировано в службе DNS вместе с *IPAddress*.
    - *IPAddress* — это IP-адрес, связанный с *ListenerName*. Он также должен быть уникальным и не должен совпадать с IP-адресами серверов и узлов. Приложения и пользователи будут использовать либо *ListenerName*, либо *IPAddress* для подключения к группе доступности.
    - *SubnetMask* — это маска подсети *IPAddress*, например 255.255.255.0.

2.  В окне запроса, подключенном к другой реплике, выполните приведенные ниже команды, чтобы присоединить реплику к группе доступности и инициировать процесс заполнения из первичной во вторичную реплику.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Повторите шаг 2 для третьей реплики.

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>Пример 3. Две реплики с маршрутизацией только для чтения (тип кластера "Нет")

В этом примере демонстрируется создание конфигурации с двумя репликами с типом кластера "Нет". Она используется для сценария масштабирования для чтения, в котором отработка отказа не предусмотрена. В результате создается прослушиватель, фактически являющийся первичной репликой, а также маршрутизация только для чтения с функцией циклического перебора.

1.  Выполните приведенные ниже команды в узле, который будет первичной репликой, содержащей полную копию базы данных для чтения и записи. В этом примере используется автоматическое заполнение.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Where
    - *AGName* — это имя группы доступности.
    - *DBName* — это имя базы данных, которая будет использоваться с группой доступности. Это также может быть список имен через запятую.
    - *PortOfEndpoint* — это номер порта, используемый созданной конечной точкой.
    - *PortOfInstance* — это номер порта, используемый экземпляром [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].
    - *ListenerName* — это имя, отличающееся от имен любых базовых реплик, которое фактически не используется.
    - *PrimaryReplicaIPAddress* — это IP-адрес первичной реплики.
    - *SubnetMask* — это маска подсети *IPAddress*. Пример: 255.255.255.0.
    
2.  Присоедините вторичную реплику к группе доступности и инициируйте автоматическое заполнение.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>Создание имени входа [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] и разрешений для Pacemaker

Кластеру Pacemaker с высоким уровнем доступности, на основе которого работает [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] на Linux, требуется доступ к экземпляру [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], а также разрешения на доступ к самой группе доступности. В этой процедуре создаются имя входа и соответствующие разрешения, а также файл со сведениями для входа Pacemaker в [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

1.  В окне запроса, подключенном к первой реплике, выполните следующие команды:

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  В узле 1 введите следующую команду: 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    Откроется редактор Emacs.
    
3.  Введите в редакторе следующие две строки:

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Удерживайте нажатой клавишу CTRL и нажмите клавишу X, а затем клавишу C, чтобы выйти и сохранить файл.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    чтобы заблокировать файл.

6.  Повторите шаги 1–5 на других серверах, которые будут служить репликами.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Создание ресурсов групп доступности в кластере Pacemaker (только внешний тип)

После создания группы доступности в [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] необходимо создать соответствующие ресурсы в Pacemaker, если указан тип кластера "Внешний". С группой ресурсов связаны два ресурса: сама группа доступности и IP-адрес. Если прослушиватель не используется, настраивать ресурс IP-адреса необязательно, но рекомендуется.

Создаваемый ресурс группы доступности — это ресурс особого типа, который называется клоном. В каждом узле имеется копия этого ресурса. Помимо этого, имеется один управляющий ресурс, называемый главным. Главный ресурс связан с сервером, на котором размещается первичная реплика. Другие ресурсы используются для размещения вторичных реплик (обычных или поддерживающих только конфигурацию) и могут становиться главными в случае отработки отказа.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  Чтобы создать ресурс группы доступности, используйте следующий синтаксис:

    **Red Hat Enterprise Linux (RHEL) и Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >В RHEL 7.4 при использовании параметра --master может выдаваться предупреждение. Чтобы избежать этого, используйте команду `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    где *NameForAGResource* — это уникальное имя, присваиваемое этому ресурсу кластера для группы доступности, а *AGName* — это имя созданной группы доступности.
 
2.  Создайте ресурс IP-адреса для группы доступности, который будет связан с прослушивателем.

    **RHEL и Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    где *NameForIPResource* — это уникальное имя IP-ресурса, а *IPAddress* — статический IP-адрес, назначаемый ресурсу. В SLES необходимо также указать маску сети. Например, для 255.255.255.0 значением *Netmask* будет 24.
    
3.  Чтобы ресурс IP-адреса и ресурс группы доступности выполнялись в одном узле, необходимо настроить ограничение совместного размещения.

    **RHEL и Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    где *NameForIPResource* — это имя ресурса IP-ресурса, *NameForAGResource* — имя ресурса группы доступности, а (в SLES) *NameForConstraint* — имя ограничения.

4.  Создайте ограничение очередности, чтобы ресурс группы доступности запускался до ресурса IP-адреса. Ограничение совместного размещения предполагает такое ограничение, поэтому его необходимо задать явно.

    **RHEL и Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    где *NameForIPResource* — это имя ресурса IP-ресурса, *NameForAGResource* — имя ресурса группы доступности, а (в SLES) *NameForConstraint* — имя ограничения.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как создать и настроить группу доступности для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] на Linux. Вы ознакомились с выполнением следующих задач:
> [!div class="checklist"]
> * включение групп доступности;
> * создание конечных точек и сертификатов для групп доступности;
> * создание группы доступности с помощью [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) или Transact-SQL;
> * создание имени входа [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] и разрешений для Pacemaker;
> * создание ресурсов группы доступности в кластере Pacemaker.

Сведения о большинстве задач администрирования групп доступности, включая обновление и отработку отказа, см. в следующей статье:

> [!div class="nextstepaction"]
> [Управление группой доступности Always On для SQL Server на Linux](sql-server-linux-availability-group-failover-ha.md)

