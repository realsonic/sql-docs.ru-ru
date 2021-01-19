---
description: Инструкции RESTORE — VERIFYONLY (Transact-SQL)
title: RESTORE VERIFYONLY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: eeae562c4cfbf093d3b7237a044c51084331e8a9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172246"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>Инструкции RESTORE — VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Проверяет, но не восстанавливает резервную копию, а также проверяет полноту резервного набора данных и возможность его считывания. Однако инструкция RESTORE VERIFYONLY не проверяет структуру данных, содержащихся в томах резервной копии. В [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] инструкция RESTORE VERIFYONLY была расширена с целью проведения дополнительной проверки данных для увеличения вероятности обнаружения ошибок. Цель — приблизиться к настоящей операции восстановления, насколько это возможно. Дополнительные сведения см. в разделе «Примечания».  

 Если резервная копия достоверна, то компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] возвращает сообщение об успешном выполнении.  
  
> [!NOTE]  
>  Описания аргументов см. в разделе [Аргументы инструкции RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
 > [!NOTE] 
> URL-адрес — это формат, который используется для указания расположения и имени файла для хранилища BLOB-объектов Microsoft Azure и поддерживается начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2. Хотя хранилище Microsoft Azure является службой, реализация аналогична дисковому и ленточному хранилищу, чтобы обеспечить единообразное и эффективное восстановление для всех трех устройств.
 
## <a name="arguments"></a>Аргументы  
 Описания аргументов инструкции RESTORE VERIFYONLY см. в разделе [Аргументы инструкции RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="general-remarks"></a>Общие замечания  
 Набор носителей или резервный набор данных должен содержать минимально верные данные, чтобы интерпретироваться как формат Microsoft Tape Format. В противном случае инструкция RESTORE VERIFYONLY прекращает выполнение и показывает, что формат резервной копии недопустим.  
  
 Проверки, выполняемые инструкцией RESTORE VERIFYONLY, включают:  
  
-   Проверку полноты резервного набора данных и доступности для чтения всех томов.  
  
-   Некоторые поля заголовков страниц базы данных, например идентификатор страницы (как если бы инструкция записывала данные).  
  
-   Контрольную сумму (если она имеется на носителе).  
  
-   Проверку свободного места на целевых устройствах.  
  
> [!NOTE]  
>  Инструкция RESTORE VERIFYONLY не применяется в отношении моментальных снимков базы данных. Проверка моментального снимка базы данных перед операцией восстановления до предыдущего состояния выполняется с помощью инструкции DBCC CHECKDB.  
  
> [!NOTE]  
>  С помощью резервного копирования путем моментальных снимков RESTORE VERIFYONLY подтверждает наличие моментальных снимков в расположениях, указанных в файле резервной копии. Резервное копирование путем моментальных снимков — это новая функция в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]. Дополнительные сведения о резервном копировании путем создания моментального снимка см. в статье [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="security"></a>Безопасность  
 В операции создания резервной копии могут дополнительно указываться пароли для набора носителей, резервного набора данных или и того и другого. Если для набора носителей или резервного набора данных установлен пароль, то в инструкции RESTORE необходимо указывать правильные пароли. Эти пароли предотвращают несанкционированные операции восстановления и присоединения резервных наборов данных к носителю при помощи инструментальных средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако пароль не запрещает перезапись носителей с помощью параметра FORMAT инструкции BACKUP.  
  
> [!IMPORTANT]  
>  Данный пароль не обеспечивает надежную защиту. Он предназначен для предотвращения неверного восстановления при использовании средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] авторизованными или неавторизованными пользователями. При этом остается возможным чтение данных резервных копий с помощью других средств или замена пароля. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Рекомендуемым способом защиты резервных копий является хранение лент с резервными копиями в безопасном месте или создание резервных копий на диске в виде файлов, защищенных соответствующими списками управления доступом (ACL). Списки ACL должны располагаться в корневом каталоге, в котором создаются резервные копии.  
  
### <a name="permissions"></a>Разрешения  
 В [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях, чтобы получить сведения о резервном наборе данных или устройстве резервного копирования, необходимо разрешение CREATE DATABASE. Дополнительные сведения см. в разделе [GRANT, предоставление разрешений для базы данных (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
 
## <a name="examples"></a>Примеры  
 В следующем примере проверяется резервное копирование с диска.
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>См. также:  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [Наборы носителей, семейства носителей и резервные наборы данных (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Журнал и сведения о заголовке резервной копии (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
