---
title: Развертывание контейнеров Docker в SQL Server и подключение к ним
description: Узнайте, как развернуть SQL Server в контейнерах Docker, а также какие инструменты можно использовать для подключения к SQL Server из контейнера и из внешних источников.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: e4524dccd4241a2400e8bf1be76bbc326149a8c6
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878825"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>Развертывание контейнеров Docker в SQL Server и подключение к ним

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье описано, как развертывать контейнеры Docker в SQL Server и подключаться к ним.

Дополнительные сведения о других сценариях развертывания см. в следующих источниках:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes — кластеры больших данных](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> В этой статье особое внимание уделяется использованию образа mssql-server-linux. Образ с Windows не рассматривается, но сведения о нем вы можете найти на странице [mssql-server-windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) центра Docker Hub.

> [!IMPORTANT]
> Прежде чем запустить контейнер SQL Server для использования в рабочей среде, просмотрите нашу [политику поддержки для контейнеров SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server), чтобы убедиться, что вы используете поддерживаемую конфигурацию.

Это 6-минутное видео содержит введение в запуск SQL Server в контейнерах:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>Извлечение и запуск образа контейнера

Для извлечения и запуска образов контейнеров Docker для SQL Server 2017 и SQL Server 2019 необходимо выполнить предварительные требования и действия, описываемые в следующем кратком руководстве:

- [Запуск образа контейнера с SQL Server 2017 в Docker](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)
- [Запуск образа контейнера с SQL Server 2019 в Docker](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

Эта статья посвящена настройке и содержит дополнительные сценарии использования, описываемые в следующих разделах.

## <a name="connect-and-query"></a>Подключение и выполнение запросов

Вы можете подключаться и выполнять запросы к SQL Server в контейнере как извне контейнера, так и внутри него. Оба эти сценария описываются в следующих разделах.

### <a name="tools-outside-the-container"></a>Средства за пределами контейнера

Подключиться к экземпляру SQL Server на компьютере Docker можно с помощью любого внешнего инструмента в macOS, Windows или Linux, поддерживающего подключения SQL. Ниже перечислены некоторые распространенные средства:

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) в Windows](sql-server-linux-manage-ssms.md);

В следующем примере используется **sqlcmd** для подключения к SQL Server в контейнере Docker. IP-адрес в строке подключения соответствует IP-адресу хост-компьютера, на котором выполняется контейнер.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

Если в сопоставлении используется отличный от заданного по умолчанию порт узла (**1433**), необходимо добавить этот порт в строку подключения. Например, если вы указали `-p 1400:1433` в команде `docker run`, для подключения необходимо явно указать порт 1400.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>Средства внутри контейнера

Начиная с SQL Server 2017 [средства командной строки SQL](sql-server-linux-setup-tools.md) включены в образ контейнера. Если подключиться к образу с помощью интерактивной командной строки, можно запускать программы локально.

1. Выполните команду `docker exec -it`, чтобы запустить интерактивную оболочку bash внутри запущенного контейнера. В следующем примере `e69e056c702d` — это идентификатор контейнера.

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > Указывать идентификатор контейнера полностью во всех случаях не требуется. Достаточно указать количество символов, необходимое для его уникальной идентификации. Соответственно, вместо полного идентификатора в этом примере может использоваться форма `e6` или `e69`. Чтобы узнать идентификатор контейнера, выполните команду `docker ps -a`.

2. После входа в контейнер подключитесь локально с помощью sqlcmd. Средство sqlcmd не включено в путь по умолчанию, поэтому необходимо указать полный путь.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. По завершении работы с sqlcmd введите `exit`.

4. После завершения работы с интерактивной командной строкой введите `exit`. Контейнер продолжит работать после выхода из интерактивной оболочки bash.

## <a name="check-the-container-version"></a><a id="version"></a> Проверка версии контейнера

Чтобы просмотреть версию SQL Server в запущенном контейнере Docker, выполните следующую команду. Замените `<Container ID or name>` на идентификатор или имя целевого контейнера. Замените `<YourStrong!Passw0rd>` на пароль SQL Server для имени входа системного администратора.

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

Также можно определить версию и номер сборки SQL Server для целевого образа контейнера Docker. Приведенная ниже команда выводит сведения о версии и номере сборки SQL Server для образа **mcr.microsoft.com/mssql/server:2017-latest**. Для этого запускается новый контейнер с переменной среды **PAL_PROGRAM_INFO=1**. Полученный контейнер моментально закрывается, а команда `docker rm` удаляет его.

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

При выполнении указанных выше команд возвращаются сведения о версии примерно следующего вида:

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> Запуск определенного образа контейнера с SQL Server

> [!NOTE]
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается Ubuntu 18.04. Список всех доступных тегов для mssql/server можно найти на странице <https://mcr.microsoft.com/v2/mssql/server/tags/list>.

В некоторых сценариях не требуется использовать последнюю версию образа контейнера с SQL Server. Чтобы запустить определенный образ контейнера с SQL Server, выполните следующие действия:

1. Определите **тег** Docker для выпуска, который требуется использовать. Список всех доступных тегов см. на странице [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) центра Docker Hub.

2. Извлеките образ контейнера с SQL Server по соответствующему тегу. Например, чтобы извлечь образ 2019-CU7-ubuntu-18.04, замените `<image_tag>` в следующей команде на `2019-CU7-ubuntu-18.04`.

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. Чтобы запустить новый контейнер с этим образом, укажите название тега в команде `docker run`. В следующей команде замените `<image_tag>` на версию, которую требуется запустить.

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

