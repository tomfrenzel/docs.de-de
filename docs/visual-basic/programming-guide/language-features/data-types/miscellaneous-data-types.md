---
title: Sonstige Datentypen
ms.date: 07/20/2015
helpviewer_keywords:
- Object data type [Visual Basic], data types
- data types [Visual Basic], choosing
ms.assetid: 64c71a12-9057-4dbf-baca-7379c4aada69
ms.openlocfilehash: cc6262b5bb305bb839917e222d831fa3340a1b14
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346338"
---
# <a name="miscellaneous-data-types-visual-basic"></a>Sonstige Datentypen (Visual Basic)
Visual Basic stellt mehrere Datentypen bereit, die nicht an Zahlen oder Zeichen gerichtet sind. Stattdessen behandeln Sie spezialisierte Daten, z. b. Yes/No-Werte, Datums-/Uhrzeitwerte und Objektadressen.  
  
 Eine Tabelle, die einen parallelen Vergleich der Visual Basic-Datentypen anzeigt, finden Sie unter [Datentypen](../../../../visual-basic/language-reference/data-types/index.md).  
  
## <a name="boolean-type"></a>Boolescher Typ  
 Der [boolesche Datentyp](../../../../visual-basic/language-reference/data-types/boolean-data-type.md) ist ein nicht signierter Wert, der entweder als `True` oder `False`interpretiert wird. Die Daten Breite hängt von der implementierenden Plattform ab. Wenn eine Variable nur zwei Zustands Werte enthalten kann, z. b. true/false, Yes/No oder on/off, deklarieren Sie Sie als `Boolean`.  
  
## <a name="date-type"></a>Date-Typ  
 Der [Date-Datentyp](../../../../visual-basic/language-reference/data-types/date-data-type.md) ist ein 64-Bit-Wert, der Datums-und Uhrzeit Informationen enthält. Jedes Inkrement stellt 100 Nanosekunden der verstrichenen Zeit seit dem Beginn (12:00 Uhr) des 1. Januar des Jahres 1 des Jahres 1 im gregorianischen Kalender dar. Wenn eine Variable einen Datumswert, einen Uhrzeitwert oder beides enthalten kann, deklarieren Sie Sie als `Date`.  
  
## <a name="object-type"></a>Objekttyp  
 Der [Objekt Datentyp](../../../../visual-basic/language-reference/data-types/object-data-type.md) ist eine 32-Bit-Adresse, die auf eine Objektinstanz in Ihrer Anwendung oder in einer anderen Anwendung zeigt. Eine `Object` Variable kann auf jedes Objekt verweisen, das von der Anwendung erkannt wird, oder auf Daten eines beliebigen Datentyps. Dies schließt sowohl *Werttypen*wie z. b. `Integer`-, `Boolean`-und Struktur Instanzen als auch *Verweis Typen*ein, bei denen es sich um Instanzen von Objekten handelt, die aus Klassen wie `String` und <xref:System.Windows.Forms.Form>erstellt wurden, und Array Instanzen.  
  
 Wenn eine Variable einen Zeiger auf eine Instanz einer Klasse speichert, die Sie zum Zeitpunkt der Kompilierung nicht kennen oder auf Daten verschiedener Datentypen verweisen können, deklarieren Sie Sie als `Object`.  
  
 Der Vorteil des Datentyps `Object` ist, dass Sie ihn zum Speichern von Daten eines beliebigen Datentyps verwenden können. Der Nachteil besteht darin, dass Sie zusätzliche Vorgänge verursachen, bei denen die Ausführungszeit länger dauert und die Anwendung langsamer ausgeführt wird. Wenn Sie eine `Object` Variable für Werttypen verwenden, entstehen *Boxing* und *Unboxing*. Wenn Sie Sie für Verweis Typen verwenden, wird eine *späte Bindung*verursacht.  
  
## <a name="see-also"></a>Siehe auch

- [Typzeichen](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [Elementare Datentypen](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [Numerische Datentypen](../../../../visual-basic/programming-guide/language-features/data-types/numeric-data-types.md)
- [Zeichendatentypen](../../../../visual-basic/programming-guide/language-features/data-types/character-data-types.md)
- [Problembehandlung bei Datentypen](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Frühes und spätes Binden](../../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
