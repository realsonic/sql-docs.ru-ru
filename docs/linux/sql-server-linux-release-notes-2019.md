---
title: Заметки о выпуске для SQL Server 2019 в Linux
description: Эта статья содержит заметки о выпуске и поддерживаемые функции для SQL Server 2019 в Linux. Приведены заметки о выпуске для последнего и нескольких предыдущих выпусков.
author: VanMSFT
ms.author: vanto
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 262e2f222e6c30f3657e78c2a20862912b1aa970
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643960"
---
# <a name="release-notes-for-sql-server-2019-on-linux"></a>Заметки о выпуске для SQL Server 2019 в Linux

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md)]

Следующие заметки о выпуске применимы к версии SQL Server 2019, работающей в Linux. Эта статья разбита на разделы для каждого выпуска. Каждый выпуск содержит ссылку на статью поддержки, где описаны изменения накопительного пакета обновления, а также ссылки на скачиваемые файлы пакета Linux.

> [!TIP]
> Сведения о новых возможностях Linux в SQL Server 2019 см. в статье [Новые возможности SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true#sql-server-on-linux).

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

## <a name="tools"></a>Инструменты

Большинство существующих клиентских средств, предназначенных для SQL Server, могут без проблем работать с SQL Server, выполняющимся в Linux. Некоторые средства для нормальной работы с Linux могут иметь определенные требования к версиям. Полный список средств SQL Server см. в статье [Средства и служебные программы SQL для SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>История выпусков

В следующей таблице указана история выпусков версий SQL Server 2019.

| Release                   | Версия       | Дата выпуска |
|---------------------------|---------------|--------------|
| [CU8](#cu8)               | 15.0.4073.23  | 07.10.2020   |
| [CU7 (удален)](https://support.microsoft.com/help/4570012)     | 15.0.4063.15  | 02.09.2020   |
| [CU6](#cu6)               | 15.0.4053.23  | 04.08.2020   |
| [CU5](#cu5)               | 15.0.4043.16  | 22.06.2020   |
| [CU4](#cu4)               | 15.0.4033.1   | 2020-03-31   |
| [CU3](#cu3)               | 15.0.4023.6   | 2020-03-12   |
| [CU2](#cu2)               | 15.0.4013.40  | 2020-02-13   |
| [CU1](#cu1)               | 15.0.4003.23  | 2020-01-07   |
| [GA](#ga)                 | 15.0.2000.5   | 2019-11-04   |
| [Релиз-кандидат](#rc)  | 15.0.1900.25  | 2019-08-21   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Установка обновлений

Если вы настроили репозиторий CU (mssql-server-2019), то получите последнюю версию накопительного пакета обновления для пакетов SQL Server при выполнении новых установок. Если вам требуются образы контейнеров Docker, см. официальные образы для [Microsoft SQL Server на Linux для подсистемы Docker](https://hub.docker.com/r/microsoft/mssql-server/). Дополнительные сведения о настройке репозиториев см. в статье [Настройка репозиториев для установки и обновления SQL Server на Linux.](sql-server-linux-change-repo.md)

При обновлении существующих пакетов SQL Server выполните соответствующую команду обновления для каждого пакета, чтобы получить последний накопительный пакет обновления. Конкретные инструкции по обновлению для каждого пакета см. в следующих руководствах по установке.

- [Установка пакета SQL Server](sql-server-linux-setup.md#upgrade)
- [Установка пакета полнотекстового поиска](sql-server-linux-setup-full-text-search.md)
- [Установка служб SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Установка поддержки R и Python для служб машинного обучения SQL Server 2019 в Linux](sql-server-linux-setup-machine-learning.md)
- [Установка пакета PolyBase](../relational-databases/polybase/polybase-linux-setup.md)
- [Включение агента SQL Server](sql-server-linux-setup-sql-agent.md)

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (сентябрь 2020 г.)

Это выпуск накопительного пакета обновления 8 (CU8) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4073.23. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается **Ubuntu 18.04**. Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04. Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4073.23-4 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4073.23-4 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Пакет Debian Ubuntu 18.04 | 15.0.4073.23-4 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4073.23-4_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4073.23-4_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4073.23-4_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4073.23-4_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4073.23-4_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4073.23-4_amd64.deb)|

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (июль 2020 г.)

Это выпуск накопительного пакета обновления 6 (CU6) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4053.23. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4563110>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается **Ubuntu 18.04**. Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04. Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4053.23-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4053.23-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Пакет Debian Ubuntu 18.04 | 15.0.4053.23-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4053.23-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4053.23-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4053.23-2_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4053.23-2_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4053.23-2_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4053.23-2_amd64.deb)|

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (июнь 2020)

Это выпуск накопительного пакета обновления 5 (CU5) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4043.16. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4552255>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается **Ubuntu 18.04**. Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04. Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4043.16-4 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4043.16-4 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Пакет Debian Ubuntu 18.04 | 15.0.4043.16-4 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4043.16-4_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4043.16-4_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4043.16-4_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4043.16-4_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4043.16-4_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4043.16-4_amd64.deb)|

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (апрель 2020 г.)

Это выпуск накопительного пакета обновления 4 (CU4) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4033.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4548597>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается **Ubuntu 18.04**. Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04. Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4033.1-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4033.1-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Пакет Debian Ubuntu 18.04 | 15.0.4033.1-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4033.1-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4033.1-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4033.1-2_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4033.1-2_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4033.1-2_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4033.1-2_amd64.deb)|

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (март 2020 г.)

Это выпуск накопительного пакета обновления 3 (CU3) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4023.6. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4538853>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3), теперь поддерживается **Ubuntu 18.04**. Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04. Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4023.6-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4023.6-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Пакет Debian Ubuntu 18.04 | 15.0.4023.6-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4023.6-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4023.6-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4023.6-2_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4023.6-2_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4023.6-2_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4023.6-2_amd64.deb)|

## <a name="cu2-february-2020"></a><a id="cu2"></a> Накопительный пакет обновления 2 (CU2) (февраль 2020 г.)

Это выпуск накопительного пакета обновления 2 (CU2) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4013.40. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4536075>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4013.40-8 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4013.40-8 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Пакет Ubuntu 16.04 Debian | 15.0.4013.40-8 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4013.40-8_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4013.40-8_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4013.40-8_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4013.40-8_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4013.40-8_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4013.40-8_amd64.deb)|

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (январь 2020 г.)

Это выпуск накопительного пакета обновления 1 (CU1) для SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.4003.23. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/en-us/help/4527376>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с CU1, ссылки для установки автономного пакета в Red Hat указывают на пакеты RHEL 8. Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.4003.23-3 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Пакет SLES RPM | 15.0.4003.23-3 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Пакет Ubuntu 16.04 Debian | 15.0.4003.23-3 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4003.23-3_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4003.23-3_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4003.23-3_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4003.23-3_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4003.23-3_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4003.23-3_amd64.deb)|

## <a name="ga-november-2019"></a><a id="ga"></a> Общедоступная версия (ноябрь 2019 г.)

Это выпуск общедоступной версии (GA) SQL Server 2019 (15.x). Версия ядра СУБД SQL Server в этом выпуске — 15.0.2000.5.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.2000.5-5 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Пакет SLES RPM | 15.0.2000.5-5 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Пакет Ubuntu 16.04 Debian | 15.0.2000.5-5 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.2000.5-5_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.2000.5-5_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.2000.5-5_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.2000.5-5_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.2000.5-5_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.2000.5-5_amd64.deb)|

## <a name="release-candidate-august-2019"></a><a id="rc"></a> Релиз-кандидат (август 2019 г.)

В следующих разделах приведены сведения о расположениях пакетов и известных проблемах в релиз-кандидате. Дополнительные сведения о новых возможностях для Linux в SQL Server 2019 см. в статье [Новые возможности SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 15.0.1900.25-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Пакет SLES RPM | 15.0.1900.25-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM расширяемости](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM расширяемости Java](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Пакет Ubuntu 16.04 Debian | 15.0.1900.25-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server/mssql-server_15.0.1900.25-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.1900.25-1_amd64.deb)</br>[Пакет Debian расширяемости](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.1900.25-1_amd64.deb)</br>[Пакет Debian расширяемости Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.1900.25-1_amd64.deb)</br>[Пакет RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.1900.25-1_amd64.deb)|

## <a name="known-issues"></a>Известные проблемы

В следующих разделах описаны известные проблемы с общедоступным (GA) выпуском SQL Server 2019 (15.x) в Linux.

### <a name="general"></a>Общие сведения

- Длина имени узла, где устанавливается [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], не должна превышать 15 символов. 

    - **Решение**. Измените имя в /etc/hostname, чтобы оно содержало не более 15 символов.

- Ручной перевод системного времени назад приведет к тому, что [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] прекратит обновлять внутреннее системное время в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Решение**. Перезапустите [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Поддерживаются только установки с одним экземпляром.

    - **Решение**. Если на заданном узле требуется несколько экземпляров, рекомендуется использовать виртуальные машины или контейнеры Docker. 

- Диспетчеру конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не удается подключиться к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux.

- По умолчанию для имени входа **sa** используется английский язык.

    - **Решение**. Измените язык для имени входа **sa** с помощью инструкции **ALTER LOGIN**.

- Поставщик OLEDB записывает в журнал следующее предупреждение: `Failed to verify the Authenticode signature of 'C:\binn\msoledbsql.dll'. Signature verification of SQL Server DLLs will be skipped. Genuine copies of SQL Server are signed. Failure to verify the Authenticode signature might indicate that this is not an authentic release of SQL Server. Install a genuine copy of SQL Server or contact customer support.`

   - **Решение**. Никаких действий не требуется. Поставщик OLEDB подписан с использованием SHA256. Ядро СУБД SQL Server неправильно проверяет подписанный DLL-файл.

### <a name="databases"></a>Базы данных

- Базу данных master нельзя переместить с помощью служебной программы mssql-conf. Другие базы данных с помощью mssql-conf переместить можно.

- При восстановлении базы данных, для которой была создана резервная копия в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на базе Windows, нужно использовать предложение **WITH MOVE** в инструкции Transact-SQL.

- Некоторые алгоритмы (комплекты шифров) для протокола TLS не работают должным образом с [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux. Это приводит к сбоям подключения при попытке подключиться к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], а также к проблемам при установке соединений между репликами в группах с высокой доступностью.

   - **Решение**. Измените скрипт конфигурации **mssql.conf** для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux, чтобы отключить проблемные комплекты шифров, выполнив следующие действия.

      1. Добавьте следующий код в /var/opt/mssql/mssql.conf.

          ```
          [network]
          tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
          ```

         > [!NOTE]
         > В приведенном выше коде `!` выполняет отрицание для выражения. Это означает, что OpenSSL не будет использовать следующий комплект шифров.  

      1. Перезапустите [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с помощью следующей команды.

          ```bash
          sudo systemctl restart mssql-server
          ```

- Базы данных [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] в Windows, которые используют выполняющуюся в памяти OLTP, невозможно восстановить в SQL Server 2019 (15.x) на базе Linux. Чтобы восстановить базу данных [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], выполняющуюся в памяти OLTP, сначала обновите базы до [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SQL Server 2017, или SQL Server 2019 в Windows, прежде чем перемещать их в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на базе Linux с помощью операции резервного копирования/восстановления или подключения/отключения.

- Сейчас разрешение пользователя **ADMINISTER BULK OPERATIONS** в Linux не поддерживается.

### <a name="networking"></a>Сеть

Функции, затрагивающие исходящие подключения TCP из процесса sqlservr, такие как связанные серверы или группы доступности, могут не работать, если выполняются оба следующих условия.

1. Целевой сервер указан в виде имени узла, а не IP-адреса.

1. В ядре для экземпляра источника отключен протокол IPv6. Чтобы проверить, включена ли в ядре поддержка протокола IPv6, необходимо успешно выполнить все следующие проверки.

   - `cat /proc/cmdline` выводит на печать командную строку загрузки текущего ядра. Выходные данные не должны содержать `ipv6.disable=1`.
   - Каталог /proc/sys/net/ipv6/ должен существовать.
   - Программа на языке C, которая вызывает `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)`, должна выполняться успешно — системный вызов должен возвращать fd != -1, а не завершиться с ошибкой EAFNOSUPPORT.

Конкретная ошибка зависит от функции. Для связанных серверов это проявляется в виде ошибки времени ожидания при входе. Для групп доступности команда DDL `ALTER AVAILABILITY GROUP JOIN` на вторичной реплике завершится сбоем через 5 минут с ошибкой времени ожидания конфигурации скачивания.

Для обхода этой проблемы выполните одно из следующих действий.

1. Используйте IP-адреса вместо имен узлов, чтобы указать целевой объект подключения TCP.

1. Включите протокол IPv6 в ядре, удалив `ipv6.disable=1` из командной строки загрузки. Используемая для этого процедура зависит от дистрибутива Linux и загрузчика, такого как grub. Если вы хотите отключить протокол IPv6, это можно сделать, задав `net.ipv6.conf.all.disable_ipv6 = 1` в конфигурации `sysctl` (например, `/etc/sysctl.conf`). Это по-прежнему помешает сетевому адаптеру системы получить IPv6-адрес, но обеспечит работу функций sqlservr.

#### <a name="network-file-system-nfs"></a>Файловая система NFS
При использовании удаленных общих папок **NFS** в рабочей среде необходимо обратить внимание на следующие требования к поддержке.

- Версия NFS должна быть **4.2 или более поздняя**. Более старые версии NFS не поддерживают необходимые функции, такие как использование команды `fallocate` и создание разреженных файлов, общие для современных файловых систем.
- При подключении NFS следует указать только каталоги **/var/opt/mssql**. Другие файлы, например системные двоичные файлы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], не поддерживаются.
- При подключении удаленной общей папки клиенты NFS должны использовать параметр `nolock`.

### <a name="localization"></a>Локализация

- Если ваш языковой стандарт отличается от английского (en_us) во время установки, в сеансе или терминале bash нужно использовать кодировку UTF-8. Если использовать кодировку ASCII, появится примерно следующее сообщение об ошибке.

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Если вы не можете использовать кодировку UTF-8, запустите программу установки с помощью переменной среды MSSQL_LCID, чтобы указать требуемый язык.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- При выполнении программы установки mssql-conf и выполнении установки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на языке, отличном от английского, после локализованного текста "Настройка SQL Server..." отображаются неверные символы национального алфавита. А для установок на языках, отличных от романских, это предложение может отсутствовать полностью. В отсутствующем предложении должна отображаться следующая локализованная строка: "Идентификатор процесса лицензирования обработан. Новый выпуск: [выпуск \<Name\>]". Эта строка выводится только для информационных целей, а в следующем накопительном пакете обновления [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] эта проблема будет устранена для всех языков. Это никак не влияет на успешное выполнение установки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

#### <a name="full-text-search"></a>Компонент Full-text Search

- В этом выпуске доступны не все фильтры, включая фильтры для документов Office. Список поддерживаемых фильтров см. в статье [Установка полнотекстового поиска SQL Server в Linux](sql-server-linux-setup-full-text-search.md#filters).

### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> Службы SQL Server Integration Services

- В этом выпуске пакет **mssql-server-is** не поддерживается в SUSE. Сейчас он поддерживается в Ubuntu и Red Hat Enterprise Linux (RHEL).

- С помощью [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] в Linux CTP 2.1 и более поздних версиях пакеты [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] могут использовать подключения ODBC в Linux. Эта функция была протестирована с использованием [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и драйверов ODBC для MySQL, но она также должна работать с любым драйвером ODBC для Юникода, который поддерживает спецификацию ODBC. Во время разработки можно указать либо имя DSN, либо строку подключения для подключения к данным ODBC. Кроме того, можно использовать проверку подлинности Windows. Дополнительные сведения см. в [записи блога с объявлением поддержки ODBC в Linux.](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

- Следующие функции не поддерживаются в этом выпуске при запуске пакетов служб SSIS в Linux:
  - База данных каталогов [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Выполнение пакета агентом SQL по расписанию
  - Проверка подлинности Windows
  - Сторонние компоненты
  - Система отслеживания измененных данных (CDC)
  - Горизонтальное увеличение масштаба [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Пакет дополнительных компонентов Azure для SSIS
  - Поддержка Hadoop и HDFS
  - Соединитель Microsoft Connector для SAP BW

Список встроенных компонентов SSIS, которые сейчас не поддерживаются или поддерживаются с ограничениями, см. в статье [Ограничения и известные проблемы для служб SSIS в Linux](sql-server-linux-ssis-known-issues.md#components).

Дополнительные сведения о службах SSIS в Linux см. в следующих статьях.
-   [Запись блога с объявлением о поддержке SSIS для Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [Установка служб SQL Server Integration Services (SSIS) в Linux](sql-server-linux-setup-ssis.md)
-   [Извлечение, преобразование и загрузка данных в Linux с помощью служб SSIS](sql-server-linux-migrate-ssis.md)

### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

На [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] в Windows, подключенный к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux, распространяются следующие ограничения.

- Планы обслуживания не поддерживаются.

- Хранилище данных управления (MDW) и сборщик данных в [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] не поддерживаются. 

- Компоненты пользовательского интерфейса [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], использующие параметры проверки подлинности Windows или журнала событий Windows, не работают с Linux. Эти функции по-прежнему можно использовать с другими параметрами, такими как имена входа SQL. 

- Число сохраняемых файлов журнала не подлежит изменению.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе, ознакомьтесь со следующими краткими руководствами.

- [Установка в Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Установка в SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Установка в Ubuntu](quickstart-install-connect-ubuntu.md)
- [Запуск в Docker](quickstart-install-connect-ubuntu.md)
- [Подготовка виртуальной машины SQL в Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Запуск и подключение — облако](quickstart-install-connect-clouds.md)

Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.md).
