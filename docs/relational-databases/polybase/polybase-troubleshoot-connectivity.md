---
title: Устранение неполадок с подключением PolyBase к Kerberos | Microsoft Docs
description: Чтобы устранять неполадки с аутентификацией при использовании PolyBase с защищенным Kerberos кластером Hadoop, используйте интерактивный инструмент диагностики, встроенный в PolyBase.
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: polybase, sql-data-warehouse, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 24a228fb504f53c1afe88b0955500e811f1073b6
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171496"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>Устранение неполадок с подключением PolyBase к Kerberos

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

В PolyBase встроен интерактивный инструмент диагностики, который помогает устранять неполадки с аутентификацией при использовании PolyBase с защищенным Kerberos кластером Hadoop. 

В статье приведено пошаговое руководство по использованию этого инструмента для отладки таких проблем.

> [!TIP]
> Вместо действий, описанных в этом руководстве, можно запустить [HDFS Kerberos](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester), чтобы устранить неполадки подключений HDFS Kerberos для Polybase в случае ошибки HDFS Kerberos при создании внешней таблицы в защищенном Kerberos кластере HDFS.
> Это средство поможет вам исключить проблемы, не связанные с SQL Server, чтобы сосредоточиться на решении проблем установки HDFS Kerberos, а именно — выявлении проблем с неправильной конфигурацией имени пользователя или пароля и неправильной конфигурацией протокола Kerberos для кластера.      
> Это средство полностью независимо от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Оно доступно в виде Jupyter Notebook и требует Azure Data Studio.

## <a name="prerequisites"></a>Предварительные требования

1. [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] RTM CU6, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) CU3, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] или выше с установленной службой Polybase
1. Кластер Hadoop (Cloudera или Hortonworks), защищенный с помощью Kerberos (Active Directory или MIT)

## <a name="introduction"></a>Введение
В первую очередь рекомендуется понять, как происходит работа протокола Kerberos на высоком уровне. В ней участвуют три субъекта:

1. клиент Kerberos (SQL Server);
1. защищаемый ресурс (HDFS, MR2, YARN, журнал заданий и т. д.);
1. центр распространения ключей (называемый в Active Directory контроллером домена).

Если в кластере Hadoop настроен протокол Kerberos, каждый защищаемый ресурс регистрируется в **центре распространения ключей (KDC)** с уникальным **именем субъекта-службы**. Ожидается, что клиенту будет предоставляться временный пользовательский **билет на получение билетов**, позволяющий запрашивать в KDC **билет службы**, тоже временный, для доступа к субъекту службы с нужным именем.  

При использовании PolyBase, когда на любом защищенном Kerberos ресурсе запрашивается аутентификация, запускается процедура подтверждения, происходящая в четыре этапа:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подключается к центру распространения ключей и загружает билет на получение билетов для пользователя. Этот билет шифруется с использованием закрытого ключа от центра.
1. Затем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вызывает защищенный ресурс Hadoop (HDFS) и определяет, для какого имени субъекта-службы требуется билет службы.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает центру распространения ключей билет на получение билетов, а затем запрашивает у него билет службы для доступа к нужному защищенному ресурсу. Билет службы шифруется закрытым ключом защищенной службы.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] переадресовывает билет службы в Hadoop и проходит аутентификацию, необходимую для создания сеанса с этой службой.

![SQL Server в PolyBase](./media/polybase-sqlserver.png)

Проблемы с аутентификацией могут возникать на любых из этих четырех этапов. Для упрощения отладки в PolyBase был встроен диагностический инструмент, который помогает определять точку отказа.

## <a name="troubleshooting"></a>Устранение неполадок

В PolyBase имеются следующие XML-файлы конфигурации, содержащие свойства кластера Hadoop:

- core-site.xml
- hdfs-site.xml
- Hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

Эти файлы находятся в следующей папке:

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

Например, для [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] по умолчанию используется `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`.

Обновите файл **core-site.xml**, добавив три приведенных ниже свойства. Задайте значения в соответствии со средой:

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> Значение свойства `polybase.kerberos.realm` должно быть задано в верхнем регистре.

Если планируется использовать операции передачи, впоследствии нужно будет изменить и другие XML-файлы. Но изменения одного этого файла уже будет достаточно как минимум для доступа к файловой системе HDFS.

