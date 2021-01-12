---
title: Функции CAST и CONVERT (Transact-SQL) | Документы Майкрософт
description: Справочник по функциям CAST и CONVERT Transact-SQL. Эти функции преобразуют выражения из одного типа данных в другой.
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CAST_TSQL
- CONVERT_TSQL
- CAST
- CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- automatic data type conversion
- varbinary data type
- CONVERT function
- data types [SQL Server], converting
- large value data types
- implicit data type conversions
- image data type, converting
- explicit data type conversions [SQL Server]
- binary [SQL Server], converting
- text data type, converting
- date and time [SQL Server], cast and convert
- truncating conversions
- converting data types [SQL Server], conversion functions
- time zones [SQL Server]
- roundtrip conversions
ms.assetid: a87d0850-c670-4720-9ad5-6f5a22343ea8
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24091fa04d0db4499f03e4722977a34cf70ae5ac
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097360"
---
# <a name="cast-and-convert-transact-sql"></a>Функции CAST и CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эти функции преобразуют выражение одного типа данных в другой.  

## <a name="syntax"></a>Синтаксис  
  
```sqlsyntax 
-- CAST Syntax:  
CAST ( expression AS data_type [ ( length ) ] )  
  
-- CONVERT Syntax:  
CONVERT ( data_type [ ( length ) ] , expression [ , style ] )  
```  

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*expression*  
Любое допустимое [выражение](../../t-sql/language-elements/expressions-transact-sql.md).
  
*data_type*  
Целевой тип данных. Это может быть **xml**, **bigint** и **sql_variant**. Псевдонимы типов данных недопустимы.
  
*length*  
Указываемое дополнительно целое число, обозначающее длину целевого типа данных, который допускает указание длины пользователем. Значение по умолчанию — 30.
  
*style*  
Целочисленное выражение, определяющее, как функция CONVERT преобразует значение аргумента *expression*. Для значения стиля NULL возвращается NULL. Аргумент *data_type* определяет диапазон. 
  
## <a name="return-types"></a>Типы возвращаемых данных
Возвращает значение аргумента *expression*, преобразованное в тип *data_type*.
  
## <a name="date-and-time-styles"></a>Стили даты и времени  
Если аргумент *expression* принадлежит к типу данных даты или времени, аргумент *style* может иметь одно из значений, приведенных в таблице ниже. Другие значения обрабатываются как 0. Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], преобразование типов даты и времени в **datetimeoffset** поддерживается только для стилей 0 и 1. Все другие стили преобразования возвращают ошибку 9809.
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает формат даты в арабском стиле, используя кувейтский алгоритм.
  
|Без века (гг) (<sup>1</sup>)|С веком (гггг)|Standard|Ввод/вывод (<sup>3</sup>)|  
|---|---|--|---|
|-|**0** или **100** (<sup>1</sup>,<sup>2</sup>)|Значение по умолчанию для типа данных datetime и smalldatetime|мес дд гггг чч:мм|  
|**1**|**101**|США|  1 = мм/дд/гг<br /> 101 = мм/дд/гггг|  
|**2**|**102**|ANSI|  2 = гг.мм.дд<br /> 102 = гггг.мм.дд|  
|**3**|**103**|Британский/французский|  3 = дд/мм/гг<br /> 103 = дд/мм/гггг|  
|**4**|**104**|Немецкий|  4 = дд.мм.гг<br /> 104 = дд.мм.гггг|  
|**5**|**105**|Итальянский|  5 = дд-мм-гг<br /> 105 = дд-мм-гггг|  
|**6**|**106** <sup>(1)</sup>|-|  6 = дд мес гг<br /> 106 = дд мм гггг|  
|**7**|**107** <sup>(1)</sup>|-|  7 = Мес дд, гг<br /> 107 = Мес дд, гггг|  
|**8** или **24**|**108**|-|чч:мм:сс|  
|-|**9** или **109** (<sup>1</sup>,<sup>2</sup>)|По умолчанию + миллисекунды|мес дд гггг чч:мм:сс:ммм|  
|**10**|**110**|США| 10 = мм-дд-гг<br /> 110 = мм-дд-гггг|  
|**11**|**111**|Япония| 11 = гг/мм/дд<br /> 111 = гггг/мм/дд|  
|**12**|**112**|ISO| 12 = ггммдд<br /> 112 = ггггммдд|  
|-|**13** или **113** (<sup>1</sup>,<sup>2</sup>)|Европейский по умолчанию + миллисекунды|дд мес гггг чч:мм:сс:ммм (24-часовой формат)|  
|**14**|**114**|-|чч:мм:сс:ммм (24-часовой формат)|  
|-|**20** или **120** (<sup>2</sup>)|Канонический формат ODBC|гггг-мм-дд чч:мм:сс (24-часовой формат)|  
|-|**21** или **25** или **121** (<sup>2</sup>)|Каноническая форма ODBC (с миллисекундами) является значением по умолчанию для времени, даты, datetime2, и datetimeoffset|гггг-мм-дд чч:мм:сс.ммм (24-часовой формат)|  
|**22**|-|США| мм/дд/гг чч:мм:сс AM (или PM)|
|-|**23**|ISO8601|гггг-мм-дд|
|-|**126** (<sup>4</sup>)|ISO8601|гггг-мм-ддТчч:мм:сс.ммм (без пробелов)<br /><br /> **Примечание.** Для равного 0 значения миллисекунд (ммм) значение десятичной части не отображается. Например, значение "2012-11-07T18:26:20.000" отображается как "2012-11-07T18:26:20".| 
|-|**127**(<sup>6, 7</sup>)|ISO8601 с часовым поясом П.|гггг-ММ-ддTчч:мм:сс.fffZ (без пробелов)<br /><br /> **Примечание.** Для равного 0 значения миллисекунд (ммм) значение десятичной части не отображается. Например, значение "2012-11-07T18:26:20.000" будет отображаться как "2012-11-07T18:26:20".|  
|-|**130** (<sup>1,</sup><sup>2</sup>)|Хиджра (<sup>5</sup>)|дд мес гггг чч:ми:сс:мммAM<br /><br /> В этом стиле **мес** является представлением Юникода полного названия месяца в Хиджре. Это значение не отображается правильно при установке SSMS языковой версии "Английский (США)" по умолчанию.|  
|-|**131** (<sup>2</sup>)|Хиджра (<sup>5</sup>)|дд/мм/гггг чч:ми:сс:мммAM|  
  
