---
title: "How to: Marshal BSTR Strings for ADO.NET (C++/CLI) | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: ["cpp-windows"]
ms.tgt_pltfrm: ""
ms.topic: "get-started-article"
dev_langs: ["C++"]
helpviewer_keywords: ["BSTRs, strings", "ADO.NET [C++], marshaling BSTR strings", "strings [C++], marshaling BSTR strings"]
ms.assetid: 5daf4d9e-6ae8-4604-908f-855e37c8d636
caps.latest.revision: 11
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
ms.workload: ["cplusplus", "dotnet"]
---
# How to: Marshal BSTR Strings for ADO.NET (C++/CLI)
Demonstrates how to add a COM string (`BSTR`) to a database and how to marshal a <xref:System.String?displayProperty=fullName> from a database to a `BSTR`.  
  
## Example  
 In this example, the class DatabaseClass is created to interact with an ADO.NET <xref:System.Data.DataTable> object. Note that this class is a native C++ `class` (as compared to a `ref class` or `value class`). This is necessary because we want to use this class from native code, and you cannot use managed types in native code. This class will be compiled to target the CLR, as is indicated by the `#pragma managed` directive preceding the class declaration. For more information on this directive, see [managed, unmanaged](../preprocessor/managed-unmanaged.md).  
  
 Note the private member of the DatabaseClass class: `gcroot<DataTable ^> table`. Since native types cannot contain managed types, the `gcroot` keyword is necessary. For more information on `gcroot`, see [How to: Declare Handles in Native Types](../dotnet/how-to-declare-handles-in-native-types.md).  
  
 The rest of the code in this example is native C++ code, as is indicated by the `#pragma unmanaged` directive preceding `main`. In this example, we are creating a new instance of DatabaseClass and calling its methods to create a table and populate some rows in the table. Note that COM strings are being passed as values for the database column StringCol. Inside DatabaseClass, these strings are marshaled to managed strings using the marshaling functionality found in the <xref:System.Runtime.InteropServices?displayProperty=fullName> namespace. Specifically, the method <xref:System.Runtime.InteropServices.Marshal.PtrToStringBSTR%2A> is used to marshal a `BSTR` to a <xref:System.String>, and the method <xref:System.Runtime.InteropServices.Marshal.StringToBSTR%2A> is used to marshal a <xref:System.String> to a `BSTR`.  
  
> [!NOTE]
>  The memory allocated by <xref:System.Runtime.InteropServices.Marshal.StringToBSTR%2A> must be deallocated by calling either <xref:System.Runtime.InteropServices.Marshal.FreeBSTR%2A> or `SysFreeString`.  
  
```  
// adonet_marshal_string_bstr.cpp  
// compile with: /clr /FU System.dll /FU System.Data.dll /FU System.Xml.dll  
#include <comdef.h>  
#include <gcroot.h>  
#include <iostream>  
using namespace std;  
  
#using <System.Data.dll>  
using namespace System;  
using namespace System::Data;  
using namespace System::Runtime::InteropServices;  
  
#define MAXCOLS 100  
  
#pragma managed  
class DatabaseClass  
{  
public:  
    DatabaseClass() : table(nullptr) { }  
  
    void AddRow(BSTR stringColValue)  
    {  
        // Add a row to the table.  
        DataRow ^row = table->NewRow();  
        row["StringCol"] = Marshal::PtrToStringBSTR(  
            (IntPtr)stringColValue);  
        table->Rows->Add(row);  
    }  
  
    void CreateAndPopulateTable()  
    {  
        // Create a simple DataTable.  
        table = gcnew DataTable("SampleTable");  
  
        // Add a column of type String to the table.  
        DataColumn ^column1 = gcnew DataColumn("StringCol",  
            Type::GetType("System.String"));  
        table->Columns->Add(column1);  
    }  
  
    int GetValuesForColumn(BSTR dataColumn, BSTR *values,  
        int valuesLength)  
    {  
        // Marshal the name of the column to a managed  
        // String.  
        String ^columnStr = Marshal::PtrToStringBSTR(  
                (IntPtr)dataColumn);  
  
        // Get all rows in the table.  
        array<DataRow ^> ^rows = table->Select();  
        int len = rows->Length;  
        len = (len > valuesLength) ? valuesLength : len;  
        for (int i = 0; i < len; i++)  
        {  
            // Marshal each column value from a managed string  
            // to a BSTR.  
            values[i] = (BSTR)Marshal::StringToBSTR(  
                (String ^)rows[i][columnStr]).ToPointer();  
        }  
  
        return len;  
    }  
  
private:  
    // Using gcroot, you can use a managed type in  
    // a native class.  
    gcroot<DataTable ^> table;  
};  
  
#pragma unmanaged  
int main()  
{  
    // Create a table and add a few rows to it.  
    DatabaseClass *db = new DatabaseClass();  
    db->CreateAndPopulateTable();  
  
    BSTR str1 = SysAllocString(L"This is string 1.");  
    db->AddRow(str1);  
  
    BSTR str2 = SysAllocString(L"This is string 2.");  
    db->AddRow(str2);  
  
    // Now retrieve the rows and display their contents.  
    BSTR values[MAXCOLS];  
    BSTR str3 = SysAllocString(L"StringCol");  
    int len = db->GetValuesForColumn(  
        str3, values, MAXCOLS);  
    for (int i = 0; i < len; i++)  
    {  
        wcout << "StringCol: " << values[i] << endl;  
  
        // Deallocate the memory allocated using  
        // Marshal::StringToBSTR.  
        SysFreeString(values[i]);  
    }  
  
    SysFreeString(str1);  
    SysFreeString(str2);  
    SysFreeString(str3);  
    delete db;  
  
    return 0;  
}  
```  
  
```Output  
StringCol: This is string 1.  
StringCol: This is string 2.  
```  
  
## Compiling the Code  
  
-   To compile the code from the command line, save the code example in a file named adonet_marshal_string_native.cpp and enter the following statement:  
  
    ```  
    cl /clr /FU System.dll /FU System.Data.dll /FU System.Xml.dll adonet_marshal_string_native.cpp  
    ```  
  
## .NET Framework Security  
 For information on security issues involving ADO.NET, see [Securing ADO.NET Applications](/dotnet/framework/data/adonet/securing-ado-net-applications).  
  
## See Also  
 <xref:System.Runtime.InteropServices>   
 [Data Access Using ADO.NET (C++/CLI)](../dotnet/data-access-using-adonet-cpp-cli.md)   
 [ADO.NET](/dotnet/framework/data/adonet/index)   
 [Interoperability](http://msdn.microsoft.com/en-us/afcc2e7d-3f32-48d2-8141-1c42acf29084)   
 [Native and .NET Interoperability](../dotnet/native-and-dotnet-interoperability.md)