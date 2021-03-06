---
title: "CDataConnection::Copy | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: ["cpp-windows"]
ms.tgt_pltfrm: ""
ms.topic: "reference"
f1_keywords: ["CDataConnection.Copy", "ATL.CDataConnection.Copy", "ATL::CDataConnection::Copy", "CDataConnection::Copy"]
dev_langs: ["C++"]
helpviewer_keywords: ["Copy method"]
ms.assetid: a3dbd70d-36be-49e0-a527-00e3910a7a56
caps.latest.revision: 8
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
ms.workload: ["cplusplus", "data-storage"]
---
# CDataConnection::Copy
Creates a copy of an existing data connection.  
  
## Syntax  
  
```cpp
      CDataConnection& Copy(const CDataConnection & ds) throw();  
```  
  
#### Parameters  
 `ds`  
 [in] A reference to an existing data connection to copy.  
  
## Requirements  
 **Header:** atldbcli.h  
  
## See Also  
 [CDataConnection Class](../../data/oledb/cdataconnection-class.md)