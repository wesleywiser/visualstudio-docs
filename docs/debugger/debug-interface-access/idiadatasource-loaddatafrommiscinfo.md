---
description: Opens and prepares the debug data associated with the .exe/.dll file.
title: "IDiaDataSource::loadDataFromMiscInfo"
ms.date: "11/04/2016"
ms.topic: "reference"
dev_langs:
  - "C++"
helpviewer_keywords:
  - "IDiaDataSource::loadDataFromMiscInfo method"
author: "mikejo5000"
ms.author: "mikejo"
manager: mijacobs
ms.subservice: debug-diagnostics
---

# IDiaDataSource::loadDataFromMiscInfo

Opens and prepares the debug data associated with the .exe/.dll file.

## Syntax

```c++
HRESULT loadDataFromMiscInfo (
   LPCOLESTR executable,
   LPCOLESTR searchPath,
   DWORD     timeStampExe,
   DWORD     timeStampDbg,
   DWORD     sizeOfExe,
   DWORD     cbMiscInfo,
   BYTE*     pbMiscInfo,
   IUnknown* pCallback
);
```

#### Parameters

executable

[in] Path to the .exe or .dll file.

searchPath

[in] Alternate path to search for debug data.

timeStampExe

[in] Alternate timestamp for the executable image.

timeStampDbg

[in] Alternate timestamp for the debug information.

sizeOfExe

[in] Alternate size of the executable image.

cbMiscInfo

[in] Size in bytes of the `pbMiscInfo` paramter.

pbMiscInfo

[in] Alternative debug header in IMAGE_DEBUG_MISC format that provides the filename with the debug information.

pCallback

[in] An `IUnknown` interface for an object that supports a debug callback interface, such as the [IDiaLoadCallback](../../debugger/debug-interface-access/idialoadcallback.md), [IDiaLoadCallback2](../../debugger/debug-interface-access/idialoadcallback2.md), the [IDiaReadExeAtOffsetCallback](../../debugger/debug-interface-access/idiareadexeatoffsetcallback.md), and/or the [IDiaReadExeAtRVACallback](../../debugger/debug-interface-access/idiareadexeatrvacallback.md) interfaces.

## Return Value

If successful, returns `S_OK`; otherwise, returns an error code. The following table shows some of the possible error codes for this method.

|Value|Description|
|-----------|-----------------|
|E_PDB_NOT_FOUND|Failed to open the file, or the file has an invalid format.|
|E_PDB_FORMAT|Attempted to access a file with an obsolete format.|
|E_PDB_INVALID_SIG|Signature does not match.|
|E_PDB_INVALID_AGE|Age does not match.|
|E_INVALIDARG|Invalid parameter.|
|E_UNEXPECTED|Data source has already been prepared.|

## Remarks

The pbMiscInfo replaces the debug information in the executable image and names the associated debug data location. The timestamps and size are used to match the debug information.

If you are loading debug data from a symbol server, *symsrv.dll* must be present in the same directory where either the user's application or *msdia140.dll* is installed, or it must be present in the system directory.

This method searches for and prepares the debug data. The progress of the search may, optionally, be reported and controlled through callbacks. For example, the [IDiaLoadCallback::NotifyDebugDir](../../debugger/debug-interface-access/idialoadcallback-notifydebugdir.md) is invoked when the `IDiaDataSource::loadDataForExe` method finds and processes a debug directory.

The [IDiaReadExeAtOffsetCallback](../../debugger/debug-interface-access/idiareadexeatoffsetcallback.md) and [IDiaReadExeAtRVACallback](../../debugger/debug-interface-access/idiareadexeatrvacallback.md) interfaces allow the client application to provide alternative methods for reading data from the executable file when the file cannot be accessed directly through standard file I/O.

To load a .pdb file without validation, use the [IDiaDataSource::loadDataFromPdb](../../debugger/debug-interface-access/idiadatasource-loaddatafrompdb.md) method.

To validate the .pdb file against specific criteria, use the [IDiaDataSource::loadAndValidateDataFromPdb](../../debugger/debug-interface-access/idiadatasource-loadandvalidatedatafrompdb.md) method.

To load a .pdb file directly from memory, use the [IDiaDataSource::loadDataFromIStream](../../debugger/debug-interface-access/idiadatasource-loaddatafromistream.md) method.

To validate a .pdb file without loading it, use the [IDiaDataSourceEx::ValidatePdb](../../debugger/debug-interface-access/idiadatasourceex-validatepdb.md) method.

## Example

```c++
DWORD dwTimeStamp = 0x3BF1C039;
DWORD dwSizeImage = 0x0000A000;
BYTE pbMiscInfo[0x110] = {
  ...
};
HRESULT hr = pSource->loadDataFromMiscInfo( L"myprog.exe", L".\debug", dwTimeStamp, dwTimeStamp, dwSizeOfImage, sizeof(pbMiscInfo), pbMiscInfo, nullptr);
if (FAILED(hr))
{
    // Report error
}
```

## See also

- [IDiaDataSource](../../debugger/debug-interface-access/idiadatasource.md)
- [IDiaLoadCallback](../../debugger/debug-interface-access/idialoadcallback.md)
- [IDiaLoadCallback2](../../debugger/debug-interface-access/idialoadcallback2.md)
- [IDiaLoadCallback::NotifyDebugDir](../../debugger/debug-interface-access/idialoadcallback-notifydebugdir.md)
- [IDiaReadExeAtOffsetCallback](../../debugger/debug-interface-access/idiareadexeatoffsetcallback.md)
- [IDiaReadExeAtRVACallback](../../debugger/debug-interface-access/idiareadexeatrvacallback.md)
- [IDiaDataSource::loadDataFromPdb](../../debugger/debug-interface-access/idiadatasource-loaddatafrompdb.md)
- [IDiaDataSource::loadAndValidateDataFromPdb](../../debugger/debug-interface-access/idiadatasource-loadandvalidatedatafrompdb.md)
- [IDiaDataSource::loadDataFromIStream](../../debugger/debug-interface-access/idiadatasource-loaddatafromistream.md)
- [IDiaDataSourceEx::ValidatePdb](../../debugger/debug-interface-access/idiadatasourceex-validatepdb.md)
