---
title: ISymUnmanagedVariable::GetEndOffset-Methode
ms.date: 03/30/2017
api_name:
- ISymUnmanagedVariable.GetEndOffset
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedVariable::GetEndOffset
helpviewer_keywords:
- ISymUnmanagedVariable::GetEndOffset method [.NET Framework debugging]
- GetEndOffset method, ISymUnmanagedVariable interface [.NET Framework debugging]
ms.assetid: e5d777c5-d450-4c0f-999c-b3953ee22cfb
topic_type:
- apiref
ms.openlocfilehash: 4ac1fc0b3567c49dfb36d2886926bee72d62a8dd
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446068"
---
# <a name="isymunmanagedvariablegetendoffset-method"></a>ISymUnmanagedVariable::GetEndOffset-Methode
Ruft den Endoffset dieser Variablen in ihrem übergeordneten Element ab. Wenn es sich um eine lokale Variable innerhalb eines Bereichs handelt, fällt der Endoffset in die für den Bereich definierten Offsets.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT GetEndOffset(  
    [out, retval] ULONG32* pRetVal);  
```  
  
## <a name="parameters"></a>Parameter  
 `pRetVal`  
 vorgenommen Ein Zeiger auf eine `ULONG32`, die den Endoffset empfängt.  
  
## <a name="return-value"></a>Rückgabewert  
 S_OK, wenn die Methode erfolgreich ist. andernfalls E_FAIL oder ein anderer Fehlercode.  
  
## <a name="requirements"></a>Voraussetzungen  
 **Header:** Corsym. idl, corsym. h  
  
## <a name="see-also"></a>Siehe auch

- [ISymUnmanagedVariable-Schnittstelle](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-interface.md)
- [GetStartOffset-Methode](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-getstartoffset-method.md)
