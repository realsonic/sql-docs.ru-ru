---
description: ALTER SEQUENCE (Transact-SQL)
title: ALTER SEQUENCE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SEQUENCE_TSQL
- ALTER SEQUENCE
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, altering
- ALTER SEQUENCE statement
ms.assetid: decc0760-029e-4baf-96c9-4a64073df1c2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c32f4e9a6a020df9f7be879eb6e4327bbb9d51f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094695"
---
# <a name="alter-sequence-transact-sql"></a>ALTER SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Изменяет аргументы существующего объекта последовательности. Если последовательность создана с параметром **CACHE**, то в результате ее изменения кэш будет пересоздан.  
  
 Объекты последовательностей создаются с помощью инструкции [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). Последовательность — это целочисленное значение, она может иметь любой тип данных, возвращающий целое число. Тип данных нельзя изменить с помощью инструкции ALTER SEQUENCE. Чтобы изменить тип данных, удалите и заново создайте объект последовательности.  
  
 Последовательность представляет собой определяемый пользователем объект, привязанный к схеме, который формирует последовательность числовых значений в соответствии со спецификацией. Новые значения формируются из последовательности функцией NEXT VALUE FOR. Получить несколько значений из последовательности за один раз можно с помощью функции **sp_sequence_get_range** . Сведения и сценарии использования CREATE SEQUENCE, **sp_sequence_get_range [ и функции NEXT VALUE FOR см. в разделе](../../relational-databases/sequence-numbers/sequence-numbers.md)Порядковые номера**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER SEQUENCE [schema_name. ] sequence_name  
    [ RESTART [ WITH <constant> ] ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE <constant> } | { NO MINVALUE } ]  
    [ { MAXVALUE <constant> } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *sequence_name*  
 Указывает уникальное имя, под которым последовательность известна в базе данных. Тип **sysname**.  
  
 RESTART [ WITH \<constant> ]  
 Следующее значение, возвращаемое объектом последовательности. Если указано, то значение RESTART WITH должно быть целым числом, которое меньше максимального значения объекта последовательности или равно ему и меньше минимального или равно ему. Если значение WITH не указано, то нумерация последовательности начинается заново согласно первоначальным параметрам CREATE SEQUENCE.  
  
 INCREMENT BY \<constant>  
 Значение задает число, на которое увеличивается (или уменьшается, если оно отрицательное) базовое значение объекта последовательности при каждом вызове функции NEXT VALUE FOR. Если значение приращения отрицательно, то объект последовательности изменяется по убыванию, в противном случае — по возрастанию. Приращение не может быть равно 0.  
  
 [ MINVALUE \<constant> | NO MINVALUE ]  
 Указывает граничные значения для объекта последовательности. Если указано NO MINVALUE, то используется минимальное возможное значение для типа данных последовательности.  
  
 [ MAXVALUE \<constant> | NO MAXVALUE  
 Указывает граничные значения для объекта последовательности. Если указано NO MAXVALUE, то используется максимально возможное значение для типа данных последовательности.  
  
 [ CYCLE | NO CYCLE ]  
 Это свойство указывает, перезапускается ли объект последовательности с минимального значения (или максимального для последовательностей по убыванию) или вызывает исключение, когда достигнуто максимальное (или максимальное) значение.  
  
> [!NOTE]  
>  При таком переходе следующим значением будет минимальное или максимальное значение, а не START VALUE последовательности.  
  
 [ CACHE [\<constant> ] | NO CACHE ]  
 Повышает производительность для приложений, использующих объекты последовательностей, сводя к минимуму число операций ввода-вывода, которые требуются для сохранения сформированных значений в системных таблицах.  
  
 Дополнительные сведения о работе кэша см. в разделе [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md).  
  
## <a name="remarks"></a>Remarks  
 Сведения о создании последовательностей и управлении кэшем последовательностей см. в разделе [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md).  
  
 Значение MINVALUE для последовательностей по возрастанию и значение MAXVALUE для последовательностей по убыванию не может быть изменено на значение, которое не допускается значением START WITH последовательности. Чтобы изменить значение MINVALUE последовательности по возрастанию на число больше значения START WITH, либо изменить значение MAXVALUE для последовательности по убыванию, на значение меньше значения START WITH, укажите аргумент RESTART WITH, чтобы перезапустить последовательность с нужной точки, которая находится между минимальным и максимальным значениями.  
  
## <a name="metadata"></a>Метаданные  
 Чтобы получить сведения о последовательностях, запросите представление [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуется разрешение **ALTER** для последовательности или разрешение **ALTER** для схемы. Предоставить разрешение **ALTER** для последовательности можно с помощью инструкции **ALTER ON OBJECT** в следующем формате:  
  
```sql  
GRANT ALTER ON OBJECT::Test.TinySeq TO [AdventureWorks\Larry]  
```  
  
 Владение объектом последовательности может быть передано с помощью инструкции **ALTER AUTHORIZATION**.  
  
### <a name="audit"></a>Аудит  
 Для аудита инструкции **ALTER SEQUENCE** отслеживайте **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Примеры  
 Примеры создания последовательностей и использования функции **NEXT VALUE FOR** для формирования порядковых номеров см. в разделе [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
### <a name="a-altering-a-sequence"></a>A. Изменение последовательности  
 В приведенном ниже примере создается схема Test и последовательность TestSeq с типом данных **int**, имеющим диапазон от 100 до 200. Последовательность начинается со 125 и увеличивается на 25 при каждом создании номера. Поскольку последовательность зациклена, то при превышении максимального значения 200 она перезапускается с минимального значения 100.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.TestSeq  
    AS int   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
GO  
```  
  
 В приведенном ниже примере последовательность TestSeq изменяется для работы в диапазоне от 50 до 200. Последовательность перезапускает нумерацию рядов со 100 с увеличением на 50 при каждом формировании номера.  
  
```sql  
ALTER SEQUENCE Test. TestSeq  
    RESTART WITH 100  
    INCREMENT BY 50  
    MINVALUE 50  
    MAXVALUE 200  
    NO CYCLE  
    NO CACHE  
;  
GO  
```  
  
 Поскольку последовательность не зациклена, функция **NEXT VALUE FOR** выдаст ошибку, когда ее значение превысит 200.  
  
### <a name="b-restarting-a-sequence"></a>Б. Перезапуск последовательности  
 В следующем примере создается последовательность CountBy1. В ней используются значения по умолчанию.  
  
```sql  
CREATE SEQUENCE Test.CountBy1 ;  
```  
  
 Для формирования значения последовательности ее владелец выполняет следующую инструкцию:  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1  
```  
  
 Возвращенное значение -9 223 372 036 854 775 808 — это наименьшее возможное значение для типа данных **bigint**. Владелец решает начать последовательность с 1, однако при ее создании предложение **START WITH** указано не было. Чтобы исправить эту ошибку, владелец выполняет следующую инструкцию.  
  
```sql  
ALTER SEQUENCE Test.CountBy1 RESTART WITH 1 ;  
```  
  
 Затем он снова выполняет следующую инструкцию для формирования последовательного номера.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1;  
```  
  
 Это номер 1, как и ожидалось.  
  
 Последовательность CountBy1 создана со значением по умолчанию NO CYCLE, поэтому она перестанет работать после того, как будет сформирован номер 9 223 372 036 854 775 807. Последующие обращения к этому объекту последовательности будут возвращать ошибку 11728. Следующая инструкция включает зацикливание для объекта и устанавливают его кэш в значение 20.  
  
```sql  
ALTER SEQUENCE Test.CountBy1  
    CYCLE  
    CACHE 20 ; 
```  
  
 Теперь, когда будет достигнут номер 9 223 372 036 854 775 807, следующим номером будет минимально возможный для этого типа данных — -9 223 372 036 854 775 808.  
  
 Владелец обнаружил, что тип данных **bigint** возвращает 8 байт при каждом обращении. Типа данных **int**, занимающего 4 байта, будет достаточно. Однако тип данных последовательности изменить нельзя. Чтобы изменить тип данных на **int**, он должен удалить объект последовательности и создать его заново с нужным типом данных.  
  
## <a name="see-also"></a>См. также:  
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [sp_sequence_get_range (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
