---
description: CREATE SPATIAL INDEX (Transact-SQL)
title: CREATE SPATIAL INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SPATIAL INDEX
- CREATE SPATIAL INDEX
- CREATE_SPATIAL_INDEX_TSQL
- SPATIAL_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], creating
- index creation [SQL Server], spatial indexes
- CREATE SPATIAL INDEX statement
- CREATE INDEX statement
ms.assetid: ee6b9116-a7ff-463a-a9f0-b360804d8678
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce2693109669da084a67849105e859066f818213
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102380"
---
# <a name="create-spatial-index-transact-sql"></a>CREATE SPATIAL INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает пространственный индекс на указанной таблице и столбце в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Индекс может быть создан до появления данных в таблице. Можно создать индексы для таблиц или представлений в другой базе данных, если указать полное имя этой базы данных. Для создания пространственного индекса необходимо, чтобы у таблицы был кластеризованный первичный ключ. Пространственные индексы не могут быть заданы в индексированных представлениях. Сведения о пространственных индексах см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE SPATIAL INDEX index_name
  ON <object> ( spatial_column_name )  
    {  
       <geometry_tessellation> | <geography_tessellation>  
    }
  [ ON { filegroup_name | "default" } ]  
[;]
  
<object> ::=  
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<geometry_tessellation> ::=  
{
  <geometry_automatic_grid_tessellation>
| <geometry_manual_grid_tessellation>
}  
  
<geometry_automatic_grid_tessellation> ::=  
{  
    [ USING GEOMETRY_AUTO_GRID ]  
          WITH  (  
        <bounding_box>  
            [ [,] <tessellation_cells_per_object> [ ,...n] ]  
            [ [,] <spatial_index_option> [ ,...n] ]  
                 )  
}  
  
<geometry_manual_grid_tessellation> ::=  
{  
       [ USING GEOMETRY_GRID ]  
         WITH (  
                    <bounding_box>  
                        [ [,]<tessellation_grid> [ ,...n] ]  
                        [ [,]<tessellation_cells_per_object> [ ,...n] ]  
                        [ [,]<spatial_index_option> [ ,...n] ]  
   )  
}
  
<geography_tessellation> ::=  
{  
      <geography_automatic_grid_tessellation> | <geography_manual_grid_tessellation>  
}  
  
<geography_automatic_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_AUTO_GRID ]  
    [ WITH (  
        [ [,] <tessellation_cells_per_object> [ ,...n] ]  
        [ [,] <spatial_index_option> ]  
     ) ]  
}  
  
<geography_manual_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_GRID ]  
    [ WITH (  
                [ <tessellation_grid> [ ,...n] ]  
                [ [,] <tessellation_cells_per_object> [ ,...n] ]  
                [ [,] <spatial_index_option> [ ,...n] ]  
                ) ]  
}  
  
<bounding_box> ::=  
{  
      BOUNDING_BOX = ( {  
       xmin, ymin, xmax, ymax
       | <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>
  } )  
}  
  
<named_bb_coordinate> ::= { XMIN = xmin | YMIN = ymin | XMAX = xmax | YMAX=ymax }  
  
<tessellation_grid> ::=  
{
    GRIDS = ( { <grid_level> [ ,...n ] | <grid_size>, <grid_size>, <grid_size>, <grid_size>  }
        )  
}  
<tessellation_cells_per_object> ::=  
{
   CELLS_PER_OBJECT = n
}  
  
<grid_level> ::=  
{  
     LEVEL_1 = <grid_size>
  |  LEVEL_2 = <grid_size>
  |  LEVEL_3 = <grid_size>
  |  LEVEL_4 = <grid_size>
}  
  
<grid_size> ::= { LOW | MEDIUM | HIGH }  
  
<spatial_index_option> ::=  
{  
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE }  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

 *index_name*     
 Имя индекса. Имена индексов должны быть уникальными в пределах таблицы, но не обязательно должны быть уникальными в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 ON \<object> ( *spatial_column_name* )     
 Указывает объект (база данных, схема или таблица), для которого будет создан индекс, и имя пространственного столбца.  
  
 Аргумент *spatial_column_name* указывает пространственный столбец, на котором основан индекс. В одном определении пространственного индекса может быть задан только один пространственный столбец, но на основе одного столбца типа **geometry** или **geography** можно создать несколько пространственных индексов.  
  
 USING      
 Указывает схему тесселяции пространственного индекса. Этому параметру присваивается значение определенного типа, указанное в следующей таблице:  
  
