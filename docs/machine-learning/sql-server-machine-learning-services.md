---
title: Что такое службы машинного обучения SQL Server (Python и R)?
titleSuffix: ''
description: Службы машинного обучения — это компонент SQL Server, который дает возможность выполнять скрипты Python и R с реляционными данными. Вы можете использовать платформы и пакеты с открытым исходным кодом и пакеты Майкрософт Python и R для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы Служб машинного обучения SQL Server и описывается, как приступить к работе с ними.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 98e9dd928f2639bfd1df3e6782bcc9aaed60f31e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102880"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>Что такое службы машинного обучения SQL Server с Python и R?
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Службы машинного обучения — это компонент SQL Server, который дает возможность выполнять скрипты Python и R с реляционными данными. Вы можете использовать платформы и пакеты с открытым кодом и [пакеты Microsoft Python и R](#packages) для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы Служб машинного обучения SQL Server и описывается, как приступить к работе с ними.

Сведения о применении машинного обучения на других платформах SQL доступны в [документации по машинному обучению SQL](index.yml).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Сведения о запуске Java в SQL Server см. в [документации по расширению языка Java](../language-extensions/java-overview.md).
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>Выполнение сценариев Python и R в среде SQL Server

Службы машинного обучения SQL Server можно использовать для запуска скриптов R или Python в базе данных. С их помощью можно подготавливать и очищать данные, выполнять проектирование признаков, а также обучать, оценивать и развертывать модели машинного обучения в базе данных. Этот компонент выполняет скрипты там, где хранятся данные, и устраняет необходимость перемещения данных по сети на другой сервер.

Вы выполните хранимую процедуру [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для запуска сценариев Python и R в экземпляре SQL Server.

Базовые распределения Python и R включены в Службы машинного обучения. Вы можете установить и использовать платформы и пакеты с открытым кодом, такие как PyTorch, TensorFlow и scikit-learn, в дополнение к пакетам Microsoft.

Службы машинного обучения используют платформу расширяемости для выполнения скриптов Python и R на SQL Server. Дополнительные сведения о том, как это работает:

+ [Платформа расширяемости](concepts/extensibility-framework.md)
+ [Расширение Python](concepts/extension-python.md)
+ [Расширение R](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>Приступая к работе со Службами машинного обучения

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [Установите Службы машинного обучения SQL Server в Windows](install/sql-machine-learning-services-windows-install.md) или [Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json). Вы также можете использовать [службы машинного обучения в кластерах больших данных](../big-data-cluster/machine-learning-services.md) и [службы машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

1. Настройте средства разработки. Вы можете [выполнять сценарии Python и R в записных книжках Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). Можно также выполнять T-SQL в [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Создайте свой первый сценарий Python или R.

   + [Учебники по использованию Python для машинного обучения SQL](tutorials/python-tutorials.md)
   + [Учебники по использованию R для машинного обучения SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ Создайте свой первый сценарий Python или R.

   + [Учебники по использованию Python для машинного обучения SQL](tutorials/python-tutorials.md)
   + [Учебники по использованию R для машинного обучения SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [Установка служб машинного обучения SQL Server в Windows](install/sql-machine-learning-services-windows-install.md).

1. Настройте средства разработки. Вы можете [выполнять сценарии Python и R в записных книжках Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). Можно также использовать T-SQL в [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Создайте свой первый сценарий Python или R.

   + [Учебники по использованию Python для машинного обучения SQL](tutorials/python-tutorials.md)
   + [Учебники по использованию R для машинного обучения SQL](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Версии Python и R

Ниже перечислены версии Python и R, включенные в Службы машинного обучения.

| Версия SQL Server | Версия Python | Версия R |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

Сведения о версии R в SQL Server 2016 см. в [разделе о версии R в статье с описанием служб R Services](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version).

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Пакеты Python и R

В дополнение к корпоративным пакетам Майкрософт можно использовать платформы и пакеты с открытым кодом. Наиболее распространенные пакеты Python и R с открытым кодом предварительно установлены в Службах машинного обучения. Также включены следующие пакеты Python и R от Майкрософт:

| Язык | Пакет | Описание |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Основной пакет для масштабируемого Python. Преобразования и обработка данных, статистическая сводка, визуализация и многие виды моделирования. Кроме того, функции в этом пакете автоматически распределяют рабочие нагрузки между доступными ядрами для параллельной обработки. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Добавляет алгоритмы машинного обучения для создания пользовательских моделей для анализа текста, анализа изображений и анализа тональности. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Основной пакет для масштабируемого R. Преобразования и обработка данных, статистическая сводка, визуализация и многие виды моделирования. Кроме того, функции в этом пакете автоматически распределяют рабочие нагрузки между доступными ядрами для параллельной обработки. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Добавляет алгоритмы машинного обучения для создания пользовательских моделей для анализа текста, анализа изображений и анализа тональности. |
| R | [olapR](r/ref-r-olapr.md) | Функции R, используемые для запросов многомерных выражений к кубу OLAP SQL Server Analysis Services. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Механизм для использования скриптов R в хранимой процедуре T-SQL, регистрации этой хранимой процедуры в базе данных и ее запуска из [среды разработки R](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) — это улучшенная версия R от Майкрософт. Это полная платформа с открытым кодом для статистического анализа и обработки и анализа данных. Она основана на R, полностью совместима с ним и включает дополнительные возможности для повышения производительности и воспроизводимости. |

Дополнительные сведения о том, какие пакеты устанавливаются со Службами машинного обучения и как устанавливать другие пакеты, см. в следующих статьях:

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [Получение сведений о пакете Python](package-management/python-package-information.md)
+ [Установка пакетов Python с помощью sqlmlutils](package-management/install-additional-python-packages-on-sql-server.md)
+ [Получение сведений о пакете R](package-management/r-package-information.md)
+ [Установка новых пакетов R с помощью sqlmlutils](package-management/install-additional-r-packages-on-sql-server.md)
::: moniker-end
::: moniker range="=sql-server-2017"
+ [Получение сведений о пакете Python](package-management/python-package-information.md)
+ [Установка пакетов с инструментами Python в SQL Server](package-management/install-python-packages-standard-tools.md)
+ [Получение сведений о пакете R](package-management/r-package-information.md)
+ [Использование инструкции T-SQL (CREATE EXTERNAL LIBRARY) для установки пакетов R на SQL Server](package-management/install-r-packages-with-tsql.md)
::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

+ [Установите Службы машинного обучения SQL Server в Windows](install/sql-machine-learning-services-windows-install.md) или [Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json).
+ [Учебники по использованию Python для машинного обучения SQL](tutorials/python-tutorials.md)
+ [Учебники по использованию R для машинного обучения SQL](tutorials/r-tutorials.md)