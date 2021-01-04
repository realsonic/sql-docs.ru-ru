---
title: Установка пакетов Python с помощью sqlmlutils
description: Узнайте, как использовать Python pip для установки новых пакетов Python на экземпляре Служб машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/26/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cd0528125a4bd74b259fd02facb0589f4e123aad
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617553"
---
# <a name="install-python-packages-with-sqlmlutils"></a>Установка пакетов Python с помощью sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этой статье описывается использование функций из пакета [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) для установки новых пакетов Python в экземпляре [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md) и в [кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Устанавливаемые пакеты можно использовать в сценариях Python, выполняющихся в базе данных, с помощью инструкции T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
В этой статье описывается использование функций из пакета [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) для установки новых пакетов Python в экземпляре [Служб машинного обучения в Управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Устанавливаемые пакеты можно использовать в сценариях Python, выполняющихся в базе данных, с помощью инструкции T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

Дополнительные сведения о расположении пакетов и путях установки см. в разделе [Получение сведений о пакете Python](../package-management/python-package-information.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Описанный в этой статье пакет **sqlmlutils** используется для добавления пакетов Python в SQL Server 2019 и более поздние версии. Для SQL Server 2017 и более ранних версий обратитесь к разделу [Установка пакетов с инструментами Python](./install-python-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ Необходимо установить [Службы машинного обучения SQL Server](../install/sql-machine-learning-services-windows-install.md) с параметром языка Python.
::: moniker-end

+ Установите [Azure Data Studio](../../azure-data-studio/what-is.md) на клиентском компьютере, который используется для подключения к SQL Server. Вы можете использовать другие средства запросов и управления базами данных, но в этой статье рассматривается Azure Data Studio.

+ Установите ядро Python в Azure Data Studio. Вы также можете установить и использовать Python из командной строки, и вам может потребоваться альтернативная среда разработки Python, например [Visual Studio Code](https://code.visualstudio.com/download) с [расширением Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

  Версия Python на клиентском компьютере должна соответствовать версии Python на сервере, а устанавливаемые пакеты должны быть совместимы с используемой версией Python.
  Сведения о том, какая версия Python устанавливается вместе с той или иной версией SQL Server, приведены в разделе [Версии Python и R](../sql-server-machine-learning-services.md#versions).
  
  Чтобы проверить версию Python для конкретного экземпляра SQL Server, используйте следующую команду T-SQL.

  ```sql
  EXECUTE sp_execute_external_script
    @language = N'Python',
    @script = N'
  import sys
  print(sys.version)
  '
  ```

### <a name="other-considerations"></a>Другие замечания

+ Библиотека пакетов Python находится в папке Program Files своего экземпляра SQL Server и по умолчанию для установки в этой папке требуются права администратора. Дополнительные сведения см. в статье [Расположение библиотеки пакетов](../package-management/python-package-information.md#default-python-library-location).

+ Установка пакета зависит от экземпляра SQL, базы данных и пользователя, указанных в сведениях о подключении, которые вы укажете для **sqlmlutils**. Чтобы использовать пакет в нескольких экземплярах или базах данных SQL или для разных пользователей, необходимо установить пакет для каждого из них. Исключением является только если пакет устанавливается членом `dbo`, когда пакет *общедоступный* и является общим для всех пользователей. Если пользователь устанавливает более новую версию общедоступного пакета, это не повлияет на общедоступный пакет, но такой пользователь будет иметь доступ к более новой версии.

+ Перед добавлением пакета определите, подходит ли этот пакет для среды SQL Server.

  + Мы рекомендуем использовать Python в базе данных для задач, которые используют преимущества тесной интеграции с ядром СУБД, например машинное обучение, а не задач, которые просто отправляют запросы в базу данных.

  + Если вы добавляете пакеты, которые приводят к слишком большой вычислительной нагрузке на сервер, производительность снизится.

  + В защищенной среде SQL Server может потребоваться отказаться от следующих пакетов:
    + пакеты, которым требуется доступ к сети;
    + пакеты, которым требуется доступ к файловой системе с повышенными правами;
    + пакеты, используемые для веб-разработки или других задач, малоэффективных в SQL Server.

  + Пакет **tensorflow** Python невозможно установить с использованием sqlmlutils. Дополнительные сведения и обходной путь см. в статье [Известные проблемы служб машинного обучения SQL Server](../troubleshooting/known-issues-for-sql-server-machine-learning-services.md#9-cannot-install-tensorflow-package-using-sqlmlutils).

## <a name="install-sqlmlutils-on-the-client-computer"></a>Установка пакета sqlmlutils на клиентском компьютере

Сначала пакет **sqlmlutils** необходимо установить на клиентском компьютере, который используется для подключения к SQL Server.

### <a name="in-azure-data-studio"></a>Azure Data Studio

Если вы будете использовать пакет **sqlmlutils** в Azure Data Studio, вы можете установить его с помощью функции "Управление пакетами" в записной книжке ядра Python.

1. В [записной книжке ядра Python в Azure Data Studio](../../azure-data-studio/notebooks/notebooks-python-kernel.md) щелкните **Управление пакетами**.
1. Щелкните **Добавить новую**.
1. В поле **Поиск пакетов PIP** введите "sqlmlutils" и нажмите кнопку **Поиск**.
1. Выберите **версию пакета**, которую необходимо установить (рекомендуется последняя версия).
1. Нажмите кнопку **Установить**, а затем кнопку **Закрыть**.

### <a name="from-python-command-line"></a>Командная строка Python

Если вы будете использовать **sqlmlutils** из командной строки Python или интегрированной среды разработки (IDE), пакет sqlmlutils можно установить с помощью простой команды **pip**:

```console
pip install sqlmlutils
```

Пакет **sqlmlutils** можно также установить из ZIP-файла:

1. Убедитесь в том, что установлена программа **pip**. Дополнительные сведения см. в [статье об установке pip](https://pip.pypa.io/en/stable/installing/).
1. Скачайте последнюю версию ZIP-файла **sqlmlutils** со страницы https://github.com/Microsoft/sqlmlutils/tree/master/Python/dist на клиентский компьютер. Не распаковывайте файл.
1. Откройте **командную строку** и выполните следующие команды для установки пакета **sqlmlutils**. Замените полный путь к скачанному ZIP-файлу **sqlmlutils** (в этом примере предполагается, что загружен файл `c:\temp\sqlmlutils-1.0.0.zip`).
   ```console
   pip install --upgrade --upgrade-strategy only-if-needed c:\temp\sqlmlutils-1.0.0.zip
   ```

## <a name="add-a-python-package-on-sql-server"></a>Добавление пакета Python в SQL Server

Используя **sqlmlutils**, можно добавлять пакеты Python в экземпляр SQL. Затем эти пакеты можно использовать в коде Python, который выполняется в экземпляре SQL. **sqlmlutils** использует команду [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) для установки пакета и всех его зависимостей.

В следующем примере вы добавите пакет [text-tools](https://pypi.org/project/text-tools/) в SQL Server.

### <a name="add-the-package-online"></a>Добавление пакета в сетевом режиме

Если клиентский компьютер, используемый для подключения к SQL Server, имеет доступ к Интернету, с помощью пакета **sqlmlutils** можно найти пакет **text-tools** и все зависимости через Интернет, а затем удаленно установить пакет в экземпляре SQL Server.

::: moniker range=">=sql-server-ver15"

1. На клиентском компьютере откройте **Python** или среду Python.

1. Для установки пакета **text-tools** используйте следующие команды. Замените сведения о подключении к базе данных SQL Server (если используется проверка подлинности Windows, вам не нужны параметры `uid` и `pwd`).

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

1. На клиентском компьютере откройте **Python** или среду Python.

1. Для установки пакета **text-tools** используйте следующие команды. Подставьте собственные значения для подключения к базе данных SQL Server.

::: moniker-end

   ```python
   import sqlmlutils
   connection = sqlmlutils.ConnectionInfo(server="server", database="database", uid="username", pwd="password")
   sqlmlutils.SQLPackageManager(connection).install("text-tools")
   ```

### <a name="add-the-package-offline"></a>Добавление пакета в автономном режиме

Если клиентский компьютер, используемый для подключения к SQL Server, не имеет подключения к Интернету, можно использовать **pip** на компьютере с доступом в Интернет для загрузки пакета и всех зависимых пакетов в локальную папку. Затем папка копируется на клиентский компьютер для установки в автономном режиме.

#### <a name="on-a-computer-with-internet-access"></a>На компьютере с доступом к Интернету

1. Откройте **командную строку** и выполните следующую команду, чтобы создать локальную папку, содержащую пакет **text-tools**. В этом примере создается папка `c:\temp\text-tools`.

   ```console
   pip download text-tools -d c:\temp\text-tools
   ```

1. Скопируйте папку `text-tools` на клиентский компьютер. В следующем примере предполагается, что вы скопировали ее в `c:\temp\packages\text-tools`.

#### <a name="on-the-client-computer"></a>На клиентском компьютере

Используйте **sqlmlutils**, чтобы установить каждый пакет (WHL-файл), который находится в локальной папке, созданной с помощью **pip**. Порядок установки пакетов не имеет значения.

В этом примере **text-tools** не имеет зависимостей, поэтому нужно установить только один файл из папки `text-tools`. Зато, например, у пакета **scikit-plot** имеется 11 зависимостей, поэтому в папке будет находиться 12 файлов (**scikit-plot** и 11 зависимых пакетов), и каждый из них будет установлен.

::: moniker range=">=sql-server-ver15"

Выполните следующий скрипт Python. Замените фактический путь к файлу и имя пакета, а также сведения о подключении к базе данных SQL Server (если используется проверка подлинности Windows, вам не нужны параметры `uid` и `pwd`). Повторите инструкцию `sqlmlutils.SQLPackageManager` для каждого файла пакета в папке.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

Выполните следующий скрипт Python. Замените фактический путь к файлу и имя пакета, а также сведения о подключении к базе данных SQL Server. Повторите инструкцию `sqlmlutils.SQLPackageManager` для каждого файла пакета в папке.

::: moniker-end

```python
import sqlmlutils
connection = sqlmlutils.ConnectionInfo(server="yourserver", database="yourdatabase", uid="username", pwd="password"))
sqlmlutils.SQLPackageManager(connection).install("text_tools-1.0.0-py3-none-any.whl")
```

## <a name="use-the-package"></a>Использование пакета

Теперь пакет можно использовать в скрипте Python в SQL Server. Пример:

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
from text_tools.finders import find_best_string
corpus = "Lorem Ipsum text"
query = "Ipsum"
first_match = find_best_string(query, corpus)
print(first_match)
  '
```

## <a name="remove-the-package-from-sql-server"></a>Удаление пакета из SQL Server

Если вы хотите удалить пакет **text-tools**, используйте следующую команду Python на клиентском компьютере, указав переменную подключения, которая была определена ранее.

```python
sqlmlutils.SQLPackageManager(connection).uninstall("text-tools")
```

## <a name="more-sqlmlutils-functions"></a>Дополнительные функции sqlmlutils

Пакет **sqlmlutils** содержит ряд функций для управления пакетами Python, а также для создания, выполнения хранимых процедур и запросов в SQL Server и управления ими. Дополнительные сведения см. в [файле сведений sqlmlutils для Python](https://github.com/microsoft/sqlmlutils/tree/master/Python).

Дополнительные сведения о любых функциях **sqlmlutils** можно получить с помощью функции **справки** Python. Пример:

```Python
import sqlmlutils
help(SQLPackageManager.install)
```

## <a name="next-steps"></a>Дальнейшие действия

+ Сведения о том, как просмотреть пакеты Python, установленные в Службах машинного обучения SQL Server, см. в разделе [Получения сведений о пакете Python](../package-management/python-package-information.md).

+ Сведения об установке пакетов R в Службы машинного обучения SQL Server см. в разделе [Установка новых пакетов R в SQL Server](install-additional-r-packages-on-sql-server.md).
