---
title: Защита контейнеров Docker в SQL Server
description: Узнайте о различных способах защиты контейнеров Docker на SQL Server и о том, как запускать контейнеры на узле от имени другого непривилегированного пользователя.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 88c179b1a17dd84dc33c2ae77979507e82b7a55a
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878780"
---
# <a name="secure-sql-server-docker-containers"></a>Защита контейнеров Docker в SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Контейнеры SQL Server 2017 по умолчанию запускаются как привилегированные пользователи. Это может привести к некоторым проблемам безопасности. В этой статье рассказывается о доступных вариантах безопасности при запуске контейнеров Docker SQL Server, а также о том, как создать контейнер SQL Server в качестве непривилегированного пользователя.

## <a name="build-and-run-non-root-sql-server-2017-containers"></a><a id="buildnonrootcontainer"></a> Создание и запуск контейнеров SQL Server 2017, не являющихся корневыми

Чтобы создать контейнер SQL Server 2017, который запускается от имени пользователя `mssql` (не являющегося привилегированным), выполните указанные ниже действия.

> [!NOTE]
> Контейнеры SQL Server 2019 автоматически запускаются как не корневые, поэтому следующие шаги применяются только к контейнерам SQL Server 2017, которые по умолчанию запускаются как корневые.

1. Скачайте [образец файла dockerfile для непривилегированного контейнера SQL Server](https://raw.githubusercontent.com/microsoft/mssql-docker/master/linux/preview/examples/mssql-server-linux-non-root/Dockerfile) и сохраните его как `dockerfile`.

2. Выполните следующую команду в контексте каталога dockerfile, чтобы выполнить сборку непривилегированного контейнера SQL Server:

    ```bash
    cd <path to dockerfile>
    docker build -t 2017-latest-non-root .
    ```

3. Запустите контейнер.

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword@" --cap-add SYS_PTRACE --name sql1 -p 1433:1433 -d 2017-latest-non-root
    ```

    > [!NOTE]
    > Флаг `--cap-add SYS_PTRACE` необходим для создания дампов в целях устранения неполадок непривилегированных контейнеров SQL Server.

4. Убедитесь в том, что контейнер запущен от имени пользователя, не являющегося привилегированным:

    ```bash
    docker exec -it sql1 bash
    ```

    Выполните команду `whoami`, которая вернет пользователя, от имени которого выполняется контейнер.
    
    ```bash
    whoami
    ```

## <a name="run-container-as-a-different-non-root-user-on-the-host"></a><a id="nonrootuser"></a> Запуск контейнера от имени другого непривилегированного пользователя в узле

Чтобы запустить контейнер SQL Server от имени другого непривилегированного пользователя, добавьте флаг -u в команду docker run. Непривилегированный контейнер должен выполняться в составе привилегированной группы, если только том не подключен к каталогу `/var/opt/mssql`, к которому есть доступ у непривилегированного пользователя. Привилегированная группа не предоставляет дополнительных привилегированных разрешений пользователю, не являющемуся привилегированным.

#### <a name="run-as-a-user-with-a-uid-4000"></a>Выполнение от имени пользователя с ИД пользователя 4000

SQL Server можно запускать с настраиваемым идентификатором пользователя. Например, следующая команда запускает SQL Server с идентификатором пользователя 4000:

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u 4000:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

> [!Warning]
> Убедитесь в том, что в контейнере SQL Server есть именованный пользователь, например mssql или root. В противном случае SQLCMD невозможно будет запустить в контейнере. Вы можете проверить, выполняется ли контейнер SQL Server от имени именованного пользователя, запустив `whoami` в контейнере.

#### <a name="run-the-non-root-container-as-the-root-user"></a>Запуск непривилегированного контейнера от имени привилегированного пользователя

При необходимости непривилегированный контейнер можно запустить от имени привилегированного пользователя. При этом контейнер также автоматически получает все разрешения на доступ к файлам.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -u 0:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-user-on-your-host-machine"></a>Запуск от имени пользователя на хост-компьютере

SQL Server можно запустить от имени существующего пользователя на хост-компьютере с помощью следующей команды:
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u $(id -u myusername):0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-different-user-and-group"></a>Запуск от имени другого пользователя и группы

SQL Server можно запускать от имени произвольного пользователя и группы. В этом примере подключенный том имеет разрешения, настроенные для пользователя или группы на хост-компьютере.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u (id -u myusername):(id -g myusername) -v /path/to/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

## <a name="configure-persistent-storage-permissions-for-non-root-containers"></a><a id="storagepermissions"></a> Настройка разрешений на постоянное сохранение для непривилегированных контейнеров

Чтобы разрешить непривилегированному пользователю доступ к файлам базы данных в подключенных томах, пользователь или группа, от имени которых выполняется контейнер, должны иметь доступ на чтение и запись к постоянному хранилищу файлов.  

Узнать текущего владельца файлов базы данных можно с помощью приведенной ниже команды.

```bash
ls -ll <database file dir>
```

Если у SQL Server нет доступа к сохраненным файлам базы данных, выполните одну из приведенных ниже команд.

#### <a name="grant-the-root-group-rw-access-to-the-db-files"></a>Предоставление привилегированной группе доступа к файлам базы данных для чтения и записи

Предоставьте привилегированной группе разрешения на доступ к указанным ниже каталогам, чтобы у непривилегированного контейнера SQL Server был доступ к файлам базы данных.

```bash
chgrp -R 0 <database file dir>
chmod -R g=u <database file dir>
```

#### <a name="set-the-non-root-user-as-the-owner-of-the-files"></a>Назначение непривилегированного пользователя владельцем файлов

Это может быть непривилегированный пользователь по умолчанию или любой другой непривилегированный пользователь на ваш выбор. В этом примере в качестве непривилегированного пользователя задан пользователь с идентификатором 10001.

```bash
chown -R 10001:0 <database file dir>
```
## <a name="encrypting-connections-to-sql-server-linux-containers"></a>Шифрование соединений с контейнерами Linux SQL Server

Чтобы шифровать соединения с контейнерами Linux SQL Server, вам потребуется сертификат, требования к которому задокументированы [здесь].

Ниже приведен пример того, как можно зашифровать соединения с контейнерами Linux SQL Server. Здесь мы используем самозаверяющий сертификат, который не следует использовать в рабочих сценариях для таких сред, в них следует использовать сертификаты от центра сертификации.

1. Создайте самозаверяющий сертификат, который подходит только для тестовых и нерабочих сред.
  
      ```bash
      openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=sql1.contoso.com' -keyout /container/sql1/mssql.key -out /container/sql1/mssql.pem -days 365
      ```
     Где sql1 — имя узла контейнера SQL, поэтому при подключении к этому контейнеру именем, используемым в строке подключения, будет \'sql1.contoso.com.port\'.

    > [!NOTE]
    > Перед выполнением указанной выше команды убедитесь, что путь к папке /container/sql1/ уже существует.

2. Убедитесь, что установлены правильные разрешения на файлы mssql.key and mssql.pem, чтобы избежать ошибок при подключении файлов к контейнеру SQL:

    ```bash
    chmod 440 /container/sql1/mssql.pem
    chmod 440 /container/sql1/mssql.key
    ```

3. Теперь создайте файл mssql.conf с приведенным ниже содержимым, чтобы включить шифрование, инициированное сервером. Для шифрования, инициированного клиентом, измените последнюю строку на 'forceencryption = 0\'.

    ```bash
    [network]
    tlscert = /etc/ssl/certs/mssql.pem
    tlskey = /etc/ssl/private/mssql.key
    tlsprotocols = 1.2
    forceencryption = 1
    ```

    > [!NOTE]
    > Для некоторых дистрибутивов Linux путь для хранения сертификата и ключа также может быть следующим: /etc/pki/tls/certs/ и /etc/pki/tls/private/ соответственно. Проверьте путь перед обновлением mssql.conf для контейнеров SQL. Расположение, заданное в mssql.conf, будет совпадать с расположением, где SQL Server в контейнере будет искать сертификат и его ключ. В данном случае это расположение — etc/ssl/certs/ и /etc/ssl/private/.

    Файл mssql.conf также создается в том же расположении папки /container/sql1/. После выполнения описанных выше действий в папке sql1 должны быть три файла: mssql.conf, mssql.key и mssql.pem.

4. Разверните контейнер SQL с помощью команды, показанной ниже:

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5434:1433 --name sql1 -h sql1 -v /container/sql1/mssql.conf:/var/opt/mssql/mssql.conf -v   /container/sql1/mssql.pem:/etc/ssl/certs/mssql.pem -v /container/sql1/mssql.key:/etc/ssl/private/mssql.key -d mcr.microsoft.com/mssql/server:2019-latest
    ```

    В приведенной выше команде мы подключили файлы mssql.conf, mssql.pem и mssql.keyк контейнеру и сопоставили порт 1433 (порт SQL Server по умолчанию) в контейнере с портом 5434 узла. 

    > [!NOTE]
    > При использовании RHEL 8 и более поздних версий можно также использовать команду \'podman run\', вместо \'docker run\'. 

Следуйте указаниям в разделах \"Регистрация сертификата на клиентском компьютере\" и \"Примеры строк подключения\", приведенных [здесь][1], чтобы начать шифрование подключений к контейнерам SQL Server в Linux.

  [Encrypting connection to SQL Server Linux]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true
  [здесь]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#requirements-for-certificates
  [1]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#client-initiated-encryption

## <a name="next-steps"></a>Дальнейшие действия

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Сведения о начале работы с образами контейнеров с SQL Server 2017 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Сведения о начале работы с образами контейнеров с SQL Server 2019 в Docker можно найти в [кратком руководстве](quickstart-install-connect-docker.md)

::: moniker-end

- [Развертывание контейнеров Docker в SQL Server и подключение к ним](sql-server-linux-docker-container-deployment.md)

- [Ссылка на дополнительную конфигурацию и настройку контейнеров Docker](sql-server-linux-docker-container-configure.md)

- [Устранение неполадок контейнеров Docker на SQL Server](sql-server-linux-docker-container-troubleshooting.md)
