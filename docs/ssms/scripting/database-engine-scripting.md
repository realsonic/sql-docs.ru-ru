---
title: Работа со сценариями компонента Database Engine
description: Узнайте, как использовать среду сценариев Microsoft PowerShell для управления экземплярами ядра СУБД SQL Server, а также как создавать и выполнять запросы к ядру СУБД, содержащие Transact-SQL и XQuery.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], PowerShell
- scripts [SQL Server]
- scripting [SQL Server Database Engine]
- scripting [SQL Server Database Engine], PowerShell
ms.assetid: 9978a884-59a2-4e7f-a82a-335149f3a261
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 54297946ffc20ec7699536e5bd5c0504497d1f84
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642417"
---
# <a name="database-engine-scripting"></a>Работа со сценариями компонента Database Engine
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] поддерживает среду скриптов [!INCLUDE[msCoName](../../includes/msconame-md.md)] PowerShell для управления экземплярами компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] и объектами в экземплярах. Можно также строить и запускать запросы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , содержащие [!INCLUDE[tsql](../../includes/tsql-md.md)] и XQuery, в средах, подобных средам сценариев.  
  
## <a name="sql-server-powershell"></a>SQL Server PowerShell  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] включает две оснастки PowerShell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые реализуют следующее:  
  
-   Поставщик [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell, отображающий иерархии моделей управляющих объектов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в виде путей PowerShell, подобных путям файловой системы. С помощью классов модели управляющих объектов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно управлять объектами, представленными на каждом узле пути.  
  
-   Набор командлетов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , реализующих команды [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Одним из командлетов является **Invoke-Sqlcmd**. Он используется для запуска скриптов запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , выполняемых с помощью программы **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает эти возможности для запуска PowerShell.  
  
-   Модуль PowerShell **sqlps** , который может быть импортирован в сеанс PowerShell, после чего модуль загружает оснастки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Можно запускать нерегламентированные команды PowerShell в интерактивном режиме. Файлы скриптов можно запускать с помощью команды вида «.\MyFolder\MyScript.ps1».  
  
-   Файлы скриптов PowerShell можно использовать в качестве ввода для шагов заданий PowerShell агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые запускают скрипты через назначенные интервалы времени или в ответ на системные события.  
  
-   Программа **sqlps** , которая запускает PowerShell и импортирует модуль [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Затем можно выполнять все действия, поддерживаемые в модуле. Программу **sqlps** можно запустить либо из командной строки, либо щелкнув правой кнопкой мыши узлы дерева обозревателя объектов среды [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio и выбрав команду **Запустить PowerShell**.  
  
## <a name="database-engine-queries"></a>Запросы к компоненту Database Engine  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] содержат три типа элементов.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Инструкции языка XQuery.  
  
-   Команды и переменные из программы **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает три среды для построения и запуска запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
-   Запросы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] можно запускать в интерактивном режиме и отлаживать в редакторе запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. В одном сеансе можно закодировать и отладить несколько инструкций, а затем сохранить их все в одном файле скрипта.  
  
-   Программа командной строки **sqlcmd** позволяет запускать запросы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в интерактивном режиме, а также запускать существующие файлы скриптов с запросами компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] обычно кодируются в интерактивном режиме в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] с помощью редактора запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] . В дальнейшем файл можно открыть в одной из следующих сред.  
  
-   Чтобы открыть файл в новом окне редактора запросов [!INCLUDE[ssDE](../../includes/ssde-md.md)], воспользуйтесь меню **Файл**/**Открыть** в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   Чтобы запустить файл с помощью программы **sqlcmd** _, укажите параметр_ -i **input_file** .  
  
-   Чтобы запустить файл с помощью командлета **Invoke-Sqlcmd** в скриптах **PowerShell, укажите параметр** -QueryFromFile [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Для запуска скриптов через назначенные интервалы времени или в ответ на системные события используются шаги заданий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] агента [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Кроме того, для формирования скриптов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать мастер формирования скриптов [!INCLUDE[tsql](../../includes/tsql-md.md)] . Можно щелкнуть объекты правой кнопкой мыши в обозревателе объектов [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , а затем выбрать пункт меню **Создать скрипт** . Команда **Создать скрипт** запускает мастер, который облегчает процесс создания скрипт.  
  
## <a name="database-engine-scripting-tasks"></a>Работа со скриптами компонента Database Engine  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Описывает порядок использования редактора кода и текстового редактора в среде [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] для интерактивной разработки, отладки и выполнения скриптов [!INCLUDE[tsql](../../includes/tsql-md.md)] .|[Редакторы запросов и текста (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)|  
|Описывает порядок использования программы **sqlcmd** для выполнения скриптов [!INCLUDE[tsql](../../includes/tsql-md.md)] из командной строки, включая возможность интерактивной разработки скриптов.|[Связанные инструкции по sqlcmd](./sqlcmd-start-the-utility.md)|  
|Описывает порядок интеграции компонентов SQL Server в среду Windows PowerShell с последующей сборкой скриптов PowerShell для управления экземплярами и объектами SQL Server.|[SQL Server PowerShell](../../powershell/sql-server-powershell.md)|  
|Описывает порядок использования мастера **формирования и публикации скриптов** для создания скриптов [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые повторно создают один или несколько объектов из базы данных.|[Формирование скриптов (среда SQL Server Management Studio)](./generate-scripts-sql-server-management-studio.md)|  
  
## <a name="see-also"></a>См. также:  
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)   
 [Руководство. Составление инструкций Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
