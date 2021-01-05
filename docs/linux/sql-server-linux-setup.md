---
title: Руководство по установке SQL Server на Linux
titleSuffix: SQL Server
description: Установка, обновление и удаление SQL Server на Linux. В этой статье рассматриваются сценарии сетевой, автономной и автоматической установки.
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: 4abb6180d60b149fe8b1b18118c98afd04c2e2e8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642420"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Руководство по установке SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье приводятся инструкции по установке, обновлению и удалению SQL Server 2017 и SQL Server 2019 на Linux.

Дополнительные сведения о других сценариях развертывания см. в следующих источниках:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Контейнеры Docker](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes — кластеры больших данных](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> Здесь рассматривается несколько сценариев развертывания. Если вам нужны пошаговые инструкции по установке, перейдите к одному из приведенных далее кратких руководств.
> - [Краткое руководство по RHEL](quickstart-install-connect-red-hat.md)
> - [Краткое руководство по SLES](quickstart-install-connect-suse.md)
> - [Краткое руководство по Ubuntu](quickstart-install-connect-ubuntu.md)
> - [Краткое руководство по Docker](quickstart-install-connect-docker.md)

Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](../linux/sql-server-linux-faq.md).

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> Поддерживаемые платформы

SQL Server поддерживается на платформах Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) и Ubuntu. Он также поддерживается в виде образа Docker, который можно запускать в подсистеме Docker в Linux или Docker для Windows или Mac.

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Платформа | Поддерживаемые версии | Получить
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3, 7.4, 7.5, 7.6, 8  | [Получить RHEL 7.6](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 с пакетом обновления 2 (SP2) | [Получить SLES v12 с пакетом обновления 2 (SP2)](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Получить Ubuntu 16.04](http://releases.ubuntu.com/xenial/)
| **Подсистема Docker** | 1.8 и выше | [Получить Docker](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Корпорация Майкрософт также поддерживает развертывание контейнеров SQL Server и управление ими с помощью OpenShift и Kubernetes.

> [!NOTE]
> SQL Server протестирован и поддерживается в Linux для перечисленных дистрибутивов. Чтобы установить SQL Server в неподдерживаемой операционной системе, ознакомьтесь с разделом **Политика поддержки** в статье о [технической поддержке Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="system-requirements"></a><a id="system"></a> Требования к системе

Ниже перечислены требования к системе для установки SQL Server на Linux:

|||
|-----|-----|
| **Память** | 2 ГБ |
| **Файловая система** | **XFS** или **EXT4** (другие файловые системы, такие как **BTRFS**, не поддерживаются) |
| **Место на диске** | 6 ГБ |
| **Частота процессора** | 2 ГГц |
| **Ядра процессора** | 2 ядра |
| **Тип процессора** | только совместимый с x64 |

При использовании удаленных общих папок **NFS** в рабочей среде необходимо обратить внимание на следующие требования к поддержке.

- Версия NFS должна быть **4.2 или более поздняя**. Более старые версии NFS не поддерживают необходимые функции, такие как использование команды fallocate и создание разреженных файлов, общие для современных файловых систем.
- При подключении NFS следует указать только каталоги **/var/opt/mssql**. Другие файлы, например системные двоичные файлы SQL Server, не поддерживаются.
- При подключении удаленной общей папки клиенты NFS должны использовать параметр nolock.

## <a name="configure-source-repositories"></a><a id="repositories"></a> Настройка исходных репозиториев

При установке или обновлении SQL Server вы получите последнюю версию SQL Server из настроенного репозитория Майкрософт. В кратких руководствах используется репозиторий накопительного обновления **CU** для SQL Server. Но вместо этого можно настроить репозиторий **GDR**. Дополнительные сведения о репозиториях и их настройке см. в статье [Настройка репозиториев для установки и обновления SQL Server на Linux.](sql-server-linux-change-repo.md)

## <a name="install-sql-server"></a><a id="platforms"></a> Установка SQL Server

Вы можете установить SQL Server 2017 или SQL Server 2019 на Linux из командной строки. Пошаговые инструкции см. в следующих кратких руководствах.

| Платформа | Краткие руководства по установке |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SUSE Linux Enterprise Server (SLES) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

Вы также можете запустить SQL Server на Linux на виртуальной машине Azure. Дополнительные сведения см. в статье [Подготовка виртуальной машины SQL в Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json).

После установки рекомендуется внести дополнительные изменения в конфигурацию для достижения оптимальной производительности. Дополнительные сведения см. в статье [Рекомендации по производительности и конфигурации для SQL Server на Linux](sql-server-linux-performance-best-practices.md).

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> Обновление SQL Server

Чтобы обновить пакет **mssql-server** до последнего выпуска, используйте одну из следующих команд в зависимости от платформы:

| Платформа | Команды для обновления пакета |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Эти команды скачивают новейшие пакеты и заменяют двоичные файлы, расположенные в папке `/opt/mssql/`. Эта операция не влияет на созданные пользователем базы данных и системные базы данных.

Чтобы обновить SQL Server, сначала [измените настроенный репозиторий](sql-server-linux-change-repo.md) на нужную версию SQL Server. Затем используйте ту же команду **update**, чтобы обновить версию SQL Server. Это возможно только в том случае, если два репозитория поддерживают этот вариант обновления.

## <a name="rollback-sql-server"></a><a id="rollback"></a> Откат SQL Server

Чтобы выполнить откат или перейти на использование предыдущего выпуска SQL Server, выполните следующие действия.

1. Определите номер версии для пакета SQL Server, на который будет выполняться возврат. Список номеров пакетов см. в [заметках о выпуске](../linux/sql-server-linux-release-notes.md).

1. Перейдите на предыдущую версию SQL Server. В следующих командах замените `<version_number>` номером версии SQL Server, который был определен на шаге 1.

   | Платформа | Команды для обновления пакета |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> Поддерживается только переход на использование более раннего выпуска с тем же основным номером версии, например SQL Server 2019.

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> Проверка установленной версии SQL Server

Чтобы проверить текущую версию и выпуск SQL Server на Linux, выполните следующую процедуру.

1. Установите [программы командной строки SQL Server](sql-server-linux-setup-tools.md), если это еще не сделано.

1. С помощью программы **sqlcmd** выполните команду Transact-SQL, которая выводит версию и выпуск SQL Server.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> Удаление SQL Server

Чтобы удалить пакет **mssql-server** на Linux, используйте одну из следующих команд в зависимости от платформы:

| Платформа | Команды для удаления пакетов |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

При удалении пакета созданные файлы базы данных не удаляются. Чтобы удалить файлы базы данных, выполните следующую команду:

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a> Автоматическая установка

Автоматическая установка выполняется следующим образом.

- Выполните начальные шаги в [кратких руководствах](#platforms), чтобы зарегистрировать репозитории и установить SQL Server.
- При запуске `mssql-conf setup` задайте [переменные среды](sql-server-linux-configure-environment-variables.md) и используйте параметр `-n` (запросы выводиться не будут).

В следующем примере показана настройка выпуска SQL Server Developer с помощью переменной среды **MSSQL_PID**. В нем также принимаются условия лицензионного соглашения (**ACCEPT_EULA**) и задается пароль системного администратора (**MSSQL_SA_PASSWORD**). Параметр `-n` выполняет установку без вывода запросов, где значения конфигурации извлекаются из переменных среды.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

Можно также создать сценарий, выполняющий другие действия. Например, установку других пакетов SQL Server.

Более подробный пример сценария см. в следующих примерах:

- [Сценарий автоматической установки Red Hat](sample-unattended-install-redhat.md)
- [Сценарий автоматической установки SUSE](sample-unattended-install-suse.md)
- [Сценарий автоматической установки Ubuntu](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> Автономная установка

Если компьютер Linux не имеет доступа к онлайн-репозиториям, которые используются в [кратких руководствах](#platforms), вы можете скачать файлы пакетов напрямую. Эти пакеты находятся в репозитории Майкрософт по адресу [https://packages.microsoft.com](https://packages.microsoft.com).

> [!TIP]
> После успешного выполнения действий по установке, приведенных в кратких руководствах, скачивать или вручную устанавливать пакеты SQL Server не требуется. Сведения в этом разделе актуальны только для автономных сценариев.

1. **Скачайте пакет ядра СУБД для своей платформы**. Ссылки для скачивания пакета находятся в разделе сведений о пакете в [заметках о выпуске](../linux/sql-server-linux-release-notes.md).

1. **Переместите скачанный пакет на компьютер Linux**. Если для скачивания пакетов вы использовали другой компьютер, переместить пакеты на компьютер Linux можно с помощью команды **scp**.

1. **Установите пакет ядра СУБД**. В зависимости от платформы выполните одну из приведенных ниже команд. Замените имя файла пакета в этом примере именем скачанного пакета.

   | Платформа | Команда для установки пакета |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > Пакеты RPM (RHEL и SLES) можно установить с помощью команды `rpm -ivh`, однако команды из предыдущей таблицы автоматически устанавливают зависимости (если они доступны) из утвержденных репозиториев.

1. **Разрешите отсутствующие зависимости**. На этом этапе зависимости могут отсутствовать. Если это не так, пропустите этот шаг. Если в Ubuntu у вас есть доступ к утвержденным репозиториям, содержащим эти зависимости, самым простым решением является использование команды `apt-get -f install`. Эта команда также завершает установку SQL Server. Чтобы проверить зависимости вручную, выполните приведенные ниже команды.

   | Платформа | Команда для вывода списка зависимостей |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   После разрешения отсутствующих зависимостей попытайтесь установить пакет mssql-server еще раз.

1. **Завершите установку SQL Server**. Для завершения установки SQL Server воспользуйтесь средством **mssql-conf**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>Лицензирование и цены

SQL Server лицензируется одинаково для Linux и Windows. Дополнительные сведения о лицензировании и ценах на SQL Server см. на странице о [лицензировании SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

## <a name="optional-sql-server-features"></a>Дополнительные функции и компоненты SQL Server

После установки SQL Server можно установить или включить дополнительные функции.

- [Программы командной строки SQL Server](sql-server-linux-setup-tools.md)
- [Агент SQL Server](sql-server-linux-setup-sql-agent.md)
- [Полнотекстовый поиск SQL Server](sql-server-linux-setup-full-text-search.md)
- [Службы машинного обучения (R, Python)](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.md).
