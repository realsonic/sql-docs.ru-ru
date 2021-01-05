---
title: Выпуски и поддерживаемые функции
titleSuffix: SQL Server 2016
description: В этой статье описаны функции, которые поддерживаются различными выпусками SQL Server 2016 и удовлетворяют разным требованиям к производительности, времени выполнения и ценам.
ms.custom:
- seo-lt-2019
- sqlfreshmay19
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
- CPSA
ms.assetid: e5186f02-dd91-47d0-8fa4-de3f41c76903
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8e1544a46e84b77ce290dded936f86b75a251f13
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642946"
---
# <a name="editions-and-supported-features-of-sql-server-2016"></a>Выпуски и поддерживаемые функции SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

В этом разделе подробно описаны функции, поддерживаемые различными выпусками SQL Server.

Сведения о других ранних версиях:

* [SQL Server 2019](editions-and-components-of-sql-server-version-15.md).  
* [SQL Server 2017](editions-and-components-of-sql-server-2017.md).  
* [SQL Server 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true).

Требования для установки сильно зависят от потребностей приложения. Различные выпуски [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] удовлетворяют индивидуальным требованиям каждой организации или отдельного лица к производительности, среде выполнения и цене. Набор устанавливаемых компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] зависит от потребностей конкретного пользователя. В следующих разделах содержатся сведения, на основе которых из множества выпусков и компонентов, доступных в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], можно сделать наилучший выбор.  

Выпуск SQL Server Evaluation доступен для ознакомления в течение 180 дней.  
  
