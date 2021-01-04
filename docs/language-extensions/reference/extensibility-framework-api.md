---
title: API платформы расширений
titleSuffix: SQL Server Language Extensions
description: Платформу расширяемости можно использовать для написания расширений языка программирования для SQL Server. API платформы расширяемости для Microsoft SQL Server — это API, который может использоваться расширением языка для взаимодействия и обмена данными с SQL Server.
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 244a8ce680704b0b25f9d0499e1a7802661d56cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471695"
---
# <a name="extensibility-framework-api-for-sql-server"></a>API платформы расширяемости для SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Платформу расширяемости можно использовать для написания расширений языка программирования для SQL Server. API платформы расширяемости для Microsoft SQL Server — это API, который может использоваться расширением языка для взаимодействия и обмена данными с SQL Server.

Как автор расширения языка вы можете использовать эту ссылку вместе с расширениями языка с открытым кодом, чтобы понять, как использовать API для написания собственных расширений языка. Исходный код для расширения языка можно найти по адресу [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions).

Сведения о синтаксисе и аргументах для всех функций API приведены ниже.

## <a name="return-value"></a>Возвращаемое значение

Все функции возвращают параметр *SQLRETURN*. Если значение отличается от *SQL_SUCCESS*, оно обрабатывается как ошибка и выполнение скрипта останавливается.

## <a name="standard-output"></a>Стандартные выходные данные

Все выходные данные расширения для стандартных потоков вывода или потоков ошибок будут отслеживаться в файле журнала сеанса и со временем будут отслеживаться обратно до SQL Server (как и все, что отображается на вкладке сообщений SSMS).


## <a name="init"></a>Init

Эта функция вызывается только один раз и используется для инициализации среды выполнения. 

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>Аргументы

*ExtensionParams*  
\[Входные данные\] Строка, заканчивающаяся символом NULL, которая содержит значение `PARAMETERS`, указанное в рамках операции [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) или [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md).

*ExtensionParamsLength*  
\[Входные данные\] Длина *ExtensionParams* в байтах (за исключением завершающего символа NULL).

*ExtensionPath*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит абсолютный путь к каталогу установки расширения.

*ExtensionPathLength*  
\[Входные данные\] Длина *ExtensionPath* в байтах (за исключением завершающего символа NULL).

*PublicLibraryPath*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит абсолютный путь к каталогу открытых внешних библиотек для этого внешнего языка.

*PublicLibraryPathLength*  
\[Входные данные\] Длина *PublicLibraryPath* в байтах (за исключением завершающего символа NULL).

*PrivateLibraryPath*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит абсолютный путь к каталогу закрытых внешних библиотек для этого пользователя и этого внешнего языка.

*PrivateLibraryPathLength*  
\[Входные данные\] Длина *PrivateLibraryPath* в байтах (за исключением завершающего символа NULL).

## <a name="initsession"></a>InitSession

Эта функция вызывается один раз для каждого сеанса и инициализирует определенные параметры сеанса.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*Скрипт*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ScriptLength*  
\[Входные данные\] Длина *ScriptScript* в байтах (за исключением завершающего символа NULL).

*InputSchemaColumnsNumber*  
\[Входные данные\] Число столбцов в результирующем наборе из `@input_data_1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ParametersNumber*  
\[Входные данные\] Число входных параметров из `@params` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataName*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит `@input_data_1_name` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataNameLength*  
\[Входные данные\] Длина *InputDataName* в байтах (за исключением завершающего символа NULL).

*OutputDataName*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит `@output_data_1_name` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*OutputDataNameLength*  
\[Входные данные\] Длина *OutputDataName* в байтах (за исключением завершающего символа NULL).

## <a name="initcolumn"></a>InitColumn

Инициализирует сведения для заданного столбца в определенном сеансе.

Эта функция вызывается для каждого столбца результирующего набора из `@input_data_1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Структура столбцов этого результирующего набора будет называться *входной схемой*.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*ColumnNumber*  
\[Входные данные\] Целое число, определяющее индекс этого столбца во входной схеме. Столбцы нумеруются последовательно в порядке возрастания, начиная с 0.

*ColumnName*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит имя столбца.

*ColumnNameLength*  
\[Входные данные\] Длина *ColumnName* в байтах (за исключением завершающего символа NULL).

*DataType*  
\[Входные данные\] Тип C ODBC, идентифицирующий тип данных этого столбца.

*ColumnSize*  
\[Входные данные\] Максимальный размер базовых данных (в байтах) в этом столбце.

Для типов данных SQL_C_CHAR, SQL_C_WCHAR и SQL_C_BINARY значения, превышающие 8000, указывают, что этот столбец представляет объект типа LOB и допускает размер до 2 ГБ.

