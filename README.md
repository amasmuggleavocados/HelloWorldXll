# Installing Requirements
* Download and Install **Excel 2013 XLL Software Development Kit**
  * If you choose a different year, make sure you apply the proper pathing below
* Visual Studio 2019 Community Edition

# Visual Studio 2019
1. Create a new solution (**File** -> **New** -> **Create New Project**)
2. Change **All Languages** -> **C++** and **All Platforms** -> **Windows** -> **Dynamic-Link Library (DLL)**
3. Under **Project name** name it **HelloWorldXll**
4. In the **Solution Explorer**, right click the **HelloWorldXll** and select **Properties**.
5. On to top of this screen **Configuration: All Configurations**, **Platform: x64**
6. Under **C/C++**
	* Click **Additional Include Directories** then the dropdown arrow, then **Edit** and finally the **New Line** icon
	  * Navigate to where you installed the XLL SDK. In my case, **C:\2013 Office System Developer Resources\Excel2013XLLSDK\INCLUDE**
7. Under **Linker** -> **Input**
	* Click **Additional Dependancies** then the dropdown arrow, then **Edit**
	    * Add the following text, **C:\2013 Office System Developer Resources\Excel2013XLLSDK\LIB\x64\XLCALL32.LIB**
8. In **pch.h**, add the following lines:
	* Earlier versions of Visual Studio (<= 2017) this file was named **stdafx.h**
    * If you are having issues including **XLCALL.H**, try changing your Build (DEBUG vs RELEASE)

```
#include "framework.h"
#include "XLCALL.h"
```

9. Under **Solution Explorer** right click **Source Files** -> **Add** -> **New Item** -> **C++ File (.cpp)**
    * Name it **HelloWorldXll.cpp**
10. In **HelloWorldXll.cpp** add the following lines:

```
#include "pch.h"
#include <stdlib.h>

short __stdcall xlAutoOpen()
{
	const char* text = "Hello world";
	size_t text_len = strlen(text);
	XLOPER message;
	message.xltype = xltypeStr;
	message.val.str = (char*)malloc(text_len + 2);
	memcpy(message.val.str + 1, text, text_len + 1);
	message.val.str[0] = (char)text_len;
	XLOPER dialog_type;
	dialog_type.xltype = xltypeInt;
	dialog_type.val.w = 2;
	Excel4(xlcAlert, NULL, 2, &message, &dialog_type);
	return 1;
}
```

11. In the **Solution Explorer**, right click **HelloWorldXll** -> **Add** -> **New Item**
    * Under **Visual C++** -> **Code** -> **Module-Definition File (.def)**
    * Change the name to **HelloWorldXll.def**

12. Change the contents of **HelloWorldXll.def** to:

```
LIBRARY

EXPORTS
    xlAutoOpen
```

You should now be able to **Build Solution**
