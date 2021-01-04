---
title: SQLPackage.exe
description: Узнайте, как автоматизировать задачи разработки баз данных с помощью SqlPackage.exe. Ознакомьтесь с примерами и параметрами публикации, свойствами и переменными SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577900"
---
# <a name="sqlpackageexe"></a>SQLPackage.exe

Программа командной строки **SqlPackage.exe** автоматизирует следующие задачи разработки баз данных.  
  
- [Версия.](#version) Возвращает номер сборки приложения SqlPackage.  Этот параметр добавлен в версии 18.6.

- [Extract](sqlpackage-extract.md): создает файл моментального снимка базы данных (DACPAC) из активной Базы данных SQL Server или SQL Azure.  
  
- [Publish](sqlpackage-publish.md): выполняет добавочное обновление схемы базы данных в соответствии со схемой исходного DACPAC-файла. Если база данных не существует на сервере, операция публикации создаст ее. В противном случае обновляется существующая база данных.  
  
- [Export](sqlpackage-export.md): экспортирует активную базу данных, включая схему базы данных и пользовательские данные, из Базы данных SQL Azure или SQL Server в пакет BACPAC (BACPAC-файл).  
  
- [Import](sqlpackage-import.md): импортирует данные схемы и таблиц из пакета BACPAC в новую пользовательскую базу данных в экземпляре Базы данных SQL Server или SQL Azure.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md): создает XML-отчет по изменениям, которые должны быть внесены в результате публикации.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): создает XML-отчет по изменениям, которые были внесены в зарегистрированную базу данных со времени ее последней регистрации.  
  
- [Script](sqlpackage-script.md): создает скрипт добавочного обновления на языке Transact-SQL, который обновляет схему целевой базы данных до соответствия схеме базы данных-источника.  
  
Программа командной строки **SqlPackage.exe** позволяет указывать эти действия вместе с соответствующими параметрами и свойствами.  

**[Скачать последнюю версию](sqlpackage-download.md)** . Подробнее см. в [заметках о выпуске](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Синтаксис командной строки

Программа **SqlPackage.exe** инициирует действия, заданные с использованием параметров, свойств и переменных SQLCMD, указанных в командной строке.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Примеры использования

**Создание сравнения баз данных с помощью DACPAC-файлов с выходными данными скрипта SQL**

Сначала создайте DACPAC-файл с последними изменениями базы данных:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Затем создайте DACPAC-файл целевого объекта базы данных (без изменений):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Создайте скрипт SQL, который создает различия между двумя DACPAC-файлами:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Версия

Выводит версию sqlpackage в виде номера сборки.  Может использоваться в интерактивных запросах, а также в [автоматизированных конвейерах](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Коды выхода

Команды, возвращающие следующие коды выхода:

- 0 = успешное завершение;
- ненулевое значение = сбой.


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об [извлечении SqlPackage](sqlpackage-extract.md)
- Дополнительные сведения о [публикации SqlPackage](sqlpackage-publish.md)
- Дополнительные сведения об [экспорте SqlPackage](sqlpackage-export.md)
- Дополнительные сведения об [импорте SqlPackage](sqlpackage-import.md)