*DecimalDigits*  
\[Входные данные\] Десятичные разряды базовых данных в этом столбце, как определено в разделе [Десятичные разряды](../../odbc/reference/appendixes/decimal-digits.md).

*Допускает значения NULL*  
\[Входные данные\] Значение, указывающее, может ли этот столбец содержать значения NULL. Возможные значения:

- SQL_NO_NULLS: столбец не может содержать значения NULL.
- SQL_NULLABLE: столбец может содержать значения NULL.

*PartitionByNumber*  
\[Входные данные\] Значение, указывающее индекс этого столбца в последовательности `@input_data_1_partition_by_columns` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Столбцы нумеруются последовательно в порядке возрастания, начиная с 0. Если этот столбец не включен в последовательность, значение равно –1.

*OrderByNumber*  
\[Входные данные\] Значение, указывающее индекс этого столбца в последовательности `@input_data_1_order_by_columns` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Столбцы нумеруются последовательно в порядке возрастания, начиная с 0. Если этот столбец не включен в последовательность, значение равно –1.

## <a name="initparam"></a>InitParam

Инициализирует сведения для заданного входного параметра в определенном сеансе.

Эта функция вызывается для каждого параметра из `@params` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*ParamNumber*  
\[Входные данные\] Целое число, определяющее индекс этого параметра. Параметры нумеруются последовательно в порядке возрастания, начиная с 0.

*ParamName*  
\[Входные данные\] Строка UTF-8, заканчивающаяся символом NULL, которая содержит имя параметра.

*ParamNameLength*  
\[Входные данные\] Длина *ParamName* в байтах (за исключением завершающего символа NULL).

*DataType*  
\[Входные данные\] Тип C ODBC, идентифицирующий тип данных этого параметра.

*ParamSize*  
\[Входные данные\] Максимальный размер базовых данных (в байтах) в этом параметре.

Для типов данных SQL_C_CHAR, SQL_C_WCHAR и SQL_C_BINARY значения, превышающие 8000, указывают, что этот параметр представляет объект типа LOB и допускает размер до 2 ГБ.

*DecimalDigits*  
\[Входные данные\] Десятичные разряды базовых данных в этом параметре, как определено в разделе [Десятичные разряды](../../odbc/reference/appendixes/decimal-digits.md).

*ParamValue*  
\[Входные данные\] Указатель на буфер, содержащий значение параметра.

*StrLen_or_Ind*  
\[Входные данные\] Целочисленное значение, указывающее длину *ParamValue* в байтах или SQL_NULL_DATA, чтобы обозначить, что данные имеют значение NULL.

StrLen_or_Ind\[col\] можно игнорировать, если столбец не допускает значение NULL и не представляет один из следующих типов данных: SQL_C_CHAR, SQL_C_WCHAR и SQL_C_BINARY, SQL_C_NUMERIC или SQL_C_TYPE_TIMESTAMP. В противном случае он указывает на допустимый массив с числом элементов, равным \[RowsNumber\], где каждый элемент содержит данные о длине или индикаторе NULL.

*InputOutputType*  
\[Входные данные\] Тип параметра. Аргумент *InputOutputType* может иметь одно из следующих значений:

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>Execute