<sup>1</sup> Эти значения стилей возвращают недетерминированные результаты. Включают в себя все стили "гг" (без номера века) и часть стилей "гггг" (с номером века).
  
<sup>2</sup> Значения по умолчанию (**0** или **100**, **9** или **109**, **13** или **113**, **20** или **120**, **23** и **21** или **25** или **121**) всегда возвращают год с веком (гггг).

<sup>3</sup> Вход при преобразовании в тип **datetime**; выход при преобразовании в символьные данные.

<sup>4</sup> Для использования в формате XML. Для преобразования из **datetime** или **smalldatetime** в символьные данные формат вывода должен быть таким, как описано в предыдущей таблице.

<sup>5</sup> Хиджра — календарная система с несколькими вариантами. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется кувейтский алгоритм.

> [!IMPORTANT]
>  По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] интерпретирует двузначные значения года с пороговым значением 2049. Это означает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] интерпретирует двухзначное значение года 49 как 2049, а двухзначное значение 50 — как 1950. В большинстве клиентских приложений, основанных, в частности, на объектах автоматизации, 2030  год используется в качестве порогового значения. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляет параметр настройки двузначного порогового значения года, с помощью которого можно изменить пороговое значение года, используемое в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это позволяет обеспечить согласованную обработку дат. Рекомендуется использовать четырехзначные года.

<sup>6</sup> Поддерживается только при приведении символьных данных к типу **datetime** или **smalldatetime**. При приведении символьных данных, представляющих только дату или только время, к типам **datetime** и **smalldatetime** неуказанное время устанавливается в 00:00:00.000, а неуказанная дата — в 1900-01-01.
  
<sup>7</sup> Необязательный признак часового пояса **П** используется для упрощения сопоставления XML-значений типа **datetime**, содержащих сведения о часовом поясе, со значениями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа **datetime** без таких сведений. Признак П указывает на часовой пояс UTC-0. Другие часовые пояса обозначаются смещением в формате ЧЧ:ММ в направлении + или –. Например: `2006-12-12T23:45:12-08:00`.
  
При преобразовании в символьные данные из **smalldatetime** стили, включающие секунды или миллисекунды, будут содержать нули в соответствующих позициях. При преобразовании из **datetime** или **smalldatetime** ненужные части даты можно усекать с помощью типа данных **char** или **varchar** соответствующей длины.
  
При преобразовании в тип данных **datetimeoffset** из символьных данных со стилем, включающим время, смещение часового пояса добавляется к результату.
  
## <a name="float-and-real-styles"></a>Стили данных float и real
Если аргумент *expression* принадлежит к типу данных **float** или **real**, аргумент *style* может иметь одно из значений, приведенных в таблице ниже. Другие значения обрабатываются как 0.
  
|Значение|Выходные данные|  
|---|---|
|**0** (по умолчанию)|Не более 6 разрядов. По необходимости используется экспоненциальное представление чисел.|  
|**1**|Всегда 8 разрядов. Всегда используется экспоненциальное представление чисел.|  
|**2**|Всегда 16 разрядов. Всегда используется экспоненциальное представление чисел.|  
|**3**|Всегда 17 разрядов. Используется для преобразования без потери данных. При использовании этого стиля каждое отдельное значение типа float или real гарантированно преобразуется в отдельную строку символов.<br /><br /> **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**126, 128, 129**|Включено для совместимости с прежними версиями. В следующих версиях эти значения могут быть исключены.|  
  
## <a name="money-and-smallmoney-styles"></a>Стили данных money и smallmoney
Если аргумент *expression* принадлежит к типу данных **money** или **smallmoney**, аргумент *style* может иметь одно из значений, приведенных в таблице ниже. Другие значения обрабатываются как 0.
  
|Значение|Выходные данные|  
|---|---|
|**0** (по умолчанию)|Без запятых, разделяющих группы разрядов, с двумя цифрами справа от десятичного разделителя.<br /><br />Пример 4235.98.|  
|**1**|Запятые, разделяющие группы из трех разрядов слева от десятичного разделителя, с двумя цифрами справа от десятичного разделителя.<br /><br />Пример 3,510.92.|  
|**2**|Без запятых, разделяющих группы разрядов, с четырьмя цифрами справа от десятичного разделителя.<br /><br />Пример 4235.9819.|  
|**126**|Эквивалентно стилю 2 при преобразовании в char(n) или varchar(n)|  
  
## <a name="xml-styles"></a>Стили данных XML
Если аргумент *expression* принадлежит к типу данных **xml**, аргумент *style* может иметь одно из значений, приведенных в таблице ниже. Другие значения обрабатываются как 0.
  
