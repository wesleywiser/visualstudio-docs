---
description: "Returns the equivalent address for the specified virtual address (VA)."
title: "IDiaSession::addressForVA"
ms.date: "07/03/2024"
ms.topic: "reference"
dev_langs:
  - "C++"
helpviewer_keywords:
  - "IDiaSession::addressForVA method"
author: "grantri"
ms.author: "grantri"
manager: twhitney
ms.subservice: debug-diagnostics
---
# `IDiaSession::addressForVA`

Returns the equivalent address for the specified virtual address (VA).

## Syntax

```C++
HRESULT addressForVA(
    ULONGLONG va,
    DWORD* pISect,
    DWORD* pOffset);
```

#### Parameters

 `va`

[in] The virtual address to translate.

 `pISect`

[out] Returns the equivalent section for the specified address.

 `pOffset`

[out] Returns the equivalent offset within the section for the specified address.


## Return Value

 If successful, returns `S_OK`; otherwise, returns an error code.

## Example

```C++
DWORD sect = 0, offset = 0;
pSession->addressForVA( va, &sect, &offset );
```

## See also

- [IDiaSession](../../debugger/debug-interface-access/idiasession.md)
