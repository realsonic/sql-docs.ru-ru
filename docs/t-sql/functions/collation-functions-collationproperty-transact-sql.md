---
description: Функции параметров сортировки — COLLATIONPROPERTY (Transact-SQL)
title: COLLATIONPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLLATIONPROPERTY_TSQL
- COLLATIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], properties
- COLLATIONPROPERTY function
ms.assetid: f5029e74-a1db-4f69-b0f5-5ee920c3311d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b56940c6b0c3f8bacc193a1cfe7149117dc3d20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98089340"
---
# <a name="collation-functions---collationproperty-transact-sql"></a>Функции параметров сортировки — COLLATIONPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает запрошенное свойство указанных параметров сортировки.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
COLLATIONPROPERTY( collation_name , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*collation_name*  
Имена параметров сортировки. Аргумент *collation_name* имеет тип данных **nvarchar(128)** без значения по умолчанию.
  
*property*  
Свойство сортировки. Аргумент *property* имеет тип данных **varchar(128)** и может иметь любое одно из следующих значений.
  
|Имя свойства|Описание|  
|---|---|
|**CodePage**|Кодовая страница параметров сортировки не в Юникоде. Эта кодировка используется для данных типа **varchar**. Сведения о преобразовании этих значений и сопоставлении символов см. в разделах [Приложение Ж. Таблицы сопоставления DBCS и Юникода](/previous-versions/cc194886(v=msdn.10)) и [Приложение З. Кодовые страницы](/previous-versions/cc195051(v=msdn.10)).<br /><br />Базовый тип данных: **int**|  
|**LCID**|Код языка Windows для параметров сортировки. Это язык и региональные параметры, используемые для правил сортировки и сравнения. Сведения о преобразовании этих значений см. в статье [Структура кода языка](/openspecs/windows_protocols/ms-lcid/63d3d639-7fd2-4afb-abbe-0d5b5551eef8) (сначала их необходимо преобразовать в тип **varbinary**).<br /><br />Базовый тип данных: **int**|  
|**ComparisonStyle**|Стиль сравнения Windows для параметров сортировки. Возвращает значение 0 для двоичных параметров сортировки (как (\_BIN), так и (\_BIN2)), а также если учитываются все свойства ((\_CS\_AS\_KS\_WS), (\_CS\_AS\_KS\_WS\_SC) и (\_CS\_AS\_KS\_WS\_VSS)). Значения битовой маски:<br /><br /> Игнорировать регистр: 1<br /><br /> Не учитывать диакритические знаки: 2<br /><br /> Не учитывать тип японской азбуки: 65536<br /><br /> Не учитывать ширину: 131072<br /><br /> Примечание. Хотя параметр variation-selector-sensitive (\_VSS) влияет на то, как производится сравнение, он не представлен в этом значении.<br /><br />Базовый тип данных: **int**|  
|**Version**|Версия параметров сортировки. Возвращает значение в диапазоне от 0 до 3.<br /><br /> Параметры сортировки, имя которых содержит "140", возвращают значение 3.<br /><br /> Параметры сортировки, имя которых содержит "100", возвращают значение 2.<br /><br /> Параметры сортировки, имя которых содержит "90", возвращают значение 1.<br /><br /> Все остальные параметры сортировки возвращают 0.<br /><br />Базовый тип данных: **tinyint**|  
  
## <a name="return-types"></a>Типы возвращаемых данных
**sql_variant**
  
## <a name="examples"></a>Примеры  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage')  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
## <a name="see-also"></a>См. также
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
  
