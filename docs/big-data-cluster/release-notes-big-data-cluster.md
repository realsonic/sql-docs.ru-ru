---
title: Заметки о выпуске кластеров больших данных SQL Server
titleSuffix: SQL Server big data clusters
description: В этой статье описаны последние обновления и известные проблемы для Кластеров больших данных SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f328e136bc0b07f23083153277ac98588702b373
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489664"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>Заметки о выпуске Кластеров больших данных SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Следующие заметки о выпуске применимы к [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Эта статья разбита на разделы для каждого выпуска. Каждый выпуск содержит ссылку на статью поддержки, где описаны изменения накопительного пакета обновления, а также ссылки на скачиваемые файлы пакета Linux. В этой статье также перечислены [известные проблемы](#known-issues) для последних выпусков [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC).

## <a name="supported-platforms"></a>Поддерживаемые платформы

В этом разделе описаны платформы, поддерживаемые BDC.

### <a name="kubernetes-platforms"></a>Платформы Kubernetes

|Платформа|Поддерживаемые версии|
|---------|---------|
|Vanilla (вышестоящий) Kubernetes|Развертывание BDC в локальной среде с помощью кластера Kubernetes минимальной версии 1.13. См. раздел [Версия Kubernetes и версия политики поддержки отклонения версий](https://kubernetes.io/docs/setup/release/version-skew-policy/).|
|Red Hat OpenShift.|Развертывание BDC в локальной среде с помощью кластера OpenShift минимальной версии 4.3. См. раздел [Политика жизненного цикла платформы контейнеров Red Hat OpenShift](https://access.redhat.com/support/policy/updates/openshift).<br><br> Поддержка представлена в SQL Server 2019 CU5.|
|Служба Azure Kubernetes (AKS)|Развертывание BDC в кластере AKS минимальной версии 1.13.<br/>См. информацию о политике поддержки версий в разделе [Поддерживаемые версии Kubernetes в AKS](/azure/aks/supported-kubernetes-versions).|
|Azure Red Hat OpenShift (ARO)|Развертывание BDC в ARO минимальной версии 4.3. См. статью [Azure Red Hat OpenShift](/azure/openshift/). <br><br> Поддержка представлена в SQL Server 2019 CU5.|

### <a name="host-os-for-kubernetes"></a>Операционная система узла для Kubernetes

|Платформа|ОС узла|Поддерживаемые версии|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|7.3, 7.4, 7.5, 7.6|
|OpenShift|Red Hat Enterprise Linux/CoreOS |См. [заметки о выпуске OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release).|

### <a name="sql-server-editions"></a>Выпуски SQL Server

|Выпуск|Примечания|
|---------|---------|
|Enterprise<br/>Standard<br/>Разработчик| Выпуск кластера больших данных определяется выпуском основного экземпляра SQL Server. Во время развертывания выпуск Developer развертывается по умолчанию. После развертывания выпуск можно изменить. См. [Настройка основного экземпляра SQL Server](./configure-sql-server-master-instance.md). |

## <a name="tools"></a>Инструменты

|Платформа|Поддерживаемые версии|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|Рекомендуется использовать последнюю доступную версию. Начиная с выпуска SQL Server 2019 CU5, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] имеет независимую семантическую версию от сервера. <br/><br/>Запустите `azdata –-version`, чтобы проверить версию.<br/><br/>Последнюю версию см. в разделе [История выпусков](#release-history).|
|Azure Data Studio|Получите последнюю сборку [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).|

Полный список см. в разделе [Какие средства требуются?](deploy-big-data-tools.md#which-tools-are-required).

## <a name="release-history"></a>История выпусков

В следующей таблице указана история выпусков для [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

| Выпуск <sup>1</sup> | Версия BDC    | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] Версия <sup>2</sup>| Дата выпуска |
|------------------|----------------|-----------------|--------------|
| [CU8](#cu8)      | 15.0.4073.23   | 20.2.2          | 19.10.2020   |
| [CU6](#cu6)      | 15.0.4053.23   | 20.0.1          | 04.08.2020   |
| [CU5](#cu5)      | 15.0.4043.16   | 20.0.0          | 22.06.2020   |
| [CU4](#cu4)      | 15.0.4033.1    | 15.0.4033       | 2020-03-31   |
| [CU3](#cu3)      | 15.0.4023.6    | 15.0.4023       | 2020-03-12   |
| [CU2](#cu2)      | 15.0.4013.40   | 15.0.4013       | 2020-02-13   |
| [CU1](#cu1)      | 15.0.4003.23   | 15.0.4003       | 2020-01-07   |
| [GDR1](#rtm)     | 15.0.2070.34   | 15.0.2070       | 2019-11-04   |

<sup>1</sup> CU7 недоступен для BDC.

<sup>2</sup> Версия [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] отражает версию средства во время выпуска CU. Выпуски `azdata` и сервера могу не совпадать, поэтому при установке последних пакетов вы можете получать новые версии. Более новые версии совместимы с ранее выпущенными CU.

## <a name="how-to-install-updates"></a>Установка обновлений

Сведения об установке обновлений см. в статье [Обновление [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (сентябрь 2020 г.)

Выпуск накопительного пакета обновления 8 (CU8) для версии SQL Server 2019.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

Этот выпуск включает несколько исправлений и некоторые дополнительные возможности.

### <a name="added-capabilities"></a>Добавленные возможности

- [Шифрование неактивных данных Кластеров больших данных SQL Server](encryption-at-rest-concepts-and-configuration.md) с помощью управляемых системой ключей и сертификатов.
   > [!CAUTION]
   > Это первоначальный выпуск шифрования неактивных данных кластеров больших данных SQL Server. Ознакомьтесь со следующими статьями: 
   > - [Основные понятия безопасности для [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](concept-security.md)
   > - [Основные понятия шифрования неактивных данных и рекомендации по настройке](encryption-at-rest-concepts-and-configuration.md)
- [Пользователь прокси-сервера Oracle](tutorial-query-oracle.md) поддерживает сценарий виртуализации данных.

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (июль 2020 г.)

Выпуск накопительного пакета обновления 6 (CU6) для SQL Server 2019.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

Этот выпуск содержит небольшие исправления и улучшения. В следующих статьях содержатся сведения, относящиеся к таким обновлениям:

- [Управление доступом к кластеру больших данных в режиме AD DS](manage-user-access.md)
- [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в режиме Active Directory](active-directory-deploy.md)
- [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в AKS в режиме Active Directory](active-directory-deployment-aks.md)
- [Развертывание кластеров больших данных в частном кластере службы Azure Kubernetes (AKS)](private-deploy.md)
- [Ограничения для исходящего трафика Кластеров больших данных (BDC) в частном кластере Службы Azure Kubernetes (AKS)](private-restrict-egress-traffic.md)
- [Развертывание кластера больших данных SQL Server с высокой доступностью](deployment-high-availability.md)
- [Настройка кластера больших данных SQL Server](configure-cluster.md)
- [Настройка Apache Spark и Apache Hadoop в Кластерах больших данных](configure-spark-hdfs.md)
- [Свойства конфигурации главного экземпляра SQL Server](reference-config-master-instance.md)
- [Свойства конфигурации Apache Spark и Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Модель Kubernetes RBAC и ее влияние на пользователей и учетные записи служб, управляющие BDC](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (июнь 2020)

Выпуск накопительного пакета обновления 5 (CU5) для SQL Server 2019.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>Добавленные возможности

- Поддержка развертывания кластеров больших данных в Red Hat OpenShift. Поддержка включает платформу контейнеров OpenShift, развернутую в локальной версии 4.3 и более поздней, и Azure Red Hat OpenShift. См. статью [Развертывание кластеров больших данных SQL Server в OpenShift](deploy-openshift.md)
- Обновлена модель безопасности развертывания BDC. Теперь привилегированные контейнеры, развернутые в составе BDC, больше не являются *обязательным условием*. Помимо непривилегированных, контейнеры работают как пользователи, не являющиеся корневыми по умолчанию для всех новых развертываний, использующих SQL Server 2019 CU5. 
- Добавлена поддержка развертывания нескольких кластеров больших данных в домене Active Directory.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] имеет собственную семантическую версию, не зависящую от сервера. Все зависимости между клиентом и версией сервера azdata удалены. Чтобы воспользоваться преимуществами последних улучшений и исправлений, рекомендуется использовать последнюю версию как для клиента, так и для сервера.
- Появились две новые хранимые процедуры, sp_data_source_objects и sp_data_source_table_columns, для поддержки самоанализа определенных внешних источников данных. Они могут использоваться клиентами непосредственно через T-SQL для обнаружения схемы, а также для просмотра таблиц, доступных для виртуализации. Мы будем использовать эти изменения в мастере внешних таблиц [расширения виртуализации данных](../azure-data-studio/extensions/data-virtualization-extension.md) для Azure Data Studio, что позволяет создавать внешние таблицы из SQL Server, Oracle, MongoDB и Teradata.
- Добавлена поддержка сохранения настроек, выполненных в Grafana. До выпуска CU5 клиенты сообщали, что изменения в конфигурациях Grafana не сохранялись при перезапуске модуля pod `metricsui` (на котором размещается панель мониторинга Grafana). Эта проблема исправлена, и все конфигурации теперь сохраняются. 
- Исправлена проблема безопасности, связанная с API, который использовался для сбора метрик модуля pod и узла с помощью Telegraf (размещенных в модулях pod `metricsdc`). В результате этого изменения для Telegraf теперь необходима учетная запись службы, роль кластера и привязки кластера с требуемыми разрешениями на сбор метрик модулей pod и узла. Дополнительные сведения см. в разделе [Роль кластера, необходимая для сбора метрик модулей pod и узлов](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection).
- Добавлены два параметра для управления сбором метрик модулей pod и узлов. Если вы используете различные решения для мониторинга инфраструктуры Kubernetes, вы можете отключить встроенный сбор метрик для модулей pod и узлов, присвоив параметрам *allowNodeMetricsCollection* и *allowPodMetricsCollection* значения false в файле конфигурации развертывания control.json. Для сред OpenShift для этих параметров по умолчанию задано значение false во встроенных профилях развертывания, так как сбор метрик модулей pod и узлов требует привилегированных возможностей.

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (апрель 2020 г.)

Выпуск накопительного пакета обновления 4 (CU4) для SQL Server 2019. Версия ядра СУБД SQL Server в этом выпуске — 15.0.4033.1.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (март 2020 г.)

Выпуск накопительного пакета обновления 3 (CU3) для SQL Server 2019. Версия ядра СУБД SQL Server в этом выпуске — 15.0.4023.6.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>устраненные проблемы.

SQL Server 2019 CU3 устраняет следующие проблемы, возникшие в предыдущих выпусках.

- [Развертывание в частном репозитории](#deployment-with-private-repository)
- [Сбой обновления из-за истечения времени ожидания](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> Накопительный пакет обновления 2 (CU2) (февраль 2020 г.)

Выпуск накопительного пакета обновления 2 (CU2) для SQL Server 2019. Версия ядра СУБД SQL Server в этом выпуске — 15.0.4013.40.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (январь 2020 г.)

Выпуск накопительного пакета обновления 1 (CU1) для SQL Server 2019. Версия ядра СУБД SQL Server в этом выпуске — 15.0.4003.23.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> GDR1 (ноябрь 2019 г.)

Выпуск SQL Server 2019 для общего распространения 1 (GDR1) — общедоступная версия [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)]. Версия ядра СУБД SQL Server в этом выпуске — 15.0.2070.34.

|Версия пакета | Тег образа |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>Известные проблемы

### <a name="msdtc-capabilities-can-not-be-enabled-for-sql-server-master-instance-running-within-bdc"></a>Не удается включить возможности MSDTC для главного экземпляра SQL Server, работающего в BDC

- **Затронутые выпуски**: Все конфигурации развертывания кластера больших данных, независимо от выпуска.

- **Проблема и последствия для клиентов**: Если SQL Server развернут в BDC как главный экземпляр SQL Server, функцию MSDTC включить нельзя. Для этой проблемы не существует обходного решения.

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>Смена шифратора ключа шифрования базы данных SQL Server с высоким уровнем доступности

- **Затронутые выпуски**: Все развертывания кластера больших данных высокого уровня доступности независимы от их выпуска.

- **Проблема и последствия для клиентов**: При развертывании SQL Server с высоким уровнем доступности происходит сбой смены сертификата для зашифрованной базы данных. Если следующая команда выполняется в главном пуле, появится сообщение об ошибке:
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    Воздействия нет, команда не работает, а шифрование целевой базы данных сохраняется с помощью предыдущего сертификата.
    
### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>Пустые задания Livy перед применением накопительных обновлений

- **Затронутые выпуски**: Все версии до CU6. Разрешено для CU8.

- **Проблема и последствия для клиентов**: Во время обновления `sparkhead` возвращает ошибку 404.

- **Возможное решение**: Перед обновлением BDC убедитесь, что отсутствуют активные сеансы Livy или пакетные задания. Чтобы избежать этого, следуйте инструкциям в разделе [Обновление поддерживаемого выпуска](deployment-upgrade.md#upgrade-from-supported-release). 

   Если Livy возвращает ошибку 404 во время процесса обновления, перезапустите сервер Livy на обоих узлах `sparkhead`. Пример:

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>Истечение срока действия паролей учетных записей службы созданного кластера больших данных

- **Затронутые выпуски**: Все развертывания кластера больших данных с интеграцией Active Directory, независимо от выпуска

- **Проблема и последствия для клиентов**: Во время развертывания кластера больших данных рабочий процесс создает набор [учетных записей служб](active-directory-objects.md). В зависимости от политики истечения срока действия паролей, заданной на контроллере домена, срок действия паролей для этих учетных записей истекает (по умолчанию — 42 дня). В настоящее время не существует механизма для смены учетных данных для всех учетных записей в BDC, поэтому кластер станет неработоспособным по истечении этого срока действия.

- **Возможное решение**: Обновите политику срока действия учетных записей служб BDC, чтобы на контроллере домена не был задан срок действия пароля. Полный список этих учетных записей см. в разделе [Автоматически созданные объекты Active Directory](active-directory-objects.md). Это действие можно выполнить до или после истечения срока действия. В последнем случае Active Directory повторно активирует пароли с истекшим сроком действия.

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>Учетные данные для доступа к службам через конечную точку шлюза

- **Затронутые выпуски**: Развернуты новые кластеры, начиная с CU5.

- **Проблема и последствия для клиентов**: Для новых кластеров больших данных, развернутых с помощью SQL Server 2019 CU5, имя пользователя шлюза не является **root**. Если приложение, применяемое для подключения к конечной точке шлюза, использует неверные учетные данные, выводится ошибка проверки подлинности. Это изменение является результатом запуска приложений в кластере больших данных в качестве непривилегированного пользователя (новое поведение по умолчанию, начиная с SQL Server 2019 CU5), при развертывании нового кластера больших данных с помощью CU5 имя пользователя для конечной точки шлюза основывается на значении, переданном через переменную среды **AZDATA_USERNAME**. Это то же имя пользователя, которое используется для контроллера и конечных точек SQL Server. Это изменение влияет только на новые развертывания. Существующие кластеры больших данных предыдущих выпусков будут по-прежнему использовать **root**. При развертывании кластера, использующего проверку подлинности на основе Active Directory, учетные данные остаются прежними. 

- **Возможное решение**: Azure Data Studio автоматически обрабатывает изменение учетных данных для подключения через шлюз, чтобы обеспечить возможность просмотра системы HDFS в обозревателе объектов. Необходимо установить [последнюю версию Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), которая включает в себя необходимые изменения, устраняющие этот вариант использования.
Для других сценариев, в которых необходимо предоставлять учетные данные для доступа к службе через шлюз (например, для входа с помощью [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] и доступа к веб-панелям мониторинга Spark), необходимо использовать правильные учетные данные. В существующем кластере, развернутом до выпуска CU5, вы продолжите использовать имя пользователя **root** для подключения к шлюзу даже после обновления кластера до CU5. При развертывании нового кластера с использованием сборки CU5 для входа необходимо предоставить имя пользователя, соответствующее переменной среды **AZDATA_USERNAME**.

### <a name="pods-and-nodes-metrics-not-being-collected"></a>Метрики для модулей pod и узлов не собираются

- **Затронутые выпуски**: Новые и существующие кластеры, использующие образы CU5

- **Проблема и последствия для клиентов**: В результате исправления безопасности, связанного с API, который `telegraf` использовал для сбора метрик модулей pod и узлов, клиенты могут заметить, что метрики не собираются. Это возможно как в новых, так и в существующих развертываниях BDC (после обновления до CU5). В результате этого исправления для Telegraf теперь требуется учетная запись службы с разрешениями роли в масштабе кластера. Развертывание пытается создать необходимую учетную запись службы и роль кластера, но, если пользователь, развертывающий кластер или выполняющий обновление, не имеет достаточных разрешений, развертывание или обновление продолжится с предупреждением и завершится успешно, но метрики модуля pod и узла не будут собираться.

- **Возможное решение**: Можно попросить администратора создать роль и учетную запись службы (до или после развертывания или обновления) и использовать их в BDC. [В этой статье](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) описано, как создать необходимые артефакты.

### <a name="azdata-bdc-copy-logs-command-failure"></a>Сбой команды `azdata bdc copy-logs`

- **Затронутые выпуски**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] версия *20.0.0*

- **Проблема и последствия для клиентов**: Для выполнения команды *copy-logs* предполагается, что на клиентском компьютере, с которого была запущена команда, установлено клиентское средство `kubectl`. Если команда выполняется в кластере BDC, установленном в OpenShift, на клиенте, где установлено только средство `oc`, возникнет ошибка: *Произошла ошибка при сборе журналов: [WinError 2]. Системе не удается найти указанный файл*.

- **Возможное решение**: Установите средство `kubectl` на тот же клиентский компьютер и повторно выполните команду `azdata bdc copy-logs`. Инструкции по установке `kubectl` см. [здесь](deploy-big-data-tools.md).

### <a name="deployment-with-private-repository"></a>Развертывание в частном репозитории

- **Затронутые выпуски**: GDR1, CU1, CU2. Устранено для CU3.

- **Проблема и последствия для клиентов**: К обновлению из частного репозитория предъявляются особые требования.

- **Возможное решение**: Если вы используете частный репозиторий, чтобы предварительно извлечь образы для развертывания или обновления BDC, убедитесь, что текущие образы сборки, а также образы целевой сборки находятся в частном репозитории. Так вы сможете при необходимости успешно выполнить откат. Кроме того, если вы изменили учетные данные частного репозитория с момента первоначального развертывания, обновите соответствующий секрет в Kubernetes, прежде чем выполнять обновление. [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] не поддерживает обновление учетных данных с помощью переменных среды `AZDATA_PASSWORD` и `AZDATA_USERNAME`. Создайте секрет с помощью [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret). 

Обновление с использованием других репозиториев не поддерживается для текущих и целевых сборок.

### <a name="upgrade-may-fail-due-to-timeout"></a>Сбой обновления из-за истечения времени ожидания

- **Затронутые выпуски**: GDR1, CU1, CU2. Устранено для CU3.

- **Проблема и последствия для клиентов**: Обновление может завершиться ошибкой из-за истечения времени ожидания.

   В приведенном ниже коде показано, как может выглядеть ошибка:

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   Эта ошибка чаще всего возникает при обновлении BDC в Службе Kubernetes Azure (AKS).

- **Возможное решение**: Увеличьте время ожидания для обновления. 

   Чтобы увеличить время ожидания для обновления, измените схему конфигурации обновления. Чтобы изменить схему конфигурации обновления, выполните следующие действия:

   1. Выполните следующую команду:

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. Измените следующие поля:

       **`controllerUpgradeTimeoutInMinutes`**  — указывает количество минут ожидания завершения обновления базы данных контроллера или контроллера. Значение по умолчанию — 5. Используйте значение не ниже 20.

       **`totalUpgradeTimeoutInMinutes`** : определяет совокупный интервал времени, в течение которого контроллер и база данных контроллера должны завершить обновление (обновление `controller` + `controllerdb`). Значение по умолчанию — 10. Используйте значение не ниже 40.

       **`componentUpgradeTimeoutInMinutes`** : Определяет время, необходимое для выполнения каждого следующего этапа обновления. Значение по умолчанию — 30. Замените на 45.

   3. Сохраните и закройте.

   Ниже приведен скрипт Python для установки времени ожидания.

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>Сбой отправки задания Livy из Azure Data Studio (ADS) или выполнения команды curl с ошибкой 500

- **Проблема и последствия для клиентов**: В конфигурации высокой доступности общие ресурсы Spark `sparkhead` настраиваются с несколькими репликами. В этом случае могут возникать сбои при отправке задания Livy из Azure Data Studio (ADS) или `curl`. Для проверки выполните команду `curl` в отношении любых результатов `sparkhead` pod в отклоненном подключении. Так, `curl https://sparkhead-0:8998/` или `curl https://sparkhead-1:8998` возвращает ошибку 500.

   Это происходит в следующих случаях:

   - Группы pod Zookeeper или процессы для каждого экземпляра Zookeeper несколько раз перезапускаются.
   - Между pod `sparkhead` и группами pod Zookeeper нестабильное сетевое подключение.

- **Возможное решение**: Перезапуск обоих серверов Livy.

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>Создание оптимизированной для обработки в памяти таблицы, когда основной экземпляр находится в группе доступности

- **Проблема и последствия для клиентов**: Невозможно использовать основную конечную точку, предоставленную для подключения к базам данных группы доступности (прослушивателю), для создания оптимизированных для обработки в памяти таблиц.

- **Возможное решение**: Чтобы создать оптимизированные для обработки в памяти таблицы, когда основной экземпляр SQL Server находится в конфигурации группы доступности, [подключитесь к экземпляру SQL Server](deployment-high-availability.md#instance-connect), предоставьте конечную точку, подключитесь к базе данных SQL Server и создайте оптимизированные для обработки в памяти таблицы в сеансе, созданном с новым подключением.

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>Укажите для внешних таблиц режим проверки подлинности с Active Directory

- **Проблема и последствия для клиентов**: Если основной экземпляр SQL Server находится в режиме проверки подлинности с Active Directory, запрос, который выбирает элемент исключительно из внешних таблиц, где хотя бы одна из внешних таблиц находится в пуле носителей, и вставляет режим проверки подлинности в другую внешнюю таблицу, возвращает следующее:

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **Возможное решение**: Измените запрос одним из следующих способов. Присоедините таблицу пулов носителей к локальной таблице или сначала вставьте ее в локальную таблицу, а затем выполните чтение из локальной таблицы, чтобы вставить элемент в пул данных.

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>Функции прозрачного шифрования данных нельзя использовать с базами данных, которые являются частью группы доступности в главном экземпляре SQL Server.

- **Проблема и последствия для клиентов**: В конфигурации высокой доступности базы данных с включенным шифрованием нельзя использовать после отработки отказа, так как главный ключ, используемый для шифрования, отличается для каждой реплики. 

- **Возможное решение**: Решение для этой проблемы отсутствует. Рекомендуется не включать шифрование в этой конфигурации, пока не выйдет исправление.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).