Инструмент работает независимо от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], поэтому его не требуется запускать заранее или перезапускать при изменении XML-конфигураций. Для запуска инструмента выполните на хост-компьютере с установленным [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следующие команды:

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>Аргументы

| Аргумент | Описание|
| --- | --- |
| *Адрес узла имен* | IP-адрес или полное доменное имя узла имен. Относится к аргументу LOCATION в вашей инструкции CREATE EXTERNAL DATA SOURCE (T-SQL). Примечание. Для версии SQL Server 2019 необходимо, чтобы перед IP-адресом или полным доменным именем стоял *hdfs:\/\/* .|
| *Порт узла имен* | Соответствует порту узла имен. Относится к аргументу LOCATION в вашей инструкции CREATE EXTERNAL DATA SOURCE (T-SQL). Например, 8020. |
| *Субъект-служба* | Субъект-служба администрирования для вашего центра распространения ключей. Соответствует аргументу IDENTITY в инструкции T-SQL `CREATE DATABASE SCOPED CREDENTIAL`.|
| *Пароль службы* | Вместо того, чтобы вводить пароль в консоли, сохраните его в файле и передайте сюда путь. Содержимое файла должно соответствовать аргументу SECRET в вашей инструкции `CREATE DATABASE SCOPED CREDENTIAL` (T-SQL). |
| *Путь к файлу на удаленной HDFS (необязательно)* | Путь для доступа к существующему файлу. Если его не указать, будет использоваться корневая папка ("/"). |

## <a name="example"></a>Пример

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

Полученный результат будет очень подробным, что позволяет проводить тщательную отладку. Но и при использовании MIT, и Active Directory следует в первую очередь обращать внимание на четыре главные контрольные точки. Они соответствуют четырем этапам, описанным выше. 

Следующие фрагменты получены при работе с центром распространения ключей MIT. Полные примеры выходных данных для MIT и Active Directory см. в ссылках в конце статьи.

## <a name="checkpoint-1"></a>Контрольная точка 1
В выходных данных должен быть шестнадцатеричный дамп билета со значением `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM`. Его наличие показывает, что сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] успешно прошел проверку подлинности в центре распространения ключей и ему был предоставлен билет на получение билетов. Если такого дампа нет, проблема лежит строго между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и центром; Hadoop здесь ни при чем.

Технология PolyBase **не поддерживает** установление отношений доверия между Active Directory и MIT, поэтому она должна быть настроена на тот же центр получения ключей, что и Hadoop-кластер. В таких средах вы можете вручную создать в центре учетную запись службы и использовать ее для аутентификации.

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>Контрольная точка 2
PolyBase пытается получить доступ к HDFS и не может это сделать, так как запрос не содержит нужного билета службы.

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>Контрольная точка 3
Второй шестнадцатеричный дамп показывает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] успешно воспользовался билетом на получение билетов и загрузил из центра распространения ключей нужный билет для имени субъекта-службы в узле имен.

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>Контрольная точка 4
Наконец, должны выводиться свойства файла по целевому пути и подтверждение. Свойства файла подтверждают, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прошел в Hadoop проверку подлинности с билетом службы и был создан сеанс для доступа к защищенному ресурсу.

Достижение этой точки подтверждает, что: (i) три субъекта могут обмениваться друг с другом данными, (ii) файлы core-site.xml и jaas.conf корректные, (iii) центр распространения ключей подтвердил ваши учетные данные.

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>Распространенные ошибки
Если после запуска инструмента *не были* выведены свойства файла по целевому пути (контрольная точка 4), в ходе работы должно было быть выдано исключение. Проверьте его и рассмотрите контекст, то есть на каком из четырех этапов потока оно возникло. Рассмотрите по порядку следующие распространенные проблемы, которые могли возникнуть:

| Исключение и сообщения | Причина | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>Не включена аутентификация SIMPLE. Доступно: [TOKEN, KERBEROS] | В файле "core-site.xml" для свойства "hadoop.security.authentication" не задано значение KERBEROS.|
|javax.security.auth.login.LoginException<br>Клиент не найден в базе данных Kerberos (6) — CLIENT_NOT_FOUND |    Предоставленная субъект-служба администрирования не существует в области, указанной в файле "core-site.xml".|
| javax.security.auth.login.LoginException<br> Сбой контрольной суммы |Субъект-служба администрирования существует, но отправлен неправильный пароль. |
| Имя собственной конфигурации: C:\Windows\krb5.ini<br>Загружено из собственной конфигурации | Это сообщение указывает, что модуль Java krb5LoginModule обнаружил на вашем компьютере настроенные конфигурации клиента. Проверьте параметры настроенного клиента, так как они могут быть причиной проблемы. |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>Недопустимое имя субъекта admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: нет примененных правил к admin_user@CONTOSO.COM | Добавьте свойство hadoop.security.auth_to_local в файл core-site.xml с необходимыми правилами для Hadoop-кластера. |
| java.net.ConnectException<br>Попытка доступа к внешней файловой системе с универсальным кодом ресурса hdfs://10.193.27.230:8020<br>Сбой вызова с IAAS16981207/10.107.0.245 на 10.193.27.230:8020 с исключением подключения | Аутентификация в центре распространения ключей прошла успешно, но не удалось получить доступ к узлу имен Hadoop. Проверьте IP-адрес и порт узла имен. На Hadoop не должен работать брандмауэр. |
| java.io.FileNotFoundException<br>Файл не существует: /test/data.csv |    Аутентификация прошла успешно, но указанного расположения не существует. Проверьте путь или попробуйте сначала использовать корневую папку ("/"). |

## <a name="debugging-tips"></a>Советы по отладке

### <a name="mit-kdc"></a>Центр распространения ключей MIT  
Все имена субъектов-служб, в том числе служб администрирования, зарегистрированные в центре распространения ключей, можно просмотреть, запустив **kadmin.local** > (войти в качестве администратора) > **listprincs** на хост-компьютере или любом настроенном клиенте центра. Если для кластера Hadoop корректно настроена защита Kerberos, имена субъектов-служб должны иметься для каждой из служб, доступных в кластере (например, `nn`, `dn`, `rm`, `yarn`, `spnego` и т. д.). Соответствующие им файлы KEYTAB (заменители паролей) по умолчанию находятся в папке **/etc/security/keytabs**. Они шифруются с использованием закрытого ключа центра распространения ключей.  

Попробуйте также проверить учетные данные администратора в центре распространения ключей локально с помощью инструмента [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html). Пример его использования: `kinit identity@MYREALM.COM`. Если запрашивается пароль, значит, такой идентификатор существует.  
Журналы центра распространения ключей по умолчанию находятся здесь: **/var/log/krb5kdc.log**. Они включают все запросы билетов и IP-адрес клиента, с которого поступал запрос. Должно быть два запроса с IP-адреса компьютера, где работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и где был запущен инструмент. Первый — запрос билета на получение билетов к серверу аутентификации в качестве **AS\_REQ**. Второй — запрос билета службы к северу выдачи билетов в качестве **TGS\_REQ**.

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
В Active Directory вы можете просмотреть имена объектов-служб, перейдя в Панель управления > Пользователи и компьютеры Active Directory > *MyRealm* > *MyOrganizationalUnit*. Если для кластера Hadoop корректно настроена защита Kerberos, имена субъектов-служб должны иметься для каждой из доступных служб (например, `nn`, `dn`, `rm`, `yarn`, `spnego` и т. д.).

### <a name="general-debugging-tips"></a>Общие советы по отладке
Для просмотра журналов и устранения неполадок Kerberos, которые не связаны с компонентом PolyBase SQL Server, полезно иметь некоторый опыт работы с Java.

Если у вас возникают проблемы с доступом посредством Kerberos, выполните указанные ниже действия по отладке.

1. Убедитесь в наличии доступа к данным Kerberos HDFS извне [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Вы можете сделать одно из двух: 

    - Создайте свою программу на Java или
    - используйте класс `HdfsBridge` из папки установки PolyBase. Пример:

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     В приведенном выше примере `admin_user` включает в себя только имя пользователя, но не доменное имя.

2. Если вы не можете получить доступ к данным Kerberos HDFS извне PolyBase:
    - Существуют два типа проверки подлинности Kerberos: проверка подлинности Kerberos в Active Directory и проверка подлинности Kerberos MIT.
    - Убедитесь в том, что пользователь имеется в учетной записи домена, и используйте ту же учетную запись пользователя при попытке доступа к HDFS.

3. Если применяется проверка подлинности Kerberos в Active Directory, убедитесь в том, что при использовании команды `klist` в Windows отображается кэшированный билет.
    - Войдите на компьютер PolyBase и выполните в командной строке команды `klist` и `klist tgt`, чтобы проверить правильность центра распространения ключей, имени пользователя и типов шифрования.

4. Если центр распространения ключей поддерживает только алгоритм AES256, должны быть установлены [файлы политик JCE](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

## <a name="see-also"></a>См. также раздел
[Integrating PolyBase with Cloudera using Active Directory Authentication](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication) (Интеграция PolyBase и Cloudera с помощью аутентификации Active Directory)  
[Руководство Cloudera по настройке Kerberos для CDH](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html)  
[Руководство Hortonworks по настройке Kerberos для HDP](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html)  
[Устранение неполадок c PolyBase](polybase-troubleshooting.md)