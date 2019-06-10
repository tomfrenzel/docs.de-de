---
title: 'Bitweise und Schiebeoperatoren: C#-Referenz'
description: Enthält Informationen zu C#-Operatoren, die bitweise bzw. Schiebeoperationen mit Operanden mit integralen Typen durchführen.
ms.date: 04/18/2019
author: pkulikov
f1_keywords:
- ~_CSharpKeyword
- <<_CSharpKeyword
- '>>_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
helpviewer_keywords:
- bitwise logical operators [C#]
- shift operators [C#]
- tilde operator [C#]
- one's complement operator [C#]
- bitwise complement operator [C#]
- ~ operator [C#]
- left shift operator [C#]
- << operator [C#]
- right shift operator [C#]
- '>> operator [C#]'
- bitwise logical AND operator [C#]
- ampersand operator [C#]
- '& operator [C#]'
- bitwise logical exclusive OR operator [C#]
- bitwise logical XOR operator [C#]
- ^ operator [C#]
- bitwise logical OR operator [C#]
- '| operator [C#]'
ms.openlocfilehash: bf42a53a89676f457d3d2df8d193a83299c3e4cc
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758369"
---
# <a name="bitwise-and-shift-operators-c-reference"></a>Bitweise und Schiebeoperatoren (C#-Referenz)

Mit den folgenden Operatoren werden bitweise oder Schiebeoperationen mit Operanden durchgeführt, die [integrale Typen](../keywords/integral-types-table.md) aufweisen:

- Unärer Operator [`~` (bitweises Komplement)](#bitwise-complement-operator-)
- Binäre Schiebeoperatoren [`<<` (Linksverschiebung)](#left-shift-operator-) und [`>>` (Rechtsverschiebung)](#right-shift-operator-)
- Binäre Operatoren [`&` (logisches UND)](#logical-and-operator-), [`|` (logisches ODER)](#logical-or-operator-) und [`^` (logisches exklusives ODER)](#logical-exclusive-or-operator-)

Diese Operatoren werden für die Typen `int`, `uint`, `long` und `ulong` definiert. Wenn beide Operanden andere integrale Typen aufweisen (`sbyte`, `byte`, `short`, `ushort` oder `char`), werden ihre Werte in den Typ `int` konvertiert. Hierbei handelt es sich auch um den Ergebnistyp einer Operation. Wenn die Operanden abweichende integrale Typen aufweisen, werden ihre Werte in den enthaltenden integralen Typ konvertiert, der am besten geeignet ist. Weitere Informationen finden Sie im Abschnitt [Numerische Heraufstufungen](~/_csharplang/spec/expressions.md#numeric-promotions) der [Spezifikation für die Sprache C#](~/_csharplang/spec/introduction.md).

Die Operatoren `&`, `|` und `^` werden auch für die Operanden des Typs `bool` definiert. Weitere Informationen finden Sie unter [Logische boolesche Operatoren](boolean-logical-operators.md).

Bitweise und Schiebeoperationen verursachen niemals Überläufe und führen sowohl in [geprüften als auch in ungeprüften](../keywords/checked-and-unchecked.md) Kontexten zu identischen Ergebnissen.

## <a name="bitwise-complement-operator-"></a>Bitweiser Komplementoperator ~

Mit dem Operator `~` wird ein bitweises Komplement seines Operanden erzeugt, indem jedes Bit umgekehrt wird:

[!code-csharp-interactive[bitwise NOT](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#BitwiseComplement)]

Sie können das Symbol `~` auch verwenden, um Finalizers zu deklarieren. Weitere Informationen finden Sie unter [Finalizer](../../programming-guide/classes-and-structs/destructors.md).

## <a name="left-shift-operator-"></a>Operator für Linksverschiebung \<\<

Mit dem Operator `<<` wird der erste Operand um die Anzahl von Bits nach links verschoben, die durch den zweiten Operanden angegeben wird.

Bei der Operation zum Verschieben nach links werden die hohen Bits, die außerhalb des Bereichs des Ergebnistyps liegen, verworfen, und die niedrigen leeren Bitpositionen werden auf null festgelegt. Dies ist im folgenden Beispiel dargestellt:

[!code-csharp-interactive[left shift](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#LeftShift)]

Da die Verschiebeoperatoren nur für die Typen `int`, `uint`, `long` und `ulong` definiert werden, enthält das Ergebnis einer Operation immer mindestens 32 Bit. Wenn der erste Operand einen abweichenden integralen Typ aufweist (`sbyte`, `byte`, `short`, `ushort` oder `char`), wird sein Wert in den Typ `int` konvertiert. Dies ist im folgenden Beispiel dargestellt:

[!code-csharp-interactive[left shift with promotion](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#LeftShiftPromoted)]

Informationen dazu, wie der zweite Operand des Operators `<<` die Anzahl für die Verschiebung definiert, finden Sie im Abschnitt [Anzahl für die Verschiebung durch Schiebeoperatoren](#shift-count-of-the-shift-operators).

## <a name="right-shift-operator-"></a>Operator für Rechtsverschiebung >>

Mit dem Operator `>>` wird der erste Operand um die Anzahl von Bits nach rechts verschoben, die durch den zweiten Operanden angegeben wird.

Bei der Operation zum Verschieben nach rechts werden die niedrigen Bits verworfen. Dies ist im folgenden Beispiel dargestellt:

[!code-csharp-interactive[right shift](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#RightShift)]

Die höheren leeren Bitpositionen werden basierend auf dem Typ des ersten Operanden wie folgt festgelegt:

- Wenn der erste Operand vom Typ [int](../keywords/int.md) oder [long](../keywords/long.md) ist, führt der Operator zur Rechtsverschiebung eine *arithmetische* Verschiebung durch: Der Wert des Bits mit dem höchsten Stellenwert (MSB, „most significant bit“) des ersten Operanden wird auf die hohen leeren Bitpositionen übertragen. Die hohen leeren Bitpositionen werden daher auf 0 festgelegt, wenn der erste Operand nicht negativ ist, bzw. auf 1, wenn der erste Operand negativ ist.

  [!code-csharp-interactive[arithmetic right shift](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#ArithmeticRightShift)]

- Wenn der erste Operand vom Typ [uint](../keywords/uint.md) oder [ulong](../keywords/ulong.md) ist, führt der Operator zur Rechtsverschiebung eine *logische* Verschiebung durch: Die hohen leeren Bitpositionen werden immer auf 0 (null) festgelegt.

  [!code-csharp-interactive[logical right shift](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#LogicalRightShift)]

Informationen dazu, wie der zweite Operand des Operators `>>` die Anzahl für die Verschiebung definiert, finden Sie im Abschnitt [Anzahl für die Verschiebung durch Schiebeoperatoren](#shift-count-of-the-shift-operators).

## <a name="logical-and-operator-amp"></a>Logischer AND-Operator &amp;

Mit dem Operator `&` wird „bitweises logisches UND“ für die Operanden berechnet:

[!code-csharp-interactive[bitwise AND](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#BitwiseAnd)]

Für die Operanden vom Typ `bool` berechnet der Operator `&` [logisches UND](boolean-logical-operators.md#logical-and-operator-) für die Operanden. Der unäre `&`-Operator ist der [address-of-Operator](pointer-related-operators.md#address-of-operator-).

## <a name="logical-exclusive-or-operator-"></a>Logischer exklusiver OR-Operator: ^

Mit dem Operator `^` wird „bitweises logisches exklusives ODER“, auch als „bitweises logisches XOR“ bezeichnet, seiner Operanden berechnet:

[!code-csharp-interactive[bitwise XOR](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#BitwiseXor)]

Für die Operanden vom Typ `bool` berechnet der Operator `^` [logisches exklusives ODER](boolean-logical-operators.md#logical-exclusive-or-operator-) für die Operanden.

## <a name="logical-or-operator-"></a>Logischer OR-Operator: |

Mit dem Operator `|` wird „bitweises logisches ODER“ der Operanden berechnet:

[!code-csharp-interactive[bitwise OR](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#BitwiseOr)]

Für die Operanden vom Typ `bool` berechnet der Operator `|` [logisches ODER](boolean-logical-operators.md#logical-or-operator-) für die Operanden.

## <a name="compound-assignment"></a>Verbundzuweisung

Bei einem binären Operator `op` entspricht ein Verbundzuweisungsausdruck der Form

```csharp
x op= y
```

für die folgende Syntax:

```csharp
x = x op y
```

außer dass `x` nur einmal überprüft wird.

Im folgenden Beispiel wird die Verwendung von Verbundzuweisungen mit bitweisen und Schiebeoperatoren veranschaulicht:

[!code-csharp-interactive[compound assignment](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#CompoundAssignment)]

Aufgrund von [numerischen Höherstufungen](~/_csharplang/spec/expressions.md#numeric-promotions) kann das Ergebnis der Operation `op` ggf. nicht implizit in den Typ `T` von `x` konvertiert werden. In diesem Fall gilt Folgendes: Wenn `op` ein vordefinierter Operator ist und das Ergebnis der Operation explizit in den Typ `T` von `x` konvertiert werden kann, entspricht ein Verbundzuweisungsausdruck der Form `x op= y` dem Ausdruck `x = (T)(x op y)`. Der einzige Unterschied ist, dass `x` nur einmal ausgewertet wird. Das folgende Beispiel veranschaulicht dieses Verhalten:

[!code-csharp-interactive[compound assignment with cast](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#CompoundAssignmentWithCast)]

## <a name="operator-precedence"></a>Operatorrangfolge

In der folgenden Liste sind die bitweisen und Schiebeoperatoren absteigend nach Rangfolge sortiert:

- Bitweiser Komplementoperator `~`
- Schiebeoperatoren `<<` und `>>`
- Logischer AND-Operator `&`
- Logischer exklusiver OR-Operator `^`
- Logischer OR-Operator `|`

Verwenden Sie Klammern `()`, wenn Sie die Reihenfolge der Auswertung ändern möchten, die durch Operatorrangfolge festgelegt wird:

[!code-csharp-interactive[operator precedence](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#Precedence)]

Die vollständige Liste der nach Rangfolgenebene sortierten C#-Operatoren finden Sie unter [C#-Operatoren](index.md).

## <a name="shift-count-of-the-shift-operators"></a>Anzahl für die Verschiebung durch Schiebeoperatoren

Für die Schiebeoperatoren `<<` und `>>` muss der Typ des zweiten Operanden [int](../keywords/int.md) lauten oder ein Typ sein, der eine [vordefinierte implizite numerische Konvertierung](../keywords/implicit-numeric-conversions-table.md) in `int` aufweist.

Für die Ausdrücke `x << count` und `x >> count` hängt die tatsächliche Verschiebungsanzahl wie folgt vom Typ von `x` ab:

- Lautet der Typ von `x` [int](../keywords/int.md) oder [uint](../keywords/uint.md), wird die Verschiebungsanzahl durch die niedrigen *fünf* Bits des zweiten Operanden definiert. Die Verschiebungsanzahl errechnet sich daher aus `count & 0x1F` (oder `count & 0b_1_1111`).

- Lautet der Typ von `x` [long](../keywords/long.md) oder [ulong](../keywords/ulong.md), wird die Verschiebungsanzahl durch die niedrigen *sechs* Bits des zweiten Operanden definiert. Die Verschiebungsanzahl errechnet sich daher aus `count & 0x3F` (oder `count & 0b_11_1111`).

Das folgende Beispiel veranschaulicht dieses Verhalten:

[!code-csharp-interactive[shift count example](~/samples/csharp/language-reference/operators/BitwiseAndShiftOperators.cs#ShiftCount)]

## <a name="enumeration-logical-operators"></a>Logische Enumerationsoperatoren

Die Operatoren `~`, `&`, `|` und `^` werden auch für jeden [Enumerationstyp](../keywords/enum.md) definiert. Für die Operanden mit dem gleichen Enumerationstyp wird eine logische Operation für die entsprechenden Werte des zugrunde liegenden integralen Typs durchgeführt. Für alle `x`- und `y`-Elemente des Enumerationstyps `T` mit dem zugrunde liegenden Typ `U` führt der Ausdruck `x & y` zum gleichen Ergebnis wie der Ausdruck `(T)((U)x & (U)y)`.

Normalerweise verwenden Sie bitweise logische Operatoren mit einem Enumerationstyp, der mit dem [Flags](xref:System.FlagsAttribute)-Attribut definiert wird. Weitere Informationen finden Sie im Abschnitt [Enumerationstypen als Bitflags](../../programming-guide/enumeration-types.md#enumeration-types-as-bit-flags) des Artikels [Enumerationstypen](../../programming-guide/enumeration-types.md).

## <a name="operator-overloadability"></a>Operatorüberladbarkeit

Ein benutzerdefinierter Typ kann die Operatoren `~`, `<<`, `>>`, `&`, `|` und `^` [überladen](../keywords/operator.md). Wenn ein binärer Operator überladen ist, wird der zugehörige Verbundzuweisungsoperator implizit auch überladen. Ein benutzerdefinierter Typ kann einen Verbundzuweisungsoperator nicht explizit überladen.

Wenn ein benutzerdefinierter Typ `T` den Operator `<<` oder `>>` überlädt, muss der Typ des ersten Operanden `T` und der Typ des zweiten Operanden `int` lauten.

## <a name="c-language-specification"></a>C#-Sprachspezifikation

Weitere Informationen finden Sie in den folgenden Abschnitten der [C#-Sprachspezifikation](~/_csharplang/spec/introduction.md):

- [Bitweiser Komplementoperator](~/_csharplang/spec/expressions.md#bitwise-complement-operator)
- [Shift operators (Schiebeoperatoren)](~/_csharplang/spec/expressions.md#shift-operators)
- [Logical operators (Logische Operatoren)](~/_csharplang/spec/expressions.md#logical-operators)
- [Verbundzuweisung](~/_csharplang/spec/expressions.md#compound-assignment)
- [Numerische Heraufstufungen](~/_csharplang/spec/expressions.md#numeric-promotions)

## <a name="see-also"></a>Siehe auch

- [C#-Referenz](../index.md)
- [C#-Programmierhandbuch](../../programming-guide/index.md)
- [C#-Operatoren](index.md)
- [Logische boolesche Operatoren](boolean-logical-operators.md)