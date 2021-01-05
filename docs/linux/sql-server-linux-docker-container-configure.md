---
title: Настройка контейнеров Docker на SQL Server
description: Узнайте о различных способах настройки контейнеров Docker на SQL Server и о том, как их можно настроить в соответствии со своими требованиями.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 3fa589fdb522014994923edfa1a7d7858cfa7b72
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878827"
---
# <a name="configure-and-customize-sql-server-docker-containers"></a>Настройка контейнеров Docker на SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье объясняется, как настроить контейнеры Docker на SQL Server, например для сохранения данных, перемещения файлов из контейнеров и в контейнеры, а также изменения параметров по умолчанию.

## <a name="create-a-customized-container"></a><a id="customcontainer"></a> Создание настраиваемого контейнера

Для создания настраиваемого контейнера SQL Server вам потребуется собственный [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage). Дополнительные сведения см. в [демонстрации совместной работы SQL Server и приложения Node](https://github.com/twright-msft/mssql-node-docker-demo-app). При создании Dockerfile обратите внимание на процесс переднего плана, который управляет жизненным циклом контейнера. В случае выхода из этого процесса контейнер будет закрыт. Например, если вы хотите выполнить скрипт и запустить SQL Server, процесс SQL Server должен указываться в команде в крайней правой позиции. Все остальные команды выполняются в фоновом режиме. Это демонстрируется на примере следующей команды в Dockerfile:

```bash
/usr/src/app/do-my-sql-commands.sh & /opt/mssql/bin/sqlservr
```

Если отменить команды в предыдущем примере, контейнер закроется после выполнения скрипта do-my-sql-commands.sh.

## <a name="persist-your-data"></a><a id="persist"></a> Сохранение данных

Изменения в конфигурации SQL Server и файлы базы данных сохраняются в контейнере даже в том случае, если он был перезапущен с использованием команд `docker stop` и `docker start`. Тем не менее, если удалить контейнер с помощью команды `docker rm`, будет удалено все его содержимое, включая SQL Server и ваши базы данных. В следующем разделе описывается, как можно использовать **тома данных** для сохранения файлов базы данных даже в случае удаления связанных контейнеров.

> [!IMPORTANT]
> При работе с SQL Server крайне важно понимать принципы обеспечения сохраняемости данных в Docker. Помимо этого раздела, мы также рекомендуем вам ознакомиться с информацией об [управлении данными в контейнерах Docker](https://docs.docker.com/engine/tutorials/dockervolumes/) в документации по Docker.

### <a name="mount-a-host-directory-as-data-volume"></a>Подключение каталога узла в качестве тома данных

Первый способ состоит в подключении каталога на вашем узле в качестве тома данных для контейнера. Для этого используйте команду `docker run` с флагом `-v <host directory>:/var/opt/mssql`. Такой подход позволяет восстанавливать данные в перерывах между выполнениями контейнера.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Кроме того, этот способ позволяет предоставлять общий доступ к файлам на узле и просматривать их за пределами Docker.

> [!IMPORTANT]
> Сопоставление томов узла для **Docker в Windows** в настоящее время не поддерживает сопоставление полного каталога `/var/opt/mssql`. Однако можно сопоставить подкаталог, например `/var/opt/mssql/data`, с хост-компьютером.
>
> На данный момент не поддерживается сопоставление томов узла для **Docker на Mac** с образом SQL Server на Linux. Вместо этого следует использовать контейнеры томов данных. Это ограничение относится только к каталогу `/var/opt/mssql`. Операции чтения из подключенного каталога осуществляются в нормальном режиме. Например, вы можете подключить каталог узла с помощью команды -v на Mac и восстановить резервную копию из файла с расширением BAK, который находится на узле.

### <a name="use-data-volume-containers"></a>Использование контейнеров томов данных

Второй способ подразумевает использование контейнеров томов данных. Чтобы создать контейнер тома данных, укажите имя тома вместо каталога узла с параметром `-v`. В следующем примере создается общий том данных с именем **sqlvolume**.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

> [!NOTE]
> Этот способ неявного создания тома данных в рамках команды выполнения не работает в старых версиях Docker. В таком случае следует явно выполнить действия, которые описываются в разделе [Создание и подключение контейнера тома данных](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container) документации по Docker.

Даже если вы остановите и удалите этот контейнер, том данных будет сохранен. Вы сможете просмотреть его с помощью команды `docker volume ls`.

```command
docker volume ls
```

Если затем создать другой контейнер с тем же именем тома, в новом контейнере будут использоваться данные SQL Server, располагающиеся на этом томе.

Чтобы удалить контейнер тома данных, воспользуйтесь командой `docker volume rm`.

> [!WARNING]
> Если вы удалите контейнер тома данных, все содержащиеся в нем данные SQL Server будут удалены *без возможности восстановления*.

### <a name="backup-and-restore"></a>Резервное копирование и восстановление

Помимо этих способов, вы также можете использовать стандартные методы резервного копирования и восстановления SQL Server. Резервные копии файлов можно использовать для защиты данных или их переноса на другой экземпляр SQL Server. Дополнительные сведения см. в статье [Резервное копирование и восстановление баз данных SQL Server на Linux](sql-server-linux-backup-and-restore-database.md).

> [!WARNING]
> Обратите внимание, что создаваемые резервные копии должны размещаться за пределами контейнера. В противном случае при удалении контейнера файлы резервных копий также будут удалены.

## <a name="copy-files-from-a-container"></a>Копирование файлов из контейнера

Чтобы скопировать файл из контейнера, выполните следующую команду:

```command
docker cp <Container ID>:<Container path> <host path>
```

Идентификатор контейнера можно получить, выполнив команду `docker ps -a`.

**Пример**.

::: zone pivot="cs1-bash"
```bash
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog /tmp/errorlog
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

## <a name="copy-files-into-a-container"></a>Копирование файлов в контейнер

Чтобы скопировать файл в контейнер, выполните следующую команду:

```command
docker cp <Host path> <Container ID>:<Container path>
```

**Пример**.

::: zone pivot="cs1-bash"
```bash
docker cp /tmp/mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

## <a name="configure-the-timezone"></a><a id="tz"></a> Настройка часового пояса

Чтобы запустить SQL Server в контейнере Linux с определенным часовым поясом, настройте переменную среды `TZ`. Чтобы определить соответствующее значение часового пояса, выполните команду `tzselect` из командной строки Bash в Linux:

```command
tzselect
```

После выбора часового пояса команда `tzselect` выводит данные примерно следующего вида:

```output
The following information has been given:

        United States
        Pacific

Therefore TZ='America/Los_Angeles' will be used.
```

Эти сведения можно использовать для установки соответствующей переменной среды в контейнере с Linux. В следующем примере демонстрируется, как запустить SQL Server в контейнере с часовым поясом `Americas/Los_Angeles`:

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'A_PASSWORD=<YourStrong!Passw0rd>' \
-p 1433:1433 --name sql1 \
-e 'TZ=America/Los_Angeles'\
-d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 `
-e "TZ=America/Los_Angeles" `
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 ^
-e "TZ=America/Los_Angeles" ^
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -p 1433:1433 --name sql1 \
   -e 'TZ=America/Los_Angeles'\
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="change-the-default-file-location"></a><a id="changefilelocation"></a> Изменение расположения файлов по умолчанию

Добавьте переменную `MSSQL_DATA_DIR`, чтобы изменить каталог данных в команде `docker run`, а затем подключите том к этому расположению, к которому имеет доступ пользователь контейнера.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Сведения о начале работы с образами контейнеров с SQL Server 2017 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Сведения о начале работы с образами контейнеров с SQL Server 2019 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md)

::: moniker-end

- [Развертывание контейнеров Docker в SQL Server и подключение к ним](sql-server-linux-docker-container-deployment.md)

- [Устранение неполадок контейнеров Docker на SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Защита контейнеров Docker в SQL Server](sql-server-linux-docker-container-security.md)
