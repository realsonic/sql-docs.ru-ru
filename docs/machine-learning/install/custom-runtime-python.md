---
title: Установка настраиваемой среды выполнения Python
description: Узнайте, как установить настраиваемую среду выполнения Python для SQL Server с помощью расширений языка. Настраиваемую среду выполнения Python можно использовать для машинного обучения.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804335"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Установка настраиваемой среды выполнения Python для SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

В этой статье объясняется, как установить настраиваемую среду выполнения Python для запуска внешних скриптов Python в SQL Server. Настраиваемая среда выполнения использует [расширения языка SQL Server](../../language-extensions/language-extensions-overview.md) и позволяет выполнять скрипты машинного обучения.

Настраиваемая среда выполнения Python позволяет использовать для SQL Server собственную версию среды выполнения Python, а не версию среды выполнения по умолчанию, устанавливаемую вместе со [Службами машинного обучения SQL Server](../sql-server-machine-learning-services.md).

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>Предварительные требования

Перед установкой настраиваемой среды выполнения Python установите следующие компоненты.

+ Установите [накопительное обновление 3 или более поздней версии](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) для SQL Server 2019.

+ Установите на сервере [Python 3.7](https://www.python.org/downloads/).

    Расширение языка Python, используемое для настраиваемой среды выполнения Python, сейчас поддерживает только Python 3.7. Если вы хотите использовать другую версию Python, выполните инструкции из [репозитория расширения языка Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), чтобы изменить и скомпилировать расширение.

    > [!IMPORTANT]
    > Во время установки Python установите флажок **Add Python 3.7 to PATH** (Добавить Python 3.7 в переменную PATH).

## <a name="install-language-extensions"></a>Установка расширений языка

> [!NOTE]
> Если в SQL Server 2019 установлены [Службы машинного обучения](../sql-server-machine-learning-services.md), значит, расширения языка уже установлены и этот шаг можно пропустить.

Выполните описанные ниже действия, чтобы установить [расширения языка SQL Server](../../language-extensions/language-extensions-overview.md), которые используются для настраиваемой среды выполнения Python.

1. Запустите мастер установки SQL Server 2019.
  
1. На вкладке **Установка** выберите параметр **Новая установка изолированного экземпляра SQL Server или добавление компонентов к существующей установке**.

1. На странице **Выбор компонентов** выберите следующие компоненты:
  
    + **Службы ядра СУБД**
  
        Чтобы использовать расширения языка с SQL Server, необходимо установить экземпляр ядра СУБД. Можно использовать новый или уже существующий экземпляр.
  
    + **Службы машинного обучения и расширения языка**

        Выберите **Службы машинного обучения и расширения языка**. Не выбирайте Python, так как настраиваемую среду выполнения Python вы установите позже.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Установка расширений языка для SQL Server 2019.":::

1. На странице **Все готово для установки** проверьте, включены ли указанные ниже компоненты, и нажмите **Установить**.
  
    + Службы ядра СУБД
    + Службы машинного обучения и расширения языка

1. Когда установка завершится, перезагрузите компьютер, если появится соответствующее предложение.

> [!IMPORTANT]
> Если вы устанавливаете новый экземпляр SQL Server 2019 с расширениями языка, обязательно установите [накопительный пакет обновления 3 или более поздней версии](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md), прежде чем перейти к следующему шагу.

## <a name="install-pandas"></a>Установка Pandas

Установите пакет [Pandas](https://pandas.pydata.org/) для Python из командной строки *с повышенными привилегиями*:

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>Добавление переменной среды

Добавьте или измените системную переменную среды **PYTHONHOME**.

1. В поле поиска Windows введите слово *среда* и выберите элемент **Изменение системных переменных среды**.
1. На вкладке **Дополнительно** выберите **Переменные среды**.
1. В разделе **Системные переменные** выберите элемент **Создать**, чтобы создать **PYTHONHOME** с указанием на расположение установки Python 3.7. Если PYTHONHOME уже существует, выберите **Изменить**, чтобы он указывал на расположение установки Python 3.7.
1. Нажмите кнопку **ОК**, чтобы закрыть все окна.

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="Переменная среды PYTHONHOME.":::

## <a name="grant-access-to-python-folder"></a>Предоставление доступа к папке Python

Выполните приведенные ниже команды **icacls** из нового окна командной строки *с повышенными привилегиями*, чтобы предоставить разрешения на доступ для **чтения и выполнения** к папке **PYTHONHOME** для **службы панели запуска SQL Server** и идентификатора безопасности **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

1. Предоставьте разрешения **имени пользователя службы панели запуска SQL Server**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Для именованного экземпляра с именем **SQL01** используется команда `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T`.

2. Предоставьте разрешения **идентификатору безопасности S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Приведенная выше команда предоставляет разрешения идентификатору безопасности компьютера **S-1-15-2-1**, что эквивалентно разрешению **ALL APPLICATION PACKAGES** (Все пакеты приложений) в английской версии Windows. Кроме того, можно использовать `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` в английской версии Windows.

## <a name="restart-sql-server-launchpad"></a>Перезапуск панели запуска SQL Server

Выполните описанные ниже действия, чтобы перезапустить службу "Панель запуска SQL Server".

1. Откройте [Диспетчер конфигурации SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. В разделе **Службы SQL Server** щелкните элемент **Панель запуска SQL Server (MSSQLSERVER)** правой кнопкой мыши и выберите действие **Перезапустить**. Если используется именованный экземпляр, вместо **(MSSQLSERVER)** будет отображаться имя этого экземпляра.

## <a name="register-language-extension"></a>Регистрация расширения языка

Выполните описанные ниже действия, чтобы скачать и зарегистрировать расширение языка Python, которое используется для настраиваемой среды выполнения Python.

1. Скачайте файл **python-lang-extension-windows.zip** из [репозитория GitHub для расширений языка SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Для среды разработки или тестирования также можно использовать отладочную версию (**python-lang-extension-windows-debug.zip**). Отладочная версия сохраняет в журнал подробные сведения, которые помогают изучать ошибки, и не рекомендуется для рабочих сред.

1. С помощью [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) подключитесь к экземпляру SQL Server и выполните приведенную ниже команду T-SQL, чтобы зарегистрировать расширение языка Python с помощью инструкции [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Измените путь в этой инструкции таким образом, чтобы он указывал расположение скачанного ZIP-файла для расширения языка (**python-lang-extension-windows.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    Выполните эту инструкцию для каждой базы данных, в которой вы намерены использовать расширение языка Python.

    > [!NOTE]
    > **Python** является зарезервированным словом, то есть вы не сможете указать такое имя для нового внешнего языка. Выберите другое имя. Например, в приведенной выше инструкции это имя **myPython**.

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>Предварительные требования

Перед установкой настраиваемой среды выполнения Python установите следующие компоненты.

+ Установите SQL Server 2019 для Linux. Вы можете установить SQL Server на платформах Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) и Ubuntu. Дополнительные сведения см. в статье [Руководство по установке SQL Server на Linux](../../linux/sql-server-linux-setup.md).

+ Перейдите на накопительное обновление 3 или более поздней версии для SQL Server 2019. Выполните следующие действия:
    1. Настройте репозитории для накопительных обновлений. Дополнительные сведения см. в статье [Настройка репозиториев для установки и обновления SQL Server на Linux](../../linux/sql-server-linux-change-repo.md).

    1. Обновите пакет **mssql-server** до последнего накопительного обновления. Дополнительные сведения см. в [разделе об обновлении SQL Server в руководстве по установке SQL Server на Linux](../../linux/sql-server-linux-setup.md#upgrade).

+ Установите на сервере [Python 3.7](https://www.python.org/downloads/).

    Расширение языка Python, используемое для настраиваемой среды выполнения Python, сейчас поддерживает только Python 3.7. Если вы хотите использовать другую версию Python, выполните инструкции из [репозитория расширения языка Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), чтобы изменить и скомпилировать расширение.

## <a name="install-language-extensions"></a>Установка расширений языка

> [!NOTE]
> Если в SQL Server 2019 установлены [Службы машинного обучения](../sql-server-machine-learning-services.md), значит, пакет **mssql-server-extensibility** для расширений языка уже установлен и этот шаг можно пропустить.

Выполните приведенные ниже команды, чтобы установить [расширения языка SQL Server](../../language-extensions/language-extensions-overview.md) для Linux, которые используются в настраиваемой среде выполнения Python.

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. Если возможно, перед установкой выполните эту команду для обновления пакетов в системе.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu может не иметь параметр https apt transport. Чтобы установить его, выполните приведенную ниже команду.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Установите **mssql-server-extensibility** с помощью этой команды.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>Установка Python 3.7 и Pandas

Установите Python 3.7, библиотеку libpython3.7 и пакет Pandas. 

Ниже приведены примеры команд для Ubuntu.

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## <a name="custom-installation-of-python"></a>Настраиваемая установка Python

> [!NOTE]
> Если вы установили Python 3.7 в расположение `/usr/lib/python3.7` по умолчанию, этот раздел можно пропустить и сразу перейти к разделу [Регистрация расширения языка](#register-language-extension-linux).

Если вы скомпилировали собственную версию Python 3.7, выполните приведенные ниже команды, чтобы сообщить SQL Server о настраиваемой установке.

### <a name="add-environment-variable"></a>Добавление переменной среды

Измените службу **mssql-launchpadd**, добавив переменную среды **PYTHONHOME** в файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Откройте файл с помощью systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Вставьте следующий текст в открывающийся файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`. Установите значение параметра **PYTHONHOME** в соответствии с путем установки настраиваемой версии Python.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Сохраните файл и закройте редактор.

Убедитесь, что удается загрузить `libpython3.7m.so.1.0`.

1. Создайте файл custom-python.conf в `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. В открывшийся файл добавьте путь к **libpython3.7m.so.1.0** из настраиваемой установки Python.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Сохраните новый файл и закройте редактор.

1. Запустите `ldconfig` и убедитесь, что можно загрузить `libpython3.7m.so.1.0`, выполнив следующую команду и проверив наличие всех зависимых библиотек.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Предоставление доступа к папке Python

Присвойте параметру `datadirectories` в разделе "Расширяемость" файла /var/opt/mssql/mssql.conf значение настраиваемой установки Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Перезапуск службы mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Регистрация расширения языка

Выполните описанные ниже действия, чтобы скачать и зарегистрировать расширение языка Python, которое используется для настраиваемой среды выполнения Python.

1. Скачайте файл **python-lang-extension-linux.zip** из [репозитория GitHub для расширений языка SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Для среды разработки или тестирования также можно использовать отладочную версию (**python-lang-extension-linux-debug.zip**). Отладочная версия сохраняет в журнал подробные сведения, которые помогают изучать ошибки, и не рекомендуется для рабочих сред.

1. С помощью [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) подключитесь к экземпляру SQL Server и выполните приведенную ниже команду T-SQL, чтобы зарегистрировать расширение языка Python с помощью инструкции [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Измените в этом операторе путь таким образом, чтобы он отражал расположение скачанного ZIP-файла расширения языка (**python-lang-extension-linux.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    Выполните эту инструкцию для каждой базы данных, в которой вы намерены использовать расширение языка Python.

    > [!NOTE]
    > **Python** является зарезервированным словом, то есть вы не сможете указать такое имя для нового внешнего языка. Выберите другое имя. Например, в приведенной выше инструкции это имя **myPython**.

::: zone-end

## <a name="enable-external-script"></a>Включение внешнего скрипта

Внешний скрипт Python можно выполнить с помощью хранимой процедуры [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Чтобы включить внешние скрипты, выполните с помощью [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) приведенную ниже инструкцию.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Проверка установки

Используйте приведенный ниже скрипт SQL для проверки установки и функций настраиваемой среды выполнения Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Установка настраиваемой среды выполнения R для SQL Server](custom-runtime-r.md)
+ [Платформа расширяемости в SQL Server](../concepts/extensibility-framework.md)
+ [Общие сведения о расширениях языка](../../language-extensions/language-extensions-overview.md)