|Столбец типа данных|Схема тесселяции|  
|-------------------------|-------------------------|  
|**geometry**|GEOMETRY_GRID|  
|**geometry**|GEOMETRY_AUTO_GRID|  
|**geography**|GEOGRAPHY_GRID|  
|**geography**|GEOGRAPHY_AUTO_GRID|  
  
 Пространственный индекс можно создать только для столбца типа **geometry** или **geography**, иначе происходит ошибка. Ошибка происходит при передаче недопустимого параметра для данного типа.  
  
 Сведения о реализации тесселяции в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ON *filegroup_name*      
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Создает заданный индекс в указанной файловой группе. Если местоположение не указано и таблица не секционирована, индекс использует ту же файловую группу, что и базовая таблица. Файловая группа должна существовать.  
  
 ON «по умолчанию»     
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Создает заданный индекс в файловой группе, используемой по умолчанию.  
  
 Слово "default" в этом контексте не является ключевым. Это идентификатор файловой группы, используемой по умолчанию, который должен быть ограничен, как например в выражении ON "default" или ON [default]. Если указано значение "default" (по умолчанию), параметр QUOTED_IDENTIFIER должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в статье [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **\<object>::=**     
 Полное или неполное имя индексируемого объекта.  
  
 *database_name*      
 Имя базы данных.  
  
 *schema_name*     
 Имя схемы, которой принадлежит таблица.  
  
 *table_name*      
 Имя таблицы для индексирования.  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживает формат трехкомпонентного имени database_name.[schema_name].object_name, если database_name — это текущая база данных или database_name — это tempdb и object_name начинается с символа #.  
  
### <a name="using-options"></a>Параметры USING

 GEOMETRY_GRID      
 Указывает используемую схему тесселяции сетки типа **geometry**. Параметр GEOMETRY_GRID может быть указан только для столбца данных типа **geometry**.  GEOMETRY_GRID позволяет вручную корректировать схему тесселяции.  
  
 GEOMETRY_AUTO_GRID      
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Может быть указан только для столбца данных типа geometry. Это значение используется по умолчанию для этого типа данных, его не нужно указывать.  
  
 GEOGRAPHY_GRID      
 Указывает схему тесселяции географической сетки. Параметр GEOGRAPHY_GRID может быть указан только для столбца данных типа **geography**.  
  
 GEOGRAPHY_AUTO_GRID      
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Может быть указан только для столбца данных типа geography.  Это значение используется по умолчанию для этого типа данных, его не нужно указывать.  
  
### <a name="with-options"></a>Параметры WITH

BOUNDING_BOX      
Указывает числовой четырехэлементный кортеж, который определяет четыре координаты ограничивающего прямоугольника: координаты x-min и y-min нижнего левого угла, и координаты x-max и y-max верхнего правого угла.  
  
 *xmin*      
 Задает координату левого нижнего угла ограничивающего прямоугольника по оси X.  
  
 *ymin*      
 Задает координату левого нижнего угла ограничивающего прямоугольника по оси Y.  
  
 *xmax*     
 Задает координату x правого верхнего угла ограничивающего прямоугольника.  
  
 *ymax*     
 Задает координату правого верхнего угла ограничивающего прямоугольник по оси Y.  
  
 XMIN = *xmin*     
 Указывает имя свойства и значение для координаты по оси X левого нижнего угла ограничивающего прямоугольника.  
  
 YMIN =*ymin*      
 Указывает имя свойства и значение для координаты по оси Y левого нижнего угла ограничивающего прямоугольника.  
  
 XMAX =*xmax*      
 Указывает имя свойства и значение для координаты по оси X правого верхнего угла ограничивающего прямоугольника.  
  
 YMAX =*ymax*     
 Указывает имя свойства и значение для координаты по оси Y правого верхнего угла ограничивающего прямоугольника.  
  
 > [!NOTE]
 > Координаты ограничивающего прямоугольника применяются только в предложении USING GEOMETRY_GRID.  
 >
 > *xmax* должен быть больше *xmin*, *ymax* должен быть больше *ymin*. Можно задать любое допустимое представление значения [float](../../t-sql/data-types/float-and-real-transact-sql.md), при условии, что *xmax* > *xmin* и *ymax* > *ymin*. В противном случае формируются соответствующие ошибки.  
 >
 > Значения по умолчанию отсутствуют.  
 >
 > В именах свойств ограничивающего прямоугольника учитывается регистр, независимо от параметров сортировки базы данных.  
  
 Каждое имя свойства необходимо указать только один раз. Их можно указывать в любом порядке. Например, следующие предложения эквивалентны:  
  
- BOUNDING_BOX =( XMIN =*xmin*, YMIN =*ymin*, XMAX =*xmax*, YMAX =*ymax* )  
  
- BOUNDING_BOX =( XMIN =*xmin*, XMAX =*xmax*, YMIN =*ymin*, YMAX =*ymax*)  
  
GRIDS      
Определяет плотность сетки на каждом уровне схемы тесселяции. Если выбраны GEOMETRY_AUTO_GRID и GEOGRAPHY_AUTO_GRID, этот параметр отключен.  
  
 Сведения о тесселяции в см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 Параметры GRIDS имеют следующие значения:  
  
 LEVEL_1     
 Указывает сетку первого (верхнего) уровня.  
  
 LEVEL_2    
 Указывает сетку второго уровня.  
  
 LEVEL_3    
 Указывает сетку третьего уровня.  
  
 LEVEL_4    
 Указывает сетку четвертого уровня.  
  
 LOW    
 Указывает наименьшую возможную плотность сетки на данном уровне. LOW соответствует 16 ячейкам (сетка 4x4).  
  
 **MEDIUM**     
 Указывает среднюю плотность сетки на данном уровне. MEDIUM соответствует 64 ячейкам (сетка 8x8).  
  
 HIGH.     
 Указывает наибольшую возможную плотность сетки на данном уровне. HIGH соответствует 256 ячейкам (сетка 16x16).  
  
> [!NOTE]
> С помощью имен уровней можно указывать уровни в любом порядке, а также пропускать уровни. Если используется имя для любого уровня, то необходимо использовать имя любого другого указанного уровня. Если уровень пропущен, то по умолчанию назначается плотность MEDIUM.  

> [!WARNING]
> Если указана недопустимая плотность, возвращается ошибка.  
  
CELLS_PER_OBJECT =*n*     
Задает количество ячеек тесселяции на объект, которое может использоваться процессом тесселяции для одного пространственного объекта в индексе. Аргумент *n* может иметь любое целочисленное значение от 1 до 8192 включительно. Если передано неверное число или число превышает максимальное число ячеек для указанной тесселяции, то возникает ошибка.  
  
 Параметр CELLS_PER_OBJECT имеет следующие значения по умолчанию:  
  
|параметр USING|Число ячеек на объект по умолчанию|  
|------------------|------------------------------|  
|GEOMETRY_GRID|**16**|  
|GEOMETRY_AUTO_GRID|**8**|  
|GEOGRAPHY_GRID|**16**|  
|GEOGRAPHY_AUTO_GRID|**12**|  
  
Если объект на верхнем уровне охватывает больше ячеек, чем указано в параметре *n*, в ходе индексирования используется столько ячеек, сколько необходимо для полной тесселяции на верхнем уровне. В подобных случаях объекту может быть выделено больше ячеек, чем было указано. В этом случае максимальное число представляет собой число ячеек, формируемых сеткой верхнего уровня, которое зависит от плотности.  
  
Значение CELLS_PER_OBJECT используется правилом тесселяции для числа ячеек на объект. Сведения о правилах тесселяции в см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
PAD_INDEX = { ON | **OFF** }     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
Определяет разреженность индекса. Значение по умолчанию — OFF.  
  
ON     
Указывает, что процент свободного места, определяемый аргументом *fillfactor*, применяется к страницам индекса промежуточного уровня.  
  
OFF или *fillfactor* не указан     
Указывает, что страницы промежуточного уровня заполняются почти полностью, при этом остается достаточно места по крайней мере для одной строки максимального размера, возможного в этом индексе при заданном наборе ключей на промежуточных страницах.  
  
Параметр PAD_INDEX имеет смысл только в случае, если указан параметр FILLFACTOR, так как использует процентное значение, указанное в нем. Если процент, заданный аргументом FILLFACTOR, недостаточно велик для размещения одной строки, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] внутренне переопределит это значение, чтобы обеспечить минимум. Количество строк на странице индекса промежуточного уровня никогда не бывает менее двух даже при самых малых значениях аргумента *fillfactor*.  
  
