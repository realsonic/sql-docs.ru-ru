---
title: Вопросы и ответы по SQL Server на Linux
description: В этой статье содержатся ответы на часто задаваемые вопросы о сервере SQL Server, работающем в Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dc5d54c3d64d43eb4b68a6762e3bd024587937ab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094654"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Часто задаваемые вопросы об SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В следующих разделах приведены ответы на распространенные вопросы о сервере SQL Server, работающем в Linux.

## <a name="general-questions"></a>Общие вопросы

1. **Какие платформы Linux поддерживаются?**

   SQL Server в настоящее время поддерживается в Red Hat Enterprise Server, SUSE Linux Enterprise Server и Ubuntu. Также поддерживается его выполнение в контейнере Docker. Актуальные сведения о поддерживаемых версиях см. в разделе [Поддерживаемые платформы](sql-server-linux-setup.md#supportedplatforms).

1. **Будет ли SQL Server на Linux работать на других платформах**?

   SQL Server протестирован и поддерживается в Linux для перечисленных дистрибутивов. Другие дистрибутивы Linux тесно связаны с ними и могут поддерживать SQL Server (например, CentOS тесно связан с Red Hat Enterprise Server). Если вы хотите установить SQL Server в неподдерживаемой операционной системе, ознакомьтесь с разделом **Политика поддержки** в статье о [технической поддержке Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server). Также обратите внимание, что для некоторых дистрибутивов Linux, разрабатываемых сообществом, официально поддержка не предоставляется, если проблема связана с базовой операционной системой.

1. **Отличается ли SQL Server на Linux от версии для Windows?**

   Основное ядро СУБД для SQL Server в Linux и Windows одинаковое. Однако некоторые функции в настоящее время не поддерживаются в Linux. Список функций, которые недоступны в Linux, см. в статье [Неподдерживаемые функции и службы](sql-server-linux-editions-and-components-2019.md#Unsupported). Кроме того, ознакомьтесь с [известными проблемами](sql-server-linux-release-notes.md#known-issues). Функции и службы SQL Server, не указанные в этих списках, поддерживаются в Linux.

1. **Какова политика поддержки для SQL Server?**

   Сведения о политике поддержки см. в статье [Политика технической поддержки для SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **У меня есть опыт работы с SQL Server в Windows. Существуют ли ресурсы, помогающие освоить SQL Server на Linux?**

   [Краткие руководства](sql-server-linux-setup.md#platforms) содержат пошаговые инструкции по установке SQL Server на Linux и выполнению запросов Transact-SQL. В других руководствах приводятся дополнительные инструкции по использованию SQL Server на Linux. Сторонний список советов см. в [списке советов MSSQLTIPS по SQL Server на Linux](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="licensing"></a>Лицензирование

1. **Как производится лицензирование в Linux?**

   SQL Server лицензируется одинаково для Linux и Windows. Вы просто получаете лицензию на SQL Server, а затем можете использовать ее на любой платформе. Дополнительные сведения см. в статье о [лицензировании SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

1. **Какой выпуск SQL Server следует выбрать после приобретения продукта?**

   При запуске программы установки mssql-conf предлагаются следующие варианты:
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   Если вы получили лицензию по программе корпоративного лицензирования в рамках Соглашения Enterprise или по подписке MSDN, необходимо выбрать один из вариантов с 4 по 7. На этом этапе вводить лицензию не нужно, однако соответствующая лицензия для вашей конфигурации уже должна быть приобретена. Если вы приобрели выпуск Standard через розничный канал, выберите вариант 8. При этом необходимо ввести ключ. 

1. **Как проверить установленную версию и выпуск SQL Server на Linux?**

   Подключитесь к экземпляру SQL Server с помощью клиентского средства, такого как **sqlcmd**, **mssql-cli** или Visual Studio Code. Затем выполните следующий запрос Transact-SQL, чтобы проверить версию и выпуск SQL Server, которые вы используете: 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>Установка

1. **Как установить SQL Server на серверах Linux?**

   Корпорация Майкрософт предоставляет репозитории пакетов для установки SQL Server и поддерживает установку с помощью собственных диспетчеров пакетов, таких как yum, zypper и apt. Инструкции по быстрой установке см. в одном из [кратких руководств](sql-server-linux-setup.md#platforms).

1. **Можно ли установить SQL Server в подсистеме Linux для Windows 10?**

   Нет. В настоящее время Linux под управлением Windows 10 не является поддерживаемой платформой для SQL Server и связанных средств.

1. **Какие файловые системы Linux можно использовать для файлов данных в SQL Server?**

   В настоящее время SQL Server на Linux поддерживает ext4 и XFS. При необходимости в будущем будет добавлена поддержка других файловых систем.

1. **Можно ли скачать установочные пакеты, чтобы установить SQL Server в автономном режиме?**

   Да. Дополнительные сведения см. по ссылкам для скачивания пакетов в [заметках о выпуске](sql-server-linux-release-notes.md). Кроме того, ознакомьтесь с [инструкциями по установке в автономном режиме](sql-server-linux-setup.md#offline).

1. **Можно ли выполнить автоматическую установку SQL Server на Linux?**

   Да. Сведения об автоматической установке см. в [руководстве по установке SQL Server на Linux](sql-server-linux-setup.md#unattended). Ознакомьтесь с примерами скриптов для [Red Hat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md) и [Ubuntu](sample-unattended-install-ubuntu.md). Вы также можете ознакомиться с [этим примером скрипта](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux), созданным группой консультантов по SQL Server.

## <a name="tools"></a>Инструменты

1. **Можно ли использовать клиент SQL Server Management Studio в Windows для доступа к SQL Server на Linux?**

   Да, вы можете использовать все существующие средства, работающие в Windows, для доступа к SQL Server на Linux. К ним относятся средства корпорации Майкрософт, такие как SQL Server Management Studio (SSMS), SQL Server Data Tools (SSDT) и OSS, а также средства сторонних разработчиков.

1. **Существует ли такой инструмент, как SSMS, для Linux?**

   Azure Data Studio — это новое кроссплатформенное средство для управления SQL Server. Дополнительные сведения см. в статье [Что такое Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. **Доступны ли в Linux такие команды, как sqlcmd и bcp?**

   Да, команды [sqlcmd и bcp](sql-server-linux-setup-tools.md) изначально доступны в Linux, macOS и Windows. Вы также можете использовать новую программу командной строки [mssql-scripter](https://github.com/Microsoft/mssql-scripter) в Linux, macOS или Windows, чтобы создавать скрипты T-SQL для баз данных SQL на любых платформах. Кроме того, ознакомьтесь с предварительным выпуском [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/).

1. **Можно ли просматривать монитор активности для экземпляра, работающего в Linux, при подключении через среду SSMS в Windows?**

   Да, среду SSMS в Windows можно использовать для удаленного подключения и применения средств и функций, таких как команды монитора активности, применительно к экземпляру Linux.

1. **Какие средства доступны для наблюдения за производительностью SQL Server в Linux?**

   Вы можете использовать [системные динамические административные представления](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) для сбора различных сведений об SQL Server, включая сведения о процессах Linux. Для повышения производительности запросов можно использовать [хранилище запросов](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md). Другие средства, такие как встроенная [панель мониторинга производительности](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in), работают удаленно в SQL Server Management Studio (SSMS) из Windows.

   > [!TIP]
   > Одним из способов повышения производительности является правильная настройка операционной системы Linux и экземпляра SQL Server. Дополнительные сведения см. в статье [Рекомендации по производительности и конфигурации для SQL Server на Linux](sql-server-linux-performance-best-practices.md).

## <a name="administration"></a>Администрирование

1. **Предлагает ли корпорация Майкрософт такое приложение, как диспетчер конфигурации SQL Server, для Linux?**

   Да, для SQL Server на Linux существует средство настройки [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **Поддерживает ли SQL Server на Linux несколько экземпляров в одном узле?**

   Несколько экземпляров в узле рекомендуется запускать в отдельных контейнерах. Это можно легко сделать с помощью Docker, но каждый контейнер должен ожидать передачи данных через отдельный порт. Дополнительные сведения см. в статье [Запуск нескольких контейнеров SQL Server](./sql-server-linux-docker-container-deployment.md#multiple).

1. **Поддерживается ли проверка подлинности Active Directory в Linux?**

   Да. Дополнительные сведения см. в статье [Проверка подлинности Active Directory с SQL Server на Linux](sql-server-linux-active-directory-authentication.md).

1. **Поддерживаются ли Always On и кластеризация в Linux?**

   Отказоустойчивая кластеризация и высокий уровень доступности в Linux обеспечиваются с помощью Pacemaker. Дополнительные сведения см. в статье [Непрерывность бизнес-процессов и восстановление базы данных — SQL Server на Linux](sql-server-linux-business-continuity-dr.md).

1. **Можно ли настроить репликацию из Linux в Windows и наоборот?**

   Для односторонней репликации данных можно использовать реплики масштабирования для чтения между Windows и Linux.

1. **Можно ли перенести существующие базы данных в более ранних версиях SQL Server из Windows в Linux?**

   Да, это можно сделать [несколькими способами](sql-server-linux-migrate-overview.md).

1. **Можно ли переносить данные из Oracle и других СУБД в SQL Server на Linux?**

   Да. SSMA поддерживает перенос из нескольких СУБД: Microsoft Access, DB2, MySQL, Oracle и SAP ASE (прежнее название — SAP Sybase ASE). Пример использования SSMA см. в статье [Перенос схемы Oracle в SQL Server на Linux посредством Помощника по миграции SQL Server](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json).

1. **Какие разрешения требуются для файлов SQL Server?**

   Все файлы в папке `/var/opt/mssql` должны принадлежать пользователю **mssql** и относиться к группе **mssql**. Как пользователь, так и группа **mssql** должны иметь разрешения на чтение и запись для всех файлов и каталогов. Обратите внимание на указанные ниже особые сценарии, касающиеся разрешений для файлов и каталогов.

   * Пользователь и группа mssql должны иметь разрешения для подключенных сетевых папок, которые используются для хранения файлов SQL Server.
   * Если файлы или резервные копии базы данных находятся в каталоге, отличном от каталога по умолчанию, необходимо задать разрешения и для этого каталога.
   * Если изменить корневую маску umask по умолчанию с 0022, то после установки SQL Server выполнить настройку не удастся. Необходимо будет вручную предоставить необходимые разрешения стартовой учетной записи SQL Server.

1. **Можно ли назначить другого владельца файлов и каталогов SQL Server вместо установленной учетной записи и группы mssql?**

   Изменение владельца по умолчанию каталога и файлов SQL Server не поддерживается. Учетная запись и группа mssql предназначены специально для SQL Server, и интерактивный вход для них невозможен.
   
 1. **Поддерживаются ли символьные ссылки для каталогов журналов и данных SQL Server?** 
    
    Нет, символьные ссылки не поддерживаются для каталогов журналов и данных SQL Server. Сведения об изменении расположения для таких каталогов по умолчанию см. в разделе [Изменение каталога по умолчанию для данных или журналов](sql-server-linux-configure-mssql-conf.md#datadir).
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]