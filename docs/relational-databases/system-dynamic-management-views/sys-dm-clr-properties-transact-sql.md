---
description: sys.dm_clr_properties (Transact-SQL)
title: sys.dm_clr_properties (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34dfbfbecf82df79520fc5a530bcb8c18c544c77
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095293"
---
# <a name="sysdm_clr_properties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Возвращает строку для каждого свойства, связанного с интеграцией со средой CLR программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], включая версию и состояние внутрипроцессной среды CLR. Размещенная среда CLR инициализируется путем запуска инструкций [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md), [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md)или [DROP ASSEMBLY](../../t-sql/statements/drop-assembly-transact-sql.md) или путем выполнения любой подпрограммы среды CLR, типа или триггера. Представление **sys.dm_clr_properties** не указывает, включено ли выполнение ПОЛЬЗОВАТЕЛЬСКОГО кода CLR на сервере. Выполнение пользовательского кода CLR включается с помощью хранимой процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) с параметром [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) , установленным в значение 1.  
  
 Представление **sys.dm_clr_properties** содержит столбцы « **имя** » и « **значение** ». Каждая строка в этом представлении содержит подробные сведения о свойстве внутрипроцессной среды CLR. Это представление используется для сбора информации о внутрипроцессной среде CLR, например о каталоге установки, версии и текущем состоянии этой среды. Оно помогает определить, вызвана ли неработоспособность кода интеграции со средой CLR проблемами установки CLR на серверном компьютере.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Имя свойства.|  
|**value**|**nvarchar(128)**|Значение свойства.|  
  
## <a name="properties"></a>Свойства  
 Свойство **Directory** указывает каталог, на который была установлена .NET Framework на сервере. На серверном компьютере может быть несколько установок платформы .NET Framework, и значение этого свойства определяет установку, которую использует [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Свойство **Version** указывает версию .NET Framework и РАЗМЕЩЕННУЮ среду CLR на сервере.  
  
 Динамическое управляемое представление **sys.dm_clr_properties** может возвращать шесть разных значений для свойства **State** , которое отражает состояние [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] размещенной среды CLR. К ним относятся:  
  
-   Mscoree is not loaded;  
  
-   Mscoree is loaded;  
  
-   Locked CLR version with mscoree;  
  
-   CLR is initialized;  
  
-   CLR initialization permanently failed;  
  
-   CLR is stopped.  
  
 **Библиотека Mscoree не загружена** , а в состояниях " **Mscoree** " отображается ход выполнения размещенной инициализации CLR при запуске сервера и, скорее всего, они не видны.  
  
 **Заблокированная версия CLR с** состоянием Mscoree может увидеть, где не используется РАЗМЕЩЕННАЯ среда CLR, и, таким же, она еще не инициализирована. Размещенная среда CLR инициализируется при первом запуске инструкции DDL (например, [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)) или при выполнении управляемого объекта базы данных.  
  
 **Инициализированное состояние CLR** указывает на то, что РАЗМЕЩЕННАЯ среда CLR была успешно инициализирована. Обратите внимание, что оно не является признаком включения выполнения пользовательского кода CLR. Если выполнение пользовательского кода CLR сначала включено, а затем отключено с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)] хранимой процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) , значение состояния по-прежнему будет **инициализировано в среде CLR**.  
  
 Состояние **безвозвратного сбоя инициализации CLR** указывает на сбой инициализации размещенной среды CLR. Возможной причиной является нехватка памяти, также это может быть результатом сбоя при подтверждении соединения между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и средой CLR. В этом случае будет вызвано сообщение об ошибке 6512 или 6513.  
  
 **Состояние CLR остановлено** , только если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] находится в процессе завершения работы.  
  
## <a name="remarks"></a>Комментарии  
 Свойства и значения этого представления могут измениться в будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из-за улучшений функций интеграции со средой CLR.  
  
## <a name="permissions"></a>Разрешения  
  
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="examples"></a>Примеры  
 В следующем примере происходит получение данных о внутрипроцессной среде CLR:  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные со средой CLR &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
