---
title: Заметки о выпуске SQL Server 2017 | Документация Майкрософт
description: В этой статье описываются ограничения и проблемы с SQL Server 2017 и приводятся ссылки на соответствующие сведения.
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: 0c70b2a7202ea7bcd14128a544487ec011c9c7b8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643339"
---
# <a name="sql-server-2017-release-notes"></a>Заметки о выпуске SQL Server 2017
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
В этом разделе описываются ограничения и проблемы, связанные с SQL Server 2017. Связанные сведения:
- [Новые возможности в SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Заметки о выпуске для SQL Server в Linux](../linux/sql-server-linux-release-notes.md)
- [Накопительные обновления SQL Server 2017](https://aka.ms/sql2017cu) — сведения о последнем выпуске накопительного обновления

**Оцените SQL Server!**
- [![Скачать из Evaluation Center](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [Скачать SQL Server 2017](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![Создать виртуальную машину](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [Развернуть виртуальную машину с SQL Server 2017](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> Предварительная версия SQL Server 2019 теперь доступна. Дополнительные сведения см. в статье [Новые возможности в SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server 2017 — общедоступный выпуск (октябрь 2017 г.)
### <a name="database-engine"></a>Компонент Database Engine

- **Проблема и последствия для клиентов**: после обновления существующая сетевая папка FILESTREAM может стать недоступной.

- **Решение:** Сначала перезагрузите компьютер и проверьте доступность сетевой папки FILESTREAM. Если она по-прежнему недоступна, выполните следующие действия.

    1. В диспетчере конфигурации SQL Server щелкните экземпляр SQL Server правой кнопкой мыши и выберите пункт **Свойства**. 
    2. На вкладке **FILESTREAM** снимите флажок **Разрешить FILESTREAM при потоковом доступе файлового ввода-вывода**, а затем нажмите кнопку **Применить**.
    3. Снова установите флажок **Разрешить FILESTREAM при потоковом доступе файлового ввода-вывода** для имени исходной общей папки и нажмите кнопку **Применить**.

### <a name="master-data-services-mds"></a>Службы Master Data Services (MDS)
- **Проблема и последствия для клиентов:**  когда на странице разрешений пользователя предоставляется разрешение для корневого уровня в представлении сущностей в виде дерева, отображается ошибка `"The model permission cannot be saved. The object guid is not valid"`.

- **Решение:** 
  - Предоставьте разрешение для подузлов в представлении в виде дерева, а не для корневого уровня.

### <a name="analysis-services"></a>Службы Analysis Services
- **Проблема и последствия для клиентов**: соединители данных для приведенных ниже источников еще недоступны для табличных моделей на уровне совместимости 1400.
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **Решение:** Нет.   

- **Проблема и последствия для клиентов**: в моделях прямых запросов на уровне совместимости 1400 с перспективами может возникнуть сбой при запросе или обнаружении метаданных.
- **Решение:** удалите перспективы и повторите развертывание.

### <a name="tools"></a>Инструменты
- **Проблема и последствия для клиентов**: Запуск *DReplay* завершается сбоем со следующим сообщением: "Error DReplay Unexpected error occurred!" (Произошла непредвиденная ошибка с DReplay).
- **Решение:** Нет.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>Релиз-кандидат SQL Server 2017 (RC2 — август 2017 г.)
Для этого выпуска нет заметок о выпуске SQL Server на платформе Windows. См. [заметки о выпуске SQL Server на платформе Linux](../linux/sql-server-linux-release-notes.md).


![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>Релиз-кандидат SQL Server 2017 (RC1 — июль 2017 г.)
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS) (RC1 — июль 2017 г.)
- **Проблема и последствия для клиентов**: Параметр *runincluster* хранимой процедуры **[catalog].[create_execution]** переименован в *runinscaleout* для согласованности и удобства чтения.
- **Решение:** Если у вас есть скрипты для запуска пакетов в развертывании с горизонтальным увеличением масштаба, нужно изменить имя параметра с *runincluster* на *runinscaleout*, чтобы они работали в RC1.

- **Проблема и последствия для клиентов**: SQL Server Management Studio (SSMS) 17.1 и более ранние версии не могут активировать выполнение пакета в развертывании с горизонтальным увеличением масштаба в RC1. Сообщение об ошибке: " *\@runincluster* не является параметром процедуры **create_execution**". Эта проблема будет исправлена в следующем выпуске SSMS, в версии 17.2. Версии SSMS, начиная с 17.2, поддерживают новое имя параметра и выполнение пакетов в развертывании с горизонтальным увеличением масштаба. 
- **Решение:** пока не станет доступна версия SSMS 17.2, используйте следующие инструкции:
  1. Используйте существующую версию SSMS, чтобы создать скрипт выполнения пакета.
  2. Измените в скрипте имя параметра *runincluster* на *runinscaleout*.
  3. Выполните скрипт.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1 (май 2017 г.)
### <a name="documentation-ctp-21"></a>Документация (CTP 2.1)
- **Проблема и последствия для клиентов**: Объем документации по [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] ограничен, материалы включены в набор документации по [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)].  Содержимое статей, относящееся к [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)], отмечено с помощью раздела **Область применения**. 
- **Проблема и последствия для клиентов**: доступна только электронная документация по [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="sql-server-reporting-services-ctp-21"></a>Службы SQL Server Reporting Services (CTP 2.1)

- **Проблема и последствия для клиентов**: если серверы отчетов SQL Server Reporting Services и Power BI установлены на одном и том же компьютере и один из них будет удален, вы не сможете подключиться к оставшемуся серверу отчетов с помощью Configuration Manager для сервера отчетов.
- **Обходной путь.** Чтобы обойти эту проблему, после удаления одного из этих серверов выполните указанные ниже операции.

    1. Запустите командную строку с правами администратора.
    2. Откройте каталог, в который установлен оставшийся сервер отчетов.

        *Сервер отчетов Power BI по умолчанию установлен в этом расположении: C:\Program Files\Microsoft Power BI Report Server*.

        *Среда SQL Server Reporting Services по умолчанию установлена в этом расположении: C:\Program Files\Microsoft SQL Server Reporting Services*.

    3. Затем перейдите к следующей папке (*SSRS* или *PBIRS* в зависимости от оставшегося сервера отчетов).
    4. Перейдите в папку WMI.
    5. Выполните следующую команду:

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        Если появится указанное ниже сообщение об ошибке, его можно проигнорировать.

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **Проблема и последствия для клиентов**: после установки на компьютер с установленной версией *TSqlLanguageService.msi* 2016 (с помощью программы установки SQL или из отдельного дистрибутива) с него удаляются сборки *Microsoft.SqlServer.Management.SqlParser.dll* и *Microsoft.SqlServer.Management.SystemMetadataProvider.dll* версии 13.* (SQL 2016). Любое приложение с зависимостью от версий 2016 этих сборок прекращает работу, и отображается сообщение о том, что  *не удалось загрузить файл или сборку Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91 или одну из ее зависимостей. Системе не удается найти указанный файл.*

   Кроме того, попытки переустановить TSqlLanguageService.msi версии 2016 завершаются ошибкой с сообщением о том, что *не удалось установить компонент Служба языка T-SQL Microsoft SQL Server 2016, так как на этом компьютере уже установлена более поздняя версия*.

- **Обходной путь.** Чтобы обойти эту проблему и устранить неполадки с приложением, которое зависит от версии 13 указанных сборок, выполните следующие действия:

   1. Откройте раздел **Установка и удаление программ**.
   2. Найдите *Языковую службу T-SQL Microsoft SQL Server 2019 CTP 2.1*, щелкните ее правой кнопкой мыши и выберите команду **Удалить**.
   3. Когда компонент будет удален, восстановите неисправное приложение или переустановите соответствующую версию *TSqlLanguageService.MSI*.

   В результате выполнения этих действий версия 14 указанных сборок будет удалена, так что все приложения, которые зависят от версии 14, перестанут функционировать. Если вам требуются эти сборки, необходимо установить их отдельно, а не параллельно с установками версии 2016.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0 (апрель 2017 г.)
### <a name="documentation-ctp-20"></a>Документация (CTP 2.0)
- **Проблема и последствия для клиентов**: Объем документации по [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] ограничен, материалы включены в набор документации по [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)].  Содержимое статей, относящееся к [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)], отмечено с помощью раздела **Область применения**. 
- **Проблема и последствия для клиентов**: доступна только электронная документация по [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="always-on-availability-groups"></a>Группы доступности AlwaysOn

- **Проблема и последствия для клиентов**: работа экземпляра SQL Server, в котором размещена вторичная реплика группы доступности, завершается сбоем, если основная версия SQL Server ниже версии экземпляра, в котором находится первичная реплика. Это касается обновлений из всех поддерживаемых версий SQL Server, в которых хранятся группы доступности, на SQL Server [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0. Эта проблема возникает при следующих условиях. 

> 1. Пользователь обновляет экземпляр SQL Server, в котором находится вторичная реплика, в соответствии с [рекомендациями](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).
> 2. После обновления происходит сбой, и до тех пор, пока не будут обновлены все вторичные реплики в группе доступности, обновленная вторичная реплика становится первичной. При этом прежняя первичная реплика становится вторичной репликой, версия которой ниже, чем версия первичной реплики.
> 3. Группа доступности получает неподдерживаемую конфигурацию, а все оставшиеся вторичные реплики становятся подверженными сбоям. 

- **Обходной путь.** Подключитесь к экземпляру SQL Server, в котором находится новая первичная реплика, и удалите неисправную вторичную реплику из конфигурации.

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   Это восстановит экземпляр SQL Server, в котором находится вторичная реплика.

## <a name="more-information"></a>Дополнительные сведения
- [Заметки о выпуске служб SQL Server Reporting Services](../reporting-services/release-notes-reporting-services.md).
- [Known Issues for Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md) (Известные проблемы в службах машинного обучения)
- [Ссылки и сведения для всех поддерживаемых версий в Центре обновления SQL Server](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)