---
title: Устранение неполадок контейнеров Docker на SQL Server
description: Изучите различные методы устранения неполадок, которые можно использовать для исправления распространенных ошибок, возникающих при использовании контейнеров Docker в Linux с образами SQL Server
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: d94ae00cb8f3581be51f53d999068e60c4d5b7f1
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878820"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>Устранение неполадок контейнеров Docker на SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье рассматриваются распространенные ошибки, возникающие при развертывании и использовании контейнеров Docker в SQL Server, а также способы устранения этих ошибок.

## <a name="docker-command-errors"></a>Ошибки команды Docker

Если вы получаете ошибки, связанные с командами `docker`, убедитесь, что служба Docker запущена, и попробуйте выполнить запуск с повышенными привилегиями.

Например, при выполнении команд `docker` в Linux вы можете получить следующее сообщение об ошибке:

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Если вы получаете эту ошибку в Linux, попробуйте выполнить те же команды с префиксом `sudo`. Если этот способ также не сработает, убедитесь, что служба Docker запущена, и при необходимости запустите ее.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

В Windows убедитесь, что PowerShell или командная строка запущены от имени администратора.

## <a name="sql-server-container-startup-errors"></a>Ошибки при запуске контейнера с SQL Server

Если запуск контейнера SQL Server завершился сбоем, попробуйте выполнить следующие проверки:

- Если возникает ошибка `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`, вы пытаетесь сопоставить порт контейнера 1433 с уже используемым портом. Это может произойти в том случае, если вы запускаете SQL Server локально на хост-компьютере. Кроме того, такая ситуация возможна при запуске двух контейнеров с SQL Server и попытке сопоставить их с одним портом узла. В этом случае используйте параметр `-p`, чтобы сопоставить порт контейнера 1433 с другим портом узла. Пример: 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- Если при попытке запуска контейнера возникает ошибка `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied`, добавьте пользователя в группу docker в Ubuntu. После этого выйдите из системы и выполните вход снова, чтобы применить изменения к новым сеансам. 

   ```bash
    usermod -aG docker $USER
   ```

- Проверьте наличие сообщений об ошибках от контейнера.

   ```bash
   docker logs e69e056c702d
   ```

- Убедитесь, что ваша система отвечает требованиям к объему памяти и дисковому пространству, которые указаны в разделе [Предварительные требования](quickstart-install-connect-docker.md#requirements) статьи с кратким руководством.

- Если вы используете программное обеспечение для управления контейнером, убедитесь, что оно поддерживает привилегированное выполнение процессов контейнера. Процесс sqlservr в контейнере выполняется в привилегированном режиме.

- Если контейнер Docker в SQL Server выполняет выход сразу после запуска, проверьте журналы Docker. Если вы используете PowerShell в Windows с помощью команды `docker run`, используйте двойные кавычки вместо одинарных. В PowerShell Core используйте одинарные кавычки.

- Проверьте [журналы установки и ошибок SQL Server](#errorlogs).

## <a name="enable-dump-captures"></a>Включение записи дампа

Если произошел сбой процесса SQL Server внутри контейнера, необходимо создать новый контейнер и включить для него **SYS_PTRACE**. При этом будет добавлена возможность отслеживания процесса в Linux, которая необходима для создания файла дампа при возникновении исключения. Файл дампа может использоваться службой поддержки в процессе устранения неполадок. Эту возможность можно включить с помощью следующей команды выполнения Docker.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>Сбой подключений SQL Server

Если вам не удается подключиться к экземпляру SQL Server, запущенному в контейнере, попробуйте выполнить следующие проверки:

- Убедитесь, что контейнер с SQL Server запущен. Для этого проверьте содержимое столбца **STATUS** (Состояние) в выходных данных команды `docker ps -a`. При необходимости запустите его с помощью команды `docker start <Container ID>`.

- Если в сопоставлении задан отличный от установленного по умолчанию (1433) порт узла, убедитесь, что порт указан в строке подключения. Сопоставление порта можно проверить в столбце **PORTS** (Порты) в выходных данных команды `docker ps -a`. Например, следующая команда подключает sqlcmd к контейнеру, прослушивающему порт 1401:

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- Если вы использовали `docker run` с существующим сопоставленным томом данных или контейнером тома данных, SQL Server игнорирует значение `SA_PASSWORD`. Вместо этого для тома данных или контейнера тома данных используется предварительно заданный пароль системного администратора, хранящийся в данных SQL Server. Убедитесь, что используемый пароль системного администратора связан с данными, к которым вы подключаетесь.

- Проверьте [журналы установки и ошибок SQL Server](#errorlogs).

## <a name="sql-server-availability-groups"></a>Группы доступности SQL Server

Если вы используете Docker с группами доступности SQL Server, существует два дополнительных требования.

- Выполните сопоставление порта, который используется для подключения к реплике (по умолчанию 5022). Например, укажите `-p 5022:5022` в команде `docker run`.

- Явно укажите имя узла контейнера с помощью параметра `-h YOURHOSTNAME` команды `docker run`. Это имя узла используется при настройке группы доступности. Если оно не задано с помощью команды `-h`, по умолчанию в этом качестве используется **идентификатор контейнера**.

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> Журналы установки и ошибок SQL Server

Вы можете просмотреть журналы установки и ошибок SQL Server в папке **/var/opt/mssql/log**. Если контейнер не запущен, сначала запустите его. Затем проверьте журналы с помощью интерактивной командной строки. Идентификатор контейнера можно получить, выполнив команду `docker ps`.

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

Из сеанса Bash внутри контейнера выполните следующие команды:

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> Если вы подключили каталог хоста к каталогу **/var/opt/mssql** при создании контейнера, вместо этого можно выполнять поиск в подкаталоге **log** по сопоставленному пути на узле.

## <a name="execute-commands-in-a-container"></a>Выполнение команд в контейнере

Вы можете выполнять команды из работающего контейнера с помощью терминала узла.

Чтобы получить идентификатор контейнера, выполните следующую команду:

```bash
docker ps -a
```

Чтобы запустить терминал Bash в контейнере, выполните следующую команду:

```bash
docker exec -it <Container ID> /bin/bash
```

После этого вы можете выполнять команды так, как если бы они выполнялись из терминала внутри контейнера. По завершении введите `exit`. В этом случае завершается интерактивный сеанс команд, однако контейнер продолжает работать.

## <a name="next-steps"></a>Дальнейшие действия

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Сведения о начале работы с образами контейнеров с SQL Server 2017 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Сведения о начале работы с образами контейнеров с SQL Server 2019 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md).

::: moniker-end

- [Развертывание контейнеров Docker в SQL Server и подключение к ним](sql-server-linux-docker-container-deployment.md)

- [Ссылка на дополнительную конфигурацию и настройку контейнеров Docker](sql-server-linux-docker-container-configure.md)

- [Защита контейнеров Docker в SQL Server](sql-server-linux-docker-container-security.md)
