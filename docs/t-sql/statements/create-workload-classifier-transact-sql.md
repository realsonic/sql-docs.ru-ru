---
description: Классификатор CREATE WORKLOAD (Transact-SQL)
title: Классификатор CREATE WORKLOAD (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 6a81c3164e183bb8a202e4ac537298c093e6d873
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644026"
---
# <a name="create-workload-classifier-transact-sql"></a>Классификатор CREATE WORKLOAD (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Создает объект классификатора для использования при управлении рабочей нагрузкой.  Классификатор назначает входящие запросы в группу рабочей нагрузки в зависимости от параметров, указанных в инструкции определения классификатора.  Классификаторы вычисляются при каждом запросе.  Если запрос не соответствует классификатору, то он назначается в группу рабочей нагрузки по умолчанию.  Группа рабочей нагрузки по умолчанию — класс ресурсов smallrc.

> [!NOTE]
> Классификатор рабочей нагрузки используется вместо назначения класса ресурсов sp_addrolemember.  После создания классификаторов рабочей нагрузки выполните sp_droprolemember, чтобы удалить все избыточные сопоставления классов ресурсов.

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы

 *classifier_name*  
 Указывает имя, по которому идентифицируется классификатор рабочей нагрузки.  Значение classifier_name — это sysname.  Оно может иметь длину до 128 символов и должно быть уникальным в пределах экземпляра.

 *WORKLOAD_GROUP* =  *'name'*    
 При выполнении условий правилами классификатора имя name сопоставляет запрос с группой рабочей нагрузки.  Значение name — это sysname.  Оно может иметь длину до 128 символов и должно быть допустимым именем группы рабочей нагрузки во время создания классификатора.

 Доступные группы рабочей нагрузки можно найти в представлении каталога [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md).

 *MEMBERNAME* =  *'security_account'*     
 Учетная запись безопасности, используемая для классификации.  Значение security_account — это sysname, которое не имеет значения по умолчанию. Значением security_account может быть пользователь базы данных, роль базы данных, учетная запись Azure Active Directory или группа Azure Active Directory.
 
 *WLM_LABEL*   
 Указывает значение метки, относительно которого может классифицироваться запрос.  Метка является необязательным параметром типа nvarchar(255).  Используйте [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) в запросе для сопоставления конфигурации классификатора.

Пример

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
Указывает значение контекста сеанса, относительно которого может классифицироваться запрос.  Контекст является необязательным параметром типа nvarchar(255).  Используйте [sys.sp_set_session_context](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) с именем переменной, равным `wlm_context`, перед отправкой запроса на задание контекста сеанса.

Пример

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* и *END_TIME*  
Указывает значения времени начала и времени окончания, относительно которых может классифицироваться запрос.  Время начала и время окончания имеют формат ЧЧ:ММ и относятся к часовому поясу UTC.  Время начала и время окончания должны быть указаны вместе.

Пример

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Указывает относительную важность запроса.  Важность принимает одно из следующих значений:

- LOW
- BELOW_NORMAL
- NORMAL (по умолчанию)
- ABOVE_NORMAL
- HIGH.  

Если важность не указана, используется параметр важности группы рабочей нагрузки.  По умолчанию группа рабочей нагрузки имеет среднюю важность (NORMAL).  Важность влияет на порядок, в котором будут запланированы запросы, тем самым обеспечивая приоритетный доступ к ресурсам и блокировке.

## <a name="classification-parameter-weighting"></a>Взвешивание параметров классификации

Запрос может соответствовать нескольким классификаторам.  Параметры классификатора имеют веса.  Для назначения группы рабочей нагрузки и важности используется соответствующий классификатор с более высоким весом.  Весовой коэффициент выглядит следующим образом.

|Параметр классификатора |Вес   |
|---------------------|---------|
|Пользователь                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Рассмотрим следующие конфигурации классификатора.

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

Пользователь `userloginA` настроен для обоих классификаторов.  Если userloginA выполняет запрос с меткой `salesreport` между 18:00 и 7:00 (UTC), запрос будет отнесен к группе рабочей нагрузки wgDashboards с высокой важностью (HIGH).  Можно предположить, что запрос будет отнесен к wgUserQueries с низкой важностью (LOW) для ведения отчетов в нерабочее время, однако вес WLM_LABEL выше, чем у START_TIME/END_TIME.  Весовой коэффициент от classifierA — 80 (64 для пользователя и 16 для WLM_LABEL).  Весовой коэффициент от classifierB — 68 (64 для пользователя и 4 для START_TIME/END_TIME).  В этом случае можно добавить WLM_LABEL в classifierB.

 Дополнительные сведения см. в разделе [Взвешивание рабочих нагрузок](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting).

## <a name="permissions"></a>Разрешения

 Необходимо разрешение CONTROL DATABASE.  
  
## <a name="examples"></a>Примеры

 В следующем примере демонстрируется, как создать классификатор рабочей нагрузки с именем `wgcELTRole`. Он использует группу рабочей нагрузки staticrc20, имя пользователя `ELTRole` и задает уровень важности `above_normal`.

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>См. также:

[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Классификация](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

