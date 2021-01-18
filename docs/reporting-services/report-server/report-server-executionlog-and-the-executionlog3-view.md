---
title: Журнал выполнения сервера отчетов и представление ExecutionLog3 | Документы Майкрософт
description: Сведения о журнале выполнения сервера отчетов в Reporting Services, который содержит сведения о отчетах на серверах в собственном режиме или ферме SharePoint.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], execution
- execution logs [Reporting Services]
ms.assetid: a7ead67d-1404-4e67-97e7-4c7b0d942070
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 28d083e053e31c1fcce26e233ba22211e211a993
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125595"
---
# <a name="report-server-executionlog-and-the-executionlog3-view"></a>Журнал выполнения сервера отчетов и представление ExecutionLog3
  Журнал выполнения для сервера отчетов [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]содержит сведения об отчетах, которые создаются на одном или нескольких серверах в масштабном развертывании в собственном режиме или в ферме SharePoint. Журнал выполнения отчетов можно использовать для изучения частоты, с которой запрашивается отчет, частоты использования различных форматов вывода и времени обработки в миллисекундах, занимаемого каждой фазой обработки. Журнал содержит сведения о продолжительности времени, затраченного на выполнение запроса набора данных отчета, и времени, затраченного на обработку данных. Администраторы сервера отчетов могут просматривать информацию журнала, выявлять задачи, выполняющиеся продолжительное время, и вносить предложения для разработчиков отчетов в тех областях организации отчета (набор данных или обработка), в которых они могут проводить улучшения.  
  
 На серверах отчетов, настроенных для режима SharePoint, можно также использовать журналы ULS SharePoint. Дополнительные сведения см. в разделе [Включение событий служб Reporting Services для журнала трассировки SharePoint (ULS)](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
##  <a name="viewing-log-information"></a><a name="bkmk_top"></a> Просмотр данных журнала  
 Сервер отчетов записывает данные о выполнении отчетов во внутреннюю таблицу базы данных. Сведения, содержащиеся в этой таблице, могут быть получены из представлений SQL Server.  
  
 Журнал выполнения отчета хранится в базе данных сервера отчетов, которая по умолчанию носит имя **ReportServer**. Представления SQL отображают сведения журналов выполнения. Представления "2" и "3" были добавлены в последних выпусках и содержат новые поля или поля с более удобными именами, чем в предыдущих выпусках. Существовавшие ранее представления не исключены из состава продукта, поэтому зависящие от них пользовательские приложения не затрагиваются. При отсутствии зависимости от более старого представления, например ExecutionLog, рекомендуется использовать последнее по времени представление, ExecutionLog **3**.  
  
 В этом разделе:  
  
-   [Значения параметров конфигурации для сервера отчетов с режимом SharePoint](#bkmk_sharepoint)  
  
-   [Значения параметров конфигурации для сервера отчетов с собственным режимом](#bkmk_native)  
  
-   [Поля журнала (ExecutionLog3)](#bkmk_executionlog3)  
  
-   [Поле AdditionalInfo](#bkmk_additionalinfo)  
  
-   [Поля журнала (ExecutionLog2)](#bkmk_executionlog2)  
  
-   [Поля журнала (ExecutionLog)](#bkmk_executionlog)  
  
##  <a name="configuration-settings-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepoint"></a> Значения параметров конфигурации для сервера отчетов с режимом SharePoint  
 Предусмотрена возможность включать и отключать ведение журнала выполнения отчета с помощью параметров настройки системы для приложения службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 По умолчанию записи журнала хранятся 60 суток. Записи с истекшим сроком хранения удаляются в 02:00 ежедневно. При готовой установке в заданный момент времени доступны данные только за последние 60 дней.  
  
 Возможность ограничения количества строк или типа элементов, вносимых в журнал, отсутствует.  
  
 **Включение ведения журнала выполнения.**  
  
1.  В центре администрирования SharePoint щелкните **Управление приложениями службы** в группе **Управление приложениями** .  
  
2.  Щелкните имя приложения службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , которое требуется настроить.  
  
3.  Нажмите кнопку **Системные параметры**.  
  
4.  Выберите **Включить ведение журнала выполнения** в разделе **Ведение журнала** .  
  
5.  Нажмите кнопку **ОК**.  
  
 **Включение подробного ведения журнала.**  
  
 Необходимо включить ведение журнала, как описано в предыдущих шагах, а затем выполнить следующее:  
  
1.  На странице **Системные параметры** приложения служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] найдите раздел **Определяемые пользователем** .  
  
2.  Измените значение параметра **ExecutionLogLevel** на **подробный**. Это поле представляет собой поле ввода текста, и двумя возможными значениями являются **подробный** и **обычный**.  
  
##  <a name="configuration-settings-for-a-native-mode-report-server"></a><a name="bkmk_native"></a> Значения параметров конфигурации для сервера отчетов с собственным режимом  
 Предусмотрена возможность включать или отключать ведение журнала выполнения отчета на странице «Свойства сервера» в среде SQL Server Management Studio. Свойство **EnableExecutionLogging** является расширенным.  
  
 По умолчанию записи журнала хранятся 60 суток. Записи с истекшим сроком хранения удаляются в 02:00 ежедневно. При готовой установке в заданный момент времени доступны данные только за последние 60 дней.  
  
 Возможность ограничения количества строк или типа элементов, вносимых в журнал, отсутствует.  
  
 **Включение ведения журнала выполнения.**  
  
1.  Запустите среду SQL Server Management Studio с правами администратора. Например, щелкните правой кнопкой мыши значок среды Management Studio и выберите команду "Запуск от имени администратора".  
  
2.  Установите соединение с требуемым сервером отчетов.  
  
3.  Щелкните правой кнопкой мыши имя сервера и выберите пункт **Свойства**. Если параметр «Свойства» отключен, убедитесь в том, что запуск среды SQL Server Management Studio был выполнен с административными привилегиями.  
  
4.  Перейдите на страницу **Ведение журнала** .  
  
5.  Выберите **Включить ведение журнала выполнения отчета**.  
  
 **Включение подробного ведения журнала.**  
  
 Необходимо включить ведение журнала, как описано в предыдущих шагах, а затем выполнить следующее:  
  
1.  В окне **Свойства сервера** перейдите на страницу **Дополнительно** .  
  
2.  В разделе **Определяемый пользователем** измените значение **ExecutionLogLevel** на **подробный**. Это поле представляет собой поле ввода текста, и двумя возможными значениями являются **подробный** и **обычный**.  
  
##  <a name="log-fields-executionlog3"></a><a name="bkmk_executionlog3"></a> Поля журнала (ExecutionLog3)  
 Это представление добавило дополнительный узел диагностики производительности в столбец на основе XML **AdditionalInfo** . Столбец AdditionalInfo содержит XML-структуру дополнительных полей данных вида «один ко многим». Ниже приведен пример инструкции Transact SQL для выборки строк из представления ExecutionLog3. В этом образце предполагается, что база данных сервера отчетов носит имя **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog3 order by TimeStart DESC  
```  
  
 В следующей таблице описаны данные, которые перехватываются в журнале выполнения отчетов  
  
|Столбец|Описание|  
|------------|-----------------|  
|InstanceName|Имя экземпляра сервера отчетов, обработавшего запрос. Если в применяемой среде имеется больше одного сервера отчетов, можно анализировать распределение InstanceName для контроля и определения того, распределяет ли программа уравновешивания сетевой нагрузки запросы по серверам отчетов в соответствии с ожидаемым.|  
|ItemPath|Путь, по которому сохраняется отчет или элемент отчета.|  
|UserName|Идентификатор пользователя.|  
|ExecutionID|Внутренний идентификатор, связанный с запросом. Запросы, выполняемые в рамках одного сеанса пользователя, имеют одинаковый идентификатор выполнения.|  
|RequestType|Возможные значения:<br /><br /> Интерактивно<br /><br /> Подписка<br /><br /> <br /><br /> Анализ данных журнала, отфильтрованных по значению RequestType=Subscription и отсортированных по значению TimeStart, может показать периоды интенсивной нагрузки на подписку, и может потребоваться изменить параметры некоторых подписок на отчет для другого вызова.|  
|Формат|Формат подготовки к просмотру.|  
|Параметры|Значения параметров, используемых при выполнении отчета.|  
|ItemAction|Возможные значения:<br /><br /> Render<br /><br /> Сортировка<br /><br /> BookMarkNavigation<br /><br /> DocumentNavigation<br /><br /> GetDocumentMap<br /><br /> Findstring<br /><br /> Execute<br /><br /> RenderEdit|  
|TimeStart|Время начала и завершения обработки отчета, указывающее на его продолжительность.|  
|TimeEnd||  
|TimeDataRetrieval|Количество миллисекунд, затраченных на выборку данных.|  
|TimeProcessing|Количество миллисекунд, затраченных на обработку отчета.|  
|TimeRendering|Количество миллисекунд, затраченных на подготовку отчета к просмотру.|  
|Источник|Источник выполнения отчета. Возможные значения:<br /><br /> Реальные данные<br /><br /> Кэш. Указывает кэшированное выполнение. Например, запросы к наборам данных не выполняются немедленно.<br /><br /> Моментальный снимок<br /><br /> Журнал<br /><br /> AdHoc. Указывает либо детализированный отчет на основе динамически формируемой модели отчета, либо отчет построителя отчетов, который просматривается на клиенте с помощью сервера отчетов для обработки и подготовки.<br /><br /> Сеанс. Указывает на последующий запрос в рамках уже созданного сеанса.  Например, первоначальный запрос был на просмотр страницы 1, а последующий ― на экспорт в Excel с текущим состоянием сеанса.<br /><br /> RDCE.  Указывает модуль настройки определения отчета. Пользовательское расширение RDCE позволяет динамически настраивать определение отчета до передачи его в модуль обработки при выполнении отчета.|  
|Состояние|Состояние (либо rsSuccess, либо код ошибки. Если ошибок несколько, то записывается только первая).|  
|ByteCount|Размер отчетов, готовых для просмотра (в байтах).|  
|RowCount|Количество возвращенных по запросам строк.|  
|AdditionalInfo|Контейнер свойств XML, содержащий дополнительные сведения о выполнении. Содержимое может быть разным для каждой строки.|  
  
##  <a name="the-additionalinfo-field"></a><a name="bkmk_additionalinfo"></a> Поле AdditionalInfo  
 Поле AdditionalInfo представляет собой контейнер свойств XML или структуру, содержащую дополнительные сведения о выполнении. Содержимое может быть разным для каждой строки журнала.  
  
 Ниже приведены примеры содержимого в поле AdditionalInfo для стандартного и подробного ведения журнала.  
  
 **Пример столбца AddtionalInfo при стандартном ведении журнала**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>147</ConnectionOpenTime>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>642</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>63</ExecuteReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>157</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>60</ExecuteReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 **Пример столбца AddtionalInfo при подробном ведении журнала**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>127</ConnectionOpenTime>  
      <DataSource>  
        <Name>DataSource1</Name>  
        <DataExtension>SQL</DataExtension>  
      </DataSource>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>33</ExecuteReaderTime>  
          <DataReaderMappingTime>30</DataReaderMappingTime>  
          <DisposeDataReaderTime>1</DisposeDataReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>1</ExecuteReaderTime>  
          <DataReaderMappingTime>0</DataReaderMappingTime>  
          <DisposeDataReaderTime>0</DisposeDataReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 Ниже приведены некоторые значения, которые можно увидеть в поле AdditionalInfo.  
  
-   **ProcessingEngine**  
  
     1=SQL Server 2005, 2=новое ядро обработки по требованию. Если в большинстве отчетов все еще отображается значение 1, можно изучить вопрос о том, как их перепроектировать в целях применения более нового и более эффективного ядра обработки по требованию.  
  
     `<ProcessingEngine>2</ProcessingEngine>`  
  
-   **ScalabilityTime**  
  
     Количество миллисекунд, затраченных на выполнение операций, связанных с масштабированием, в обрабатывающем ядре. Значение 0 указывает, что на операции масштабирования не было затрачено дополнительное время, а также свидетельствует о том, что запрос не выполнялся в условиях нехватки памяти.  
  
    ```  
    <ScalabilityTime>  
        <Processing>0</Processing>  
    </ScalabilityTime>  
    ```  
  
-   **EstimatedMemoryUsageKB**  
  
     Оценка пикового объема памяти в килобайтах, расходуемой каждым компонентом в течение конкретного запроса.  
  
    ```  
    <EstimatedMemoryUsageKB>  
        <Processing>38</Processing>  
    </EstimatedMemoryUsageKB>  
    ```  
  
-   **DataExtension**  
  
     Типы модулей обработки данных или источников данных, используемых в отчете. Это число показывает количество вхождений конкретного источника данных.  
  
    ```  
    <DataExtension>  
       <DAX>2</DAX>  
    </DataExtension>  
    ```  
  
-   **ExternalImages**  
  
     Значение указывается в миллисекундах. Эти данные могут использоваться для диагностики проблем производительности. Время, необходимое для получения изображений от внешнего веб-сервера, может замедлить выполнение отчета в целом.  
  
    ```  
    <ExternalImages>  
        <Count>3</Count>  
        <ByteCount>9268</ByteCount>  
        <ResourceFetchTime>9</ResourceFetchTime>  
    </ExternalImages>  
    ```  
  
-   **Соединения**  
  
     Многоуровневая структура  
  
    ```  
    <Connections>  
        <Connection>  
          <ConnectionOpenTime>127</ConnectionOpenTime>  
          <DataSource>  
            <Name>DataSource1</Name>  
            <DataExtension>SQL</DataExtension>  
          </DataSource>  
          <DataSets>  
            <DataSet>  
              <Name>DataSet1</Name>  
              <RowsRead>16</RowsRead>  
              <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>33</ExecuteReaderTime>  
              <DataReaderMappingTime>30</DataReaderMappingTime>  
              <DisposeDataReaderTime>1</DisposeDataReaderTime>  
            </DataSet>  
            <DataSet>  
              <Name>DataSet2</Name>  
              <RowsRead>3</RowsRead>  
              <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>1</ExecuteReaderTime>  
              <DataReaderMappingTime>0</DataReaderMappingTime>  
              <DisposeDataReaderTime>0</DisposeDataReaderTime>  
            </DataSet>  
          </DataSets>  
        </Connection>  
    </Connections>  
  
    ```  
  
##  <a name="log-fields-executionlog2"></a><a name="bkmk_executionlog2"></a> Поля журнала (ExecutionLog2)  
 В этом представлении добавлено несколько новых полей, а некоторые другие поля переименованы. Ниже приведен пример инструкции Transact SQL для выборки строк из представления ExecutionLog2. В этом образце предполагается, что база данных сервера отчетов носит имя **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog2 order by TimeStart DESC  
```  
  
 В следующей таблице описаны данные, которые перехватываются в журнале выполнения отчетов  
  
|Столбец|Описание|  
|------------|------------------------------------------------------------|  
|InstanceName|Имя экземпляра сервера отчетов, обработавшего запрос.|  
|ReportPath|Структура пути к отчету. Отчет, сохраняемый в корневой папке под именем "test", будет иметь путь ReportPath вида "/test".<br /><br /> Отчет с именем "test", сохраняемый в папке "samples", будет иметь ReportPath "/Samples/test/".|  
|UserName|Идентификатор пользователя.|  
|ExecutionID||  
|RequestType|Тип запроса (пользовательский или системный).|  
|Формат|Формат подготовки к просмотру.|  
|Параметры|Значения параметров, используемых при выполнении отчета.|  
|ReportAction|Возможные значения: Render, Sort, BookMarkNavigation, DocumentNavigation, GetDocumentMap, Findstring|  
|TimeStart|Время начала, указывающее на продолжительность обработки отчета.|
|TimeEnd|Время окончания, указывающее на продолжительность обработки отчета.|
|TimeDataRetrieval|Количество миллисекунд, затраченных на выборку данных.|
|TimeProcessing|Количество миллисекунд, затраченных на обработку отчета.|
|TimeRendering|Количество миллисекунд, затраченных на подготовку отчета к просмотру.|
|Источник|Источник выполнения запроса (1 = напрямую, 2 = кэш, 3 = моментальный снимок, 4 = журнал).|  
|Состояние|Состояние (либо rsSuccess, либо код ошибки. Если ошибок несколько, то записывается только первая).|  
|ByteCount|Размер отчетов, готовых для просмотра (в байтах).|  
|RowCount|Количество возвращенных по запросам строк.|  
|AdditionalInfo|Контейнер свойств XML, содержащий дополнительные сведения о выполнении.|  
  
##  <a name="log-fields-executionlog"></a><a name="bkmk_executionlog"></a> Поля журнала (ExecutionLog)  
 Ниже приведен пример инструкции Transact SQL для выборки строк из представления ExecutionLog. В этом образце предполагается, что база данных сервера отчетов носит имя **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog order by TimeStart DESC  
  
```  
  
 В следующей таблице описаны данные, которые перехватываются в журнале выполнения отчетов  
  
|Столбец|Описание|  
|------------|-----------------|  
|InstanceName|Имя экземпляра сервера отчетов, обработавшего запрос.|  
|ReportID|Идентификатор отчета.|  
|UserName|Идентификатор пользователя.|  
|RequestType|Возможные значения:<br /><br /> True = запрос на подписку<br /><br /> False= интерактивный запрос|  
|Формат|Формат подготовки к просмотру.|  
|Параметры|Значения параметров, используемых при выполнении отчета.|  
|TimeStart|Время начала и завершения обработки отчета, указывающее на его продолжительность.|  
|TimeEnd||  
|TimeDataRetrieval|Число миллисекунд, затраченных на получение данных, обработку запроса и его подготовку.|  
|TimeProcessing||  
|TimeRendering||  
|Источник|Источник выполнения отчета. Возможные значения: (1 = реальные данные, 2 = кэш, 3 = моментальный снимок, 4 = журнал, 5 = нерегламентированные данные, 6 = данные сеанса, 7 = данные RDCE).|  
|Состояние|Возможные значения: rsSuccess, rsProcessingAborted или код ошибки. Если происходит несколько ошибок, регистрируется только первая ошибка.|  
|ByteCount|Размер отчетов, готовых для просмотра (в байтах).|  
|RowCount|Количество возвращенных по запросам строк.|  
  
## <a name="see-also"></a>См. также:  
 [Включение событий служб Reporting Services для журнала трассировки SharePoint (ULS)](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)   
 [Файлы и источники журналов служб Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Справочник по ошибкам и событиям (службы Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
