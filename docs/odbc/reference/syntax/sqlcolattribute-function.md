---
description: Функция SQLColAttribute
title: Функция SQLColAttribute | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43980caf7a592284aba87d57725c30ae9fe8e949
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151249"
---
# <a name="sqlcolattribute-function"></a>Функция SQLColAttribute
**Соответствия**  
 Представленная версия: соответствие стандартам ODBC 3,0: ISO 92  
  
 **Сводка**  
 **SQLColAttribute** возвращает сведения о дескрипторе для столбца в результирующем наборе. Сведения о дескрипторе возвращаются в виде символьной строки, зависящего от дескриптора значения или целочисленного значения.  
  
> [!NOTE]  
>  Дополнительные сведения о том, что диспетчер драйверов сопоставляет эту функцию при использовании ODBC 3. Приложение *x* работает с ODBC 2. драйвер *x* см. в разделе [Сопоставление функций замены для обратной совместимости приложений](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Аргументы  
 *статеменсандле*  
 Входной Маркер инструкции.  
  
 *ColumnNumber*  
 Входной Номер записи в IRD, из которой должно быть получено значение поля. Этот аргумент соответствует номеру столбца результирующих данных, упорядоченным последовательно в возрастающем порядке столбцов, начиная с 1. Столбцы можно описать в любом порядке.  
  
 В этом аргументе можно указать столбец 0, но все значения, кроме SQL_DESC_TYPE и SQL_DESC_OCTET_LENGTH, будут возвращать неопределенные значения.  
  
 *FieldIdentifier*  
 Входной Дескриптор дескриптора. Этот маркер определяет, какое поле в IRD должно быть запрошено (например, SQL_COLUMN_TABLE_NAME).  
  
 *CharacterAttributePtr*  
 Проверки Указатель на буфер, в котором возвращается значение в поле *фиелдидентифиер* строки *columnNumber* IRD, если поле является символьной строкой. В противном случае поле не используется.  
  
 Если *чарактераттрибутептр* имеет значение null, *стрингленгсптр* будет возвращать общее число байтов (исключая символ завершения null для символьных данных), доступный для возврата в буфер, на который указывает *чарактераттрибутептр*.  
  
 *BufferLength*  
 Входной Если *фиелдидентифиер* является определяемым ODBC полем, а *чарактераттрибутептр* указывает на символьную строку или двоичный буфер, этот аргумент должен иметь длину \* *чарактераттрибутептр*. Если *фиелдидентифиер* является определяемым ODBC полем, а \* *чарактераттрибуте* PTR является целым числом, это поле игнорируется. Если *\* чарактераттрибутептр* является строкой в Юникоде (при вызове **Склколаттрибутев**), аргумент *BufferLength* должен быть четным числом. Если *фиелдидентифиер* является полем, определенным драйвером, приложение указывает природу поля в диспетчере драйверов, установив аргумент *BufferLength* . *BufferLength* могут иметь следующие значения:  
  
-   Если *чарактераттрибутептр* является указателем на указатель, *BufferLength* должен иметь значение SQL_IS_POINTER.  
  
-   Если *чарактераттрибутептр* является указателем на символьную строку, *BufferLength* — это длина буфера.  
  
-   Если *чарактераттрибутептр* является указателем на двоичный буфер, приложение помещает результат макроса SQL_LEN_BINARY_ATTR (*length*) в *BufferLength*. Это помещает отрицательное значение в *BufferLength*.  
  
-   Если *чарактераттрибутептр* является указателем на тип данных фиксированной длины, *BufferLength* должен быть одним из следующих: SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT или SQL_IS_USMALLINT.  
  
 *стрингленгсптр*  
 Проверки Указатель на буфер, в котором возвращается общее число байтов (за исключением байта завершения null для символьных данных), доступных для возвращения в **чарактераттрибутептр*.  
  
 Для символьных данных, если число возвращаемых байт больше или равно *BufferLength*, сведения о дескрипторе в \* *чарактераттрибутептр* усекаются до *BufferLength* минус длину завершающего символа NULL и завершаются драйвером.  
  
 Для всех остальных типов данных значение *BufferLength* игнорируется, и драйвер предполагает, что размер **чарактераттрибутептр* составляет 32 бит.  
  
 *нумерикаттрибутептр*  
 Проверки Указатель на целочисленный буфер, в котором возвращается значение в поле *фиелдидентифиер* строки *columnNumber* IRD, если поле имеет числовой тип дескриптора, например SQL_DESC_COLUMN_LENGTH. В противном случае поле не используется. Обратите внимание, что некоторые драйверы могут записывать только более низкие 32-или 16-битные буферы и не изменять бит более высокого порядка. Поэтому приложения должны инициализировать значение 0 перед вызовом этой функции.  
  
## <a name="returns"></a>Возвращаемое значение  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR или SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Диагностика  
 Если **SQLColAttribute** возвращает либо SQL_ERROR, либо SQL_SUCCESS_WITH_INFO, связанное значение SQLSTATE может быть получено путем вызова **SQLGetDiagRec** с *параметром handletype* SQL_HANDLE_STMT и *маркером* *статеменсандле*. В следующей таблице перечислены значения SQLSTATE, обычно возвращаемые функцией **SQLColAttribute** , и объясняется каждый из них в контексте этой функции. Нотация "(DM)" предшествует описаниям SQLSTATE, возвращаемым диспетчером драйверов. Код возврата, связанный с каждым значением SQLSTATE, имеет SQL_ERROR, если не указано иное.  
  
|SQLSTATE|Ошибка|Описание|  
|--------------|-----------|-----------------|  
|01000|Общее предупреждение|Информационное сообщение для конкретного драйвера. (Функция возвращает SQL_SUCCESS_WITH_INFO.)|  
|01004|Строковые данные, усеченные справа|Буфер \* *чарактераттрибутептр* недостаточно велик для возврата всего строкового значения, поэтому строковое значение было усечено. Длина неусеченного строкового значения возвращается в **стрингленгсптр*. (Функция возвращает SQL_SUCCESS_WITH_INFO.)|  
|07005|Подготовленная инструкция не является *спецификацией курсора*|Инструкция, связанная с *статеменсандле* , не вернула результирующий набор, а *фиелдидентифиер* не SQL_DESC_COUNT. Нет столбцов для описания.|  
|07009|Недопустимый индекс дескриптора|(DM) значение, указанное для *columnNumber* , равно 0, а атрибут инструкции SQL_ATTR_USE_BOOKMARKS был SQL_UB_OFF.<br /><br /> Значение, указанное для аргумента *columnNumber* , больше числа столбцов в результирующем наборе.|  
|HY000|Общая ошибка|Произошла ошибка, для которой нет определенного SQLSTATE и для которого не определен SQLSTATE для конкретной реализации. Сообщение об ошибке, возвращаемое функцией **SQLGetDiagField** из структуры диагностических данных, описывает ошибку и ее причину.|  
|HY001|Ошибка выделения памяти|Драйверу не удалось выделить память, необходимую для поддержки выполнения или завершения функции.|  
|HY008|Operation canceled|Асинхронная обработка включена для *статеменсандле*. Функция была вызвана, и до ее завершения была вызвана **SQLCancel** или **склканцелхандле** для *статеменсандле*. Затем функция была вызвана в *статеменсандле*.<br /><br /> Функция была вызвана и до ее завершения была вызвана **SQLCancel** или **склканцелхандле** в *статеменсандле* из другого потока многопоточного приложения.|  
|HY010|Ошибка последовательности функций|(DM) вызвана асинхронно исполняемая функция для маркера соединения, связанного с *статеменсандле*. Эта функция айнчронаус все еще выполнялась при вызове SQLColAttribute.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** или **SQLMoreResults** были вызваны для *статеменсандле* и возвращены SQL_PARAM_DATA_AVAILABLE. Эта функция была вызвана до получения данных для всех потоковых параметров.<br /><br /> (DM) функция была вызвана до вызова **SQLPrepare**, **SQLExecDirect** или функции каталога для *статеменсандле*.<br /><br /> (DM) вызывается асинхронно исполняемая функция (не эта одна) для *статеменсандле* и все еще выполнялась при вызове этой функции.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** или **SQLSetPos** были вызваны для *статеменсандле* и возвращены SQL_NEED_DATA. Эта функция была вызвана перед отправкой данных для всех параметров или столбцов данных, выполняемых во время выполнения.|  
|HY013|Ошибка управления памятью|Не удалось обработать вызов функции, так как не удалось получить доступ к базовым объектам памяти, возможно, из-за нехватки памяти.|  
|HY090|Недопустимая длина строки или буфера|(DM) *\* чарактераттрибутептр* является символьной строкой, а *BufferLength* имеет значение меньше 0, но не равно SQL_NTS.|  
|HY091|Недопустимый идентификатор поля дескриптора|Значение, указанное для аргумента *фиелдидентифиер* , не является одним из определенных значений и не является значением, определенным реализацией.|  
|HY117|Подключение приостановлено из-за неизвестного состояния транзакции. Допускаются только функции отключения и только для чтения.|(DM) Дополнительные сведения о состоянии SUSPENDED см. в разделе [функция SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Драйвер не поддерживается|Значение, указанное для аргумента *фиелдидентифиер* , не поддерживается драйвером.|  
|HYT01|Время ожидания подключения истекло|Время ожидания соединения истекло до ответа источника данных на запрос. Время ожидания соединения задается через **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Драйвер не поддерживает эту функцию|(DM) драйвер, связанный с *статеменсандле* , не поддерживает функцию.|  
|IM017|Опрос отключен в режиме асинхронного уведомления|При использовании модели уведомления опрос отключен.|  
|IM018|**Склкомплетеасинк** не был вызван для завершения предыдущей асинхронной операции с этим обработчиком.|Если предыдущий вызов функции в обработчике возвращает SQL_STILL_EXECUTING и если включен режим уведомления, то для обработки после обработки и завершения операции необходимо вызвать **склкомплетеасинк** .|  
  
 При вызове после **SQLPrepare** и до **SQLExecute** **SQLColAttribute** может возвращать любое значение SQLSTATE, которое может быть возвращено **SQLPrepare** или **SQLExecute**, в зависимости от того, когда источник данных оценивает инструкцию SQL, связанную с *статеменсандле*.  
  
 Из соображений производительности приложение не должно вызывать **SQLColAttribute** перед выполнением инструкции.  
  
## <a name="comments"></a>Комментарии  
 Сведения о том, как приложения используют сведения, возвращаемые **SQLColAttribute**, см. в разделе [метаданные результирующего набора](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** возвращает сведения либо в \* *нумерикаттрибутептр* , либо в \* *чарактераттрибутептр*. Целочисленная информация возвращается в \* *нумерикаттрибутептр* как значение SQLLEN. все остальные форматы данных возвращаются в \* *чарактераттрибутептр*. Когда сведения возвращаются в \* *нумерикаттрибутептр*, драйвер не учитывает *Чарактераттрибутептр*, *BufferLength* и *стрингленгсптр*. Когда сведения возвращаются в \* *чарактераттрибутептр*, драйвер игнорирует *нумерикаттрибутептр*.  
  
 **SQLColAttribute** возвращает значения из полей дескриптора IRD. Функция вызывается с дескриптором инструкции, а не дескриптором дескриптора. Значения, возвращаемые функцией **SQLColAttribute** для значений *фиелдидентифиер* , перечисленных далее в этом разделе, можно также получить, вызвав **SQLGETDESCFIELD** с соответствующим обработчиком IRD.  
  
 Указанные в настоящий момент поля дескриптора, версия ODBC, в которой они были представлены, и аргументы, в которых для них возвращены сведения, показаны Далее в этом разделе. Дополнительные типы дескрипторов могут быть определены драйверами для использования преимуществ различных источников данных.  
  
 ODBC 3. драйвер *x* должен возвращать значение для каждого поля дескриптора. Если поле дескриптора не применяется к драйверу или источнику данных, а если не указано иное, драйвер возвращает 0 в \* *стрингленгсптр* или пустую строку в **чарактераттрибутептр*.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3.  функция x **SQLColAttribute** заменяет устаревший ODBC 2. *x* функция **SQLColAttributes**. При сопоставлении **SQLColAttributes** с **SQLColAttribute** (при использовании ODBC 2).*приложение x* работает с ODBC 3. *x* Driver) или сопоставление **SQLColAttribute** с **SQLColAttributes** (при использовании ODBC 3.*приложение x* работает с ODBC 2. *x* Driver), диспетчер драйверов либо передает значение *фиелдидентифиер* через, сопоставляет его с новым значением, либо возвращает ошибку, как показано ниже:  
  
> [!NOTE]  
>  Префикс, используемый в значениях *фиелдидентифиер* в ODBC 3. *x* был изменен с использованием в ODBC 2. *x*. Новый префикс — "SQL_DESC"; Старый префикс — "SQL_COLUMN".  
  
-   Если **#define** значение ODBC 2. *x* *фиелдидентифиер* совпадает со значением **#define** ODBC 3. *x* *фиелдидентифиер*, значение в вызове функции просто передается через.  
  
-   **#Define** значения ODBC 2. *x* *фиелдидентифиерс* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION и SQL_COLUMN_SCALE ОТЛИЧАЮТСЯ от **#define** значений ODBC 3. *x* *фиелдидентифиерс* SQL_DESC_PRECISION, SQL_DESC_SCALE и SQL_DESC_LENGTH. ODBC 2. драйверу *x* требуется только поддержка ODBC 2. значения *x* . ODBC 3. драйвер *x* должен поддерживать значения "SQL_COLUMN" и "SQL_DESC" для этих трех *фиелдидентифиерс*. Эти значения отличаются, так как точность, масштаб и длина определяются по-разному в ODBC 3. *x* , чем в ODBC 2. *x*. Дополнительные сведения см. в разделе [размер столбца, десятичные цифры, длина октета и размер отображаемого](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)данных.  
  
-   Если **#define** значение ODBC 2. *x* *фиелдидентифиер* отличается от значения **#define** ODBC 3. *x* *фиелдидентифиер*, как и в случае с Count, Name и допускающими значение NULL значениями, значение в вызове функции сопоставляется с соответствующим значением. Например, SQL_COLUMN_COUNT сопоставляется с SQL_DESC_COUNT, а SQL_DESC_COUNT сопоставляется с SQL_COLUMN_COUNT в зависимости от направления сопоставления.  
  
-   Если *фиелдидентифиер* — новое значение в ODBC 3. *x*, для которого нет соответствующего значения в ODBC 2. *x*, он не будет сопоставляться при использовании ODBC 3. Приложение *x* использует его при вызове **SQLColAttribute** в ODBC 2. драйвер *x* , а вызов вернет значение SQLSTATE HY091 (недопустимый идентификатор поля дескриптора).  
  
 В следующей таблице перечислены типы дескрипторов, возвращаемые функцией **SQLColAttribute**. Тип для значений *нумерикаттрибутептр* — **SQLLEN \** _.  
  
|_FieldIdentifier *|Сведения<br /><br /> возвращено в|Описание|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1,0)|*нумерикаттрибутептр*|SQL_TRUE, если столбец является автоматически увеличивающимся столбцом.<br /><br /> SQL_FALSE, если столбец не является столбцом с автоматическим приращением или не является числовым.<br /><br /> Это поле допустимо только для столбцов с числовыми типами данных. Приложение может вставлять значения в строку, содержащую столбец с автоувеличением, но обычно не может обновлять значения в столбце.<br /><br /> При вставке в столбец с автоувеличением в столбец во время вставки добавляется уникальное значение. Шаг приращения не определен, но зависит от конкретного источника данных. В приложении не следует рассчитывать, что столбец AutoIncrement начинается с любой определенной точки или увеличивается по определенному значению.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3,0)|*CharacterAttributePtr*|Имя базового столбца для столбца результирующего набора. Если имя базового столбца не существует (как в случае столбцов, которые являются выражениями), то эта переменная содержит пустую строку.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_BASE_COLUMN_NAME IRD, которое является полем только для чтения.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Имя базовой таблицы, содержащей столбец. Если имя базовой таблицы не может быть определено или неприменимо, то эта переменная содержит пустую строку.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_BASE_TABLE_NAME IRD, которое является полем только для чтения.|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1,0)|*нумерикаттрибутептр*|SQL_TRUE, если столбец обрабатывается как регистр для параметров сортировки и сравнений.<br /><br /> SQL_FALSE, если столбец не обрабатывается с учетом регистра для параметров сортировки и сравнений или является несимвольным.|  
|SQL_DESC_CATALOG_NAME (ODBC 2,0)|*CharacterAttributePtr*|Каталог таблицы, содержащей столбец. Возвращаемое значение определяется реализацией, если столбец является выражением или если столбец является частью представления. Если источник данных не поддерживает каталоги или не удается определить имя каталога, возвращается пустая строка. Это поле записи типа VARCHAR не может содержать 128 символов.|  
|SQL_DESC_CONCISE_TYPE (ODBC 1,0)|*нумерикаттрибутептр*|Тип данных краткая.<br /><br /> Для типов данных DateTime и Interval это поле возвращает тип данных краткая. Например, SQL_TYPE_TIME или SQL_INTERVAL_YEAR. (Дополнительные сведения см. в разделе [идентификаторы типов данных и дескрипторы](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) в приложении г: типы данных.)<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_CONCISE_TYPE в IRD.|  
|SQL_DESC_COUNT (ODBC 1,0)|*нумерикаттрибутептр*|Число столбцов, доступных в результирующем наборе. Возвращает 0, если в результирующем наборе нет столбцов. Значение в аргументе *columnNumber* игнорируется.<br /><br /> Эти сведения возвращаются из поля заголовка SQL_DESC_COUNT IRD.|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1,0)|*нумерикаттрибутептр*|Максимальное число символов, необходимое для вывода данных из столбца. Дополнительные сведения о размере отображаемого изображения см. в разделе [размер столбца, десятичные цифры, длина октета и размер дисплея](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) в приложении г: типы данных.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1,0)|*нумерикаттрибутептр*|SQL_TRUE, если столбец имеет фиксированную точность и ненулевое масштабирование, зависящее от источника данных.<br /><br /> SQL_FALSE, если столбец не имеет фиксированной точности и ненулевого масштаба, который зависит от источника данных.|  
|SQL_DESC_LABEL (ODBC 2,0)|*CharacterAttributePtr*|Метка или заголовок столбца. Например, столбец с именем EmpName может быть помечен именем сотрудника или помечен псевдонимом.<br /><br /> Если столбец не имеет метки, возвращается имя столбца. Если столбец не имеет метки и безымянный, возвращается пустая строка.|  
|SQL_DESC_LENGTH (ODBC 3,0)|*нумерикаттрибутептр*|Числовое значение, которое является либо максимальной, либо действительной символьной длиной символьной строки или двоичного типа данных. Это максимальная длина символьного типа данных фиксированной длины или фактическая длина символа для типа данных переменной длины. Его значение всегда исключает завершающий нуль-байт, который завершает строку символов.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_LENGTH в IRD.<br /><br /> Дополнительные сведения о длине см. в разделе [размер столбца, десятичные цифры, длина октета и размер дисплея](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) в приложении г: типы данных.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3,0)|*CharacterAttributePtr*|В этом поле типа VARCHAR (128) содержится символ или символы, распознаваемые драйвером в качестве префикса для литерала с этим типом данных. Это поле содержит пустую строку для типа данных, для которого префикс литерала неприменим. Дополнительные сведения см. в разделе [литеральные префиксы и суффиксы](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3,0)|*CharacterAttributePtr*|В этом поле типа VARCHAR (128) содержится символ или символы, распознаваемые драйвером в качестве суффикса для литерала с этим типом данных. Это поле содержит пустую строку для типа данных, для которого литеральный суффикс неприменим. Дополнительные сведения см. в разделе [литеральные префиксы и суффиксы](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Это поле типа VARCHAR (128) содержит локализованные (собственные) имена для типов данных, которые могут отличаться от обычных имен типов данных. Если локализованное имя отсутствует, возвращается пустая строка. Это поле предназначено исключительно для просмотра. Кодировка строки зависит от языкового стандарта и обычно является кодировкой по умолчанию для сервера.|  
|SQL_DESC_NAME (ODBC 3,0)|*CharacterAttributePtr*|Псевдоним столбца, если он применяется. Если псевдоним столбца не применяется, возвращается имя столбца. В любом случае SQL_DESC_UNNAMED имеет значение SQL_NAMED. Если имя столбца или псевдоним столбца отсутствуют, возвращается пустая строка, а для SQL_DESC_UNNAMED устанавливается значение SQL_UNNAMED.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_NAME в IRD.|  
|SQL_DESC_NULLABLE (ODBC 3,0)|*нумерикаттрибутептр*|SQL_ NULLABLE, если столбец может иметь значения NULL; SQL_NO_NULLS, если столбец не имеет значений NULL; или SQL_NULLABLE_UNKNOWN, если неизвестно, допускает ли столбец значения NULL.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_NULLABLE в IRD.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3,0)|*нумерикаттрибутептр*|Если тип данных в поле SQL_DESC_TYPE является приблизительным числовым типом данных, это поле SQLINTEGER содержит значение 2, поскольку поле SQL_DESC_PRECISION содержит количество битов. Если тип данных в поле SQL_DESC_TYPE является точным числовым типом данных, это поле содержит значение 10, поскольку поле SQL_DESC_PRECISION содержит число десятичных цифр. Это поле имеет значение 0 для всех нечисловых типов данных.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3,0)|*нумерикаттрибутептр*|Длина символьной строки или двоичного типа данных в байтах. Для символьных или двоичных типов фиксированной длины это фактическая длина в байтах. Для символьных или двоичных типов с переменной длиной это максимальная длина в байтах. Это значение не включает признак конца null.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_OCTET_LENGTH в IRD.<br /><br /> Дополнительные сведения о длине см. в разделе [размер столбца, десятичные цифры, длина октета и размер дисплея](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) в приложении г: типы данных.|  
|SQL_DESC_PRECISION (ODBC 3,0)|*нумерикаттрибутептр*|Числовое значение, соответствующее числовому типу данных, обозначает применимую точность. Для типов данных SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP и всех типов данных интервалов, представляющих интервал времени, его значение является применимой точностью компонента доли секунды.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_PRECISION в IRD.|  
|SQL_DESC_SCALE (ODBC 3,0)|*нумерикаттрибутептр*|Числовое значение, которое является подходящим масштабом для числового типа данных. Для типов данных DECIMAL и NUMERIC это определенный масштаб. Он не определен для всех других типов данных.<br /><br /> Эти сведения возвращаются из поля записи МАСШТАБИРОВАНИЯ IRD.|  
|SQL_DESC_SCHEMA_NAME (ODBC 2,0)|*CharacterAttributePtr*|Схема таблицы, которая содержит столбец. Возвращаемое значение определяется реализацией, если столбец является выражением или если столбец является частью представления. Если источник данных не поддерживает схемы или не удается определить имя схемы, возвращается пустая строка. Это поле записи типа VARCHAR не может содержать 128 символов.|  
|SQL_DESC_SEARCHABLE (ODBC 1,0)|*нумерикаттрибутептр*|SQL_PRED_NONE, если столбец не может быть использован в предложении WHERE. (Это то же самое, что SQL_UNSEARCHABLE значение в ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR, если столбец может использоваться в предложении WHERE, но только с предикатом LIKE. (Это то же самое, что SQL_LIKE_ONLY значение в ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC, если столбец может использоваться в предложении WHERE со всеми операторами сравнения, кроме LIKE. (Это то же самое, что SQL_EXCEPT_LIKE значение в ODBC 2. *x*.)<br /><br /> SQL_PRED_SEARCHABLE, если столбец может использоваться в предложении WHERE с любым оператором сравнения.<br /><br /> Столбцы типов SQL_LONGVARCHAR и SQL_LONGVARBINARY обычно возвращают SQL_PRED_CHAR.|  
|SQL_DESC_TABLE_NAME (ODBC 2,0)|*CharacterAttributePtr*|Имя таблицы, содержащей столбец. Возвращаемое значение определяется реализацией, если столбец является выражением или если столбец является частью представления.<br /><br /> Если имя таблицы не может быть определено, возвращается пустая строка.|  
|SQL_DESC_TYPE (ODBC 3,0)|*нумерикаттрибутептр*|Числовое значение, указывающее тип данных SQL.<br /><br /> Если *columnNumber* равен 0, то возвращается SQL_BINARY для закладок с переменной длиной и SQL_INTEGER возвращается для закладок с фиксированной длиной.<br /><br /> Для типов данных DateTime и Interval это поле возвращает подробный тип данных: SQL_DATETIME или SQL_INTERVAL. (Дополнительные сведения см. в разделе [идентификаторы типов данных и дескрипторы](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) в приложении г: типы данных.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_TYPE в IRD. **Примечание.**  Для работы с ODBC 2. драйверы *x* используйте вместо них SQL_DESC_CONCISE_TYPE.|  
|SQL_DESC_TYPE_NAME (ODBC 1,0)|*CharacterAttributePtr*|Имя типа данных, зависящее от источника данных; Например, «CHAR», «VARCHAR», «MONEY», «LONG VARBINARY» или «CHAR () для данных BIT».<br /><br /> Если тип неизвестен, возвращается пустая строка.|  
|SQL_DESC_UNNAMED (ODBC 3,0)|*нумерикаттрибутептр*|SQL_NAMED или SQL_UNNAMED. Если поле SQL_DESC_NAME IRD содержит псевдоним столбца или имя столбца, возвращается SQL_NAMED. Если имя столбца или псевдоним столбца отсутствуют, возвращается SQL_UNNAMED.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_UNNAMED в IRD.|  
|SQL_DESC_UNSIGNED (ODBC 1,0)|*нумерикаттрибутептр*|SQL_TRUE, если столбец не подписан (или не является числовым).<br /><br /> SQL_FALSE, если столбец подписан.|  
|SQL_DESC_UPDATABLE (ODBC 1,0)|*нумерикаттрибутептр*|Столбец описывается значениями для определенных констант:<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE описывает возможность обновления столбца в результирующем наборе, а не столбца в базовой таблице. Возможность обновления базового столбца, на котором основан столбец результирующего набора, может отличаться от значения в этом поле. Может ли столбец быть основан на типе данных, привилегиях пользователя и определении самого результирующего набора. Если неясно, является ли столбец обновляемым, возвращается SQL_ATTR_READWRITE_UNKNOWN.|  
  
 **SQLColAttribute** является расширяемой альтернативой для **SQLDescribeCol**. **SQLDescribeCol** возвращает фиксированный набор сведений о дескрипторе на основе ANSI-89 SQL. **SQLColAttribute** предоставляет доступ к более широкому набору сведений о дескрипторах, доступных в расширениях поставщиков ANSI SQL-92 и СУБД.  
  
## <a name="related-functions"></a>Связанные функции  
  
|Сведения о|См.|  
|---------------------------|---------|  
|Привязка буфера к столбцу в результирующем наборе|[SQLBindCol, функция](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Отмена обработки инструкции|[Функция SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Возврат сведений о столбце в результирующем наборе|[SQLDescribeCol, функция](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Выборка блока данных или прокрутка результирующего набора|[Функция SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Выборка нескольких строк данных|[Функция SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Пример  
 В следующем примере кода не освобождаются дескрипторы и соединения. Примеры кода для бесплатных дескрипторов и инструкций см. в разделе [функция SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md), [Пример программы ODBC](../../../odbc/reference/sample-odbc-program.md)и [Функция SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md) .  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>См. также:  
 [Справочник по API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Файлы заголовков ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Образец программы ODBC](../../../odbc/reference/sample-odbc-program.md)