FILLFACTOR =*fillfactor*     
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Определяет величину в процентах, показывающую, насколько компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять конечный уровень каждой страницы индекса во время его создания или перестроения. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0. Если параметр *fillfactor* равен 100 или 0, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] создает индексы с полностью заполненными страницами конечного уровня.  
  
> [!NOTE]  
> Значения коэффициентов заполнения 0 и 100 идентичны.
  
 Аргумент FILLFACTOR действует только при создании или перестройке индекса. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не сохраняет динамически указанный процентный объем свободного места на страницах. Значение коэффициента заполнения можно увидеть в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
> Создание кластеризованного индекса с аргументом FILLFACTOR меньше 100 влияет на объем пространства хранения, занимаемого данными, т. к. компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] перераспределяет данные, когда создает кластеризованный индекс.  
  
 Дополнительные сведения см. в статье [Указание коэффициента заполнения для индекса](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
SORT_IN_TEMPDB = { ON | **OFF** }       
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Указывает, сохранять ли временные результаты сортировки в базе данных tempdb. Значение по умолчанию — OFF.  
  
 ON     
 Промежуточные результаты сортировки, используемые для создания индекса, хранятся в базе данных tempdb. Это может уменьшить время, необходимое для создания индекса, если база данных tempdb и база данных пользователя находятся на разных наборах дисков. Однако это увеличивает использование места на диске, которое используется при индексировании.  
  
 OFF     
 Промежуточные результаты сортировки хранятся в той же базе данных, где и индекс.  
  
 Кроме места в базе данных пользователя, необходимого для создания индекса, требуется примерно столько же дополнительного места в базе данных tempdb для хранения промежуточных результатов сортировки. Дополнительные сведения см. в разделе [Параметр SORT_IN_TEMPDB для индексов](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
IGNORE_DUP_KEY =**OFF**     
Не влияет на пространственные индексы, поскольку для этого типа индекса ограничение уникальности неприменимо. Не устанавливайте этот параметр в значение ON, иначе произойдет ошибка.  
  
STATISTICS_NORECOMPUTE = { ON | **OFF**}     
Указывает, выполнялся ли перерасчет статистики распределения. Значение по умолчанию — OFF.  
  
 ON    
 Устаревшие статистики не пересчитываются автоматически.  
  
 OFF    
 Автоматическое обновление статистических данных включено.  
  
 Чтобы восстановить автоматическое обновление статистики, следует установить STATISTICS_NORECOMPUTE в значение OFF или выполнить UPDATE STATISTICS без предложения NORECOMPUTE.  
  
> [!IMPORTANT]  
> Отключение автоматического пересчета статистики распределения может помешать оптимизатору запросов выбрать оптимальные планы выполнения запросов, обращенных к таблице.  
  
DROP_EXISTING = { ON | **OFF** }     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Указывает, что именованный существующий пространственный индекс удален и перестраивается. Значение по умолчанию — OFF.  
  
 ON     
 Существующий индекс удаляется и перестраивается. Указанное имя индекса должно совпадать с уже существующим индексом, но определение индекса может быть изменено. Например, можно указать другие столбцы, порядок сортировки, схему секционирования или параметры индекса.  
  
 OFF     
 Выдается ошибка, если индекс с указанным именем уже существует.  
  
 Тип индекса не может быть изменен с помощью аргумента DROP_EXISTING.  
  
ONLINE =**OFF**      
Указывает, что базовые таблицы и связанные индексы будут недоступны для запросов и изменения данных во время операций с индексами. В этой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не разрешено применять построение индекса в сети для пространственных индексов. Если этому параметру присвоено значение ON для пространственного индекса, формируется ошибка. Не указывайте параметр ONLINE или установите его в значение OFF.  
  
 Операция с индексами вне сети, в ходе которой создается, перестраивается или удаляется пространственный индекс, получает блокировку изменения схемы для таблицы. Это предотвращает доступ к базовой таблице всех пользователей во время операции.  
  
> [!NOTE]  
> Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
ALLOW_ROW_LOCKS = { **ON** | OFF }     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.  
  
 ON     
 Блокировки строк допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.  
  
 OFF     
 Блокировки строк не используются.  
  
ALLOW_PAGE_LOCKS = { **ON** | OFF }     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.  
  
 ON    
 Блокировки страниц возможны при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.  
  
 OFF     
 Блокировки страниц не используются.  
  
MAXDOP =*max_degree_of_parallelism*      
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Переопределяет параметр конфигурации `max degree of parallelism` только на время выполнения операции с индексами. MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
> [!IMPORTANT]  
> Несмотря на синтаксическую поддержку параметра MAXDOP, в настоящее время инструкция CREATE SPATIAL INDEX всегда использует только один процессор.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 1     
 Подавляет формирование параллельных планов.  
  
 \>1    
 Ограничивает максимальное количество процессоров, используемых в параллельных операциях с индексами, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы.  
  
 0 (по умолчанию)    
 В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
DATA_COMPRESSION = {NONE | ROW | PAGE}     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Определяет уровень сжатия данных, используемого индексом.  
  
 None     
 Сжатие данных не используется.  
  
 ROW    
 Используется сжатие строк.  
  
 PAGE    
 Используется сжатие страниц.  
  
## <a name="remarks"></a>Remarks
Каждый параметр можно указывать только один раз для инструкции CREATE SPATIAL INDEX. При повторении любого параметра формируется ошибка.  
  
Можно создать до 249 пространственных индексов для каждого пространственного столбца в таблице. Создание более чем одного пространственного индекса для определенного пространственного столбца может быть полезно, например, для индексации различных параметров тесселяции в одном столбце.  
  
> [!IMPORTANT]  
> Существует ряд других ограничений на создание пространственного индекса. Дополнительные сведения см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
Для построения индексов нельзя воспользоваться доступным параллелизмом процессов.  
  
## <a name="methods-supported-on-spatial-indexes"></a>Поддерживаемые методы для пространственных индексов
При определенных условиях пространственные индексы поддерживают несколько геометрических методов, ориентированных на наборы. Дополнительные сведения см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="spatial-indexes-and-partitioning"></a>Пространственные индексы и секционирование
По умолчанию, если пространственный индекс создан для секционированной таблицы, индекс секционируется в соответствии со схемой секционирования таблицы. Это обеспечивает сохранение данных индекса и связанной строки в одной секции.  
  
В этом случае при изменении схемы секционирования базовой таблицы нужно удалить пространственный индекс, прежде чем заново секционировать базовую таблицу. Чтобы обойти это ограничение при создании пространственного индекса, можно указать параметр «ON filegroup». Дополнительные сведения см. в разделе «Пространственные индексы и файловые группы» далее в разделе.  
  
## <a name="spatial-indexes-and-filegroups"></a>Пространственные индексы и файловые группы
По умолчанию пространственные индексы секционируются на те же файловые группы, что и таблица, для которой назначен индекс. Это можно переопределить при помощи спецификации для файловой группы:  
  
[ ON { *filegroup_name* | "default" } ]     
Если указать файловую группу для пространственного индекса, то индекс помещается в эту файловую группу, независимо от схемы секционирования таблицы.  
  
## <a name="catalog-views-for-spatial-indexes"></a>Представления каталога для пространственных индексов

 Следующие представления каталога используются только с пространственными индексами.  
  
 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)  
 Представляет в главном индексе сведения о пространственных индексах.  
  
 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)  
 Представляет сведения о схеме тесселяции и параметрах каждого пространственного индекса.  
  
