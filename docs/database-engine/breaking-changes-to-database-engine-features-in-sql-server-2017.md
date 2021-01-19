---
title: Ядро СУБД. Критические изменения | Документация Майкрософт
titleSuffix: SQL Server 2017
description: Узнайте об изменениях, которые могут нарушать работу приложений, скриптов или функций, основанных на более ранних версиях SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- breaking changes 2017 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: ddb25e9fe9e4463d47b595c77cd6e24c7e0d0976
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171036"
---
# <a name="breaking-changes-to-database-engine-features-in-sssqlv14-md"></a>Критические изменения в функциях ядра СУБД в [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]


  В этом разделе описываются критические изменения в [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)]. Эти изменения могут нарушать работу приложений, скриптов или механизмов, основанных на более ранних версиях [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. При обновлении могут возникнуть следующие проблемы.  
  
## <a name="breaking-changes-in-sssqlv14-md-ssde"></a>Критические изменения в [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)]  
  
-  Среда CLR использует управление доступом для кода (CAS) в .NET Framework, которое больше не поддерживается в качестве границы безопасности. Начиная с [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)][!INCLUDE[ssDE](../includes/ssde-md.md)], появился параметр `sp_configure`, называемый `clr strict security`, для повышения безопасности сборок среды CLR. Параметр clr strict security включен по умолчанию и рассматривает сборки `SAFE` и `EXTERNAL_ACCESS`, как если бы они были помечены `UNSAFE`. Параметр `clr strict security` можно отключить для обеспечения обратной совместимости, но это делать не рекомендуется. Если параметр `clr strict security` отключен, сборки среды CLR, созданные с помощью `PERMISSION_SET = SAFE`, могут получать доступ к внешним системным ресурсам, вызывать неуправляемый код и получать права **sysadmin**. После включения строгой безопасности сборки без подписи загружаться не будут. Кроме того, если в базе данных есть сборки `SAFE` или `EXTERNAL_ACCESS` сборки либо можно выполнить инструкции `RESTORE` или `ATTACH DATABASE`, могут возникать ошибки загрузки сборок.   
  Для загрузки сборок необходимо изменить или повторно создать каждую сборку, чтобы она была подписана сертификатом или асимметричным ключом, имеющим соответствующее имя входа с разрешением `UNSAFE ASSEMBLY` на сервере. Дополнительные сведения см. в статье о параметре [clr strict security](../database-engine/configure-windows/clr-strict-security.md). 
  
-  Алгоритмы MD2, MD4, MD5, SHA и SHA1 отмечены как нерекомендуемые в [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]. В версиях, предшествующих [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], самозаверяющий сертификат создается с помощью SHA1. Начиная с [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] самозаверяющий сертификат создается с помощью SHA2_256.

## <a name="previous-versions"></a><a name="previous-versions"></a> Предыдущие версии  

- [Критические изменения в функциях ядра СУБД в SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)

- [Критические изменения в функциях компонента ядра СУБД в SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?view=sql-server-2014&preserve-view=true#SQL14)

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Архивная документация по очень старым версиям SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>См. также:  
 [Нерекомендуемые функции ядра СУБД в SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Нерекомендуемые функции ядра СУБД в SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Обратная совместимость компонента ядра СУБД SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Уровень совместимости инструкции ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
