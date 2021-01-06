---
title: Просмотр или изменение свойств сервера (SQL Server) | Документы Майкрософт
description: Узнайте, как использовать SQL Server Management Studio, Transact-SQL или диспетчер SQL Server Configuration Manager для просмотра или изменения свойств экземпляра SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectionproperties.f1
helpviewer_keywords:
- viewing server properties
- server properties [SQL Server]
- displaying server properties
- servers [SQL Server], viewing
- Connection Properties dialog box
ms.assetid: 55f3ac04-5626-4ad2-96bd-a1f1b079659d
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.openlocfilehash: aabe0f1c418440053998ce1a1b388e82d74b06c9
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878867"
---
# <a name="view-or-change-server-properties-sql-server"></a>Просмотр или изменение свойств сервера (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается просмотр и изменение свойств экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]или диспетчера конфигурации SQL Server.  

Шаги зависят от средства.
+ [Среда SQL Server Management Studio](#SSMSProcedure)  
+ [Transact-SQL](#TsqlProcedure)  
+ [Диспетчер конфигурации SQL Server](#PowerShellProcedure)  
    
## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Используя sp_configure, необходимо выполнить инструкцию RECONFIGURE или инструкцию RECONFIGURE WITH OVERRIDE после установки параметра конфигурации. Инструкция RECONFIGURE WITH OVERRIDE обычно употребляется для параметров конфигурации, которые должны использоваться с особой осторожностью. Однако инструкция RECONFIGURE WITH OVERRIDE пригодна для всех параметров конфигурации, и ее можно использовать вместо инструкции RECONFIGURE.  
  
    > [!NOTE]  
    >  Инструкция RECONFIGURE выполняется внутри транзакции. Если какая-либо из операций повторной настройки завершится ошибкой, ни одна из операций повторной настройки не возымеет действия.  
  
-   Некоторые страницы со свойствами предоставляют сведения, полученные через инструментарий управления Windows (WMI). Для отображения этих страниц на компьютере, где работает среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], должен быть установлен инструментарий WMI.  
  
## <a name="server-level-roles"></a><a name="Security"></a> Роли уровня сервера  
  
Дополнительные сведения см. в статье [Роли уровня сервера](../../relational-databases/security/authentication-access/server-level-roles.md).  
  
Разрешения на выполнение хранимой процедуры **sp_configure** без параметров или только с первым параметром по умолчанию предоставляются всем пользователям. Для выполнения процедуры **sp_configure** с обоими параметрами для изменения параметра конфигурации или запуска инструкции RECONFIGURE необходимо иметь разрешение ALTER SETTINGS на уровне сервера. Разрешение ALTER SETTINGS неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin** .  
  
## <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a>SQL Server Management Studio  
  
#### <a name="to-view-or-change-server-properties"></a>Просмотр или изменение свойств сервера  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите пункт **Свойства**.  
  
2.  В диалоговом окне **Свойства сервера** щелкните страницу, чтобы просмотреть или изменить содержащиеся на сервере сведения об этой странице. Некоторые свойства доступны только для чтения.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a>Transact-SQL  
  
### <a name="to-view-server-properties-by-using-the-serverproperty-built-in-function"></a>Просмотр свойств сервера с помощью встроенной функции SERVERPROPERTY  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере используется встроенная функция [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md) в инструкции `SELECT` для возврата сведений о текущем сервере. Этот сценарий полезен, когда на сервере Windows установлено несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и клиенту приходится открывать другое соединение с тем же экземпляром, который используется текущим соединением.  
  
    ```sql  
    SELECT CONVERT( sysname, SERVERPROPERTY('servername'));  
    GO  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysservers-catalog-view"></a>Просмотр свойств сервера с помощью представления каталога sys.servers  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере запрашивается представление каталога [sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md) для получения имени (`name`) и идентификатора (`server_id`) текущего сервера, а также имя поставщика OLE DB (`provider`) для подключения к связанному серверу.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, server_id, provider  
    FROM sys.servers ;   
    GO  
  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysconfigurations-catalog-view"></a>Просмотр свойств сервера с помощью представления каталога sys.configurations  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере запрашивается представление каталога [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) , чтобы вернуть сведения о каждом параметре конфигурации для текущего сервера. В примере возвращается имя (`name`) и описание (`description`) параметра, а также указывается, является ли параметр дополнительным (`is_advanced`).  
  
    ```wmimof  
    USE AdventureWorks2012;   
    GO  
    SELECT name, description, is_advanced  
    FROM sys.configurations ;   
    GO  
  
    ```  
  
### <a name="to-change-a-server-property-by-using-sp_configure"></a>Изменение свойства сервера с помощью процедуры sp_configure  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере показано, как изменить свойство сервера с помощью процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) . В примере значение параметра `fill factor` меняется на `100`. Чтобы изменение вступило в силу, необходимо перезапустить сервер.  
  
```sql  
Use AdventureWorks2012;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'fill factor', 100;  
GO  
RECONFIGURE;  
GO  
```  
  
 Дополнительные сведения см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="sql-server-configuration-manager"></a><a name="PowerShellProcedure"></a>Диспетчер конфигурации SQL Server  
 Некоторые свойства сервера можно просматривать и изменять с помощью диспетчера конфигурации SQL Server. Например, можно просмотреть версию и выпуск экземпляра SQL Server или изменить расположение файлов журнала ошибок. Эти свойства также можно просмотреть, запросив [динамические административные представления и функции динамического управления, относящиеся к серверу](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md).  
  
### <a name="to-view-or-change-server-properties"></a>Просмотр или изменение свойств сервера  
  
1.  В меню **Пуск** последовательно выберите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки** и щелкните **Диспетчер конфигурации SQL Server**.  
  
2.  В **диспетчере конфигурации SQL Server** выберите **Службы SQL Server**.  
  
3.  В области сведений щелкните правой кнопкой мыши **SQL Server (\<**_instancename_**>)** и выберите пункт **Свойства**.  
  
4.  В диалоговом окне **SQL Server (\<**_instancename_**>) Свойства** измените свойства сервера на вкладке **Служба** или на вкладке **Дополнительно** и нажмите кнопку **OK**.  
  
## <a name="restart-after-changes"></a><a name="FollowUp"></a>Перезапуск после изменений

Для некоторых свойств, возможно, понадобится перезапустить сервер, чтобы изменения вступили в силу.  
  
## <a name="next-steps"></a>Дальнейшие действия  
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [Настройка инструментария WMI для отображения состояния сервера в инструментальных средствах SQL Server](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)   
 [Диспетчер конфигурации SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Функции настройки (Transact-SQL)](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Динамические административные представления и функции, связанные с сервером (Transact-SQL)](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