## <a name="additional-remarks-about-creating-indexes"></a>Дополнительные примечания о создании индексов

 Дополнительные сведения о создании индексов см. в подразделе "Примечания" раздела [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения
Пользователь должен иметь разрешение `ALTER` для таблицы или представления либо быть членом предопределенной роли сервера sysadmin или предопределенных ролей базы данных `db_ddladmin` и `db_owner`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-spatial-index-on-a-geometry-column"></a>A. Создание пространственного индекса для столбца типа geometry
В следующем примере создается таблица с именем `SpatialTable`, которая содержит столбец типа **geometry**, `geometry_col`. Затем для столбца `SIndx_SpatialTable_geometry_col1` создается пространственный индекс `geometry_col`. В примере используется схема тесселяции по умолчанию и назначается ограничивающий прямоугольник.  
  
```sql  
CREATE TABLE SpatialTable(id int primary key, geometry_col geometry);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col1
   ON SpatialTable(geometry_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ) );  
```  
  
### <a name="b-creating-a-spatial-index-on-a-geometry-column"></a>Б. Создание пространственного индекса для столбца типа geometry
В следующем примере для столбца `SIndx_SpatialTable_geometry_col2` в таблице `geometry_col` создается второй пространственный индекс, `SpatialTable`. В качестве схемы тесселяции в примере задается `GEOMETRY_GRID`. В примере также назначается ограничивающий прямоугольник, различные плотности на разных уровнях сетки и 64 ячейки на объект. В примере также задается значение `ON` для разреженности индекса.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col2  
   ON SpatialTable(geometry_col)  
   USING GEOMETRY_GRID  
   WITH (  
    BOUNDING_BOX = ( xmin=0, ymin=0, xmax=500, ymax=200 ),  
    GRIDS = (LOW, LOW, MEDIUM, HIGH),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="c-creating-a-spatial-index-on-a-geometry-column"></a>В. Создание пространственного индекса для столбца типа geometry
Следующий пример создает третий пространственный индекс, `SIndx_SpatialTable_geometry_col3`, для столбца `geometry_col` в таблице `SpatialTable`. В примере используется схема тесселяции по умолчанию. В примере назначается ограничивающий прямоугольник и используются различные плотности ячеек на третьем и четвертом уровнях, при этом выбрано число ячеек на объект по умолчанию.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col3  
   ON SpatialTable(geometry_col)  
   WITH (  
    BOUNDING_BOX = ( 0, 0, 500, 200 ),  
    GRIDS = ( LEVEL_4 = HIGH, LEVEL_3 = MEDIUM ) );  
```  
  
### <a name="d-changing-an-option-that-is-specific-to-spatial-indexes"></a>Г. Изменение параметра для пространственных индексов
В следующем примере перестраивается пространственный индекс `SIndx_SpatialTable_geography_col3`, созданный в предшествующем примере, путем назначения новой плотности `LEVEL_3` с помощью инструкции DROP_EXISTING = ON.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable(geography_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ),  
        GRIDS = ( LEVEL_3 = LOW ),  
        DROP_EXISTING = ON );  
```  
  
### <a name="e-creating-a-spatial-index-on-a-geography-column"></a>Д. Создание пространственного индекса для столбца типа geography
В следующем примере создается таблица с именем `SpatialTable2`, которая содержит столбец типа **geography**, `geography_col`. Затем для столбца `SIndx_SpatialTable_geography_col1` создается пространственный индекс `geography_col`. В примере используются значения параметров по умолчанию для схемы тесселяции GEOGRAPHY_AUTO_GRID.  
  
```sql  
CREATE TABLE SpatialTable2(id int primary key, object GEOGRAPHY);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col1
   ON SpatialTable2(object);  
```  
  
> [!NOTE]  
> Для индексов географической сетки нельзя указать параметры ограничивающего прямоугольника.
  
### <a name="f-creating-a-spatial-index-on-a-geography-column"></a>Е. Создание пространственного индекса для столбца типа geography
В следующем примере для столбца `SIndx_SpatialTable_geography_col2` в таблице `geography_col` создается второй пространственный индекс, `SpatialTable2`. В качестве схемы тесселяции в примере задается `GEOGRAPHY_GRID`. В примере также назначаются различные плотности сетки на разных уровнях и 64 ячейки на объект. В примере также задается значение `ON` для разреженности индекса.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col2  
   ON SpatialTable2(object)  
   USING GEOGRAPHY_GRID  
   WITH (  
    GRIDS = (MEDIUM, LOW, MEDIUM, HIGH ),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="g-creating-a-spatial-index-on-a-geography-column"></a>Ж. Создание пространственного индекса для столбца типа geography
В следующем примере создается третий пространственный индекс, `SIndx_SpatialTable_geography_col3`, для столбца `geography_col` в таблице `SpatialTable2`. В примере используется схема тесселяции по умолчанию, GEOGRAPHY_GRID, и значение CELLS_PER_OBJECT по умолчанию (16).  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable2(object)  
   WITH ( GRIDS = ( LEVEL_3 = HIGH, LEVEL_2 = HIGH ) );  
```  
  
## <a name="see-also"></a>См. также раздел
[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)       
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)       
[CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)       
[CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)       
[CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)       
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)       
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)       
[DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)       
[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)       
[EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)       
[sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)       
[sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)       
[sys.spatial_index_tessellations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)       
[sys.spatial_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)       
[Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md)       
