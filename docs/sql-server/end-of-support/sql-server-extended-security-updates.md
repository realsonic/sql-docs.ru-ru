---
title: Что такое дополнительные обновления системы безопасности?
description: Узнайте, как использовать реестр SQL Server, чтобы получать дополнительные обновления системы безопасности для продуктов SQL Server, срок поддержки которых истек или жизненный цикл которых завершается (например, SQL Server 2008 и SQL Server 2008 R2).
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 94939adb2934e16ed7d1c81c33ebab4631c4b114
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172516"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>Что такое дополнительные обновления системы безопасности SQL Server?
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

В этой статье содержатся сведения об использовании службы реестра SQL Server для получения дополнительных обновлений системы безопасности [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. Более подробные сведения о других вариантах см. в статье [SQL Server end of support options](sql-server-end-of-life-overview.md) (Варианты при окончании поддержки SQL Server). 

## <a name="overview"></a>Обзор
После завершения жизненного цикла поддержки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вы можете оформить подписку на получение дополнительных обновлений системы безопасности (ESU) для своих серверов и сохранить защиту еще на три года, пока не будете готовы обновиться до новой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или перейти на [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Эта подписка доступна двумя способами:
-  Ее можно приобрести для локальных или размещенных серверов среды.
-  Она предоставляется бесплатно и включена по умолчанию при переносе локальных серверов на виртуальные машины Azure. Затем можно использовать службу **реестра SQL Server** на портале Azure, чтобы зарегистрировать конечный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и скачивать обновления по мере их доступности. 

Чтобы поддерживать защищенность экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Майкрософт рекомендует применять исправления ESU сразу же, как только они станут доступны. Подробные сведения об ESU см. на [странице часто задаваемых вопросов о ESU](https://www.microsoft.com/cloud-platform/extended-security-updates).

> [!IMPORTANT]
> [Расширенная поддержка SQL Server 2008 и SQL Server 2008 R2 закончилась 10 июля 2019 года](https://www.microsoft.com/cloud-platform/windows-sql-server-2008). Для этих версий рекомендуется использовать описанные в этой статье дополнительные обновления безопасности или другие варианты миграции. Дополнительные сведения см. [здесь](sql-server-end-of-life-overview.md).

## <a name="what-are-extended-security-updates"></a>Что такое дополнительные обновления системы безопасности
Дополнительные обновления безопасности (ESU) для [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] предусматривают подготовку обновлений системы безопасности для тех клиентов, которые приобрели подписку на обновления в рамках расширенной поддержки.

Обновления ESU становятся доступными **по мере необходимости**, когда в системе безопасности обнаруживается уязвимость и [Центр Майкрософт по реагированию на угрозы](https://portal.msrc.microsoft.com) присваивает этой угрозе **критический** уровень. Таким образом, регулярные выпуски ESU для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не предусмотрены.

ESU не содержат:
- новые функции;
- функциональные улучшения;
- запрошенные клиентом исправления.

### <a name="support"></a>Поддержка
Обновления ESU не предусматривают техническую поддержку. Но если вы решили остаться в локальной среде, вы можете использовать активный контракт на поддержку [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] (например, [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3), поддержка Premier или Единая поддержка) и получать техническую поддержку по рабочим нагрузкам, которые предусмотрены подпиской на ESU. Кроме того, если вы перенесли свою среду в Azure, используйте для получения технической поддержки план поддержки Azure. 

  > [!NOTE]
  > Майкрософт не предоставляет техническую поддержку для экземпляров [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] (как в локальной, так и облачной среде), которые не включены в подписку на ESU. 

## <a name="esu-availability-and-deployment"></a>Доступность и развертывание ESU
ESU доступны для клиентов, рабочие нагрузки которых выполняются в Azure, локальной среде или в размещенных средах.

### <a name="azure-virtual-machines"></a>Виртуальные машины Azure
Если вы перенесете рабочие нагрузки на Виртуальные машины Azure (IaaS), у вас будет доступ к дополнительным обновлениям системы безопасности [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] в течение трех лет после окончания поддержки. При этом у вас **не будет дополнительных расходов** свыше стоимости использования виртуальной машины. Клиентам не нужна программа Software Assurance, чтобы получать дополнительные обновления системы безопасности в Azure. 

Виртуальные машины Azure с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] под управлением **Windows Server 2008 R2 и более поздней версии** получат ESU автоматически через существующие каналы обновления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если на виртуальной машине настроено [автоматизированное применение исправлений](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching).

На виртуальных машинах Azure с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] под управлением **Windows Server 2008** и на виртуальных машинах, на которых **_не_ настроено [автоматизированное применение исправлений](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)** , потребуется вручную скачать и развернуть исправления ESU, как описано в разделе [Локальные и размещенные среды](#on-premises-or-hosted-environments).

### <a name="on-premises-or-hosted-environments"></a>Локальные и размещенные среды
Если вы подписаны на программу Software Assurance, в рамках соглашения Enterprise Agreement (EA), соглашения Enterprise Subscription (EAS), Server & Cloud Enrollment (SCE) или Enrollment for Education Solutions (EES), вы можете приобрести подписку на расширенные обновления системы безопасности (ESU) на период до трех лет после окончания срока поддержки. Вы можете приобрести ESU только для тех серверов, которые необходимо защитить. ESU можно приобрести непосредственно у корпорации Майкрософт или у партнера Майкрософт по лицензированию. 

Клиенты, на которых распространяются соглашения ESU, должны выполнить следующие действия для скачивания и развертывания исправления ESU:
-  [Зарегистрируйте подходящие экземпляры](#register-instances-for-esus) в **[Реестре SQL Server](#create-sql-server-registry)** . 
-  После регистрации при каждом выпуске исправлений ESU на портале Azure будет доступна ссылка для скачивания пакета. 
-  Скачанный пакет можно развернуть в локальных или размещенных средах вручную или с помощью любого решения по согласованию обновлений в вашей организации, например Microsoft Endpoint Configuration Manager (прежнее название — System Center Configuration Manager). 

> [!NOTE]
> Кроме того, этот процесс клиентам необходимо выполнить для Azure Stack и виртуальных машин Azure, которые не настроены для получения автоматических обновлений.

Дополнительные сведения см. в статье [Часто задаваемые вопросы об обновлениях системы безопасности](https://www.microsoft.com/cloud-platform/extended-security-updates). 

## <a name="create-sql-server-registry"></a>Создание реестра SQL Server
Чтобы зарегистрировать экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для ESU, сначала необходимо создать реестр SQL Server на портале Azure. 

> [!IMPORTANT]
> Нет необходимости регистрировать экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для ESU при использовании виртуальной машины Azure, настроенной для [автоматического обновления](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching). 

Чтобы создать реестр SQL Server, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com). 
1. Выберите элемент **Создать ресурс**. 
1. Введите `SQL Server registry` в поле поиска.  
1. Выберите элемент **Реестр SQL Server**, опубликованный [!INCLUDE[msCoName](../../includes/msconame-md.md)], а затем выберите команду **Создать**. 

   ![Снимок экрана: портал Azure, на котором создается реестр SQL Server.](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. В разделе **Сведения о проекте** в раскрывающемся списке выберите свою подписку. Затем выберите существующую **группу ресурсов** или выберите команду **Создать**, чтобы создать новую группу ресурсов для новой службы реестра SQL Server. 
1. В разделе **Сведения о службе** укажите имя и регион для нового ресурса **реестра SQL Server**: 

   ![Снимок экрана: вкладка "Основные сведения" для реестра SQL Server.](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. Выберите элемент **Review + create** (Проверить и создать), чтобы ознакомиться с подробными сведениями о **реестре SQL Server**. После успешной проверки нажмите кнопку **Создать**. 

## <a name="register-instances-for-esus"></a>Регистрация экземпляров для ESU

После развертывания ресурса **реестр SQL Server** можно зарегистрировать [один](#single-sql-server-instance) экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], или зарегистрировать несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [одновременно](#multiple-sql-server-instances-in-bulk). Чтобы загрузить пакеты ESU, необходимо зарегистрировать хотя бы один экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в области реестра SQL Server. 

### <a name="single-sql-server-instance"></a>Один экземпляр SQL Server

Чтобы зарегистрировать один экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com). 
1. Перейдите к ресурсу **реестра SQL Server**. 
1. Выберите элемент **+ Register** (Зарегистрировать) в области **Обзор**. 

   ![Нажмите кнопку регистрации, чтобы зарегистрировать один экземпляр SQL Server](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. Укажите необходимые сведения, как описано в этой таблице, а затем выберите элемент **Зарегистрировать**. 

   |**Значение**| **Описание**|
   | :-------| :------------- |
   | **Экземпляр** | Введите выходные данные команды `SELECT @@SERVERNAME`, такие как `MyServer\Instance01`. | 
   | **Версия SQL** | В раскрывающемся списке выберите 2008 или 2008 R2. | 
   | **Выпуск** | Выберите подходящий выпуск в раскрывающемся списке: Datacenter, Developer (бесплатное развертывание, если приобретен ESU), Enterprise, Standard, Web, Workgroup. | 
   | **Ядра** | Укажите количество ядер для этого экземпляра | 
   | **Тип размещения** | Выберите подходящий тип размещения в раскрывающемся списке: виртуальная машина (локальная среда), физический сервер (локальный), виртуальная машина Azure, Amazon EC2, Google Compute Engine, другое. |
   | **SubscriptionID**<sup>1</sup> | Введите идентификатор подписки SubscriptionID, на котором создана виртуальная машина.  |
   | **Группа ресурсов**<sup>1</sup> | Введите группу ресурсов, в которой создана виртуальная машина.  | 
   | **Имя виртуальной машины Azure**<sup>1</sup>  | Введите имя ресурса виртуальной машины.  | 
   | **Операционная система виртуальной машины Azure**<sup>1</sup> | Выберите соответствующую версию операционной системы Windows Server в раскрывающемся списке. | 

   <sup>1</sup> Только для виртуальных машин Azure. 

Зарегистрированный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] теперь отображается в разделе **Register SQL Server instances** (Экземпляры в реестре SQL Server) в области **Обзор**. 

![Зарегистрированные экземпляры SQL Server](media/sql-server-extended-security-updates/registered-sql-instance.png)

После регистрации экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] раздел **Обновление системы безопасности** будет доступен. Все доступные ESU будут опубликованы тут. 

### <a name="multiple-sql-server-instances-in-bulk"></a>Несколько экземпляров SQL Server одновременно

Зарегистрировать сразу несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно путем отправки CSV-файла. Как только [CSV-файл будет отформатирован правильно](#formatting-requirements-for-csv-file), можно выполнить следующие действия, чтобы зарегистрировать несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для ресурса реестра SQL Server. 

1. Войдите на [портал Azure](https://portal.azure.com). 
1. Перейдите к ресурсу **реестра SQL Server**. 
1. Выберите элемент **Bulk Register** (Массовая регистрация) в области **Обзор**.  

   ![Массовая регистрация нескольких экземпляров SQL Server](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. Щелкните значок файла, чтобы перейти к расположению своего CSV-файла. Выберите этот CSV-файл. Затем выберите элемент **Зарегистрировать**, чтобы отправить файл и зарегистрировать несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

   ![Отправка CSV-файла для регистрации нескольких экземпляров SQL Server](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>Требования к форматированию CSV-файла
- Значения разделяются запятыми
- Значения нельзя заключать в одинарные или двойные кавычки
- В названиях столбцов регистр не учитывается, но их необходимо **именовать**, как показано ниже: 
  - name
  - version
  - edition
  - cores
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup> требуется только для виртуальных машин Azure. 

#### <a name="csv-example-1---on-premises"></a>Пример CSV №1 — локальная среда

Для локальных экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CSV-файл должен выглядеть следующим образом: 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

Пример CSV-файла см. в [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv).

#### <a name="csv-example-2---azure-vm"></a>Пример CSV №2 — виртуальная машина Azure

Для экземпляров виртуальных машин Azure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CSV-файл должен выглядеть следующим образом: 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

Пример CSV-файла для виртуальной машины Azure см. в [MyAzureVMs.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv). 


> [!TIP]
> Примеры сценариев PowerShell и [!INCLUDE[tsql](../../includes/tsql-md.md)], которые могут генерировать требуемые сведения о регистрации экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в CSV-файл, см. в разделе [ESU registration script examples](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md) (Примеры сценариев регистрации ESU). 

## <a name="download-esus"></a>Загрузка ESU

После регистрации экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в службе реестра SQL Server пакеты дополнительных обновлений безопасности, если и когда они станут доступны, можно загрузить по ссылке на портале Azure. 

Чтобы загрузить ESU, выполните следующие действия. 

1. Войдите на [портал Azure](https://portal.azure.com). 
1. Перейдите к ресурсу **реестра SQL Server**. 
1. В области навигации выберите **Обновления системы безопасности**. 

   ![Поиск доступных обновлений на панели "Обновления системы безопасности"](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. Загрузите обновления системы безопасности отсюда, если и когда они станут доступны. 

## <a name="supported-regions-and-data-residency"></a>Поддерживаемые регионы и размещение данных

Служба **Реестр SQL Server** (предварительная версия) доступна только в определенных регионах Azure. В таблице ниже приведены поддерживаемые регионы и типы размещения данных в каждом из них.

| **Регион** | **Местонахождение данных** |
|:--|:--|
|Восточная Австралия|Географические|
|Australia Southeast|Географические|
|Центральная Канада|Географические|
|Центральная Франция|Географические|
|Japan East|Географические|
|Западная Япония|Географические|
|Республика Корея, центральный регион|Географические|
|Республика Корея, южный регион|Географические|
|Центрально-северная часть США|Географические|
|Северная Европа|Географические|
|Центрально-южная часть США|Географические|
|Southeast Asia|Один регион|
|Южная Индия|Географические|
|Северная часть ЮАР;|Географические|
|южная часть Соединенного Королевства|Географические|
|западная часть Соединенного Королевства|Географические|
|западная часть США|Географические|
|Восточная часть США|Географические|
|Центральная часть США|Географические|
|Восточная Азия|Географические|
|Западная Европа|Географические|
|центрально-западная часть США|Географические|
|Западная часть США 2|Географические|
|восточная часть США 2|Географические|

В регионах с географическим размещением данных служба "Реестр SQL" сохраняет резервные копии данных в учетной записи с геоизбыточным хранилищем (GRS).  В регионах с размещением данных в одном регионе служба "Реестр SQL" сохраняет резервные копии данных в учетной записи с хранилищем, избыточным между зонами (ZRS). Дополнительные сведения см. на странице [Центра доверия](https://azuredatacentermap.azurewebsites.net/).

## <a name="configure-regional-redundancy"></a>Настройка региональной избыточности 

Клиенты, которым требуется региональная избыточность для **реестра SQL Server**, могут создавать регистрационные данные в двух разных регионах. После этого клиенты могут загружать обновления для системы безопасности из любого региона, основываясь на доступности службы **реестра SQL Server**. 

Для региональной избыточности службу **реестра SQL Server**  нужно создать в двух разных регионах, а инвентаризация SQL Server должна быть разделена между этими двумя службами. Таким образом, половина серверов SQL Server регистрируется в службе реестра в одном регионе, а вторая половина — в службе реестра в другом регионе. 

Чтобы настроить региональную избыточность, выполните следующие действия.

1. Разделите инвентаризацию SQL Server 2008 или 2008 R2 на два файла, например upload1.csv и upload2.csv. 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="Пример отправки файлов":::

1. Создайте первую службу **реестра SQL Server** в одном регионе, а затем выполните в нем пакетную регистрацию одного из CSV-файлов. Например, создайте первую службу **реестра SQL Server** в регионе **Западная часть США**, а затем выполните пакетную регистрацию серверов SQL Server с помощью файла upload1.csv. 
1. Создайте вторую службу **реестра SQL Server** во втором регионе, а затем выполните в нем пакетную регистрацию одного из CSV-файлов. Например, создайте вторую службу **реестра SQL Server** в регионе **Восточная часть США**, а затем выполните пакетную регистрацию серверов SQL Server с помощью файла upload2.csv. 


После регистрации данных в двух разных ресурсах **реестра SQL Server** вы сможете загружать обновления безопасности из любого региона, основываясь на доступности служб. 


## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

Часто задаваемые вопросы о дополнительных обновлениях системы безопасности можно найти на странице [Вопросы и ответы о дополнительных обновлениях безопасности](https://www.microsoft.com/cloud-platform/extended-security-updates). Часто задаваемые вопросы по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перечислены ниже. 

**Когда заканчивается поддержка SQL Server 2008 и 2008 R2?**

Поддержка [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] заканчивается 9 июля 2019 г. 

**Что означает окончание поддержки?**

Политика жизненных циклов Microsoft обеспечивает 10 лет поддержки (5 лет основной поддержки и 5 лет расширенной поддержки) для бизнес-продуктов и продуктов для разработчиков (например, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и Windows Server). В соответствии с этой политикой по окончании периода расширенной поддержки исправления или обновления безопасности выпускаться не будут, что может привести к проблемам с безопасностью и соответствием нормативным требованиям, а также подвергнуть приложения клиентов и весь бизнес серьезным системным рискам.

**Какие выпуски SQL Server могут получать дополнительные обновления безопасности?**

Выпуски Enterprise, Datacenter, Standard, Web и Workgroup [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]для версий x86 и x64. 

**Когда будет доступно предложение по дополнительным обновлениям безопасности?**

Дополнительные обновления безопасности теперь доступны для приобретения, и их можно заказать у [!INCLUDE[msCoName](../../includes/msconame-md.md)] или партнера [!INCLUDE[msCoName](../../includes/msconame-md.md)] по лицензированию. Доставка дополнительных обновлений безопасности (если и когда они будут доступны) начнется сразу же после завершения поддержки. Клиенты, заинтересованные в переходе на Azure, могут сделать это немедленно. 

**Что в себя включают дополнительные обновления безопасности?** 

Дополнительные обновления системы безопасности включают в себя предоставление обновлений безопасности и бюллетеней с пометкой **критические** [Центра Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/), на период до трех лет после 9 июля 2019 г. Дополнительные обновления безопасности будут распространяться в случае и по мере доступности. Дополнительные обновления безопасности не включают в себя техническую поддержку, однако вы можете использовать другие планы поддержки [!INCLUDE[msCoName](../../includes/msconame-md.md)], чтобы получить помощь по вопросам, касающимся рабочих нагрузок в [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], на которые будут распространяться дополнительные обновления безопасности. Дополнительные обновления безопасности не содержат новые возможности, функциональные улучшения, запрошенные клиентом исправления. Однако [!INCLUDE[msCoName](../../includes/msconame-md.md)] может по своему усмотрению включать в обновления исправления, не связанные с безопасностью.

**Почему дополнительные обновления безопасности для SQL Server 2008 и 2008 R2 содержат только критические обновления?**

Раньше после прекращения поддержки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставлялись только критические обновления системы безопасности, что соответствовало нормативным требованиям наших корпоративных клиентов. Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выпускается общее ежемесячное обновление системы безопасности. [!INCLUDE[msCoName](../../includes/msconame-md.md)] предоставляет только обновления системы безопасности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по требованию (GDR) в соответствии с бюллетенями MSRC, в которых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] указан как затронутый продукт.
Если возникнут ситуации, когда важные обновления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не предоставляются, клиент считает их критическими, а MSRC — нет, мы вместе с клиентом в индивидуальном порядке определим лучшие методы устранения рисков.

**По каким лицензионным программам можно получать дополнительные обновления безопасности?**

Клиенты, использующие Software Assurance, могут приобрести дополнительные обновления безопасности локально в рамках соглашения Enterprise (EA), Enterprise Subscription (EAS), Server & Cloud Enrollment (SCE) или Enrollment for Education Solutions (EES). Программа Software Assurance не должна использоваться обязательно в рамках того же соглашения о регистрации.

**Должны ли клиенты SQL Server использовать самые последние пакеты обновлений, чтобы применить дополнительные обновления безопасности?**

Да, клиенты должны использовать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или Windows Server 2008 и 2008 R2 с последним пакетом обновлений, чтобы применить дополнительные обновления безопасности. [!INCLUDE[msCoName](../../includes/msconame-md.md)] будет создавать только такие обновления, которые могут быть применены к последнему пакету обновления.

**Каковы варианты для клиентов SQL Server, которые не принимают участия в программе Software Assurance?** 

Для клиентов, которые не участвуют в программе Software Assurance, альтернативным вариантом получения доступа к дополнительным обновлениям безопасности является миграция в Azure. Для переменных рабочих нагрузок рекомендуется выполнить миграцию клиентов в Azure с оплатой по мере использования, что позволяет в любой момент масштабировать нагрузку. Для прогнозируемых рабочих нагрузок рекомендуется выполнять миграцию клиентов в Azure с помощью серверной подписки и зарезервированных экземпляров.
  
**Применимо ли это предложение также и к SQL Server 2005?**

Нет. Эти старые версии рекомендуется обновить до последних версий, но клиенты могут обновить их до версий [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] или [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], чтобы воспользоваться этим предложением.

**Можно ли развернуть новый экземпляр SQL Server 2008 или 2008 R2 в Azure и по-прежнему получать дополнительные обновления системы безопасности?**

Да, клиенты могут развернуть новый экземпляр [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] или [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] в виртуальной машине Azure и получить доступ к дополнительным обновлениям безопасности.

**Можно ли получить техническую поддержку локально для SQL Server 2008 или 2008 R2 по истечении срока службы поддержки без приобретения дополнительных обновлений системы безопасности?**

Нет. Если клиент использует [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] или [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] и предпочитает остаться в локальной среде во время миграции без дополнительных обновлений безопасности, он не может зарегистрировать запрос в службу поддержки, даже если у него есть план поддержки. Однако если клиент выполнит миграцию в Azure, он сможет получить поддержку с помощью своего плана поддержки Azure.

**Если клиент для SQL Server 2008 и 2008 R2 предпочитает использовать собственную лицензию (BYOL), он должен участвовать в программе Software Assurance?**

Да, клиенты должны участвовать в программе Software Assurance, чтобы воспользоваться преимуществами программы BYOL для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на виртуальных машинах Azure в составе программы "Перемещение лицензий". Клиентам, не принимающим участия в программе Software Assurance, мы рекомендуем перейти на Управляемый экземпляр SQL Azure для своих сред [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]. Клиенты также могут перейти на виртуальные машины Azure с оплатой по мере использования. Участвующие в программе Software Assurance клиенты, которые лицензируют SQL на каждое ядро, также могут переходить в Azure с помощью программы "Преимущество гибридного использования Azure".

Управляемый экземпляр SQL Azure — это служба Azure, обеспечивающая практически полную совместимость с локальными [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Управляемый экземпляр предоставляет встроенные возможности обеспечения высокой доступности и аварийного восстановления, а также интеллектуальные функции управления производительностью и возможность масштабирования на лету. Управляемый экземпляр также предоставляет возможности, не зависящие от версии и не требующие обновления и исправления системы безопасности вручную. Дополнительные сведения о программе BYOL см. на странице с рекомендациями по ценам Azure.

**Что необходимо предпринять клиентам для запуска SQL Server в Azure?**

Клиенты могут перенести устаревшие среды [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Управляемый экземпляр SQL Azure, полностью управляемую службу платформы данных (PaaS), которая предоставляет бесплатную версию для устранения проблем с окончанием сроков поддержки или на виртуальных машинах Azure для получения доступа к обновлениям системы безопасности. Перенесенные базы данных сохраняют совместимость с устаревшей системой. Дополнительные сведения см. в статье [Сертификация на совместимость](../../database-engine/install-windows/compatibility-certification.md).

Дополнительные обновления системы безопасности будут доступны для [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] на виртуальных машинах Azure по истечении срока поддержки 9 июля 2019 г. в течение следующих трех лет. Для клиентов, желающих обновить [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], будут поддерживаться все последующие версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для версий с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] клиентам необходимо иметь последний поддерживаемый пакет обновления. Начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] клиентам рекомендуется использовать последнее накопительное обновление. Обратите внимание, что пакеты обновления не будут доступны начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], доступны будут только накопительные обновления и выпуски для общего распространения (GDR).

Управляемый экземпляр SQL Azure — это вариант развертывания на уровне экземпляра в [!INCLUDE[ssSDS](../../includes/sssds-md.md)], который обеспечивает широкую совместимость [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и встроенную поддержку виртуальных сетей (VNET), поэтому вы можете перенести базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Управляемый экземпляр без изменения приложений. Он сочетает в себе многофункциональную контактную зону [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с эксплуатационными и финансовыми преимуществами интеллектуальной, полностью управляемой службы. Используйте новую службу Azure Database Migration Service для перемещения [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] в Управляемый экземпляр SQL Azure с минимальными изменениями кода приложения или вообще без них.

**Могут ли клиенты использовать Преимущество гибридного использования Azure для версий SQL Server 2008 и 2008 R2?**

Да, клиенты с активной лицензией Software Assurance или эквивалентными серверными подписками могут использовать Преимущество гибридного использования Azure с помощью существующих инвестиций в локальную лицензию со скидкой на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], работающую на [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и виртуальных машинах Azure.

**Могут ли клиенты получить бесплатные дополнительные обновления системы безопасности в регионах Azure для государственных организаций?**

Да, дополнительные обновления системы безопасности будут доступны на виртуальных машинах Azure в регионах Azure для государственных организаций.

**Могут ли клиенты получить бесплатные обновления системы безопасности Azure Stack?**

Да, клиенты могут перенести [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Windows Server 2008 и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] на Azure Stack и получить дополнительные обновления системы безопасности без дополнительных затрат по истечении сроков поддержки.

**Каковы рекомендации для клиентов с кластером SQL 2008 и 2008 R2, которые используют общее хранилище, по переносу в Azure?**

В настоящее время Azure не поддерживает кластеризацию общих хранилищ. Дополнительные сведения о том, как настроить высокодоступный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Azure, см. в [руководстве по настройке высокой доступности в SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr).

**Могут ли клиенты использовать дополнительные обновления системы безопасности для SQL Server со сторонними поставщиками услуг размещения?**

Клиенты не могут использовать дополнительные обновления системы безопасности, если они перемещают среду [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] в реализацию PaaS других поставщиков облачных служб. Если клиенты стремятся перейти на виртуальные машины (IaaS), они могут использовать Перемещение лицензий для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью программы Software Assurance для переноса и приобретения дополнительных обновлений системы безопасности с [!INCLUDE[msCoName](../../includes/msconame-md.md)], чтобы вручную применить исправления на экземплярах [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)], работающих в ВМ (IaaS) на сервере уполномоченного поставщика услуг размещения SPLA. Однако бесплатные обновления в Azure — это более привлекательное предложение.

**Каковы рекомендации по повышению производительности SQL Server на виртуальных машинах Azure?** 

Дополнительные сведения о том, как оптимизировать производительность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на виртуальных машинах Azure, см. в [руководстве по оптимизации SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance). 

## <a name="see-also"></a>См. также раздел

- [Страница жизненного цикла SQL Server 2008 / 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [Страница поддержки SQL Server 2008 / 2008 R2](https://aka.ms/sqleos)
- [Часто задаваемые вопросы о дополнительных обновлениях системы безопасности](https://aka.ms/sqleosfaq)
- [Центр Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [Управление обновлениями Windows при помощи службы автоматизации Azure](/azure/automation/update-management/overview)
- [Автоматическая установка исправлений для виртуальной машины SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Руководство по миграции данных Майкрософт](https://datamigration.microsoft.com/)
- [Миграция в Azure: параметры точности и сдвига для перемещения текущего экземпляра SQL Server 2008 / 2008 R2 в виртуальную машину Azure](https://azure.microsoft.com/services/azure-migrate/)
- [Инфраструктура внедрения в облако для переноса SQL](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [Сценарии, связанные с ESU, на GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)

