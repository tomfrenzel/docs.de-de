---
title: Benutzerdefinierte Konvertierungsoperatoren – C#-Referenz
description: Hier erfahren Sie, wie Sie benutzerdefinierte implizite und explizite Konvertierungen in C# definieren.
ms.date: 07/09/2019
f1_keywords:
- explicit_CSharpKeyword
- implicit_CSharpKeyword
helpviewer_keywords:
- explicit keyword [C#]
- implicit keyword [C#]
- conversion operator [C#]
- user-defined conversion [C#]
ms.openlocfilehash: 5d1882048b2af12c29a3771055cbeba9565b7dab
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67787396"
---
# <a name="user-defined-conversion-operators-c-reference"></a>Benutzerdefinierte Konvertierungsoperatoren (C#-Referenz)

Ein benutzerdefinierter Typ kann eine benutzerdefinierte implizite oder explizite Konvertierung von einem oder in einen anderen Typ definieren.

Zum Aufrufen von impliziten Konvertierungen ist keine spezielle Syntax erforderlich. Implizite Konvertierungen können in verschiedenen Situationen auftreten, beispielswiese in Arbeitsaufträgen und Methodenaufrufen. Vordefinierte implizite Konvertierungen in C# werden immer erfolgreich ausgeführt. Bei deren Ausführung wird nie eine Ausnahme ausgelöst, und es gehen nie Informationen verloren. Benutzerdefinierte Konvertierungen sollten sich ebenso verhalten. Wenn bei einer benutzerdefinierten Konvertierung eine Ausnahme ausgelöst werden kann oder Informationen verloren gehen können, definieren Sie sie als explizite Konvertierung.

Benutzerdefinierte Konvertierungen werden vom [is](type-testing-and-conversion-operators.md#is-operator)- und [as](type-testing-and-conversion-operators.md#as-operator)-Operator nicht berücksichtigt. Verwenden Sie den [cast-Operator ()](type-testing-and-conversion-operators.md#cast-operator-), um eine benutzerdefinierte explizite Konvertierung aufzurufen.

Verwenden Sie zum Definieren einer impliziten bzw. expliziten Konvertierung die Schlüsselwörter `operator` und `implicit` bzw. `explicit`. Bei dem Typ, der eine Konvertierung definiert, muss es sich um einen Quelltyp oder um einen Zieltyp dieser Konvertierung handeln. Eine Konvertierung zwischen zwei benutzerdefinierten Typen kann in einem der beiden Typen definiert werden.

Im folgenden Beispiel wird gezeigt, wie eine implizite und eine explizite Konvertierung definiert wird:

[!code-csharp[implicit an explicit conversions](~/samples/csharp/language-reference/operators/UserDefinedConversions.cs)]

Sie können auch das Schlüsselwort `operator` verwenden, um einen vordefinierten C#-Operator zu überladen. Weitere Informationen finden Sie unter [Operatorüberladung](operator-overloading.md).

## <a name="c-language-specification"></a>C#-Sprachspezifikation

Weitere Informationen finden Sie in den folgenden Abschnitten der [C#-Sprachspezifikation](~/_csharplang/spec/introduction.md):

- [Konvertierungsoperatoren](~/_csharplang/spec/classes.md#conversion-operators)
- [User-defined conversions (Benutzerdefinierte Konvertierungen)](~/_csharplang/spec/conversions.md#user-defined-conversions)
- [Implicit conversions (Implizite Konvertierungen)](~/_csharplang/spec/conversions.md#implicit-conversions)
- [Explicit conversions (Explizite Konvertierungen)](~/_csharplang/spec/conversions.md#explicit-conversions)

## <a name="see-also"></a>Siehe auch

- [C#-Referenz](../index.md)
- [C#-Operatoren](index.md)
- [Operatorüberladung](operator-overloading.md)
- [Typtest- und Konvertierungsoperatoren](type-testing-and-conversion-operators.md)
- [Umwandlung und Typkonvertierung](../../programming-guide/types/casting-and-type-conversions.md)
- [Chained user-defined explicit conversions in C# (Verkettete benutzerdefinierte, explizite Konvertierungen in C#)](https://blogs.msdn.microsoft.com/ericlippert/2007/04/16/chained-user-defined-explicit-conversions-in-c/)