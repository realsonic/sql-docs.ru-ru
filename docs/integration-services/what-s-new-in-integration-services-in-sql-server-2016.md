---
description: Новые возможности служб Integration Services в SQL Server 2016
title: Новые возможности служб Integration Services в SQL Server 2016 | Документы Майкрософт
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 838b01e2fd4f09b787b11f146143fc6c327504b7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172216"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>Новые возможности служб Integration Services в SQL Server 2016

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



В этой статье описаны функции, которые были добавлены или обновлены в SQL Server 2016 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. К ним также относятся функции, добавленные или обновленные в [пакете дополнительных компонентов Azure для служб Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md) в течение жизненного цикла SQL Server 2016.  

## <a name="new-for-ssis-in-azure-data-factory"></a>Новые возможности служб SSIS в фабрике данных Azure

В общедоступной предварительной версии 2 фабрики данных Azure, выпущенной в сентябре 2017 г., теперь можно выполнять следующие действия:
-   развертывать пакеты в базе данных каталога служб SSIS (SSISDB) в базе данных SQL Azure;
-   запускать пакеты, развернутые в Azure, в Azure-SSIS Integration Runtime, компоненте фабрики данных Azure версии 2.

Дополнительные сведения см. в разделе [Перенос рабочих нагрузок SQL Server Integration Services в облако](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Для этих новых возможностей требуются средства SQL Server Data Tools (SSDT) версии 17.2 или более поздней, но не требуется SQL Server 2017 или SQL Server 2016. При развертывании пакетов в Azure мастер развертывания пакетов всегда обновляет их до новейшего формата.

## <a name="2016-improvements-by-category"></a>Усовершенствования в версии 2016 по категориям  
  
-   **Управляемость**  
  
    -   Улучшенное развертывание  
  
        -   [Мастер обновления SSISDB](#ssisdbupgrwiz)  
  
        -   [Поддержка Always On в каталоге служб SSIS](#AlwaysOn)  
  
        -   [Добавочное развертывание пакетов](#IncrementalDeployment)  
  
        -   [Поддержка постоянного шифрования в каталоге SSIS](#encrypted)  
  
    -   Более эффективный способ отладки  
  
        -   [Новая роль ssis_logreader уровня базы данных в каталоге служб SSIS](#LogReader)  
  
        -   [Новый уровень ведения журнала RuntimeLineage в каталоге служб SSIS](#RuntimeLineage)  
  
        -   [Новый настраиваемый уровень ведения журнала в каталоге служб SSIS](#CustomLogging)  
  
        -   [Имена столбцов для ошибок в потоке данных](#ErrorColumn)  
  
        -   [Расширенная поддержка для имена столбцов ошибок](#getidstring)  
  
        -   [Поддержка для серверного уровня ведения журнала по умолчанию](#ServerLogLevel)  
  
        -   [Новый интерфейс IDTSComponentMetaData130 в API](#CMD130)  
  
    -   Улучшенное управление пакетами  
  
        -   [Улучшенная процедура для обновления проекта](#ProjectUpgrade)  
  
        -   [Свойство AutoAdjustBufferSize автоматически вычисляет размер буфера для потока данных](#BufferSize)  
  
        -   [Многоразовые шаблоны потока управления](#Templates)  
  
        -   [Новые шаблоны, переименованные в части](#Parts)  
  
-   **Соединение**  
  
    -   Расширенные возможности связи в локальной среде  
  
        -   [Поддержка источников данных OData версии 4](#ODatav4)  
  
        -   [Явная поддержка источников данных Excel 2013](#Excel2013)  
  
        -   [Поддержка файловой системы Hadoop (HDFS)](#HDFS)  
  
        -   [Расширенная поддержка Hadoop и HDFS](#more_hadoop)  
  
        -   [Назначение файлов HDFS теперь поддерживает формат файлов ORC.](#hdfsORC)  
  
        -   [Обновление компонентов ODBC для SQL Server 2016](#odbc2016)  
  
        -   [Явная поддержка источников данных Excel 2016](#Excel2016)  
  
        -   [Выпуск соединителя для SAP BW для SQL Server 2016](#SAPBW)
        
        -   [Выпуск соединителей версии 4.0 для Oracle и Teradata](#oracleteradata)
        
        -   [Выпуск соединителей для системы платформы аналитики (PDW) с обновлением 5](#pdwau5)
  
    -   Расширенные возможности связи в облаке  
  
        -   Соединители Azure хранилища и задачи Hive и Pig для HDInsight — [выпуск пакета дополнительных компонентов Azure для служб SSIS для SQL Server 2016](#AFP2016)
        
        -   [Поддержка ресурсов Microsoft Dynamics Online в пакете обновления 1 (SP1)](#dynamics)
        
        -   [Выпуск пакета поддержки для Azure Data Lake Store](#datalakestore)
        
        -   [Реализована поддержка Azure Synapse Analytics](#sqldwupload)
  
-   **Удобство использования и производительность**  
  
    -   Улучшенная процедура установки  
  
        -   [Блокировка обновления, когда база данных SSISDB относится к группе доступности](#Upgrade)  
  
    -   Улучшенная процедура разработки  
  
        -   [Конструктор SSIS создает и обслуживает пакеты для SQL Server 2016, 2014 или 2012](#OneDesigner)  
  
        -   Усовершенствования и исправления ошибок для конструктора.  
  
    -   Улучшение функций управления в SQL Server Management Studio
  
        -   [Улучшенная производительность для представления каталога служб SSIS](#CatViews)  
  
    -   Другие усовершенствования  
  
        -   [Преобразование сбалансированного распределителя данных теперь входит в состав служб SSIS](#BDDinbox)  
  
        -   [Компоненты публикации веб-канала данных теперь входят в состав служб SSIS](#ComplexFeedinbox)  
  
        -   [Поддержка хранилища BLOB-объектов Azure в мастере импорта и экспорта SQL Server](#AzureBlob)  
  
        -   [Выпуск конструктора и службы системы отслеживания измененных данных для Oracle для Microsoft SQL Server 2016](#CDCOracle)  
  
        -   [Обновление компонентов CDC для SQL Server 2016](#cdc2016)  
  
        -   [Обновление задачи "Выполнение DDL службами Analysis Services"](#ASDDL)  
  
        -   [Задачи служб Analysis Services поддерживают табличные модели](#ssasrc0)  
  
        -   [Поддержка встроенных служб R](#builtinR)  
  
        -   [Подробные данные о проверке XML в задачах XML](#ValidateXML)  
  
## <a name="manageability"></a>Управляемость  

### <a name="better-deployment"></a>Улучшенное развертывание

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> Мастер обновления SSISDB  
 Мастер обновления SSISDB можно использовать для обновления базы данных каталога служб SSIS (SSISDB), когда эта база данных старше текущей версии экземпляра SQL Server. Такое может произойти в следующих случаях:  
  
-   База данных восстановлена из более старой версии SQL Server.  
  
-   База данных не была удалена из группы доступности AlwaysOn перед обновлением экземпляра SQL Server. Это препятствует автоматическому обновлению базы данных. Дополнительные сведения: [Обновление SSISDB в группе доступности](../integration-services/catalog/ssis-catalog.md#Upgrade).  
  
 Дополнительные сведения см. в разделе [Каталог служб SSIS &#40;SSISDB&#41;](../integration-services/catalog/ssis-catalog.md). 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> Поддержка Always On в каталоге служб SSIS  
 Группы доступности AlwaysOn — это решение для высокой доступности и аварийного восстановления, являющееся альтернативой зеркальному отображению баз данных на уровне предприятия. Группа доступности поддерживает среду отработки отказа для дискретного набора пользовательских баз данных, известных как базы данных доступности, выполняющие совместную отработку отказа. Дополнительные сведения см. в статье [Группы доступности AlwaysOn](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 В SQL Server 2016 службы SSIS предоставляют новые возможности, позволяющие легко производить развертывание в централизованном каталоге служб SSIS (например, в пользовательской базе данных SSISDB). Чтобы обеспечить высокую доступность для базы данных SSISDB и ее содержимого (проектов, пакетов, журналов выполнения и т. п.), можно добавить ее в группу доступности Always On, как и любую другую пользовательскую базу данных. В случае сбоя один из вторичных узлов автоматически становится новым основным узлом.  
  
 Подробное описание и пошаговые инструкции по включению AlwaysOn для SSISDB см. в статье [Каталог служб SSIS](../integration-services/catalog/ssis-catalog.md).  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> Добавочное развертывание пакетов  
Функция добавочного развертывания пакетов позволяет развертывать один или несколько пакетов в существующем или новом проекте без развертывания всего проекта. Для этого можно использовать следующие средства:  
  
-   Мастер развертывания  
  
-   SQL Server Management Studio (использует мастер развертывания)  
  
-   SQL Server Data Tools (Visual Studio) (также использует мастер развертывания)  
  
-   Хранимые процедуры  
  
-   API объектной модели управления (MOM)  
  
 Дополнительные сведения см. в разделе [Развертывание проектов и пакетов служб Integration Services (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> Поддержка Always Encrypted в каталоге служб SSIS  
 Службы SSIS уже поддерживают функцию постоянного шифрования в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Дополнительные сведения см. в следующих записях блога:  
  
-   [SSIS with Always Encrypted (Службы SSIS с постоянным шифрованием)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [Lookup transformation with Always Encrypted (Преобразование "Уточняющий запрос" с постоянным шифрованием)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>Более эффективный способ отладки

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> Новая роль ssis_logreader уровня базы данных в каталоге служб SSIS  
 В предыдущих версиях каталога служб SSIS доступ к представлениям, содержащим выходные данные журнала, могли получить только пользователи с ролью **ssis_admin** . Теперь доступна новая роль уровня базы данных **ssis_logreader** , с помощью которой можно предоставлять разрешения на доступ к представлениям, содержащим выходные данные журнала, пользователям без прав администратора.  
  
 Имеется также новая роль **ssis_monitor** . Она поддерживает Always On и предназначена для внутреннего использования в каталоге служб SSIS.  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> Новый уровень ведения журнала RuntimeLineage в каталоге служб SSIS  
 Новый уровень ведения журнала **RuntimeLineage** в каталоге служб SSIS собирает данные, необходимые для отслеживания сведений о журнале преобразований в потоке данных. Вы можете проанализировать эти сведения журнала преобразований, чтобы сопоставить отношение преобразований между задачами. С помощью этой информации независимые поставщики программного обеспечения и разработчики могут создавать пользовательские средства сопоставления для журнала преобразований. 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> Новый настраиваемый уровень ведения журнала в каталоге служб SSIS  
 Предыдущие версии каталога служб SSIS позволяют выбрать один из четырех встроенных уровней ведения журнала при выполнении пакета: **None, Basic, Performance или Verbose**. В SQL Server 2016 добавлен уровень **RuntimeLineage**. Кроме того, теперь можно создавать и сохранять несколько настроенных уровней ведения журнала в каталоге служб SSIS, а также выбрать уровень, используемый при каждом запуске пакета. Для каждого настроенного уровня ведения журнала можно выбрать те статистические данные и события, которые необходимо регистрировать. При необходимости укажите контекст событий для просмотра значений переменных, строк подключения и свойств задачи. Дополнительные сведения см. в разделе [Включение ведения журналов при выполнении пакета на сервере служб SSIS](../integration-services/performance/integration-services-ssis-logging.md#server_logging). 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> Имена столбцов для ошибок в потоке данных  
 При выводе строк с ошибками в потоке данных результат вывода содержит числовые идентификаторы столбцов с ошибками, а не их имена. Теперь существует несколько способов для поиска или отображения имени столбца, где произошла ошибка.  
  
-   При настройке ведения журнала выберите событие **DiagnosticEx** для регистрации. Это событие записывает карту столбцов потока данных в журнал. Потом вы сможете найти имя столбца в этой карте столбцов с помощью идентификатора столбца, записанного в выводе ошибок. Дополнительные сведения см. в разделе [Обработка ошибок в данных](../integration-services/data-flow/error-handling-in-data.md).  
  
-   При просмотре свойств входного или выходного столбца для компонента потока данных в расширенном редакторе отображается имя исходного столбца.  
  
-   Чтобы просмотреть имена столбцов, где произошла ошибка, подключите средство просмотра данных к выводу ошибок.  Средство просмотра данных теперь отображает как описание ошибки, так и имя столбца, где она возникла.  
  
-   В компоненте скрипта или пользовательского потока данных вызовите новый метод <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> интерфейса IDTSComponentMetadata100.  
  
 Дополнительные сведения об этом улучшении см. в следующей записи блога от разработчика служб SSIS Бо Фэна (Bo Fan): [Error Column Improvements for SSIS Data Flow](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)(Улучшения столбцов с ошибками для потока данных SSIS).  
  
> [!NOTE]  
>  (Эта поддержка была расширена в последующих выпусках. Дополнительные сведения см. в разделе [Расширенная поддержка для имена столбцов ошибок](#getidstring) и [Новый интерфейс IDTSComponentMetaData130 в API](#CMD130).)  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> Расширенная поддержка для имен столбцов ошибок  
 Событие **DiagnosticEx** теперь регистрирует сведения о столбце для всех входных и выходных столбцов, а не только столбцов журнала преобразований. Поэтому теперь можно вызвать выходные данные для карты столбцов конвейера вместо карты журнала обращений конвейера.  
  
 Метод GetIdentificationStringByLineageID был переименован в <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>. Дополнительные сведения см. в разделе [Имена столбцов для ошибок в потоке данных](#ErrorColumn).  
  
 Дополнительные сведения об этом изменении, а также об усовершенствовании столбца ошибок см. в следующей обновленной записи блога: [Усовершенствования столбца ошибок для потока данных служб SSIS (обновлено для CTP 3.3) (Error Column Improvements for SSIS Data Flow (Updated for CTP3.3))](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (В RC0 этот метод был перемещен в новый интерфейс <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> . Дополнительные сведения см. в разделе [Новый интерфейс IDTSComponentMetaData130 в API](#CMD130).)  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> Поддержка для серверного уровня ведения журнала по умолчанию  
 Теперь для свойства **Уровень ведения журнала сервера** в разделе **Свойства сервера** в SQL Server можно выбрать серверный уровень ведения журнала по умолчанию. Вы можете выбрать один из встроенных уровней ведения журнала — базовый, нет, подробный, по производительности или журналу преобразований для среды выполнения — или существующий пользовательский уровень. Выбранный уровень применяется ко всем пакетам, развернутым в каталоге служб SSIS. По умолчанию этот уровень применяется также к заданию агента SQL Server, в рамках которого запущен пакет служб SSIS.  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> Новый интерфейс IDTSComponentMetaData130 в API  
 Новый уровень ведения журнала <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> добавляет новые функции в SQL Server 2016 для существующего интерфейса <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> , в частности метод <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> . (Метод **GetIdentificationStringByID** перемещен в новый интерфейс из интерфейса <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> интерфейса.) Также существуют новые интерфейсы <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> и <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> , интерфейсы, имеющие свойство **LineageIdentificationString** . Дополнительные сведения см. в разделе [Имена столбцов для ошибок в потоке данных](#ErrorColumn).  

### <a name="better-package-management"></a>Улучшенное управление пакетами

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> Улучшенная процедура для обновления проекта  
 При обновлении проектов служб SSIS с предыдущих версий до текущей диспетчеры подключений на уровне проекта продолжают работать должным образом, а макет пакета и заметки сохраняются.  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> Свойство AutoAdjustBufferSize автоматически вычисляет размер буфера для потока данных  
 При задании значения **true** для нового свойства **AutoAdjustBufferSize** подсистема обработки потока данных автоматически вычисляет размер буфера для потока. Дополнительные сведения см. в разделе [Возможности для повышения производительности потока данных](../integration-services/data-flow/data-flow-performance-features.md).  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> Многоразовые шаблоны потока управления  
 Можно сохранить часто используемую задачу или контейнер потока управления в отдельный файл шаблона и повторно использовать их несколько раз в одном или нескольких пакетах проекта с помощью шаблонов потока управления. Такая возможность повторного использования упрощает разработку и обслуживание пакетов служб SSIS. Дополнительные сведения см. в разделе [Повторное использование потока управления для нескольких пакетов с помощью частей пакета потока управления](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> Новые шаблоны, переименованные в части  
 Новые многоразовые шаблоны потока управления, выпущенные в CTP 3.0, были переименованы в части потока управления или части пакета. Дополнительные сведения см. в разделе [Повторное использование потока управления для нескольких пакетов с помощью частей пакета потока управления](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

## <a name="connectivity"></a>Соединение  

### <a name="expanded-connectivity-on-premises"></a>Расширенные возможности связи в локальной среде

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> Поддержка источников данных OData версии 4  
 Источник OData и диспетчер подключений OData теперь поддерживают протоколы OData v3 и v4.  
  
-   Для протокола OData версии 3 компонент поддерживает форматы данных ATOM и JSON.  
  
-   Для протокола OData версии v4; компонент поддерживает формат данных JSON.  
  
 Дополнительные сведения см. в разделе [OData Source](../integration-services/data-flow/odata-source.md).  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> Явная поддержка источников данных Excel 2013  
 Диспетчер подключений, источник, назначение Excel и мастер импорта и экспорта SQL Server теперь явным образом поддерживают источники данных Excel 2013. 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> Поддержка файловой системы Hadoop (HDFS)  
 Поддержка HDFS включает в себя диспетчеры подключений для соединения с кластерами Hadoop, а также задачи для выполнения общих операций HDFS. Дополнительные сведения см в разделе [Поддержка Hadoop и HDFS в службах Integration Services (SSIS)](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md).  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> Расширенная поддержка Hadoop и HDFS  
  
-   Диспетчер подключений Hadoop теперь поддерживает обычную проверку подлинности и проверку подлинности Kerberos. Дополнительные сведения см. в разделе [Hadoop Connection Manager](../integration-services/connection-manager/hadoop-connection-manager.md).  
  
-   Источник и назначение файлов HDFS теперь поддерживают форматы "Текст" и "Avro". Дополнительные сведения см. в разделах  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) и  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  
  
-   Задача файловой системы Hadoop теперь поддерживает параметр CopyWithinHadoop, дополняющий параметры CopyToHadoop и CopyFromHadoop. Дополнительные сведения см. в разделе [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md).  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> HDFS-файлы в качестве назначения теперь поддерживают формат файлов ORC  
 Назначение файлов HDFS теперь поддерживает формат ORC в дополнение к форматам "Текст" и "Avro". (Источник файлов HDFS поддерживает только "Текст" и "Avro"). Дополнительные сведения об этом компоненте см. в статье [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> Обновление компонентов ODBC для SQL Server 2016  
 Компоненты источника и назначения ODBC были обновлены для обеспечения полной совместимости с SQL Server 2016. Какие -либо новые функции и изменения в поведении отсутствуют.  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> Явная поддержка источников данных Excel 2016  
 Диспетчер подключений, источник и назначение Excel теперь явным образом поддерживают источники данных Excel 2016.  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Выпуск соединителя для SAP BW для SQL Server 2016  
 Соединитель с SAP BW (Microsoft®) для Microsoft SQL Server® 2016 выпущен в составе пакета дополнительных компонентов SQL Server 2016. Скачать эти компоненты можно здесь: [Пакет дополнительных компонентов для Microsoft® SQL Server® 2016](https://www.microsoft.com/download/details.aspx?id=56833).
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> Выпуск соединителей версии 4.0 для Oracle и Teradata
Были выпущены соединители Майкрософт версии v4.0 для Oracle и Teradata. Скачать их можно здесь: [соединители Microsoft версии 4.0 для Oracle и Teradata](https://www.microsoft.com/download/details.aspx?id=52950).

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> Выпуск соединителей для системы платформы аналитики (PDW) с обновлением 5
Были выпущены адаптеры назначения для загрузки данных в PDW с AU5. Сведения о скачивании адаптеров см. в разделе [Analytics платформы System Appliance Update 5 Documentation и клиентские средства](https://www.microsoft.com/download/details.aspx?id=51610).

### <a name="expanded-connectivity-to-the-cloud"></a>Расширенные возможности связи в облаке

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> выпуск пакета дополнительных компонентов Azure для служб SSIS для SQL Server 2016  
 Состоялся выпуск пакета дополнительных компонентов Azure для служб Integration Services для SQL Server 2016. Он содержит диспетчеры подключений для соединения с источниками данных Azure, а также задачи для выполнения общих операций Azure. Дополнительные сведения см. в статье [Пакет дополнительных компонентов Azure для служб Integration Services (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> Поддержка ресурсов Microsoft Dynamics Online в пакете обновления 1 (SP1)

В установленном пакете обновления 1 (SP1) SQL Server 2016 источник OData и диспетчер подключений OData теперь поддерживают подключение к каналам OData в Microsoft Dynamics AX Online и Microsoft Dynamics CRM Online.

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> Выпуск пакета поддержки для Azure Data Lake Store

Последняя версия пакета дополнительных компонентов Azure включает диспетчер подключений, а также источник и место назначения для перемещения данных из Azure Data Lake Store и обратно. Дополнительные сведения см. в статье [Пакет дополнительных компонентов Azure для служб Integration Services (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a> Реализована поддержка Azure Synapse Analytics

Последняя версия пакета дополнительных компонентов Azure включает задачу отправки в хранилище данных SQL Azure, позволяющую заполнить данными Azure Synapse Analytics. Дополнительные сведения см. в статье [Пакет дополнительных компонентов Azure для служб Integration Services (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

## <a name="usability-and-productivity"></a>Удобство использования и производительность  
 
### <a name="better-install-experience"></a>Улучшенная процедура установки

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> Блокировка обновления, когда база данных SSISDB состоит в группе доступности  
 Если база данных каталога служб SSIS (SSISDB) состоит в группе доступности AlwaysOn, для обновления SQL Server необходимо сначала удалить SSISDB из группы доступности, произвести обновление, а затем добавить SSISDB обратно в группу. Дополнительные сведения: [Обновление SSISDB в группе доступности](../integration-services/catalog/ssis-catalog.md#Upgrade).  

### <a name="better-design-experience"></a>Улучшенная процедура разработки

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> Поддержка нескольких версий в конструкторе служб SSIS  
 Теперь можно использовать конструктор SSIS в SQL Server Data Tools (SSDT) для Visual Studio 2015, чтобы создавать, обслуживать и выполнять пакеты, ориентированные на SQL Server 2016, SQL Server 2014 или SQL Server 2012. Процедуру получения SSDT см. в разделе [Скачивание последней версии SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md). 

 В обозревателе решений щелкните правой кнопкой мыши проект служб Integration Services и выберите пункт **Свойства**, чтобы открыть страницу свойств проекта. На вкладке **Общие** окна **Свойства конфигурации** выберите свойство **TargetServerVersion** и затем SQL Server 2016, SQL Server 2014 или SQL Server 2012.  
   
 ![Свойство TargetServerVersion в диалоговом окне Свойства проекта](../integration-services/media/targetserverversion2.png "Свойство TargetServerVersion в диалоговом окне Свойства проекта")  

> [!IMPORTANT]
> При разработке пользовательских расширений для служб SSIS см. разделы [Поддержка нескольких версий в настраиваемых компонентах](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) и [Получение пользовательских расширений служб SSIS, поддерживаемых несколькими версиями SSDT 2015 для SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/).  

### <a name="better-management-experience-in-sql-server-management-studio"></a>Улучшение функций управления в SQL Server Management Studio

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> Улучшенная производительность для представления каталога служб SSIS  
 Теперь большинство представлений каталога служб SSIS работают лучше, когда их запускает пользователь, не являющийся элементом роли ssis_admin.  

### <a name="other-enhancements"></a>Другие усовершенствования

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> Преобразование сбалансированного распределителя данных теперь входит в состав служб SSIS  
 Преобразование сбалансированного распределителя данных, которое в предыдущих версиях [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]требовалось скачивать отдельно, теперь устанавливается вместе с [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Дополнительные сведения см. в разделе [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md).  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> Компоненты публикации веб-канала данных теперь входят в состав служб SSIS  
 Компоненты публикации веб-канала данных, которые в предыдущих версиях [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]требовалось скачивать отдельно, теперь устанавливаются вместе с [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Дополнительные сведения см. в разделе [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md).  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> Поддержка хранилища BLOB-объектов Azure в мастере импорта и экспорта SQL Server  
 Мастер импорта и экспорта SQL Server теперь может импортировать данные из хранилища BLOB-объектов и сохранять их там. Дополнительные сведения см. в разделах [Выбор источника данных (мастер импорта и экспорта SQL Server)](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md) и [Выбор назначения (мастер импорта и экспорта SQL Server)](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md). 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Выпуск конструктора и службы системы отслеживания измененных данных для Oracle для Microsoft SQL Server 2016  
 Конструктор и служба системы отслеживания измененных данных Microsoft® для Oracle от Attunity для Microsoft SQL Server® 2016 выпущены в составе пакета дополнительных компонентов SQL Server 2016.  Эти компоненты теперь поддерживают Oracle 12c в классической установке. (Мультитенантная установка не поддерживается.) Скачать компоненты пакета дополнительных компонентов можно здесь: [Пакет дополнительных компонентов Microsoft® SQL Server® 2016](https://www.microsoft.com/download/details.aspx?id=56833).  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> Обновление компонентов CDC для SQL Server 2016  
 Компоненты задачи по проверке CDC (отслеживание измененных данных), источника и преобразования разделителя были обновлены для обеспечения полной совместимости с SQL Server 2016. Какие -либо новые функции и изменения в поведении отсутствуют.  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Обновление задачи "Выполнение DDL службами Analysis Services"  
 Задача "Выполнение DDL службами Analysis Services" обновлена, чтобы принимать команды на языке TMSL.

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Задачи служб Analysis Services поддерживают табличные модели.  
 Теперь можно использовать все назначения и задачи служб SSIS, поддерживающие SQL Server Analysis Services (SSAS) в табличных моделях SQL Server 2016. Задачи служб SSIS были обновлены, чтобы представлять табличные объекты вместо многомерных. Например, при выборе объектов для обработки задача обработки служб Analysis Services автоматически обнаруживает табличную модель и отображает список табличных объектов вместо групп мер и измерений. Назначение обработки секций теперь, помимо прочего, отображает табличные объекты и поддерживает принудительную отправку данных в секцию.  
  
 Назначение обработки измерений не работает для табличных моделей с уровнем совместимости SQL 2016.  Для обработки табличных данных вам требуется только задача обработки служб Analysis Services и назначение обработки секций. 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> Поддержка встроенных служб R  
 Службы SQL Server Integration Services (SSIS) уже поддерживают встроенные службы R в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Службы SSIS можно использовать не только для извлечения данных и загрузки выходных данных анализа, но и для сборки, выполнения и периодического повторного обучения моделей R. Дополнительные сведения см. в следующей записи блога: [Ввод в эксплуатацию проекта машинного обучения с помощью SQL Server 2016 SSIS и служб R](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296)(Operationalize your machine learning project using SQL Server 2016 SSIS and R Services). 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> Подробные данные о проверке XML в задачах XML  
 Активировав в задаче XML свойство **ValidationDetails** , вы сможете получить подробные результаты проверки XML-документа. До появления свойства **ValidationDetails** проверка XML в задачах XML возвращала информацию только о том, есть ошибка в документе или нет. Сведения о самих ошибках и их расположении были недоступны. Теперь, если для свойства **ValidationDetails** задать значение True, выходной файл будет содержать подробные сведения обо всех ошибках, включая номера строк и позиции. Эти сведения можно использовать для анализа, поиска и исправления ошибок в XML-документах. Дополнительные сведения см. в разделе [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 В[!INCLUDE[ssIS](../includes/ssis-md.md)] появилось свойство **ValidationDetails** в пакете обновления 2 (SP2) для [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] . В то время о новом свойстве не было никакой информации. Кроме того, свойство **ValidationDetails** доступно в [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] и [!INCLUDE[ssSQL15](../includes/sssql16-md.md)].   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>См. также:  
 [Что нового в SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)   
 [Возможности, поддерживаемые различными выпусками SQL Server 2016](../sql-server/editions-and-components-of-sql-server-2016.md)