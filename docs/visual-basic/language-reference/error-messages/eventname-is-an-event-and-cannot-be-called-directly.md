---
title: <eventname> ist ein Ereignis und kann nicht direkt aufgerufen werden.
ms.date: 07/20/2015
f1_keywords:
- bc32022
- vbc32022
helpviewer_keywords:
- BC32022
ms.assetid: 4dcfcb8d-a9fa-46a7-a034-29d9ff3a59b3
ms.openlocfilehash: bf900566bdb4ecf8d8961a12b5dd67ba426caf27
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61803311"
---
# <a name="eventname-is-an-event-and-cannot-be-called-directly"></a>"\<Ereignisname >' ist ein Ereignis aus, und nicht direkt aufgerufen werden
' <`eventname`>' ist ein Ereignis und kann daher nicht direkt aufgerufen werden. Verwenden einer `RaiseEvent` Anweisung, um ein Ereignis auszulösen.  
  
 Ein Prozeduraufruf gibt ein Ereignis für den Namen der Prozedur an. Ein Ereignishandler ist eine Prozedur, aber das Ereignis selbst ist ein Signalisierung Gerät ausgelöst und verarbeitet werden muss.  
  
 **Fehler-ID:** BC32022  
  
## <a name="to-correct-this-error"></a>So beheben Sie diesen Fehler  
  
1. Verwenden einer `RaiseEvent` Anweisung, um ein Ereignis zu signalisieren und Aufrufen der Prozedur oder die Prozeduren, die sie verarbeiten.  
  
## <a name="see-also"></a>Siehe auch

- [RaiseEvent-Anweisung](../../../visual-basic/language-reference/statements/raiseevent-statement.md)