Эти действия также можно использовать для перехода на более раннюю версию существующего контейнера. Например, вы можете выполнить откат или перейти на более раннюю версию контейнера для устранения неполадок или тестирования. Чтобы перейти на более раннюю версию контейнера, необходимо использовать метод обеспечения сохраняемости для папки данных. Выполните действия, описываемые в разделе, посвященном [обновлению](#upgrade), однако при запуске контейнера укажите название тега, соответствующее более старой версии.

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> Запуск образов контейнеров на основе RHEL

Документация по образам контейнеров с Linux для SQL Server содержит информацию о контейнерах на основе Ubuntu. Начиная с SQL Server 2019 вы можете использовать контейнеры на основе Red Hat Enterprise Linux (RHEL). Пример образа для RHEL будет выглядеть так: **mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8**.

Например, следующая команда извлекает последнюю версию контейнера с накопительным обновлением 1 для SQL Server 2019, в которой используется RHEL 8:

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> Запуск рабочих образов контейнера

В [кратком руководстве](quickstart-install-connect-docker.md) в предыдущем разделе используется бесплатный выпуск SQL Server Developer из центра Docker Hub. Основная часть приведенных здесь сведений также применима для использования рабочих образов контейнеров, таких как выпуски Enterprise, Standard или Web. Однако между ними есть несколько различий, которые будут описываться отдельно.

- Для использования SQL Server в рабочей среде вам потребуется действительная лицензия. Чтобы получить бесплатную рабочую лицензию SQL Server Express, воспользуйтесь [этой ссылкой](https://go.microsoft.com/fwlink/?linkid=857693). Лицензии SQL Server Standard и Enterprise доступны в рамках [программы корпоративного лицензирования Майкрософт](https://www.microsoft.com/licensing/default.aspx).

- Образ контейнера с выпуском Developer при необходимости можно настроить для запуска в рабочей среде. Для запуска рабочих выпусков выполните следующие действия:

Ознакомьтесь с требованиями и выполните процедуры, описываемые в [кратком руководстве](quickstart-install-connect-docker.md). Укажите рабочий выпуск с помощью переменной среды **MSSQL_PID**. В следующем примере показано, как запустить последнюю версию образа контейнера с SQL Server 2017 для выпуска Enterprise:

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> Передавая значение **Y** в переменной среды **ACCEPT_EULA** и значение выпуска в переменной среды **MSSQL_PID**, вы подтверждаете, что у вас есть действительная лицензия на выпуск и версию SQL Server, которые вы хотите использовать. Кроме того, вы соглашаетесь с тем, что использование программного обеспечения SQL Server, выполняющегося в образе контейнера Docker, будет регламентироваться условиями вашей лицензии SQL Server.

> [!NOTE]
> Полный список поддерживаемых значений переменной **MSSQL_PID** см. в статье [Настройка параметров SQL Server с помощью переменных среды в Linux](sql-server-linux-configure-environment-variables.md).

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>Запуск нескольких контейнеров SQL Server

В Docker реализована возможность одновременно запускать несколько контейнеров SQL Server на одном хост-компьютере. Используйте этот подход в сценариях, когда на одном хост-компьютере требуется несколько экземпляров SQL Server. Каждый контейнер должен предоставляться через отдельный порт.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

В следующем примере создаются два контейнера SQL Server 2017, которые сопоставляются с портами **1401** и **1402** на хост-компьютере.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

В следующем примере создаются два контейнера SQL Server 2019, которые сопоставляются с портами **1401** и **1402** на главном компьютере.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Обратите внимание, что в этом случае два экземпляра SQL Server будут выполняться в разных контейнерах. Клиенты могут подключаться к каждому из этих экземпляров SQL Server, указав IP-адрес узла Docker и номер порта для контейнера.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> Обновление SQL Server в контейнерах

Чтобы выполнить обновление образа контейнера Docker, сначала необходимо определить тег, соответствующий нужному выпуску. Чтобы извлечь эту версию из реестра, используйте команду `docker pull`:

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

При этом образ SQL Server будет обновлен только во вновь создаваемых контейнерах. В работающих контейнерах обновление не производится. Для этого необходимо создать новый контейнер с последней версией образа контейнера с SQL Server и перенести в него данные.

1. Убедитесь, что в отношении существующего контейнера с SQL Server применяется один из [методов обеспечения сохраняемости данных](sql-server-linux-docker-container-configure.md#persist). Таким образом, вы можете запустить новый контейнер с теми же данными.

1. Остановите контейнер с SQL Server с помощью команды `docker stop`.

1. Создайте новый контейнер с SQL Server с помощью команды `docker run` и укажите сопоставленный каталог узла или контейнер тома данных. Обратите внимание на необходимость использовать тег, соответствующий обновлению SQL Server. В новом контейнере будет использоваться новая версия SQL Server с существующими данными SQL Server.

   > [!IMPORTANT]
   > На данный момент поддерживается обновление между версиями RC1, RC2 и общедоступной версией.

1. Убедитесь, что базы данных и данные перенесены в новый контейнер.

1. При необходимости удалите старый контейнер с помощью команды `docker rm`.

## <a name="next-steps"></a>Дальнейшие действия

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Сведения о начале работы с образами контейнеров с SQL Server 2017 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Сведения о начале работы с образами контейнеров с SQL Server 2019 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md)

::: moniker-end

- [Ссылка на дополнительную конфигурацию и настройку контейнеров Docker](sql-server-linux-docker-container-configure.md)

- В [репозитории GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) вы найдете полезные ресурсы, отзывы и информацию об известных проблемах.

- [Устранение неполадок контейнеров Docker на SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Обеспечение высокого уровня доступности для контейнеров SQL Server](sql-server-linux-container-ha-overview.md)

- [Защита контейнеров Docker в SQL Server](sql-server-linux-docker-container-security.md)
