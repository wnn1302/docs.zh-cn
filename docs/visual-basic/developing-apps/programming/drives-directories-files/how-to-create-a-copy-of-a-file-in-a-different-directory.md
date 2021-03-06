---
title: "如何：在 Visual Basic 中在不同的目录中创建文件的副本"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- My.Computer.FileSystem.CopyFile method, copying files [Visual Basic]
- files, copying
- CopyFile method, copying files in Visual Basic
- I/O [Visual Basic], copying files
ms.assetid: 88e2145c-d414-45a5-ad03-6f5d58ecca26
caps.latest.revision: 17
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: ef6fcfaa38343d0fb137571b82f4d2719f3d61ef
ms.contentlocale: zh-cn
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-create-a-copy-of-a-file-in-a-different-directory-in-visual-basic"></a>如何：在 Visual Basic 中在不同的目录中创建文件的副本
使用 `My.Computer.FileSystem.CopyFile` 方法可复制文件。 该方法的参数提供了各种功能，用于覆盖现有文件、重命名文件、显示操作进度以及允许用户取消操作。  
  
### <a name="to-copy-a-text-file-to-another-folder"></a>将文本文件复制到其他文件夹  
  
-   使用 `CopyFile` 方法并指定源文件和目标目录，可以复制文件。 通过 `overwrite` 参数，可以指定是否覆盖现有文件。 下面的代码示例演示如何使用 `CopyFile`。  
  
     [!code-vb[VbFileIOMisc#24](../../../../visual-basic/developing-apps/programming/drives-directories-files/codesnippet/VisualBasic/how-to-create-a-copy-of-a-file-in-a-different-directory_1.vb)]  
  
## <a name="robust-programming"></a>可靠编程  
 以下情况可能会导致异常：  
  
-   路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
-   系统无法检索绝对路径 (<xref:System.ArgumentException>)。  
  
-   路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
-   源文件无效或不存在 (<xref:System.IO.FileNotFoundException>)。  
  
-   合并路径指向现有目录 (<xref:System.IO.IOException>)。  
  
-   存在目标文件，并且 `overwrite` 设置为 `False` (<xref:System.IO.IOException>)。  
  
-   用户没有足够的权限访问文件 (<xref:System.IO.IOException>)。  
  
-   目标文件夹中的同名文件正在使用中 (<xref:System.IO.IOException>)。  
  
-   路径中的文件名或文件夹名包含冒号 (:)，或其格式无效 (<xref:System.NotSupportedException>)。  
  
-   `ShowUI` 设置为 `True`、`onUserCancel` 设置为 `ThrowException`，并且用户已取消操作 (<xref:System.OperationCanceledException>)。  
  
-   `ShowUI` 设置为 `True`、`onUserCancel` 设置为 `ThrowException`，并出现未指定的 I/O 错误 (<xref:System.OperationCanceledException>)。  
  
-   路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
-   用户没有必需的权限 (<xref:System.UnauthorizedAccessException>)。  
  
-   该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.VisualBasic.FileIO.FileSystem>   
 <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A>   
 <xref:Microsoft.VisualBasic.FileIO.UICancelOption>   
 [如何：将具有特定模式的文件复制到目录中](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-copy-files-with-a-specific-pattern-to-a-directory.md)   
 [如何：在同一目录中创建文件副本](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-create-a-copy-of-a-file-in-the-same-directory.md)   
 [如何：将一个目录复制到另一个目录](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-copy-a-directory-to-another-directory.md)   
 [如何：重命名文件](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-rename-a-file.md)

