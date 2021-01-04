---
title: Установка пакетов R с помощью sqlmlutils
description: Узнайте, как использовать пакет sqlmlutils для установки новых пакетов R в экземпляре Служб машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9db282708c8f2e9bbd4ee44d45bac0b0d25dc5b9
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617563"
---
# <a name="install-r-packages-with-sqlmlutils"></a>Установка пакетов R с помощью sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этой статье описывается использование функций из пакета [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) для установки пакетов R в экземпляре [служб машинного обучения SQL Server](../sql-server-machine-learning-services.md) и в [кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Устанавливаемые пакеты можно использовать в сценариях R, выполняющихся в базе данных, с помощью инструкции T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

> [!NOTE]
> Описанный в этой статье пакет **sqlmlutils** используется для добавления пакетов R в SQL Server 2019 и более поздних версий. Для SQL Server 2017 и более ранних версий обратитесь к разделу [Установка пакетов с инструментами R](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
В этой статье описывается использование функций из пакета [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) для установки пакетов R в экземпляре [служб машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Устанавливаемые пакеты можно использовать в сценариях R, выполняющихся в базе данных, с помощью инструкции T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

## <a name="prerequisites"></a>Предварительные требования

- Установите [R](https://www.r-project.org) и [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) на клиентском компьютере, который используется для подключения к SQL Server. Для выполнения сценариев можно использовать любую интегрированную среду разработки R, но в этой статье применяется RStudio.

  Версия R на клиентском компьютере должна соответствовать версии R на сервере, а устанавливаемые пакеты должны быть совместимы с используемой версией R.
  Сведения о том, какая версия R устанавливается вместе с той или иной версией SQL Server, приведены в разделе [Версии Python и R](../sql-server-machine-learning-services.md#versions).
  
  Чтобы проверить версию R для конкретного экземпляра SQL Server, используйте следующую команду T-SQL.

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- Установите [Azure Data Studio](../../azure-data-studio/what-is.md) на клиентском компьютере, который используется для подключения к SQL Server. Вы можете использовать другие средства запросов и управления базами данных, но в этой статье рассматривается Azure Data Studio.

### <a name="other-considerations"></a>Другие замечания

- Установка пакета зависит от экземпляра SQL, базы данных и пользователя, указанных в сведениях о подключении, которые вы укажете для **sqlmlutils**. Чтобы использовать пакет в нескольких экземплярах или базах данных SQL или для разных пользователей, необходимо установить пакет для каждого из них. Исключением является только если пакет устанавливается членом `dbo`, когда пакет *общедоступный* и является общим для всех пользователей. Если пользователь устанавливает более новую версию общедоступного пакета, это не повлияет на общедоступный пакет, но такой пользователь будет иметь доступ к более новой версии.

- Сценарий R, выполняющийся в SQL Server, может использовать только пакеты, установленные в библиотеке экземпляра по умолчанию. SQL Server не может загружать пакеты из внешних библиотек, даже если эти библиотеки находятся на том же компьютере. В их число входят библиотеки R, установленные с другими продуктами Майкрософт.

- В защищенной среде SQL Server может потребоваться отказаться от следующих пакетов:
  - пакеты, которым требуется доступ к сети;
  - пакеты, которым требуется доступ к файловой системе с повышенными правами;
  - пакеты, используемые для веб-разработки или других задач, малоэффективных в SQL Server.

## <a name="install-sqlmlutils-on-the-client-computer"></a>Установка пакета sqlmlutils на клиентском компьютере

Сначала пакет **sqlmlutils** необходимо установить на клиентском компьютере, который используется для подключения к SQL Server.

Пакет **sqlmlutils** зависит от пакета **odbc**, а **odbc** — от ряда других пакетов. Следующие процедуры позволяют установить все эти пакеты в правильном порядке.

### <a name="install-sqlmlutils-online"></a>Установка пакета sqlmlutils в сетевом режиме

Если у клиентского компьютера есть доступ к Интернету, вы можете скачать и установить пакет **sqlmlutils** и его зависимые пакеты по сети.

1. Скачайте на клиентский компьютер последнюю версию файла **sqlmlutils** (`.zip` для Windows, `.tar.gz` для Linux) с сайта https://github.com/Microsoft/sqlmlutils/tree/master/R/dist. Не разворачивайте файл.

1. Откройте **командную строку** и выполните следующие команды для установки пакетов **odbc** и **sqlmlutils**. Измените путь к скачанному файлу **sqlmlutils**. Пакет **odbc** находится в сети и установлен.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>Установка пакета sqlmlutils в автономном режиме

Если у клиентского компьютера нет подключения к Интернету, вам нужно заранее скачать пакеты **odbc** и **sqlmlutils** на компьютере с доступом к Интернету. Затем файлы можно скопировать в папку на клиентском компьютере и установить пакеты в автономном режиме.

У пакета **odbc** есть несколько зависимых пакетов, но при определении всех зависимостей для пакета возникают сложности. Рекомендуется воспользоваться [**miniCRAN**](https://andrie.github.io/miniCRAN/) и создать папку локального репозитория для пакета, включающего все зависимые пакеты.
Дополнительные сведения см. в статье [Create a local R package repository using miniCRAN](create-a-local-package-repository-using-minicran.md) (Создание локального репозитория пакетов R с помощью miniCRAN).

Пакет **sqlmlutils** состоит из одного файла, который можно скопировать на клиентский компьютер и установить.

На компьютере с доступом к Интернету

1. Установите **miniCRAN**. Дополнительные сведения см. в разделе об [установке miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

1. В RStudio выполните следующий сценарий R, чтобы создать локальный репозиторий пакета **odbc**. В этом примере предполагается, что репозиторий будет создан в папке `odbc`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   В качестве значения `Rversion` укажите версию R, установленную в SQL Server. Чтобы проверить установленную версию, используйте следующую команду T-SQL.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Скачайте последнюю версию файла **sqlmlutils** (`.zip` для Windows, `.tar.gz` для Linux) с сайта [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist). Не разворачивайте файл.

1. Скопируйте всю папку репозитория **odbc** и файл **sqlmlutils** на клиентский компьютер.

На клиентском компьютере, используемом для подключения к SQL Server:

1. Откройте командную строку.

1. Выполните следующие команды, чтобы установить **odbc** и **sqlmlutils**. Измените полные пути к папке репозитория **odbc** и файлу **sqlmlutils**, скопированным на этот компьютер.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>Добавление пакета R в SQL Server

В следующем примере вы добавите пакет [**glue**](https://cran.r-project.org/web/packages/glue/) в SQL Server.

### <a name="add-the-package-online"></a>Добавление пакета в сетевом режиме

Если клиентский компьютер, используемый для подключения к SQL Server, имеет доступ к Интернету, с помощью пакета **sqlmlutils** можно найти пакет **glue** и все зависимости через Интернет, а затем удаленно установить пакет в экземпляре SQL Server.

1. На клиентском компьютере откройте RStudio и создайте файл **сценария R**.

1. Выполните следующий сценарий R для установки пакета **glue** с помощью пакета **sqlmlutils**. Подставьте собственные значения для подключения к базе данных SQL Server.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > Параметр **scope** может иметь значение **PUBLIC** или **PRIVATE**. Общая область полезна администратору базы данных для установки пакетов, которые могут использовать все пользователи. В личной области пакет доступен только устанавливающему его пользователю. Если область не указана, по умолчанию используется область **PRIVATE**.

### <a name="add-the-package-offline"></a>Добавление пакета в автономном режиме

Если у клиентского компьютера нет подключения к Интернету, воспользуйтесь **miniCRAN**, чтобы скачать пакет **glue** на компьютере, имеющем доступ к Интернету. Затем пакет копируется на клиентский компьютер для установки в автономном режиме.
Сведения об установке **miniCRAN** см. в разделе [Установка miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

На компьютере с доступом к Интернету

1. Выполните следующий сценарий R для создания локального репозитория для пакета **glue**. В этом примере папка репозитория создается в папке `c:\downloads\glue`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   В качестве значения `Rversion` укажите версию R, установленную в SQL Server. Чтобы проверить установленную версию, используйте следующую команду T-SQL.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Скопируйте всю папку репозитория **glue** (`c:\downloads\glue`) на клиентский компьютер. Например, скопируйте ее в папку `c:\temp\packages\glue`.

На клиентском компьютере

1. Откройте RStudio и создайте файл **сценария R**.

1. Выполните следующий сценарий R для установки пакета **glue** с помощью пакета **sqlmlutils**. Замените сведения о подключении к базе данных SQL Server (если не используется проверка подлинности Windows, добавьте параметры `uid` и `pwd`).

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > Параметр **scope** может иметь значение **PUBLIC** или **PRIVATE**. Общая область полезна администратору базы данных для установки пакетов, которые могут использовать все пользователи. В личной области пакет доступен только устанавливающему его пользователю. Если область не указана, по умолчанию используется область **PRIVATE**.

## <a name="use-the-package"></a>Использование пакета

После установки пакет **glue** можно использовать в сценарии R в SQL Server с командой T-SQL **sp_execute_external_script**.

1. Откройте Azure Data Studio и подключитесь к своей базе данных SQL Server.

1. Выполните следующую команду:

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **Результаты**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>Удаление пакета

Чтобы удалить пакет **glue**, выполните следующий сценарий R. Используйте ту же переменную **connection**, которая была определена ранее.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>Дополнительные функции sqlmlutils

Пакет **sqlmlutils** содержит ряд функций для управления пакетами R, а также для создания, выполнения хранимых процедур и запросов в SQL Server и управления ими. Дополнительные сведения см. в [файле сведений sqlmlutils для R](https://github.com/microsoft/sqlmlutils/tree/master/R).

Дополнительные сведения о любых функциях **sqlmlutils** можно получить с помощью функции **справки** R или кнопки **?** . Пример:

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>Дальнейшие действия

- Сведения об установленных пакетах R см. в статье [Get R package information](r-package-information.md) (Получение сведений о пакете R).
- Сведения о работе с пакетами R см. в статье [Tips for using R packages](tips-for-using-r-packages.md) (Советы по использованию пакетов R).
- Сведения об установке пакетов Python см. в статье [Install Python packages with pip](install-additional-python-packages-on-sql-server.md) (Установка пакетов Python с помощью pip).
- Дополнительные сведения о службах машинного обучения SQL Server см. в статье [What is SQL Server Machine Learning Services (Python и R)?](../sql-server-machine-learning-services.md) (Что такое службы машинного обучения SQL Server (Python и R)?)