Выполняет `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Эту функцию можно вызывать несколько раз. По одному разу для каждого фрагмента потока и для каждого раздела в `@input_data_1_partition_by_columns` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*RowsNumber*  
\[Входные данные\] Количество строк в *Data*.

*Data*  
\[Входные данные\]. Двумерный массив, содержащий результирующий набор `@input_data_1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Общее число столбцов равно значению *InputSchemaColumnsNumber*, полученному в вызове [InitSession](#initsession). Каждый столбец содержит элементы в количестве *RowsNumber*, которые должны интерпретироваться в соответствии с типом столбца из [InitColumn](#initcolumn).

Элементы, отмеченные как NULL, в *StrLen_or_Ind* не обязательно являются допустимыми, и их следует игнорировать.

*StrLen_or_Ind*  
\[Входные данные\]. Двумерный массив, содержащий индикатор длины или NULL для каждого значения в *Data*. Возможные значения для каждой ячейки:

- n, где n > 0. Указывает длину данных в байтах.
- SQL_NULL_DATA, указывающее значение NULL.

Общее число столбцов равно значению *InputSchemaColumnsNumber*, полученному в вызове [InitSession](#initsession). Каждый столбец содержит элементы в количестве *RowsNumber*, которые должны интерпретироваться в соответствии с типом столбца из [InitColumn](#initcolumn).

StrLen_or_Ind\[col\] можно игнорировать, если один столбец не допускает значение NULL и не представляет один из следующих типов данных: SQL_C_CHAR, SQL_C_WCHAR и SQL_C_BINARY, SQL_C_NUMERIC или SQL_C_TYPE_TIMESTAMP. В противном случае он указывает на допустимый массив с числом элементов, равным *RowsNumber*, где каждый элемент содержит данные о длине или индикаторе NULL.

*OutputSchemaColumnsNumber*  
\[Выходные данные\] Указатель на буфер, куда возвращается число столбцов в ожидаемом результирующем наборе `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="getresultcolumn"></a>GetResultColumn

Возвращает сведения об указанном выходном столбце для определенного сеанса.

Эта функция вызывается для каждого столбца результирующего набора из `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
Структура столбцов этого результирующего набора будет называться выходной схемой.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*ColumnNumber*  
\[Входные данные\] Целое число, определяющее индекс этого столбца в выходной схеме. Столбцы нумеруются последовательно в порядке возрастания, начиная с 0.

*DataType*  
\[Выходные данные\] Указатель на буфер, содержащий тип C ODBC, идентифицирующий тип данных этого столбца.

*ColumnSize*  
\[Выходные данные\] Указатель на буфер, содержащий максимальный размер базовых данных (в байтах) в этом столбце.

*DecimalDigits*  
\[Выходные данные\] Указатель на буфер, содержащий десятичные разряды базовых данных в этом столбце, как определено в разделе [Десятичные разряды](../../odbc/reference/appendixes/decimal-digits.md). Если число десятичных разрядов не может быть определено или неприменимо, значение отбрасывается.

*Допускает значения NULL*  
\[Выходные данные\] Указатель на буфер, содержащий значение, которое указывает, может ли этот столбец содержать значения NULL. Возможные значения:

- SQL_NO_NULLS: столбец не может содержать значения NULL.
- SQL_NULLABLE: столбец может содержать значения NULL.

Если передаются другие значения, выполнение останавливается.

## <a name="getresults"></a>GetResults

Возвращает результирующий набор из выполнения `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Эту функцию можно вызывать несколько раз. По одному разу для каждого фрагмента потока и для каждого раздела в `@input_data_1_partition_by_columns` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER**_   StrLen_or_Ind
);
```

### <a name="arguments"></a>Аргументы

_SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*RowsNumber*  
\[Выходные данные\] Указатель на буфер, содержащий количество строк в *Data*.

*Data*  
\[Выходные данные\]. Указатель на двумерный массив, выделенный расширением и содержащий результирующий набор `@script` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Общее число столбцов должно быть равно значению *OutputSchemaColumnsNumber*, полученному в вызове [Execute](#execute). Каждый столбец должен содержать элементы в количестве *RowsNumber*, которые должны интерпретироваться в соответствии с типом столбца из [GetResultColumn](#getresultcolumn).

*StrLen_or_Ind*  
\[Выходные данные\]. Указатель на двумерный массив, выделенный расширением и содержащий индикатор длины или значения NULL для каждого значения в *Data*. Возможные значения для каждой ячейки:

- n, где n > 0. Указывает длину данных в байтах.
- SQL_NULL_DATA, указывающее значение NULL.

Общее число столбцов должно быть равно значению *OutputSchemaColumnsNumber*, полученному в вызове [Execute](#execute). Каждый столбец содержит элементы в количестве *RowsNumber*, которые должны интерпретироваться в соответствии с типом столбца из [GetResultColumn](#getresultcolumn).

StrLen_or_Ind\[col\] будет игнорироваться, если один столбец не допускает значение NULL и не представляет один из следующих типов данных: SQL_C_CHAR, SQL_C_WCHAR и SQL_C_BINARY [добавление дат]. В противном случае он указывает на допустимый массив с числом элементов, равным *RowsNumber*, где каждый элемент содержит данные о длине или индикаторе NULL.

## <a name="getoutputparam"></a>GetOutputParam

Возвращает сведения об указанном выходном параметре для определенного сеанса.

Эта функция вызывается для каждого параметра из `@params` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), помеченного с помощью OUTPUT.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*ParamValue*  
\[Выходные данные\] Указатель на буфер, содержащий значение параметра.

*StrLen_or_Ind* \[Выходные данные\] Указатель на буфер, содержащий целочисленное значение, указывающее длину *ParamValue* в байтах или SQL_NULL_DATA, чтобы обозначить, что данные имеют значение NULL.

## <a name="getinterfaceversion"></a>GetInterfaceVersion

Возвращает версию интерфейса.
Эта функция возвращает целое число, представляющее версию интерфейса расширения. Поддерживаемые значения:
1. Версия 1 является исходной версией API. Поддерживается в SQL Server 2019 RTM.
1. В версии 2 добавлена поддержка API InstallExternalLibrary и UninstallExternalLibrary, и она поддерживается, начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3).                            

### <a name="syntax"></a>Синтаксис

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

Очищает сведения для каждого сеанса.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

## <a name="cleanup"></a>Очистка

Очищает глобальную общую информацию (например, виртуальную машину Java).

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

Извлекает данные телеметрии (пары "ключ — значение") из расширения. Функция является необязательной и не требует реализации. Данные телеметрии предоставляются динамическим административным представлением `dm_db_external_script_execution_stats`.

Существует счетчик script_executions, который отправляется платформой. Расширение не должно использовать это имя.

Каждая запись телеметрии является парой "ключ — значение". Ключи представляют собой строки, а значения являются 64-разрядными целыми числами — счетчиками. Таким образом, выходные данные состоят из двух логических массивов: имен и соответствующих им счетчиков. Каждый массив является выходным.

Длина каждого массива равна значению *RowsNumber*, которое относится к выходным данным. Первый логический выход содержит указатели на строки и поэтому представлен двумя массивами: *CounterNames* (фактические строковые данные) и *CounterNamesLength* (длина каждой строки). Второй логический выход хранится в указателе *CounterValues*.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>Аргументы

*SessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*TaskId*  
\[Входные данные\] Целое число, однозначно идентифицирующее этот процесс выполнения.

При наличии `@parallel = 1` в [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) это значение лежит в диапазоне от 0 до степени параллелизма запроса.

*RowsNumber*  
\[Выходные данные\] Число пар "ключ — значение".

*CounterNames*  
\[Выходные данные\] Строковые данные, содержащие ключи.

*CounterNamesLength*  
\[Выходные данные\] Длина каждой строки ключа.

*CounterValues*  
\[Выходные данные\] 64-разрядные целочисленные данные, содержащие значения.

## <a name="installexternallibrary"></a>InstallExternalLibrary

Устанавливает библиотеку. Функция является необязательной и не требует реализации. Реализация по умолчанию заключается в копировании содержимого библиотеки (см. [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)) в файл в нужном месте. Имя файла соответствует имени библиотеки.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Аргументы

*SetupSessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*LibraryName*  
\[Входные данные\] Имя библиотеки.

*LibraryNameLength*  
\[Входные данные\] Длина имени библиотеки.

*LibraryFile*  
\[Входные данные\] Путь (в виде строки) к файлу библиотеки, содержащему двоичное содержимое, указанное [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

*LibraryFileLength*  
\[Входные данные\] Длина строки LibraryFile.

*LibraryInstallDirectory:*  
\[Входные данные\] Корневой каталог для установки библиотеки.

*LibraryInstallDirectoryLength*  
\[Входные данные\] Длина строки LibraryInstallDirectory.

*LibraryError*  
\[Выходные данные\] Необязательный выходной параметр. Если во время установки библиотеки произошла ошибка, LibraryError указывает на строку, описывающую ошибку.

*LibraryErrorLength*  
\[Выходные данные\] Длина строки LibraryError.

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

Удаляет библиотеку. Функция является необязательной и не требует реализации. Реализация по умолчанию заключается в отмене работы, выполненной реализацией по умолчанию InstallExternalLibrary. Реализация по умолчанию удаляет содержимое файла *LibraryName* в *LibraryInstallDirectory*.

### <a name="syntax"></a>Синтаксис

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Аргументы

*SetupSessionId*  
\[Входные данные\] GUID, однозначно определяющий этот сеанс скрипта.

*LibraryName*  
\[Входные данные\] Имя библиотеки.

*LibraryNameLength*  
\[Входные данные\] Длина имени библиотеки.

*LibraryFile*  
\[Входные данные\] Путь (в виде строки) к файлу библиотеки, содержащему двоичное содержимое, указанное CREATE EXTERNAL LIBRARY.

*LibraryFileLength*  
\[Входные данные\] Длина строки LibraryFile.

*LibraryInstallDirectory*  
\[Входные данные\] Корневой каталог для установки библиотеки.

*LibraryInstallDirectoryLength*  
\[Входные данные\] Длина строки LibraryInstallDirectory.

*LibraryError*  
\[Выходные данные\] Строка ошибки библиотеки.

*LibraryErrorLength*  
\[Выходные данные\] Длина строки LibraryError.

## <a name="next-steps"></a>Дальнейшие действия

- [Пакет SDK Майкрософт для расширения возможностей Java в SQL Server](../how-to/extensibility-sdk-java-sql-server.md)
- [Настраиваемая среда выполнения Python](../../machine-learning/install/custom-runtime-python.md)
- [Настраиваемая среда выполнения R](../../machine-learning/install/custom-runtime-r.md).