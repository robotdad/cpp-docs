---
title: "CDynamicStringAccessor::GetString | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: ["cpp-windows"]
ms.tgt_pltfrm: ""
ms.topic: "reference"
f1_keywords: ["CDynamicStringAccessor.GetString", "CDynamicStringAccessor::GetString"]
dev_langs: ["C++"]
helpviewer_keywords: ["GetString method"]
ms.assetid: 4af27f27-7589-49f5-93d8-6ef05c023c8a
caps.latest.revision: 10
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
ms.workload: ["cplusplus", "data-storage"]
---
# CDynamicStringAccessor::GetString
Retrieves the specified column data as a string.  
  
## Syntax  
  
```cpp
      BaseType* GetString(DBORDINAL nColumn) const throw();  

BaseType* GetString(const CHAR* pColumnName) const throw();  

BaseType* GetString(const WCHAR* pColumnName) const throw();  
```  
  
#### Parameters  
 `nColumn`  
 [in] The column number. Column numbers start with 1. A value of 0 refers to the bookmark column, if any.  
  
 `pColumnName`  
 [in] A pointer to a character string that contains the column name.  
  
## Return Value  
 A pointer to the string value retrieved from the specified column. The value is of type ***BaseType***, which will be **CHAR** or **WCHAR** depending on whether _UNICODE is defined or not. Returns NULL if the specified column is not found.  
  
## Remarks  
 The second override form takes the column name as an ANSI string. The third override form takes the column name as a Unicode string.  
  
## Requirements  
 **Header:** atldbcli.h  
  
## See Also  
 [CDynamicStringAccessor Class](../../data/oledb/cdynamicstringaccessor-class.md)