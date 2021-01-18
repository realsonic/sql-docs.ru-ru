---
title: Установка из командной строки
description: Запустите программу установки SQL Server из командной строки, чтобы добавить Службы машинного обучения с R и Python в экземпляр ядра СУБД SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005329"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>Установка Служб машинного обучения с R и Python в SQL Server из командной строки
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

В этой статье приводятся инструкции по установке [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md) с Python и R из командной строки.

Можно выбрать автоматическое, базовое или полное взаимодействие с пользовательским интерфейсом программы установки. Эта статья дополняет статью [Установка SQL Server из командной строки](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), охватывающую параметры, уникальные для компонентов машинного обучения R и Python.

## <a name="pre-install-checklist"></a>Контрольный список перед установкой

+ В командной строке с повышенными привилегиями выполните команды. 

+ Экземпляр ядра СУБД необходим для установки в базе данных. Вы не можете установить только функции R или Python, хотя их можно [добавить в существующий экземпляр постепенно](#add-existing). Если вы хотите использовать только R и Python без ядра СУБД, установите [изолированный сервер](#shared-feature).

+ Не устанавливайте в отказоустойчивом кластере. Механизм безопасности, используемый для изолирования процессов R и Python, несовместим со средой с отказоустойчивым кластером Windows Server.

+ Не устанавливайте на контроллере домена. Этап установки служб машинного обучения завершится с ошибкой.

+ Не следует устанавливать автономные и экземпляры в базе данных на одном компьютере. Изолированный сервер будет конкурировать за те же ресурсы, снижая производительность обеих установок.

## <a name="command-line-arguments"></a>Аргументы командной строки

Аргумент **/FEATURES** является обязательным, как и принятие условий лицензирования. 

При установке из командной строки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает полностью тихий режим (включается параметром **/Q**) и простой тихий режим (включается параметром **/QS**). При указании параметра **/QS** показывается только ход выполнения, не запрашивается ввод данных и не выводятся сообщения об обнаруженных ошибках. Параметр **/QS** поддерживается только в случае, когда указан режим **/Action=install**.

::: moniker range="=sql-server-2016"
| Аргументы | Описание |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Устанавливает версию в базе данных: Службы SQL Server R (в базе данных)  |
| /FEATURES = SQL_SHARED_MR | Устанавливает компонент R для изолированной версии: Сервер SQL Server R (изолированный). Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД.|
| /IACCEPTROPENLICENSETERMS  | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов R с открытым исходным кодом. |
| /IACCEPTPYTHONLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования SQL Server.|
| /MRCACHEDIRECTORY | Для автономной установки указывает папку, которая содержит CAB-файлы компонентов R. |
::: moniker-end

::: moniker range="=sql-server-2017"
| Аргументы | Описание |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Устанавливает версию в базе данных: Службы машинного обучения SQL Server (в базе данных)  |
| /FEATURES = SQL_INST_MR | Объедините с AdvancedAnalytics. Устанавливает функцию R (в базе данных), включая Microsoft R Open и частные пакеты R. |
| /FEATURES = SQL_INST_MRY | Объедините с AdvancedAnalytics. Устанавливает функцию Python (в базе данных), включая Anaconda и частные пакеты R. |
| /FEATURES = SQL_SHARED_MR | Устанавливает компонент R для изолированной версии: Сервер машинного обучения SQL Server (изолированный) Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД.|
| /FEATURES = SQL_SHARED_MRY | Устанавливает компонент Python для изолированной версии: Сервер машинного обучения SQL Server (изолированный) Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД.|
| /IACCEPTROPENLICENSETERMS  | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов R с открытым исходным кодом. |
| /IACCEPTPYTHONLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования SQL Server.|
| /MRCACHEDIRECTORY | Для автономной установки указывает папку, которая содержит CAB-файлы компонентов R. |
| /MPYCACHEDIRECTORY | Зарезервировано для последующего использования. Используйте %TEMP% для сохранения CAB-файлов компонента Python для установки на компьютере, где нет подключения к Интернету. |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| Аргументы | Описание |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Устанавливает версию в базе данных: Службы машинного обучения SQL Server (в базе данных)  |
| /FEATURES = SQL_INST_MR | Объедините с AdvancedAnalytics. Устанавливает функцию R (в базе данных), включая Microsoft R Open и частные пакеты R. |
| /FEATURES = SQL_INST_MRY | Объедините с AdvancedAnalytics. Устанавливает функцию Python (в базе данных), включая Anaconda и частные пакеты R. |
| /FEATURES = SQL_INST_MJAVA | Объедините с AdvancedAnalytics. Устанавливает компонент Java (в базе данных), включая открытый JRE. Применяется к [расширению языка Java для SQL Server](../../language-extensions/install/windows-java.md).|
| /FEATURES = SQL_SHARED_MR | Устанавливает компонент R для изолированной версии: Сервер машинного обучения SQL Server (изолированный) Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД.|
| /FEATURES = SQL_SHARED_MRY | Устанавливает компонент Python для изолированной версии: Сервер машинного обучения SQL Server (изолированный) Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД.|
| /IACCEPTROPENLICENSETERMS  | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов R с открытым исходным кодом. |
| /IACCEPTPYTHONLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования компонентов Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Указывает, что вы приняли условия лицензионного соглашения для использования SQL Server.|
| /MRCACHEDIRECTORY | Для автономной установки указывает папку, которая содержит CAB-файлы компонентов R. |
| /MPYCACHEDIRECTORY | Зарезервировано для последующего использования. Используйте %TEMP% для сохранения CAB-файлов компонента Python для установки на компьютере, где нет подключения к Интернету. |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> Установка экземпляра в базе данных

Аналитика в базе данных доступна для экземпляров ядра СУБД, что необходимо для добавления компонента **Углубленная аналитика** в установку. Можно установить экземпляр ядра СУБД с углубленной аналитикой или [добавить его к существующему экземпляру](#add-existing). 

Чтобы просмотреть сведения о ходе выполнения без интерактивных подсказок на экране, используйте аргумент/QS.

> [!IMPORTANT]
> После установки остаются два дополнительных этапа настройки. Интеграция не будет завершена до выполнения этих задач. Инструкции см. в разделе [Задачи, выполняемые после установки](#post-install).

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>Службы машинного обучения SQL Server: ядро СУБД, расширенная аналитика с Python и R

Для параллельной установки экземпляра ядра СУБД укажите имя экземпляра и имя для входа администратора (Windows). Включает функции для установки основных и языковых компонентов, а также принятие всех условий лицензирования.

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Это та же команда, но с именем для входа в SQL Server в ядре СУБД с использованием смешанной проверки подлинности.

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Этот пример только для Python, который демонстрирует, что можно добавить один язык, опустив функцию.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>Службы R в SQL: ядро СУБД и расширенная аналитика с помощью R

Для параллельной установки экземпляра ядра СУБД укажите имя экземпляра и имя для входа администратора (Windows). Включает функции для установки основных и языковых компонентов, а также принятие всех условий лицензирования.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> Настройка после установки (обязательно)

Применяется только к установкам в базе данных.

После завершения установки вы получите экземпляр ядра СУБД с R и Python, пакетами Microsoft R и Python, Microsoft R Open, Anaconda, инструментами, образцами и сценариями, которые являются частью дистрибутива. 

Для завершения установки требуются еще две задачи:


::: moniker range=">=sql-server-2017"
1. Перезапустите компонент ядро СУБД.

1. Службы машинного обучения SQL Server: Перед использованием этой функции необходимо включить внешние сценарии. Следуйте инструкциям в разделе [Установка служб машинного обучения SQL Server (в базе данных)](sql-machine-learning-services-windows-install.md) в качестве следующего шага. 
::: moniker-end

::: moniker range="=sql-server-2016"
1. Перезапустите компонент ядро СУБД.

1. Службы R SQL Server: Перед использованием этой функции необходимо включить внешние сценарии. Следуйте инструкциям в разделе [Установка служб R SQL Server (в базе данных)](sql-r-services-windows-install.md) в качестве следующего шага. 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a>Добавить углубленную аналитику в существующий экземпляр ядра СУБД

При добавлении углубленной аналитики в базе данных к существующему экземпляру ядра СУБД укажите имя экземпляра. Например, если ранее вы установили ядро СУБД SQL Server 2017 или более поздней версии и Python, эту команду можно использовать для добавления R.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> Автоматическая установка

Автоматическая установка отключает проверку расположения CAB-файлов. По этой причине необходимо указать расположение, в котором будут распакованы CAB-файлы. Для Python CAB-файлы должны находиться в папке% TEMP *. Для R можно задать путь к папке, используя для этого каталог Temp.
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> Установки изолированного сервера

Изолированный сервер — это "общий компонент", не привязанный к экземпляру ядра СУБД. В следующих примерах показан допустимый синтаксис для установки изолированного сервера.

::: moniker range=">=sql-server-2017"
Сервер машинного обучения SQL Server поддерживает Python и R на изолированном сервере:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
Сервер R SQL Server доступен только для R:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

После завершения установки вы получите сервер, пакеты Microsoft, дистрибутивы R и Python, средства, примеры и сценарии, которые являются частью дистрибутива. 

Чтобы открыть окно консоли R, перейдите в `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64` и дважды щелкните **RGui. exe**. Не знакомы с R? Попробуйте этот учебник: [Основные команды R и функции RevoScaleR: 25 распространенных примеров](/machine-learning-server/r/tutorial-r-to-revoscaler).

Чтобы открыть команду Python, перейдите в `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64` и дважды щелкните **Python.exe**.

## <a name="next-steps"></a>Дальнейшие действия

Разработчики на языке Python могут узнать, как использовать Python с SQL Server, изучив следующие руководства.

+ [Учебник по Python. Прогнозирование проката лыж с помощью линейной регрессии в Службах машинного обучения SQL Server](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Учебник по Python. Классификация клиентов на основе кластеризации методом k-средних с помощью служб машинного обучения SQL Server](../tutorials/python-clustering-model.md)

Разработчики на языке R могут ознакомиться с простыми примерами, а также узнать, как код R работает с SQL Server. Дополнительные сведения см. в следующих статьях.

+ [Краткое руководство. Запуск R в T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Руководство. Аналитические функции в базе данных для разработчиков R](../tutorials/r-taxi-classification-introduction.md)
