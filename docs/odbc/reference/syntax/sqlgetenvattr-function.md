---
description: Функция SQLGetEnvAttr
title: Функция SQLGetEnvAttr | Документация Майкрософт
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetEnvAttr
helpviewer_keywords:
- SQLGetEnvAttr function [ODBC]
ms.assetid: 01f4590f-427a-4280-a1c3-18de9f7d86c1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 457896a5c845bc42e446f1a115ee47863f7fced3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171856"
---
# <a name="sqlgetenvattr-function"></a>Функция SQLGetEnvAttr
**Соответствия**  
 Представленная версия: соответствие стандартам ODBC 3,0: ISO 92  
  
 **Сводка**  
 **SQLGetEnvAttr** возвращает текущее значение атрибута среды.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
  
SQLRETURN SQLGetEnvAttr(  
     SQLHENV        EnvironmentHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Аргументы  
 *енвиронменсандле*  
 Входной Маркер среды.  
  
 *Attribute*  
 Входной Извлекаемый атрибут.  
  
 *ValuePtr*  
 Проверки Указатель на буфер, в котором возвращается текущее значение атрибута, указанного *атрибутом*.  
  
 Если *ValuePtr* имеет значение null, *стрингленгсптр* будет возвращать общее число байтов (исключая символ завершения null для символьных данных), доступный для возврата в буфер, на который указывает *ValuePtr*.  
  
 *BufferLength*  
 Входной Если *ValuePtr* указывает на символьную строку, этот аргумент должен иметь длину \* *ValuePtr*. Если \* *ValuePtr* является целым числом, *BufferLength* игнорируется. Если *\* ValuePtr* является строкой Юникода (при вызове **Склжетенваттрв**), аргумент *BufferLength* должен быть четным числом. Если значение атрибута не является символьной строкой, *BufferLength* не используется.  
  
 *стрингленгсптр*  
 Проверки Указатель на буфер, в котором возвращается общее число байтов (за исключением символа завершения null), доступного для возврата в *\* ValuePtr*. Если значение атрибута является символьной строкой и число возвращаемых байт больше или равно *BufferLength*, то данные в \* *ValuePtr* усекаются до *BufferLength* минус длину завершающего символа NULL и завершаются драйвером.  
  
## <a name="returns"></a>Возвращаемое значение  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR или SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Диагностика  
 Когда **SQLGetEnvAttr** возвращает SQL_ERROR или SQL_SUCCESS_WITH_INFO, связанное значение SQLSTATE может быть получено путем вызова **SQLGetDiagRec** с *параметром handletype* SQL_HANDLE_ENV и *маркером* *енвиронменсандле*. В следующей таблице перечислены значения SQLSTATE, обычно возвращаемые функцией **SQLGetEnvAttr** , и объясняется каждый из них в контексте этой функции. Нотация "(DM)" предшествует описаниям SQLSTATE, возвращаемым диспетчером драйверов. Код возврата, связанный с каждым значением SQLSTATE, имеет SQL_ERROR, если не указано иное.  
  
|SQLSTATE|Ошибка|Описание|  
|--------------|-----------|-----------------|  
|01000|Общее предупреждение|Информационное сообщение для конкретного драйвера. (Функция возвращает SQL_SUCCESS_WITH_INFO.)|  
|01004|Строковые данные, усеченные справа|Данные, возвращаемые в \* *ValuePtr* , были усечены до *BufferLength* , а не с завершающим символом NULL. Длина неусеченного строкового значения возвращается в **стрингленгсптр*. (Функция возвращает SQL_SUCCESS_WITH_INFO.)|  
|HY000|Общая ошибка|Произошла ошибка, для которой нет определенного SQLSTATE и для которого не определен SQLSTATE для конкретной реализации. Сообщение об ошибке, возвращаемое функцией **SQLGetDiagRec** в буфере *\* MessageText* , описывает ошибку и ее причину.|  
|HY001|Ошибка выделения памяти|Драйверу не удалось выделить память, необходимую для поддержки выполнения или завершения функции.|  
|HY010|Ошибка последовательности функций|(DM) **SQL_ATTR_ODBC_VERSION** еще не задан через **SQLSetEnvAttr**. Не нужно явно задавать **SQL_ATTR_ODBC_VERSION** , если используется **склаллочандлестд**.|  
|HY013|Ошибка управления памятью|Не удалось обработать вызов функции, так как не удалось получить доступ к базовым объектам памяти, возможно, из-за нехватки памяти.|  
|HY092|Недопустимый идентификатор атрибута или параметра|Значение, указанное для *атрибута* Argument, недопустимо для версии ODBC, поддерживаемой драйвером.|  
|HY117|Подключение приостановлено из-за неизвестного состояния транзакции. Допускаются только функции отключения и только для чтения.|(DM) Дополнительные сведения о состоянии SUSPENDED см. в разделе [функция SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Необязательная функция не реализована|Значение, указанное для *атрибута* Argument, является допустимым атрибутом среды ODBC для версии ODBC, поддерживаемой драйвером, но не поддерживается драйвером.|  
|IM001|Драйвер не поддерживает эту функцию|(DM) драйвер, соответствующий *енвиронменсандле* , не поддерживает функцию.|  
  
## <a name="comments"></a>Комментарии  
 Список атрибутов см. в разделе [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Атрибуты среды, специфичные для драйвера, отсутствуют. Если *атрибут* указывает атрибут, возвращающий строку, *ValuePtr* должен быть указателем на буфер, в котором возвращается строка. Максимальная длина строки, включая байт завершения null, будет *BufferLength* байт.  
  
 **SQLGetEnvAttr** можно вызывать в любое время между выделением и освобождением обработчика среды. Все атрибуты среды, успешно заданные приложением для среды, сохраняются до тех пор, пока не будет вызван **SQLFreeHandle** в *енвиронменсандле* с *параметром handletypeом* SQL_HANDLE_ENV. В ODBC 3 *. x* можно одновременно выделить более одного маркера среды. При выделении другой среды атрибут среды в одной среде не затрагивается.  
  
> [!NOTE]
>  Атрибут среды SQL_ATTR_OUTPUT_NTS поддерживается приложениями, совместимыми со стандартами. При вызове **SQLGetEnvAttr** диспетчер драйверов ODBC 3 *. x* всегда возвращает SQL_TRUE для этого атрибута. SQL_ATTR_OUTPUT_NTS можно задать для SQL_TRUE только путем вызова **SQLSetEnvAttr**.  
  
## <a name="related-functions"></a>Связанные функции  
  
|Сведения о|См.|  
|---------------------------|---------|  
|Возврат значения атрибута соединения|[Функция SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Возврат значения атрибута инструкции|[SQLGetStmtAttr, функция](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Установка атрибута соединения|[Функция SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Задание атрибута среды|[SQLSetEnvAttr, функция](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Задание атрибута инструкции|[Функция SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>См. также:  
 [Справочник по API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Файлы заголовков ODBC](../../../odbc/reference/install/odbc-header-files.md)