|Значение|Выходные данные|  
|---|---|
|**0** (по умолчанию)|Использовать синтаксический разбор по умолчанию, при котором незначащие пробельные символы удаляются, а внутренние подмножества DTD не разрешены.<br /><br />**Примечание.** При преобразовании в тип данных **xml**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обрабатывает незначащие пробелы не так, как это описано в стандарте XML 1.0. Дополнительные сведения см. в статье [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md).|  
|**1**|Сохранять незначащие пробельные символы. Этот параметр стиля задает обработку **xml:space** по умолчанию в соответствии с поведением **xml:space="preserve"** .|  
|**2**|Использовать ограниченную обработку внутреннего подмножества DTD.<br /><br /> При этом для выполнения операций синтаксического анализа без проверки действительности сервер может пользоваться следующей информацией, предоставляемой внутренним подмножеством DTD.<br /><br />   — Применяются атрибуты по умолчанию.<br />   — Ссылки на внутренние сущности разрешаются и раскрываются.<br />   — Проверяется синтаксическая правильность модели содержимого DTD.<br /><br /> Синтаксический анализатор пропускает внешние подмножества DTD. Также не выполняется оценка объявления XML, чтобы определить, имеет ли атрибут **standalone** значение **yes** или **no**. Вместо этого выполняется анализ экземпляра XML как отдельного документа.|  
|**3**|Сохранять незначащие пробельные символы и использовать ограниченную обработку внутреннего подмножества DTD.|  
  
## <a name="binary-styles"></a>Стили двоичных данных
Если аргумент *expression* принадлежит к типу данных **binary(n)** , **char(n)** , **varbinary(n)** или **varchar(n)** , аргумент *style* может иметь одно из значений, приведенных в таблице ниже. При использовании значений стиля, отсутствующих в этой таблице, возвращается ошибка.
  
|Значение|Выходные данные|  
|---|---|
|**0** (по умолчанию)|Преобразует символы ASCII в двоичные байты либо двоичные байты в символы ASCII. Каждый символ или байт преобразуется в соотношении 1:1.<br /><br /> Если параметр *data_type* имеет значение binary, к результату слева добавляются символы 0x.|  
|**1**, **2**|Если параметр *data_type* имеет значение binary, выражение должно быть символьным. Значение аргумента *expression* должно состоять из **четного** числа шестнадцатеричных знаков (0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, a, b, c, d, e, f). Если аргумент *style* имеет значение 1, в качестве первых двух символов выражения обязательно использовать 0x. Если выражение содержит нечетное число символов или использованы недопустимые символы, возникает ошибка.<br /><br /> Если длина преобразованного выражения превышает длину типа данных *data_type*, результат усекается справа.<br /><br /> При использовании значений аргумента *data_type* фиксированной длины, превышающей длину преобразованного результата, к результату справа добавляются нули.<br /><br /> Аргумент *data_type* символьного типа требует двоичного выражения. Каждый двоичный символ преобразуется в два шестнадцатеричных символа. Предположим, длина преобразованного выражения превышает длину типа данных *data_type*. В этом случае оно усекается.<br /><br /> Если для параметра *data_type* используется значение символьного типа фиксированного размера и длина преобразованного результата меньше длины типа данных *data_type*, к преобразованному выражению справа добавляются символы пробела, чтобы сохранить четность числа шестнадцатеричных знаков.<br /><br /> Символы 0x не добавляются слева к преобразованному результату для параметра *style*, равного 2.|  
  
## <a name="implicit-conversions"></a>Неявные преобразования
Для неявных преобразований не требуется указывать функции CAST или CONVERT. Для явных преобразований указывать функции CAST или CONVERT необходимо. На следующей иллюстрации показаны все явные и неявные преобразования типов данных, допустимые для системных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это могут быть типы **bigint**, **sql_variant** и **xml**. При присваивании неявного преобразования из типа **sql_variant** не происходит, но неявное преобразование в тип **sql_variant** производится.
  
> [!TIP]  
> В [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=35834) эта диаграмма представлена в виде PNG-файла, который можно скачать.  
  
![Таблица преобразования типов данных](../../t-sql/data-types/media/lrdatahd.png "Таблица преобразования типов данных")
  
На приведенной выше диаграмме показаны все явные и неявные преобразования, которые допускаются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], однако результирующий тип данных преобразования зависит от выполняемой операции.

-  При явных преобразованиях сам оператор определяет результирующий тип данных.    
-  При неявных преобразованиях операторы назначения, такие как установка значения переменной или вставка значения в столбец, дают в результате тип данных, определенный в объявлении переменной или в определении столбца.    
-  Для операторов сравнения или других выражений результирующий тип данных зависит от правил [приоритета типов данных](../../t-sql/data-types/data-type-precedence-transact-sql.md).

> [!TIP]
> Практический пример [применения приоритета типов данных при преобразованиях](#precedence-example) приводится далее в этом разделе.

При преобразованиях между типом **datetimeoffset** и символьными типами **char**, **nchar**, **nvarchar** и **varchar** преобразованная часть смещения часового пояса должна иметь по два разряда для часов и минут. Пример: -08:00.
  
> [!NOTE]   
> Так как у данных в Юникоде всегда четное число байтов, будьте осторожны при преобразовании значений типа **binary** или **varbinary** в типы данных, поддерживающие Юникод, и наоборот. Например, при следующем преобразовании не будет возвращено шестнадцатеричное значение 41. В данном случае будет получено шестнадцатеричное значение 4100: `SELECT CAST(CAST(0x41 AS nvarchar) AS varbinary)`. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). 
  
## <a name="large-value-data-types"></a>Типы данных больших значений
Типы данных большого объема демонстрируют то же поведение при явных и неявных преобразованиях, что и их аналоги меньшего объема, а именно типы данных **nvarchar**, **varbinary** и **varchar**. Тем не менее необходимо учитывать следующие правила:
-   Преобразование из **image** в **varbinary(max)** и обратно неявное, как и преобразования между **text** и **varchar(max)** , а также **ntext** и **nvarchar(max)** .  
-   Преобразование из типов данных большого объема, например **varchar(max)** , в аналогичный тип данных меньшего объема, например **varchar**, неявное, но если объем данных слишком велик, будет произведено усечение данных до указанной длины конкретного типа данных меньшего объема.  
-   Преобразование из **nvarchar**, **varbinary** или **varchar** в соответствующие им типы данных большого объема выполняется неявно.  
-   Преобразование из типа данных **sql_variant** в типы данных большого объема выполняется явно.  
-   Типы данных большого объема не могут быть преобразованы в тип данных **sql_variant**.  
  