Актуальные заметки о выпуске и сведения о новых возможностях содержатся в следующих разделах:
- [Заметки о выпуске для SQL Server 2017](../sql-server/sql-server-2017-release-notes.md)
- [Заметки о выпуске для SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
- [Новые возможности в SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Что нового в SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)

### <a name="try-sql-server"></a>Попробуйте SQL Server!    
    
> [![Скачать на странице центра оценки](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) **[Скачать SQL Server 2016 на странице центра оценки](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**    
> 
> ![Значок виртуальной машины Azure](/analysis-services/analysis-services/media/azure-virtual-machine-small.png) **[Разверните виртуальную машину с уже установленным SQL Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)**   
  
## <a name="ssnoversion-editions"></a>Выпуски[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 Эти выпуски [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]описаны в следующей таблице. 
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] edition|Определение|  
|---------------------------------------|----------------|  
|Enterprise|Выпуск [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition является предложением премиум класса, обеспечивающим полный набор возможностей ЦОД с исключительно высокой производительностью, неограниченными возможностями виртуализации и исчерпывающими средствами бизнес-аналитики, что позволяет добиться высокого уровня обслуживания важнейших рабочих нагрузок и предоставить конечным пользователям доступ к анализу данных.|  
|Standard|Выпуск [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard обеспечивает основные функции управления данными и предоставляет базу данных бизнес-аналитики для приложений, работающих в отделах и небольших организациях. Поддерживаются распространенные средства разработки в локальных системах и вычислительных облаках, что делает возможным эффективное управление базами данных с минимальными затратами ИТ-ресурсов.|  
|Интернет|Выпуск[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition — это вариант с низкой совокупной стоимостью владения, предназначенный для размещения веб-сайтов и дополнительных веб-услуг, который по доступной цене обеспечивает масштабируемость и функции управления для небольших и крупномасштабных веб-проектов.|  
|Разработчик|Выпуск[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition позволяет разработчикам создавать приложения любого типа на базе [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Он включает все функциональные возможности выпуска Enterprise Edition, однако лицензируется как система для разработки и тестирования, а не для применения в качестве рабочего сервера. Выпуск [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer является идеальным выбором для тех, кто создает [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] и тестирует приложения.|  
|Экспресс-выпуски|Выпуск Express является бесплатной базой данных начального уровня и идеально подходит для обучения, а также для создания управляемых данными приложений, работающих на рабочих станциях и небольших серверах. Этот выпуск — лучший выбор для независимых поставщиков программного обеспечения, непрофессиональных разработчиков и любителей, создающих клиентские приложения. Если необходимы дополнительные функции базы данных, выпуск [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express можно легко обновить до версий [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]более высокого класса. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB, облегченная версия Express, которая имеет все программные функции, запускается в пользовательском режиме, быстро устанавливается, не требует настройки; количество предварительных условий для ее установки невелико.|  
  
## <a name="using-ssnoversion-with-an-internet-server"></a>Использование [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с веб-сервером  
 На веб-сервере (например, под управлением служб IIS) обычно устанавливают клиентские средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Клиентские средства включают в себя клиентские компоненты соединения, которые используются приложениями, соединяющимися с экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]
> Хотя возможна установка экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на тот же компьютер, где работают службы IIS, обычно это делается только для небольших веб-сайтов, состоящих из одиночного серверного компьютера. У большинства веб-сайтов их системы IIS среднего уровня расположены на одном сервере или серверном кластере, а базы данных — на отдельном сервере или федерации серверов.  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Использование [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с клиентскими и серверными приложениями  
 На компьютер, где работают клиент-серверные приложения, которые подключаются непосредственно к экземпляру [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , можно установить только клиентские компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Установка клиентских компонентов будет хорошим выбором также и в том случае, если администрируется экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на сервере базы данных или планируется разработка приложений [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 При выборе установки клиентских средств будут установлены следующие компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : компоненты обеспечения обратной совместимости, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], компоненты подключения, средства управления, пакет средств разработки программного обеспечения и компоненты электронной документации по [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Установка SQL Server](../database-engine/install-windows/install-sql-server.md).  
  
## <a name="deciding-among-ssnoversion-components"></a>Выбор компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  
 На странице «Выбор компонентов» мастера установки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] выберите компоненты, которые должны быть включены в установку [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. По умолчанию в дереве не выбран ни один из компонентов.  
  
 По следующим таблицам определите набор компонентов, лучше всего соответствующий вашим потребностям.  
  
|Компоненты сервера|Описание|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] включает компонент [!INCLUDE[ssDE](../includes/ssde-md.md)], основную службу для хранения, обработки и обеспечения безопасности данных, репликации, полнотекстового поиска, средств управления реляционными и XML-данными, интеграции аналитики с базами данных и интеграции PolyBase для доступа к Hadoop и другим разнородным источникам данных, а также сервер [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS).|  
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] содержит средства создания приложений оперативной аналитической обработки (OLAP) и приложений интеллектуального анализа данных, а также средства управления ими.|  
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Службы[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] включают в себя серверные и клиентские компоненты для создания, управления и развертывания табличных, матричных и графических отчетов, а также отчетов в свободной форме. Службы[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] являются расширяемой платформой, которую можно использовать для разработки приложений отчетов.|  
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] представляют собой набор графических средств и программируемых объектов для перемещения, копирования и преобразования данных. Они также включают компонент [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) для служб [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) — это решение [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] по управлению основными данными. MDS можно настроить для управления любой структурой (товары, заказчики, счета). Поддерживаются иерархии, детальная настройка безопасности, транзакции, управление версиями данных и бизнес-правила, а также использование [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] для управления данными.|  
|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)]|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)] поддерживает распределенные и масштабируемые решения R на нескольких платформах с использованием нескольких корпоративных источников данных, включая Hadoop, Teradata и Linux.|  
  
|Средства управления|Описание|  
|----------------------|-----------------|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|Среда[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] — это интегрированная среда для доступа, настройки, управления, администрирования и разработки всех компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Среда[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] позволяет разработчикам и администраторам, обладающим различными уровнями навыков, использовать [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> Скачивание и установка <br />                [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] со страницы  [Скачивание SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager|Диспетчер конфигурации[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] обеспечивает базовые возможности управления конфигурациями для служб, серверных протоколов, клиентских протоколов и псевдонимов клиентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|  
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|Приложение[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] предоставляет графический пользовательский интерфейс для наблюдения за экземпляром компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] или служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|  
|Помощник по настройке[!INCLUDE[ssDE](../includes/ssde-md.md)]|Помощник по настройке ядра компонента[!INCLUDE[ssDE](../includes/ssde-md.md)] помогает создавать оптимальные наборы индексов, индексированных представлений и секций.|  
|Клиент Data Quality|Предоставляет очень простой и понятный графический пользовательский интерфейс для подключения к серверу DQS и выполнения операций очистки данных. Он также позволяет централизованно отслеживать различные действия, выполняемые во время операции очистки данных.|  
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] содержат интегрированную среду разработки, предназначенную для создания решений для следующих компонентов бизнес-аналитики: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]и [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Ранее — среда Business Intelligence Development Studio.)<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] также содержит компонент «Проекты баз данных», который предоставляет интегрированную среду для разработчиков, предназначенную для выполнения всех работ по разработке баз данных для любой платформы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (на самом предприятии и за его пределами) в Visual Studio. Разработчикам баз данных предлагается расширенный обозреватель серверов, который является компонентом Visual Studio, предназначенным для облегчения процессов создания и изменения объектов баз данных и данных в них, а также для выполнения запросов.|  
|Компоненты связи|Устанавливает компоненты для связи между клиентами и серверами и сетевые библиотеки для DB-библиотеки, ODBC и OLE DB.|  
  
|Документация|Описание|  
|-------------------|-----------------|  
|Электронная документация по[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Основная документация для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].| 

**Выпуски Evaluation и Developer**  
Поддерживаемые компоненты для выпусков Developer и Evaluation указаны в списке возможностей SQL Server Enterprise в приведенных ниже таблицах.


Выпуск Developer edition по-прежнему поддерживает только 1 клиент для [распределенного воспроизведения SQL Server](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Scale Limits  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|
|Максимальная вычислительная мощность, используемая одним экземпляром, — [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Максимальное значение, поддерживаемое операционной системой|Ограничение: меньшее из 4 процессоров и 24 ядер|Ограничение: меньшее из 4 процессоров и 16 ядер|Ограничение: меньшее из 1 процессора и 4 ядер|Ограничение: меньшее из 1 процессора и 4 ядер| 
|Максимальная вычислительная мощность, используемая одним экземпляром, — [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] или [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Максимальное значение, поддерживаемое операционной системой|Ограничение: меньшее из 4 процессоров и 24 ядер|Ограничение: меньшее из 4 процессоров и 16 ядер|Ограничение: меньшее из 1 процессора и 4 ядер|Ограничение: меньшее из 1 процессора и 4 ядер|  
|Максимальный объем памяти для буферного пула на экземпляр [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Максимум, поддерживаемый операционной системой|128 ГБ|64 ГБ|1410 МБ|1410 МБ|
|Максимальный объем памяти для кэша сегмента Columnstore на экземпляр [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Неограниченная память| 32 ГБ<sup>2</sup>| 16 ГБ<sup>2</sup>| 352 ГБ<sup>2</sup>| 352 ГБ<sup>2</sup>|  
|Максимальный размер данных, оптимизированных для памяти, на базу данных в [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Неограниченная память| 32 ГБ<sup>2</sup>| 16 ГБ<sup>2</sup>| 352 ГБ<sup>2</sup>| 352 ГБ<sup>2</sup>|  
|Максимальный объем используемой памяти на экземпляр [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Максимум, поддерживаемый операционной системой|Табличный: 16 ГБ<br /><br /> MOLAP: 64 ГБ|Н/Д|Недоступно|Н/Д|  
|Максимальный объем используемой памяти на экземпляр [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Максимум, поддерживаемый операционной системой|64 ГБ|64 ГБ|4 ГБ|Н/Д|
|Максимальный размер реляционной базы данных|524 ПБ|524 ПБ|524 ПБ|10 ГБ|10 ГБ|  
  
<sup>1</sup> Использование выпуска Enterprise Edition с лицензированием по принципу "лицензия на сервер и клиентские лицензии (Server+CAL)" (недоступно для новых соглашений) ограничено максимум 20 ядрами в расчете на экземпляр SQL Server. В модели лицензирования по числу ядер никаких ограничений нет. Дополнительные сведения см. в разделе [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
<sup>2</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками. 

##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS High Availability  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Поддержка Server Core <sup>1</sup>|Да|Да|Да|Да|Да|  
|доставка журналов;|Да|Да|Да|Нет|Нет|  
|Зеркальное отображение базы данных|Да|Да<br /><br /> Только полная безопасность|Только следящий сервер|Только следящий сервер|Только следящий сервер| 
|Сжатие резервных копий|Да|Да|Нет|Нет|нет| 
|Моментальный снимок базы данных|Да|Да <sup>3</sup>|Да <sup>3</sup>|Да <sup>3</sup>|Да <sup>3</sup>|
|Экземпляры отказоустойчивого кластера AlwaysOn|Да<br /><br /> 16|Да<br /><br /> Поддержка 2 узлов|Нет|Нет|Нет|  
|Группы доступности AlwaysOn|Да<br /><br /> До 8 вторичных реплик, включая 2 синхронные вторичные реплики|Нет|Нет|Нет|Нет|
|Базовые группы доступности <sup>2</sup>|Нет|Да<br /><br /> Поддержка 2 узлов|Нет|Нет|нет|
|Восстановление страниц и файлов в режиме «в сети»|Да|Нет|Нет|Нет|нет|
|Индексирование в сети|Да|Нет|Нет|Нет|нет|
|Изменение схемы в режиме «в сети»|Да|Нет|Нет|Нет|нет|
|Быстрое восстановление|Да|Нет|Нет|Нет|нет|
|Зеркальные резервные копии|Да|Нет|Нет|Нет|нет|
|Поддержка памяти и ЦП с "горячей" заменой|Да|Нет|Нет|Нет|Нет|
|Помощник по восстановлению базы данных|Да|Да|Да|Да|Да|
|Зашифрованная резервная копия|Да|Да|Нет|Нет|нет|
|Гибридное резервное копирование в Azure (резервное копирование по URL-адресу)|Да|Да|Нет|Нет|Нет|  
  
 <sup>1</sup> Дополнительные сведения об установке SQL Server на Server Core см. в разделе [установить SQL Server на Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md). 

<sup>2</sup> Дополнительные сведения о базовых группах доступности см. в разделе [Базовые группы доступности](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).  

<sup>3</sup> применяется к [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 2016 SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками. 
  
##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS Scalability and Performance  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Columnstore <sup>1</sup>|Да|Да <sup>2</sup>|Да <sup>2</sup>|Да<sup>2</sup>|Да<sup>2</sup>|  
|Выполняющаяся в памяти OLTP <sup>1</sup>|Да|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>, <sup>3</sup>|Да <sup>2</sup>|
|Stretch Database|Да|Да|Да|Да|Да|
|Постоянная основная память|Да|Да|Да|Да|Да|
|Поддержка нескольких экземпляров|50|50|50|50|50|
|Секционирование таблиц и индексов|Да|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|  
|Сжатие данных|Да|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|
|Resource Governor|Да|Нет|Нет|Нет|нет|  
|Параллелизм секционированных таблиц|Да|Нет|Нет|Нет|Нет|
|Несколько контейнеров файлового потока|Да|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|Да <sup>2</sup>|
|Память больших страниц с поддержкой NUMA и выделение массивов буферов|Да|Нет <sup>4</sup>|Нет|Нет|Нет|
|Buffer Pool Extension|Да|Да|Нет|Нет|Нет|
|Управление ресурсами ввода-вывода|Да|Нет|Нет|Нет|Нет|
|Упреждающее чтение|Да|Нет|Нет|Нет|Нет|
|Расширенный просмотр|Да|Нет|Нет|Нет|нет|
|Отложенная устойчивость|Да|Да|Да|Да|Да|

<sup>1</sup> Размер данных выполняющейся в памяти OLTP и кэша сегмента Columnstore ограничены объемом памяти, указанным в выпуске в разделе "Ограничения масштабирования". Максимальная степень параллелизма ограничена. Степень параллелизма процесса (DOP) для построения индекса ограничена значением 2 для выпуска Standard и 1 для выпусков Express и Web. Это относится к индексам columnstore, созданным на основе таблиц на диске и оптимизированных для памяти таблиц.

<sup>2</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками. Передача агрегата, передача предиката строки и оптимизация SIMD — улучшения масштабируемости в выпуске [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Дополнительные сведения см. в статье [Новые возможности индексов columnstore](../relational-databases/indexes/columnstore-indexes-what-s-new.md). 

<sup>3</sup> Эта функция не включена в вариант установки LocalDB.

<sup>4</sup> Для выпусков Standard Edition и SQL Server с клиентской лицензией число процессоров, используемых SQL Server Standard, может быть ограничено. При этом SQL Server Standard учитывает архитектуру NUMA. 
##  <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS Security  
  
|Компонент|Enterprise|Standard|Интернет|Express|Express с дополнительными службами|  
|-------------|----------------|--------------|---------|-------------|------------------------------------| 
|Безопасность на уровне строк|Да|Да|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>|  
|Always Encrypted|Да|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>| 
|Динамическое маскирование данных|Да|Да|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>|   
|Аудит сервера|Да|Да|Да|Да|Да| 
|Аудит базы данных|Да|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>|Да <sup>1</sup>| 
|Прозрачное шифрование в базе данных|Да|Нет|Нет|Нет|Нет|   
|Расширенное управление ключами (Extensible Key Management)|Да|Нет|Нет|Нет|нет| 
|Определяемые пользователем роли|Да|Да|Да|Да|Да| 
|Автономные базы данных|Да|Да|Да|Да|Да| 
|Шифрование для резервного копирования|Да|Да|Нет|Нет|Нет|  

<sup>1</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками.      

##  <a name="replication"></a><a name="Replication"></a> Репликация  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Разнородные подписчики|Да|Да|Нет|Нет|Нет|  
|Репликация слиянием|Да|Да|Да (только подписчик)|Да (только подписчик)|Да (только подписчик)|   
|публикация Oracle|Да|Нет|Нет|Нет|Нет| 
|Одноранговая репликация транзакций|Да|Нет|Нет|Нет|Нет|   
|репликация моментальных снимков;|Да|Да|Да (только подписчик)|Да (только подписчик)|Да (только подписчик)|   
|Отслеживание изменений в SQL Server|Да|Да|Да|Да|Да| 
|Репликация транзакций|Да|Да|Да (только подписчик)|Да (только подписчик)|Да (только подписчик)|   
|Репликация транзакций в Azure|Да|Да|Нет|Нет|Нет|   
|Обновляемая подписка репликации транзакций|Да|Да|Нет|Нет|Нет|  
  
##  <a name="management-tools"></a><a name="SSMS"></a> Management Tools  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Объекты SMO|Да|Да|Да|Да|Да|  
|Диспетчер конфигурации SQL Server|Да|Да|Да|Да|Да|   
|SQL CMD (программа командной строки)|Да|Да|Да|Да|Да|      
|Распределенное воспроизведение — средство администрирования|Да|Да|Да|Да|Нет|  
|Распределенное воспроизведение — клиент|Да|Да|Да|Нет|Нет|  
|Распределенное воспроизведение — контроллер|Да (до 16 клиентов)|Да (1 клиент)|Да (1 клиент)|Нет|Нет|   
|SQL Profiler|Да|Да|Нет <sup>1</sup>|Нет <sup>1</sup>|Нет <sup>1</sup>|  
|Агент SQL Server|Да|Да|Да|Нет|Нет| 
|Пакет управления Microsoft System Center Operations Manager|Да|Да|Да|Нет|Нет|  
|Помощник по настройке ядра СУБД (DTA)|Да|Да <sup>2</sup>|Да <sup>2</sup>|Нет|Нет|      
  
 <sup>1</sup> Для выпусков SQL Server Web Edition, SQL Server Express, SQL Server Express с инструментами и SQL Server, экспресс-выпуск с дополнительными службами, поддерживается профилирование с помощью выпусков SQL Server Standard Edition и SQL Server Enterprise Edition.  
  
 <sup>2</sup> Настройка включена только для компонентов выпуска Standard Edition.  
  
##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS Manageability  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Пользовательские экземпляры|Нет|Нет|Нет|Да|Да| 
|LocalDB|Нет|Нет|Нет|Да|Нет| 
|Выделенное административное соединение|Да|Да|Да|Да, с помощью флага трассировки|Да, с помощью флага трассировки|   
|Поддержка скриптов PowerShell|Да|Да|Да|Да|Да| 
|Поддержка SysPrep <sup>1</sup>|Да|Да|Да|Да|Да| 
|Поддержка операций с компонентами приложения уровня данных — извлечение, развертывание, обновление, удаление|Да|Да|Да|Да|Да| 
|Автоматизация политики (проверка по расписанию и изменение)|Да|Да|Да|Нет|нет|   
|Сборщик данных производительности|Да|Да|Да|Нет|Нет| 
|Возможность регистрации в качестве управляемого экземпляра в среде управления несколькими экземплярами|Да|Да|Да|Нет|нет|   
|Стандартный производительности отчет|Да|Да|Да|Нет|нет| 
|Структуры планов и закрепление плана для структур планов|Да|Да|Да|Нет|нет|   
|Прямой запрос индексированных представлений (с использованием указания NOEXPAND)|Да|Да|Да|Да|Да| 
|Автоматическое сопровождение индексированного представления|Да|Да|Да|Нет|нет| 
|Распределенные секционированные представления|Да|Нет|Нет|Нет|нет| 
|Параллельные операции с индексами|Да|Нет|Нет|Нет|нет|  
|Автоматическое использование индексированного представления оптимизатором запросов|Да|Нет|Нет|Нет|нет| 
|Проверка согласованности параллелизма|Да|Нет|Нет|Нет|нет| 
|Точка управления служебной программой SQL Server|Да|Нет|Нет|Нет|Нет|    
|Расширение буферного пула|Да|Да|Нет|Нет|Нет| 
  
 <sup>1</sup> Дополнительные сведения см. в разделе [Вопросы по установке SQL Server с помощью SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
 
<sup>2</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками.      
  
##  <a name="development-tools"></a><a name="DevTools"></a> Development Tools  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Интеграция со средой Microsoft Visual Studio|Да|Да|Да|Да|Да| 
|Технология IntelliSense (Transact-SQL и многомерные выражения)|Да|Да|Да|Да|Да| 
|SQL Server Data Tools (SSDT)|Да|Да|Да|Да|Нет|    
|Средства изменения, отладки и проектирования многомерных выражений|Да|Да|Нет|Нет|нет|   
  
##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Базовая интеграция R|Да|Да|Да|Да|Нет|   
|Продвинутая интеграция R|Да|Нет|Нет|Нет|Нет| 
|R Server (Standalone)|Да|Нет|Нет|Нет|Нет|   
|Вычислительный узел PolyBase|Да|Да <sup>1</sup>|Да <sup>1</sup>, <sup>2</sup>|Да <sup>1</sup>, <sup>2</sup>|Да <sup>1</sup>, <sup>2</sup>| 
|Головной узел PolyBase|Да|Нет|Нет|Нет|Нет| 
|JSON|Да|Да|Да|Да|Да|   
|Хранилище запросов|Да|Да|Да|Да|Да|   
|Temporal|Да|Да|Да|Да|Да|   
|Интеграция со средой CLR|Да|Да|Да|Да|Да|   
|Собственная поддержка XML|Да|Да|Да|Да|Да| 
|Индексирование XML|Да|Да|Да|Да|Да| 
|Возможности MERGE & UPSERT|Да|Да|Да|Да|Да|   
|поддержка FILESTREAM|Да|Да|Да|Да|Да| 
|FileTable|Да|Да|Да|Да|Да| 
|Типы данных даты и времени|Да|Да|Да|Да|Да|  
|Поддержка международного использования|Да|Да|Да|Да|Да| 
|Семантический поиск и полнотекстовый поиск|Да|Да|Да|Да|Нет| 
|Определение языка в запросе|Да|Да|Да|Да|Нет|   
|Компонент Service Broker (сообщения)|Да|Да|Нет (только клиент)|Нет (только клиент)|Нет (только клиент)|   
|конечные точки в языке Transact-SQL|Да|Да|Да|Нет|Нет| 

<sup>1</sup> Для горизонтального увеличения масштаба с несколькими вычислительными узлами требуется головной узел.

<sup>2</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками.     
  
## <a name="integration-services"></a><a name="IS"></a> Службы Integration Services

Сведения о функциях служб Integration Services (SSIS), поддерживаемых различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], см. в статье [о функциях служб Integration Services, поддерживаемых разными выпусками SQL Server 2016](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="master-data-services"></a><a name="MDS"></a> Master Data Services  
 Сведения о [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] и возможности служб качества данных, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], см. в разделе [Master Data Services и данных качества служб функции, поддерживаемые различными выпусками SQL Server](../master-data-services/master-data-services-and-data-quality-services-features-support.md). 

  
##  <a name="data-warehouse"></a><a name="DW"></a> Data Warehouse  
  
|Компонент|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Создание кубов без базы данных|Да|Да|Нет|Нет|Нет |   
|Автоматическое создание промежуточных схем и схем хранилища данных|Да|Да|Нет|Нет|Нет| 
|система отслеживания измененных данных|Да|Да <sup>1</sup>|Нет|Нет|Нет| 
|Оптимизация запросов с соединением типа «звезда»|Да|Нет|Нет|Нет|Нет| 
|Масштабируемая конфигурация служб Analysis Services, доступная только для чтения|Да|Нет|Нет|Нет|Нет| 
|Параллельная обработка запросов для секционированных таблиц и индексов|Да|Нет|Нет|Нет|Нет|   
|Глобальная статистическая обработка пакета|Да|Нет|Нет|Нет|Нет| 

<sup>1</sup> применяется к [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 в процессе формирования общей контактной зоны возможностей программирования (CPSA) между выпусками.     

##  <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services  
  
Дополнительные сведения о функциях служб Analysis Services, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016). 
  
##  <a name="bi-semantic-model-multi-dimensional"></a><a name="BIMD"></a> BI Semantic Model (Multi Dimensional)  
  
Дополнительные сведения о функциях служб Analysis Services, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
   
##  <a name="bi-semantic-model-tabular"></a><a name="BIT"></a> BI Semantic Model (Tabular)  
  
Дополнительные сведения о функциях служб Analysis Services, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="power-pivot-for-sharepoint"></a><a name="PPSP"></a> Power Pivot for SharePoint  
  
Сведения о PowerPivot для SharePoint возможности, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="data-mining"></a><a name="DM"></a> Data Mining  
  
Сведения о функции интеллектуального анализа данных, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="reporting-services"></a><a name="SSRS"></a> Службы Reporting Services  
  
Дополнительные сведения о функциях служб Reporting Services, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Reporting Services функции, поддерживаемые различными выпусками SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

##  <a name="business-intelligence-clients"></a><a name="BIC"></a> Клиенты бизнес-аналитики  

Дополнительные сведения о функциях клиента Intelligence бизнеса, поддерживаемые различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], в разделе [Analysis Services функции, поддерживаемые различными выпусками SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016) или [Reporting Services функции, поддерживаемые различными выпусками SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Spatial and Location Services  
  
|Имя функции|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Пространственные индексы|Да|Да|Да|Да|Да|   
|Плоский и геодезический типы данных|Да|Да|Да|Да|Да| 
|Дополнительные пространственные библиотеки|Да|Да|Да|Да|Да|   
|Импорт-экспорт стандартных форматов пространственных данных|Да|Да|Да|Да|Да|   
  
##  <a name="additional-database-services"></a><a name="ADS"></a> Additional Database Services  
  
|Имя функции|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Помощник по миграции|Да|Да|Да|Да|Да|   
|Компонент Database Mail|Да|Да|Да|Нет|Нет| 
  
##  <a name="other-components"></a><a name="Other"></a> Other Components  
  
|Имя функции|Enterprise|Standard|Интернет|Express с дополнительными службами|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------|  
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|Нет|Нет| 
|StreamInsight HA|StreamInsight Premium Edition|Нет|Нет|Нет|Нет|   
  
> [![Скачать SSMS](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](../ssms/download-sql-server-management-studio-ssms.md) **[Скачайте последнюю версию SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)** .      
  
## <a name="see-also"></a>См. также  
 [Установка SQL Server](../database-engine/install-windows/install-sql-server.md)  
 
  
