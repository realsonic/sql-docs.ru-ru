---
title: Установка настраиваемой среды выполнения R
description: Узнайте, как установить настраиваемую среду выполнения R для SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 9029dd87712ee7128504a9b495b699c9c58e000d
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804346"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Установка настраиваемой среды выполнения R для SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

В этой статье описывается, как установить настраиваемую среду выполнения для запуска скриптов R с SQL Server. Настраиваемая среда выполнения использует технологию расширения языка, созданную на основе платформы расширяемости, для выполнения внешнего кода. Настраиваемую среду выполнения R можно использовать в следующих сценариях.

+ Установка SQL Server с платформой расширяемости.

+ Установка Служб машинного обучения с SQL Server 2019. Расширение языка можно использовать со [Службами машинного обучения SQL Server ](../sql-server-machine-learning-services.md) после выполнения некоторых дополнительных этапов настройки.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> В этой статье описывается, как установить настраиваемую среду выполнения для R в Windows. Об установке на Linux см. статью [Установка настраиваемую среды выполнения R для SQL Server на Linux](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>Контрольный список перед установкой

Перед установкой настраиваемой среды выполнения R установите следующие компоненты.

+ [SQL Server 2019 для Windows (накопительное обновление 3 или более поздней версии)](../../database-engine/install-windows/install-sql-server.md).

+ [Расширения языка SQL Server для Windows с платформой расширяемости](../../language-extensions/install/windows-java.md).

+ [R версии 3.3 или более поздней](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-windows"></a>Добавление расширений языка SQL Server для Windows

> [!NOTE]
> Если на SQL Server 2019 установлены Службы машинного обучения, то платформа расширяемости для расширений языка со службой панели запуска уже установлена и этот шаг можно пропустить.

Расширения языка используют платформу расширяемости для исполнения внешнего кода. Выполнение кода изолировано от процессов ядра, но полностью интегрировано с выполнением запросов SQL Server.

1. Запустите мастер установки SQL Server 2019.

1. На вкладке **Установка** выберите параметр **Новая установка изолированного экземпляра SQL Server или добавление компонентов к существующей установке**.

    ![Установка SQL Server 2019 с накопительным обновлением 3 или более поздней версии](../install/media/2019setup-installation-page-mlsvcs.png)

1. На странице **Выбор компонентов** выберите следующие компоненты:

    - **Службы ядра СУБД**

        Чтобы использовать расширения языка с SQL Server, необходимо установить экземпляр ядра СУБД. Можно использовать экземпляр по умолчанию или именованный экземпляр.

    - **Службы машинного обучения и расширения языка**

        Выберите **Службы машинного обучения и расширения языка**. Нет необходимости выбирать R.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Установка расширений языка для SQL Server 2019.":::

1. На странице **Все готово для установки** проверьте, включены ли указанные ниже компоненты, и нажмите **Установить**.

    + Службы ядра СУБД
    + Службы машинного обучения и расширения языка

1. Если после завершения установки будет предложено перезагрузить компьютер, выполните перезагрузку. После завершения установки важно прочитать сообщение мастера установки. Дополнительные сведения см. в разделе [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="install-r"></a>Установка R

> [!NOTE]
>R для Служб машинного обучения SQL уже установлен в папку **R_SERVICES** экземпляра SQL Server. Например: C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES. Если вы хотите использовать этот путь в качестве R_HOME, перейдите к следующему шагу установки Rcpp. Если вы хотите использовать другую среду выполнения R, продолжайте выполнять этот шаг, чтобы установить ее.

Установите [R (версия 3.3 или более поздняя)](https://cran.r-project.org/bin/windows/base/) и обратите внимание на путь к месту установки. Этот путь — ваш **R_HOME**. Как показано в этом примере, R_HOME находится здесь: C:\Program Files\R\R-4.0.2.

![Установка настраиваемой среды выполнения R](../install/media/custom-r-installation.png)

> [!NOTE]
>В следующих инструкциях %R_HOME% — это путь к месту установки R, который следует заменить на это значение.

## <a name="install-rcpp-package"></a>Установка пакета Rcpp

+ Найдите исполняемый файл R в папке %R_HOME%\bin. По умолчанию это `C:\Program Files\R\R-4.0.2\bin`.

+ Запустите R в командной строке *с повышенными привилегиями*.

```CMD
%R_HOME%\bin\R.exe
```

В этой строке подсказки R *с повышенными привилегиями* (%R_HOME%\bin\R.exe) выполните следующий скрипт, чтобы установить пакет Rcpp в папку %R_HOME%\Library.

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>Обновление системных переменных среды

1. Добавьте или измените **R_HOME** как системную переменную среды.
    + В поле поиска Windows введите "среда" и выберите **Изменение системных переменных среды**.
    + На вкладке **Дополнительно** выберите **Переменные среды**.

    + В разделе **Системные переменные** выберите **Создать**, чтобы создать R_HOME.
    Чтобы изменить ее, нажмите **Изменить**. Измените ее значение так, чтобы она указывала на настраиваемый путь установки R.

    ![Создайте системную переменную среды R_HOME.](../install/media/sys-env-r-home.png)

2. Обновите переменную среды **PATH**.
    Добавьте путь к **R.dll** в системную переменную среды **PATH**. Выберите **PATH**, затем **Изменить** и добавьте `%R_HOME%\bin\x64` в список путей.

    ![Добавление в системную переменную среды PATH.](../install/media/sys-env-path-r.png)

3. Нажмите **ОК**, чтобы закрыть остальные окна.

    В качестве альтернативы установите эти переменные среды из командной строки *с повышенными привилегиями*, выполнив следующие команды. Обязательно используйте настраиваемый путь установки R.

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>Предоставление доступа к настраиваемой папке установки R

> [!NOTE]
>Если выбрана установка R в папку по умолчанию **C:\Program Files\R\R-номер_версии**, этот шаг можно пропустить.

Выполните команды **icacls** из новой командной строки *с повышенными привилегиями*, чтобы предоставить разрешение на чтение и выполнение для **имени пользователя службы панели запуска SQL Server** и идентификатора безопасности **S-1-15-2-1** (**ВСЕ ПАКЕТЫ ПРИЛОЖЕНИЙ**). Имя пользователя службы панели запуска имеет вид `NT Service\MSSQLLAUNCHPAD$INSTANCENAME`, где `INSTANCENAME` — имя экземпляра SQL Server.

Эти команды будут рекурсивно предоставлять доступ ко всем файлам и папкам в указанном каталоге.

Добавьте имя экземпляра в `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`). В этом примере `INSTANCENAME` — экземпляр по умолчанию `MSSQLSERVER`.

1. Предоставьте разрешения **имени пользователя службы панели запуска SQL Server**

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. Предоставьте разрешения **идентификатору безопасности S-1-15-2-1**

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

В качестве альтернативы щелкните правой кнопкой мыши службу панели запуска SQL Server в приложении системы **Службы** и выберите команду **Перезапустить**. Или используйте [диспетчер конфигурации SQL Server](../../relational-databases/sql-server-configuration-manager.md), чтобы перезапустить службу.

## <a name="download-r-language-extension"></a>Скачивание расширения языка R

Скачайте [ZIP-файл с расширением языка R для Windows](https://github.com/microsoft/sql-server-language-extensions/releases). Рекомендуется использовать версию выпуска в рабочей среде. Используйте версию для отладки в среде разработки или тестирования, так как она предоставляет журналы с подробными сведениями, позволяющими анализировать ошибки.

## <a name="register-external-language"></a>Регистрация внешнего языка

С помощью команды [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) зарегистрируйте внешний язык для каждой базы данных, в которой необходимо использовать расширение языка. Используйте [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) для подключения к SQL Server и выполнения следующей команды T-SQL.
Измените путь в этом операторе таким образом, чтобы он отражал расположение скачанного ZIP-файла расширения языка (R-lang-extension.zip).

> [!NOTE]
>**R** — зарезервированное слово. Используйте другое имя для внешнего языка, например "myR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

Вы можете установить SQL Server на платформах Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) и Ubuntu. Дополнительные сведения см. в разделе ["Поддерживаемые платформы" в руководстве по установке SQL Server на Linux](../../linux/sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> В этой статье описывается, как установить настраиваемую среду выполнения для R на Linux. Об установке в Windows см. статью [Установка настраиваемой среды выполнения R для SQL Server в Windows](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>Контрольный список перед установкой

Перед установкой настраиваемой среды выполнения R установите следующие компоненты.

+ [SQL Server 2019 для Linux (накопительное обновление 3 или более поздней версии)](../../linux/sql-server-linux-setup.md).
Перед установкой SQL Server на Linux необходимо настроить репозиторий Майкрософт. Дополнительные сведения см. в статье [настройка репозиториев](../../linux/sql-server-linux-change-repo.md).

+ [Расширения языка SQL Server на Linux с платформой расширяемости](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [R версии 3.3 или более поздней](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-linux"></a>Добавление расширений языка SQL Server для Linux

> [!NOTE]
> Если на SQL Server 2019 установлены Службы машинного обучения, то пакет **mssql-server-extensibility** для расширений языка уже установлен и этот шаг можно пропустить.

Расширения языка используют платформу расширяемости для исполнения внешнего кода. Выполнение кода изолировано от процессов ядра, но полностью интегрировано с выполнением запросов SQL Server.

Чтобы установить расширения языка, используйте следующие команды с учетом вашей версии Linux.

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> По возможности запустите `sudo apt-get update`, чтобы обновить пакеты в системе перед установкой. Ubuntu может не иметь параметр https apt transport. Чтобы установить его, используйте `sudo apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>Suse
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Установка R

>[!NOTE]
>R для Службы машинного обучения SQL уже установлен в `/opt/microsoft/ropen/3.5.2/lib64/R`. Если вы хотите использовать этот путь в качестве R_HOME, перейдите к следующему шагу **установки Rcpp**. 

Если вы хотите использовать другую среду выполнения R, то перед продолжением установки новой версии необходимо удалить `microsoft-r-open-mro`. Пример для Ubuntu:

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

Следуйте [инструкциям](https://cran.r-project.org/bin/linux/), чтобы завершить установку R (версия 3.3 или более поздняя) для соответствующей платформы Linux. По умолчанию R устанавливается в папку **/usr/lib/R**. Этот путь — ваш **R_HOME**. Если вы установили R в другом месте, зафиксируйте этот путь в качестве R_HOME.

Примеры инструкций для Ubuntu. Измените URL-адрес репозитория ниже для вашей версии R.

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>Установка пакета Rcpp

В следующих инструкциях ${R_HOME} — это путь к месту установки R. 

+ Определите местоположения двоичного файла R в папке ${R_HOME}/bin. По умолчанию он находится в папке **/usr/lib/R/bin**.

+ Запустите R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ В этой строке подсказки R *с повышенными привилегиями* (${R_HOME}/bin/R) выполните следующий скрипт, чтобы установить пакет **Rcpp** в папку ${R_HOME}/library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>Использование настраиваемой установки R

> [!NOTE]
>Если выбрана установка R в папку по умолчанию **/usr/lib/R**, этот шаг можно пропустить.

### <a name="update-the-environment-variables"></a>Обновление переменных среды

1. Измените службу mssql-launchpadd, чтобы добавить переменную среды R_HOME в файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + Вставьте следующий текст в открывающийся файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`. Установите значение параметра R_HOME в соответствии с настраиваемым путем установки R.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + Сохраните и закройте файл.

2. Убедитесь, что можно загрузить **libR.so**.

    + Создайте файл custom-r.conf в **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + В открывшийся файл добавьте путь к **libR.so** из настраиваемой установки R.

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + Сохраните и закройте новый файл.

    + Запустите `ldconfig` и убедитесь, что можно загрузить **libR.so**, выполнив следующую команду и проверив наличие всех зависимых библиотек.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Предоставление доступа к настраиваемой папке установки R

Присвойте параметру `datadirectories` в разделе "Расширяемость" файла /var/opt/mssql/mssql.conf значение настраиваемой установки R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Перезапуск службы mssql-launchpadd

> [!NOTE]
>Если выбрана установка R в папку по умолчанию **/usr/lib/R**, этот шаг можно пропустить.

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>Скачивание расширения языка R

Скачайте [ZIP-файл с расширением языка R для Linux](https://github.com/microsoft/sql-server-language-extensions/releases). Рекомендуется использовать версию выпуска в рабочей среде. Используйте версию для отладки в среде разработки или тестирования, так как она предоставляет журналы с подробными сведениями, позволяющими анализировать ошибки.

## <a name="register-external-language"></a>Регистрация внешнего языка

С помощью команды [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) зарегистрируйте внешний язык для каждой базы данных, в которой необходимо использовать расширение языка. Используйте [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) для подключения к SQL Server и выполнения следующей команды T-SQL. 
Измените путь в этом операторе таким образом, чтобы он отражал расположение скачанного ZIP-файла расширения языка (r-lang-extension.zip).


> [!NOTE]
>**R** — зарезервированное слово. Используйте другое имя для внешнего языка, например "myR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>Включение выполнения внешнего скрипта в SQL Server

Внешний скрипт в R может быть выполнен с помощью хранимой процедуры [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), запускаемой на SQL Server. 

Чтобы включить внешние скрипты, выполните следующие команды SQL с помощью [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio), подключенного к SQL Server.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>Проверка установки расширения языка

Этот скрипт SQL проверяет успешность настраиваемой установки расширения языка R. В выходных данных этого скрипта должны присутствовать R_HOME, путь к R и версия настраиваемой среды выполнения R. Это подтверждает, что скрипт использует настраиваемую среду выполнения.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>Проверка параметров и наборов данных различных типов

Этот скрипт проверяет различные типы данных для входных и выходных параметров и наборов данных.

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>См. также раздел

+ [Платформа расширяемости в SQL Server](../concepts/extensibility-framework.md)
+ [Общие сведения о расширениях языка](../../language-extensions/language-extensions-overview.md)