Дополнительные сведения о преобразовании из типа данных **xml** см. в разделе [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="xml-data-type"></a>Тип данных XML
При явном или неявном приведении типа данных **xml** к строковому или двоичному типу данных содержимое типа данных **xml** сериализуется согласно набору определенных правил. Сведения об этих правилах см. в разделе [Определение сериализации XML-данных](../../relational-databases/xml/define-the-serialization-of-xml-data.md). Дополнительные сведения о преобразовании других типов данных в тип данных **xml** см. в разделе [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="text-and-image-data-types"></a>Типы данных text и image
Для типов данных **text** и **image** автоматическое преобразование типов не поддерживается. Можно явно преобразовать **text** в символьные данные, а **image** — в **binary** или **varbinary**, но длиной не более 8000 байт. Если вы попробуете произвести неверное преобразование, например преобразовать символьное выражение, содержащее буквы, в **int**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вернет ошибку.
  
## <a name="output-collation"></a>Параметры сортировки выходных данных  
Если входные и выходные данные функций CAST и CONVERT представляют собой символьные строки, у выходных данных будут те же параметры сортировки, что и у входных. Если входные данные не символьная строка, выходным назначаются параметры сортировки по умолчанию для этой базы данных и присваивается метка о том, что этот режим используется принудительно. Дополнительные сведения см. в статье [Очередность параметров сортировки (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).
  
Чтобы назначить выходным данным другие параметры сортировки, примените предложение COLLATE к результирующему выражению функции CAST или CONVERT. Пример:
  
`SELECT CAST('abc' AS varchar(5)) COLLATE French_CS_AS`
  
## <a name="truncating-and-rounding-results"></a>Усечение и округление результатов
При преобразовании символьных или двоичных выражений (**binary**, **char**, **nchar**, **nvarchar**, **varbinary** или **varchar**) в выражение другого типа данных операция преобразования может усекать выходные данные, отображать их лишь частично или возвращать ошибку. Это происходит в тех случаях, когда результат имеет слишком малую длину для отображения. Результаты преобразований в **binary**, **char**, **nchar**, **nvarchar**, **varbinary** или **varchar** усекаются всегда, за исключением случаев, перечисленных в таблице ниже.
  
|Из типа данных|В тип данных|Результат|  
|---|---|---|
|**int**, **smallint** или **tinyint**|**char**|*|  
||**varchar**|*|  
||**nchar**|E|  
||**nvarchar**|E|  
|**money**, **smallmoney**, **numeric**, **decimal**, **float** или **real**|**char**|E|  
||**varchar**|E|  
||**nchar**|E|  
||**nvarchar**|E|  
  
\* = результат слишком мал для отображения.<br /><br />О = ошибка, так как длина результата слишком мала для отображения.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] гарантирует получение одинаковых результатов в разных версиях только для обратимых преобразований, то есть таких, когда данные преобразуются из исходного типа данных, а затем опять в него. В следующем примере показано обратимое преобразование:
  
```sql
DECLARE @myval DECIMAL (5, 2);  
SET @myval = 193.57;  
SELECT CAST(CAST(@myval AS VARBINARY(20)) AS DECIMAL(10,5));  
-- Or, using CONVERT  
SELECT CONVERT(DECIMAL(10,5), CONVERT(VARBINARY(20), @myval));  
```  
  
> [!WARNING]  
> Не пытайтесь составлять данные типа **binary**, а затем преобразовывать их в данные категории числового типа. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не гарантирует, что результат преобразования типа данных **decimal** или **numeric** в **binary** будет одинаковым в разных версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
В следующем примере показано результирующее выражение, которое слишком мало для отображения:
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, SUBSTRING(p.Title, 1, 25) AS Title,
    CAST(e.SickLeaveHours AS char(1)) AS [Sick Leave]  
FROM HumanResources.Employee e JOIN Person.Person p 
    ON e.BusinessEntityID = p.BusinessEntityID  
WHERE NOT e.BusinessEntityID > 5;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```   
FirstName   LastName      Title   Sick Leave
---------   ------------- ------- --------`
Ken         Sanchez       NULL   *
Terri       Duffy         NULL   *
Roberto     Tamburello    NULL   *
Rob         Walters       NULL   *
Gail        Erickson      Ms.    *

(5 row(s) affected)  
```
  
При преобразовании между типами данных с разными длинами дробных частей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может усекать или округлять результат. В следующей таблице описано это поведение.
  
|От|Чтобы|Поведение|  
|---|---|---|
|**numeric**|**numeric**|Round|  
|**numeric**|**int**|Truncate|  
|**numeric**|**money**|Round|  
|**money**|**int**|Round|  
|**money**|**numeric**|Round|  
|**float**|**int**|Truncate|  
|**float**|**numeric**|Round<br /><br /> Точность преобразования значений **float**, которые используют экспоненциальное представление, в **decimal** или **numeric** ограничена только 17 знаками. Любое значение с точностью, превышающей 17 знаков, округляется до нуля.|  
|**float**|**datetime**|Round|  
|**datetime**|**int**|Round|  
  
Например, значения 10,6496 и –10,6496 могут усекаться или округляться при преобразовании в тип **int** или **numeric**:
  
```sql
SELECT  CAST(10.6496 AS INT) as trunc1,
        CAST(-10.6496 AS INT) as trunc2,
        CAST(10.6496 AS NUMERIC) as round1,
        CAST(-10.6496 AS NUMERIC) as round2;
 ```
Результаты запроса показаны в приведенной ниже таблице.
 
|trunc1|trunc2|round1|round2|
|---|---|---|---|
|10|–10|11|-11|
 
При преобразовании к типам данных, у которых дробная часть короче, чем у исходного типа, значение округляется. Например, это преобразование возвращает `$10.3497`:
  
`SELECT CAST(10.3496847 AS money);`
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке при попытке преобразовать нечисловые данные типа **char**, **nchar**, **nvarchar** или **varchar** в тип **decimal**, **float**, **int** или **numeric**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также возвращает сообщение об ошибке при попытке преобразования пустой строки (" ") в тип **numeric** или **decimal**.
  
## <a name="certain-datetime-conversions-are-nondeterministic"></a>Некоторые преобразования типа данных даты и времени являются недетерминированными

В следующих стилях преобразование строк в тип datetime недетерминировано:
  
- Все стили меньше 100<sup>1</sup>
- 106  
- 107
- 109
- 113
- 130  
  
<sup>1</sup> За исключением стилей 20 и 21.

Дополнительные сведения см. в статье [Недетерминированное преобразование строк дат литералов в значения DATE](../data-types/nondeterministic-convert-date-literals.md).

## <a name="supplementary-characters-surrogate-pairs"></a>Дополнительные символы (суррогатные пары)
Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] при использовании параметров сортировки дополнительных символов (SC) операция CAST из типа **nchar** или **nvarchar** в тип **nchar** или **nvarchar** с меньшей длиной не будет выполнять усечение внутри суррогатной пары. Вместо этого усечение происходит перед дополнительным символом. Например, выполнение следующего фрагмента кода приведет к тому, что в `@x` останется лишь `'ab'`. Места недостаточно для размещения дополнительного символа.
  
```sql
DECLARE @x NVARCHAR(10) = 'ab' + NCHAR(0x10000);  
SELECT CAST (@x AS NVARCHAR(3));  
```  
  
При использовании параметров сортировки SC поведение `CONVERT` аналогично `CAST`. Дополнительные сведения см. в статье [Поддержка параметров сортировки и Юникода — дополнительные символы](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters).
  
## <a name="compatibility-support"></a>Поддержка совместимости
В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используемый по умолчанию стиль для операций CAST и CONVERT с типами данных **time** и **datetime2** — 121, кроме случая, когда любой из типов используется в выражении вычисляемого столбца. Для вычисляемых столбцов используемый по умолчанию стиль — 0. Это поведение влияет на вычисляемые столбцы при их создании и использовании в запросах с автоматической параметризацией, а также при использовании в определениях ограничений.
  
При [уровне совместимости](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) 110 и выше стиль по умолчанию для операций CAST и CONVERT с типами данных **time** и **datetime2** всегда имеет значение 121. Если запрос основан на прежнем поведении, следует использовать уровень совместимости ниже 110 либо явно задать в затрагиваемом запросе стиль 0.

|Значение [уровня совместимости](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)|Стиль по умолчанию для операций CAST и CONVERT<sup>1</sup>|Стиль по умолчанию для вычисляемого столбца|  
|------------|------------|------------|
|< **110**|121|0|  
|> = **110**|121|121|  

<sup>1</sup> Кроме вычисляемых столбцов

Обновление базы данных до уровня совместимости 110 и выше не приведет к изменению пользовательских данных, сохраненных на диске. Следует исправить эти данных соответствующим образом вручную. Например, если бы вы использовали предложение SELECT INTO для создания таблицы на основе источника, содержащего описанное выше выражение вычисляемого столбца, то сохранялись бы данные (благодаря стилю 0), а не само определение вычисляемого столбца. В таком случае необходимо вручную обновлять эти данные в соответствии со стилем 121.
  
## <a name="examples"></a><a name="BKMK_examples"></a> Примеры  
  
### <a name="a-using-both-cast-and-convert"></a>A. Использование функций CAST и CONVERT  
В этих примерах получаются имена продуктов, у которых первая цифра цены — `3`, для чего их значения `ListPrice` преобразовываются к `int`.
  
```sql
-- Use CAST  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CAST(ListPrice AS int) LIKE '33%';  
GO  
  
-- Use CONVERT.  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CONVERT(int, ListPrice) LIKE '33%';  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Образец результирующего набора одинаков для операций CAST и CONVERT. 

```
ProductName                    ListPrice
------------------------------ ---------------------
LL Road Frame - Black, 58      337.22
LL Road Frame - Black, 60      337.22
LL Road Frame - Black, 62      337.22
LL Road Frame - Red, 44        337.22
LL Road Frame - Red, 48        337.22
LL Road Frame - Red, 52        337.22
LL Road Frame - Red, 58        337.22
LL Road Frame - Red, 60        337.22
LL Road Frame - Red, 62        337.22
LL Road Frame - Black, 44      337.22
LL Road Frame - Black, 48      337.22
LL Road Frame - Black, 52      337.22
Mountain-100 Black, 38         3374.99
Mountain-100 Black, 42         3374.99
Mountain-100 Black, 44         3374.99
Mountain-100 Black, 48         3374.99
HL Road Front Wheel            330.06
LL Touring Frame - Yellow, 62  333.42
LL Touring Frame - Blue, 50    333.42
LL Touring Frame - Blue, 54    333.42
LL Touring Frame - Blue, 58    333.42
LL Touring Frame - Blue, 62    333.42
LL Touring Frame - Yellow, 44  333.42
LL Touring Frame - Yellow, 50  333.42
LL Touring Frame - Yellow, 54  333.42
LL Touring Frame - Yellow, 58  333.42
LL Touring Frame - Blue, 44    333.42
HL Road Tire                   32.60

(28 rows affected)
```
  
### <a name="b-using-cast-with-arithmetic-operators"></a>Б. Использование функции CAST с арифметическими операторами  
В этом примере вычисляется столбец значений (`Computed`) путем деления суммарных продаж за год (`SalesYTD`) на проценты комиссионных (`CommissionPCT`). Это значение округляется до ближайшего целого числа, после чего с помощью функции CAST приводится к типу данных `int`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT CAST(ROUND(SalesYTD/CommissionPCT, 0) AS INT) AS Computed  
FROM Sales.SalesPerson   
WHERE CommissionPCT != 0;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
Computed
------
379753754
346698349
257144242
176493899
281101272
0  
301872549
212623750
298948202
250784119
239246890
101664220
124511336
97688107

(14 row(s) affected)  
```  
  
### <a name="c-using-cast-to-concatenate"></a>В. Использование функции CAST для объединения строк  
В этом примере несимвольные выражения сцепляются с помощью функции CAST. В этом примере используется база данных AdventureWorksDW.
  
```sql
SELECT 'The list price is ' + CAST(ListPrice AS VARCHAR(12)) AS ListPrice  
FROM dbo.DimProduct  
WHERE ListPrice BETWEEN 350.00 AND 400.00;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ListPrice
------------------------
The list price is 357.06
The list price is 364.09
The list price is 364.09
The list price is 364.09
The list price is 364.09  
```  
  
### <a name="d-using-cast-to-produce-more-readable-text"></a>Г. Использование функции CAST для получения удобочитаемого текста  
В этом примере функция CAST используется в списке SELECT для преобразования значений столбца `Name` к значениям типа **char(10)** . В этом примере используется база данных AdventureWorksDW.
  
```sql
SELECT DISTINCT CAST(EnglishProductName AS CHAR(10)) AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE EnglishProductName LIKE 'Long-Sleeve Logo Jersey, M';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Name        ListPrice
----------  ---------
Long-Sleev  31.2437
Long-Sleev  32.4935
Long-Sleev  49.99  
```  
  
### <a name="e-using-cast-with-the-like-clause"></a>Д. Использование функции CAST с предложением LIKE  
В этом примере значения `SalesYTD` столбца `money` преобразуются в тип данных `int`, после чего преобразуются в тип данных `char(20)`, что позволяет использовать их в предложении `LIKE`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, s.SalesYTD, s.BusinessEntityID  
FROM Person.Person AS p   
JOIN Sales.SalesPerson AS s   
    ON p.BusinessEntityID = s.BusinessEntityID  
WHERE CAST(CAST(s.SalesYTD AS INT) AS char(20)) LIKE '2%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
FirstName        LastName            SalesYTD         BusinessEntityID
---------------- ------------------- ---------------- -------------
Tsvi             Reiter              2811012.7151      279
Syed             Abbas               219088.8836       288
Rachel           Valdez              2241204.0424      289

(3 row(s) affected)  
```
  
### <a name="f-using-convert-or-cast-with-typed-xml"></a>Е. Использование функции CONVERT или CAST с типизированным XML  
В этих примерах показано использование функции CONVERT для преобразования данных в типизированный XML-код с помощью [столбцов и типа данных XML (SQL Server)](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md).
  
В этом примере строка, содержащая пробельные символы, текст и разметку, преобразуется в типизированный XML, в котором удаляются все незначащие пробельные символы (пробелы, разделяющие узлы):
  
```sql
SELECT CONVERT(XML, '<root><child/></root>')  
```  
  
В этом примере похожая строка, содержащая пробельные символы, текст и разметку, преобразуется в типизированный XML, в котором сохраняются все незначащие пробельные символы (пробелы, разделяющие узлы):
  
```sql
SELECT CONVERT(XML, '<root>          <child/>         </root>', 1)  
```  
  
В этом примере строка, содержащая пробельные символы, текст и разметку, приводится к типизированному XML:
  
```sql
SELECT CAST('<Name><FName>Carol</FName><LName>Elliot</LName></Name>'  AS XML)  
```  
  
Дополнительные примеры см. в статье [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md).
  
### <a name="g-using-cast-and-convert-with-datetime-data"></a>Ж. Использование функций CAST и CONVERT с данными типа datetime  
Начиная со значений `GETDATE()` этот пример показывает текущие дату и время, использует функцию `CAST` для изменения текущей даты и времени в символьный тип данных и затем использует `CONVERT` для отображения даты и времени в формате `ISO 8601`.
  
```sql
SELECT   
   GETDATE() AS UnconvertedDateTime,  
   CAST(GETDATE() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(nvarchar(30), GETDATE(), 126) AS UsingConvertTo_ISO8601  ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast              UsingConvertTo_ISO8601
----------------------- ---------------------- ------------------------------
2006-04-18 09:58:04.570 Apr 18 2006  9:58AM    2006-04-18T09:58:04.570

(1 row(s) affected)  
```
  
Этот пример — частичная противоположность предыдущему примеру. Этот пример отображает дату и время в виде символьных данных, использует функцию `CAST` для изменения символьных данных в тип данных `datetime`, а затем функцию `CONVERT` для изменения символьных данных в тип данных `datetime`.
  
```sql
SELECT   
   '2006-04-25T15:50:59.997' AS UnconvertedText,  
   CAST('2006-04-25T15:50:59.997' AS datetime) AS UsingCast,  
   CONVERT(datetime, '2006-04-25T15:50:59.997', 126) AS UsingConvertFrom_ISO8601 ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2006-04-25T15:50:59.997 2006-04-25 15:50:59.997 2006-04-25 15:50:59.997

(1 row(s) affected)  
```
  
### <a name="h-using-convert-with-binary-and-character-data"></a>З. Использование функции CONVERT с двоичными и символьными данными  
В этих примерах показаны результаты преобразования двоичных и символьных данных с использованием различных стилей.
  
```sql
--Convert the binary value 0x4E616d65 to a character value.  
SELECT CONVERT(CHAR(8), 0x4E616d65, 0) AS [Style 0, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, binary to character
----------------------------
Name  

(1 row(s) affected)  
```
 
В этом примере показано, что стиль 1 может привести к принудительному усечению результата. К этому приводит наличие символов 0x в результате.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 1) AS [Style 1, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, binary to character
------------------------------
0x4E616D

(1 row(s) affected)  
```  
 
В этом примере показано, что стиль 2 не вызывает усечение результата, так как символы 0x не включены в результат.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 2) AS [Style 2, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, binary to character
------------------------------
4E616D65

(1 row(s) affected)  
```
  
Преобразуйте символьное значение "Name" в двоичное значение.  
```sql
SELECT CONVERT(BINARY(8), 'Name', 0) AS [Style 0, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, character to binary
----------------------------
0x4E616D6500000000

(1 row(s) affected)  
```
  
```sql
SELECT CONVERT(BINARY(4), '0x4E616D65', 1) AS [Style 1, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, character to binary
---------------------------- 
0x4E616D65

(1 row(s) affected)  
```  

```sql
SELECT CONVERT(BINARY(4), '4E616D65', 2) AS [Style 2, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, character to binary  
----------------------------------  
0x4E616D65

(1 row(s) affected)  
```  
  
### <a name="i-converting-date-and-time-data-types"></a>И. Конвертирование типов данных даты и времени  
Этот пример демонстрирует преобразование типов данных date, time и datetime.
  
```sql
DECLARE @d1 DATE, @t1 TIME, @dt1 DATETIME;  
SET @d1 = GETDATE();  
SET @t1 = GETDATE();  
SET @dt1 = GETDATE();  
SET @d1 = GETDATE();  
-- When converting date to datetime the minutes portion becomes zero.  
SELECT @d1 AS [DATE], CAST (@d1 AS DATETIME) AS [date as datetime];  
-- When converting time to datetime the date portion becomes zero   
-- which converts to January 1, 1900.  
SELECT @t1 AS [TIME], CAST (@t1 AS DATETIME) AS [time as datetime];  
-- When converting datetime to date or time non-applicable portion is dropped.  
SELECT @dt1 AS [DATETIME], CAST (@dt1 AS DATE) AS [datetime as date], 
   CAST (@dt1 AS TIME) AS [datetime as time];  
```  

### <a name="j-using-convert-with-datetime-data-in-different-formats"></a>К. Использование функции CONVERT с данными типа datetime в разных форматах  
Начиная со значений `GETDATE()` этот пример использует функцию `CONVERT` для отображения всех стилей даты и времени в разделе [Стили даты и времени](#date-and-time-styles) этой статьи.

|Номер формата|Пример запроса|Пример результата|
|--------|------------------------------------|------------------|
|0|`SELECT CONVERT(NVARCHAR, GETDATE(), 0)`|Авг 23 2019  1:39PM|
|1|`SELECT CONVERT(NVARCHAR, GETDATE(), 1)`|08/23/19|
|2|`SELECT CONVERT(NVARCHAR, GETDATE(), 2)`|19.08.23|
|3|`SELECT CONVERT(NVARCHAR, GETDATE(), 3)`|23/08/19|
|4|`SELECT CONVERT(NVARCHAR, GETDATE(), 4)`|23.08.19|
|5|`SELECT CONVERT(NVARCHAR, GETDATE(), 5)`|23-08-19|
|6|`SELECT CONVERT(NVARCHAR, GETDATE(), 6)`|23 авг 19|
|7|`SELECT CONVERT(NVARCHAR, GETDATE(), 7)`|Авг 23, 19|
|8 или 24 или 108|`SELECT CONVERT(NVARCHAR, GETDATE(), 8)`|13:39:17|
|9 или 109|`SELECT CONVERT(NVARCHAR, GETDATE(), 9)`|Авг 23 2019  1:39:17:090PM|
|10|`SELECT CONVERT(NVARCHAR, GETDATE(), 10)`|08-23-19|
|11|`SELECT CONVERT(NVARCHAR, GETDATE(), 11)`|19/08/23|
|12|`SELECT CONVERT(NVARCHAR, GETDATE(), 12)`|190823|
|13 или 113|`SELECT CONVERT(NVARCHAR, GETDATE(), 13)`|23 авг 2019 13:39:17:090|
|14 или 114|`SELECT CONVERT(NVARCHAR, GETDATE(), 14)`|13:39:17:090|
|20 или 120|`SELECT CONVERT(NVARCHAR, GETDATE(), 20)`|2019-08-23 13:39:17|
|21 или 25 или 121|`SELECT CONVERT(NVARCHAR, GETDATE(), 21)`|2019-08-23 13:39:17.090|
|22|`SELECT CONVERT(NVARCHAR, GETDATE(), 22)`|08/23/19  1:39:17 PM|
|23|`SELECT CONVERT(NVARCHAR, GETDATE(), 23)`|2019-08-23|
|101|`SELECT CONVERT(NVARCHAR, GETDATE(), 101)`|08/23/2019|
|102|`SELECT CONVERT(NVARCHAR, GETDATE(), 102)`|2019.08.23|
|103|`SELECT CONVERT(NVARCHAR, GETDATE(), 103)`|23/08/2019|
|104|`SELECT CONVERT(NVARCHAR, GETDATE(), 104)`|23.08.2019|
|105|`SELECT CONVERT(NVARCHAR, GETDATE(), 105)`|23-08-2019|
|106|`SELECT CONVERT(NVARCHAR, GETDATE(), 106)`|23 авг 2019|
|107|`SELECT CONVERT(NVARCHAR, GETDATE(), 107)`|Авг 23, 2019|
|110|`SELECT CONVERT(NVARCHAR, GETDATE(), 110)`|08-23-2019|
|111|`SELECT CONVERT(NVARCHAR, GETDATE(), 111)`|2019/08/23|
|112|`SELECT CONVERT(NVARCHAR, GETDATE(), 112)`|20190823|
|113|`SELECT CONVERT(NVARCHAR, GETDATE(), 113)`|23 авг 2019 13:39:17.090|
|120|`SELECT CONVERT(NVARCHAR, GETDATE(), 120)`|2019-08-23 13:39:17|
|121|`SELECT CONVERT(NVARCHAR, GETDATE(), 121)`|2019-08-23 13:39:17.090|
|126|`SELECT CONVERT(NVARCHAR, GETDATE(), 126)`|2019-08-23T13:39:17.090|
|127|`SELECT CONVERT(NVARCHAR, GETDATE(), 127)`|2019-08-23T13:39:17.090|
|130|`SELECT CONVERT(NVARCHAR, GETDATE(), 130)`|22 ذو الحجة 1440  1:39:17.090P|
|131|`SELECT CONVERT(NVARCHAR, GETDATE(), 131)`|22/12/1440  1:39:17.090PM|

### <a name="k-effects-of-data-type-precedence-in-allowed-conversions"></a><a name="precedence-example"></a> K. Результат применения приоритета типов данных при допустимых преобразованиях  
В приведенном ниже примере определяется переменная типа VARCHAR, ей присваивается значение, а затем выбирается объединение переменной со строкой.

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.' AS Result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Result
-----------------------
1 is a string.
```  

Значение 1 типа int было преобразовано в тип VARCHAR.

В этом примере показан похожий запрос с переменной типа int:

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.' AS Result
```

В этом случае инструкция SELECT выдает следующую ошибку:

```
Msg 245, Level 16, State 1, Line 3
Conversion failed when converting the varchar value ' is not a string.' to data type int.
```

Чтобы вычислить выражение `@notastring + ' is not a string.'`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо следовать правилам приоритета типов данных для выполнения неявного преобразования перед вычислением результата выражения. Так как int имеет более высокий приоритет, чем VARCHAR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пытается преобразовать строку в целое число, и операция завершается ошибкой, так как эта строка не может быть преобразована в целое число. 

Если предоставить строку, которую можно преобразовать, инструкция завершается успешно, как показано в следующем примере:

```SQL
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

В этом случае строка `'1'` может быть преобразована в целочисленное значение 1, поэтому инструкция SELECT возвращает значение 2. Если предоставленные типы данных являются целочисленными, оператор + становится математическим оператором сложения, а не объединения строк.

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="l-using-cast-and-convert"></a>М. Использование функций CAST и CONVERT  
В этом примере извлекаются имена продуктов, у которых первая цифра цены — `3`, а затем их значения `ListPrice` преобразовываются в **int**. В нем используется база данных `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(ListPrice AS int) LIKE '3%';  
```  
  
В этом примере показан тот же запрос, в котором вместо функции CAST используется CONVERT. В нем используется база данных `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CONVERT(INT, ListPrice) LIKE '3%';  
```  
  
### <a name="m-using-cast-with-arithmetic-operators"></a>Н. Использование функции CAST с арифметическими операторами  
В этом примере вычисляется отдельное значение столбца путем деления цены единицы товара (`UnitPrice`) на процент скидки (`UnitPriceDiscountPct`). После округления до ближайшего целого числа результат преобразуется к типу данных `int`. В этом примере используется база данных `AdventureWorksDW2016`.
  
```sql
SELECT ProductKey, UnitPrice,UnitPriceDiscountPct,  
       CAST(ROUND (UnitPrice*UnitPriceDiscountPct,0) AS int) AS DiscountPrice  
FROM dbo.FactResellerSales  
WHERE SalesOrderNumber = 'SO47355'   
      AND UnitPriceDiscountPct > .02;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ProductKey  UnitPrice  UnitPriceDiscountPct  DiscountPrice
----------  ---------  --------------------  -------------
323         430.6445   0.05                  22
213         18.5043    0.05                  1
456         37.4950    0.10                  4
456         37.4950    0.10                  4
216         18.5043    0.05                  1  
```  
  
### <a name="n-using-cast-with-the-like-clause"></a>О. Использование функции CAST с предложением LIKE  
В этом примере столбец `ListPrice` типа **money** преобразуется в тип **int**, а затем в тип **char(20)** так, чтобы его можно было использовать в предложении LIKE. В этом примере используется база данных `AdventureWorksDW2016`.  
  
```sql
SELECT EnglishProductName AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(CAST(ListPrice AS INT) AS CHAR(20)) LIKE '2%';  
```  
  
### <a name="o-using-cast-and-convert-with-datetime-data"></a>П. Использование функций CAST и CONVERT с данными типа datetime  
В этом примере отображаются текущие дата и время, используется функция CAST для преобразования текущей даты и времени в символьный тип данных, а затем функция CONVERT для отображения даты и времени в формате ISO 8601. В этом примере используется база данных `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)  
   SYSDATETIME() AS UnconvertedDateTime,  
   CAST(SYSDATETIME() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(NVARCHAR(30), SYSDATETIME(), 126) AS UsingConvertTo_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast                     UsingConvertTo_ISO8601  
---------------------   ---------------------------   ---------------------------  
07/20/2010 1:44:31 PM   2010-07-20 13:44:31.5879025   2010-07-20T13:44:31.5879025  
```  
  
Этот пример — частичная противоположность предыдущему примеру. В этом примере отображаются дата и время в виде символьных данных, используется функция CAST для преобразования символьных данных в тип **datetime**, а затем функция CONVERT для преобразования символьных данных в тип **datetime**. В этом примере используется база данных `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)   
   '2010-07-25T13:50:38.544' AS UnconvertedText,  
CAST('2010-07-25T13:50:38.544' AS DATETIME) AS UsingCast,  
   CONVERT(DATETIME, '2010-07-25T13:50:38.544', 126) AS UsingConvertFrom_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2010-07-25T13:50:38.544 07/25/2010 1:50:38 PM   07/25/2010 1:50:38 PM  
```  

## <a name="see-also"></a>См. также раздел
[Приоритет типов данных &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)       
[Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md)     
[FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)      
[STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)     
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)      
[Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)      
[Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)      
[Написание инструкций Transact-SQL, адаптированных к международному использованию](../../relational-databases/collations/write-international-transact-sql-statements.md)       
  
