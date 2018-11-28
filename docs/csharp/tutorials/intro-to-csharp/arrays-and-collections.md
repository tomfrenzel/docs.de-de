---
title: Arbeiten mit Sammlungen – Tutorial zur Einführung in C#
description: Machen Sie sich mit C# vertraut, indem Sie die Listenauflistung in diesem Tutorial erkunden.
ms.date: 10/13/2017
ms.custom: mvc
ms.openlocfilehash: eaf921be2bd50b6e346f57f42e17f151ff336821
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205281"
---
# <a name="learn-to-manage-data-collections-using-the-generic-list-type"></a><span data-ttu-id="f5325-103">Informationen zum Verwalten von Datensammlungen mithilfe des generischen Listentyps</span><span class="sxs-lookup"><span data-stu-id="f5325-103">Learn to manage data collections using the generic list type</span></span>

<span data-ttu-id="f5325-104">Dieses Tutorial bietet eine Einführung in die Sprache C# und die Grundlagen der <xref:System.Collections.Generic.List%601>-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f5325-104">This introductory tutorial provides an introduction to the C# language and the basics of the <xref:System.Collections.Generic.List%601> class.</span></span>

<span data-ttu-id="f5325-105">Für dieses Tutorial benötigen Sie einen Computer, den Sie für die Entwicklung nutzen können.</span><span class="sxs-lookup"><span data-stu-id="f5325-105">This tutorial expects you to have a machine you can use for development.</span></span> <span data-ttu-id="f5325-106">Das .NET-Thema [Erste Schritte in 10 Minuten](https://www.microsoft.com/net/core) umfasst Anweisungen zum Einrichten Ihrer lokalen Entwicklungsumgebung auf einem Mac-, Windows- oder Linux-PC.</span><span class="sxs-lookup"><span data-stu-id="f5325-106">The .NET topic [Get Started in 10 minutes](https://www.microsoft.com/net/core) has instructions for setting up your local development environment on Mac, PC or Linux.</span></span> <span data-ttu-id="f5325-107">Einen schnellen Überblick über die Befehle, die Sie verwenden werden, finden Sie im Artikel [Einführung in Entwicklungstools](local-environment.md), der Links zu weiteren Ressourcen enthält.</span><span class="sxs-lookup"><span data-stu-id="f5325-107">A quick overview of the commands you'll use is in [Become familiar with the development tools](local-environment.md), with links to more details.</span></span>

## <a name="a-basic-list-example"></a><span data-ttu-id="f5325-108">Beispiel für eine einfache Liste</span><span class="sxs-lookup"><span data-stu-id="f5325-108">A basic list example</span></span>

<span data-ttu-id="f5325-109">Erstellen Sie ein Verzeichnis mit dem Namen **list-tutorial**.</span><span class="sxs-lookup"><span data-stu-id="f5325-109">Create a directory named **list-tutorial**.</span></span> <span data-ttu-id="f5325-110">Machen Sie dieses Verzeichnis zum aktuellen Verzeichnis, und führen Sie `dotnet new console` aus.</span><span class="sxs-lookup"><span data-stu-id="f5325-110">Make that the current directory and run `dotnet new console`.</span></span>

> [!NOTE]
> <span data-ttu-id="f5325-111">Wenn Sie gerade [Erste Schritte mit .NET in 10 Minuten](https://www.microsoft.com/net) abgeschlossen haben, können Sie die gerade erstellte myApp-Anwendung weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="f5325-111">If you just completed [Get started with .NET in 10 minutes](https://www.microsoft.com/net), you can keep using the myApp application that you just created.</span></span>

<span data-ttu-id="f5325-112">Öffnen Sie **Program.cs** in Ihrem bevorzugten Editor, und ersetzen Sie den vorhandenen Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f5325-112">Open **Program.cs** in your favorite editor, and replace the existing code with the following:</span></span>

```csharp
using System;
using System.Collections.Generic;

namespace list_tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            var names = new List<string> { "<name>", "Ana", "Felipe" };
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }
        }
    }
}
```

<span data-ttu-id="f5325-113">Ersetzen Sie `<name>` durch Ihren eigenen Namen.</span><span class="sxs-lookup"><span data-stu-id="f5325-113">Replace `<name>` with your name.</span></span> <span data-ttu-id="f5325-114">Speichern Sie **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="f5325-114">Save **Program.cs**.</span></span> <span data-ttu-id="f5325-115">Geben Sie `dotnet run` in Ihrem Konsolenfenster ein, um es zu testen.</span><span class="sxs-lookup"><span data-stu-id="f5325-115">Type `dotnet run` in your console window to try it.</span></span>

<span data-ttu-id="f5325-116">Sie haben gerade eine Liste von Zeichenfolgen erstellt, dieser Liste drei Namen hinzugefügt und die Namen in GROSSBUCHSTABEN ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="f5325-116">You've just created a list of strings, added three names to that list, and printed out the names in all CAPS.</span></span> <span data-ttu-id="f5325-117">Sie verwenden Konzepte, die Sie in früheren Tutorials kennengelernt haben, um die Liste zu durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="f5325-117">You're using concepts that you've learned in earlier tutorials to loop through the list.</span></span>

<span data-ttu-id="f5325-118">Im Code zum Anzeigen von Namen wird das Feature [Zeichenfolgeninterpolation](../../language-reference/tokens/interpolated.md) genutzt.</span><span class="sxs-lookup"><span data-stu-id="f5325-118">The code to display names makes use of the [string interpolation](../../language-reference/tokens/interpolated.md) feature.</span></span>  <span data-ttu-id="f5325-119">Wenn Sie einem `string` ein `$`-Zeichen voranstellen, können Sie C#-Code in die Zeichenfolgendeklaration einbetten.</span><span class="sxs-lookup"><span data-stu-id="f5325-119">When you precede a `string` with the `$` character, you can embed C# code in the string declaration.</span></span> <span data-ttu-id="f5325-120">Der Wert, den dieser C#-Code generiert, ist eine Zeichenfolge, durch die der C#-Code ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="f5325-120">The actual string replaces that C# code with the value it generates.</span></span> <span data-ttu-id="f5325-121">In diesem Beispiel wird `{name.ToUpper()}` mit dem jeweiligen in Großbuchstaben konvertierten Namen ersetzt, da Sie die <xref:System.String.ToUpper%2A>-Methode aufgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="f5325-121">In this example, it replaces the `{name.ToUpper()}` with each name, converted to capital letters, because you called the <xref:System.String.ToUpper%2A> method.</span></span>

<span data-ttu-id="f5325-122">Setzen wir nun unsere Forschungen fort.</span><span class="sxs-lookup"><span data-stu-id="f5325-122">Let's keep exploring.</span></span>

## <a name="modify-list-contents"></a><span data-ttu-id="f5325-123">Ändern von Listeninhalten</span><span class="sxs-lookup"><span data-stu-id="f5325-123">Modify list contents</span></span>

<span data-ttu-id="f5325-124">Die Sammlung, die Sie erstellt haben, nutzt den <xref:System.Collections.Generic.List%601>-Typ.</span><span class="sxs-lookup"><span data-stu-id="f5325-124">The collection you created uses the <xref:System.Collections.Generic.List%601> type.</span></span> <span data-ttu-id="f5325-125">Dieser Typ speichert Elementsequenzen.</span><span class="sxs-lookup"><span data-stu-id="f5325-125">This type stores sequences of elements.</span></span> <span data-ttu-id="f5325-126">Sie geben den Typ der Elemente zwischen den spitzen Klammern an.</span><span class="sxs-lookup"><span data-stu-id="f5325-126">You specify the type of the elements between the angle brackets.</span></span>

<span data-ttu-id="f5325-127">Ein wichtiger Aspekt dieses <xref:System.Collections.Generic.List%601>-Typs ist, dass er wachsen oder schrumpfen kann, sodass Sie Elemente hinzufügen oder entfernen können.</span><span class="sxs-lookup"><span data-stu-id="f5325-127">One important aspect of this <xref:System.Collections.Generic.List%601> type is that it can grow or shrink, enabling you to add or remove elements.</span></span> <span data-ttu-id="f5325-128">Fügen Sie diesen Code vor der abschließenden `}` in die `Main`-Methode ein:</span><span class="sxs-lookup"><span data-stu-id="f5325-128">Add this code before the closing `}` in the `Main` method:</span></span>

```csharp
Console.WriteLine();
names.Add("Maria");
names.Add("Bill");
names.Remove("Ana");
foreach (var name in names)
{
    Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

<span data-ttu-id="f5325-129">Sie haben am Ende der Liste zwei weitere Namen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f5325-129">You've added two more names to the end of the list.</span></span> <span data-ttu-id="f5325-130">Sie haben auch einen entfernt.</span><span class="sxs-lookup"><span data-stu-id="f5325-130">You've also removed one as well.</span></span> <span data-ttu-id="f5325-131">Speichern Sie die Datei, und geben Sie `dotnet run` zum Testen ein.</span><span class="sxs-lookup"><span data-stu-id="f5325-131">Save the file, and type `dotnet run` to try it.</span></span>

<span data-ttu-id="f5325-132"><xref:System.Collections.Generic.List%601> ermöglicht Ihnen auch, mithilfe des **Indexes** auf einzelne Elemente zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="f5325-132">The <xref:System.Collections.Generic.List%601> enables you to reference individual items by **index** as well.</span></span> <span data-ttu-id="f5325-133">Platzieren Sie den Index hinter dem Listennamen zwischen den Zeichen `[` und `]`.</span><span class="sxs-lookup"><span data-stu-id="f5325-133">You place the index between `[` and `]` tokens following the list name.</span></span> <span data-ttu-id="f5325-134">C# verwendet 0 für den ersten Index.</span><span class="sxs-lookup"><span data-stu-id="f5325-134">C# uses 0 for the first index.</span></span> <span data-ttu-id="f5325-135">Fügen Sie diesen Code direkt unterhalb des Codes hinzu, den Sie gerade hinzugefügt haben, und probieren Sie es aus:</span><span class="sxs-lookup"><span data-stu-id="f5325-135">Add this code directly below the code you just added and try it:</span></span>

```csharp
Console.WriteLine($"My name is {names[0]}");
Console.WriteLine($"I've added {names[2]} and {names[3]} to the list");
```

<span data-ttu-id="f5325-136">Sie können nicht auf einen Index zugreifen, der hinter dem Ende der Liste liegt.</span><span class="sxs-lookup"><span data-stu-id="f5325-136">You cannot access an index beyond the end of the list.</span></span> <span data-ttu-id="f5325-137">Denken Sie daran, dass die Indizes mit 0 (null) beginnen, sodass der größte gültige Index um eins kleiner ist als die Anzahl der Elemente in der Liste.</span><span class="sxs-lookup"><span data-stu-id="f5325-137">Remember that indices start at 0, so the largest valid index is one less than the number of items in the list.</span></span> <span data-ttu-id="f5325-138">Sie können mit der <xref:System.Collections.Generic.List%601.Count%2A>-Eigenschaft überprüfen, wie lang die Liste ist.</span><span class="sxs-lookup"><span data-stu-id="f5325-138">You can check how long the list is using the <xref:System.Collections.Generic.List%601.Count%2A> property.</span></span> <span data-ttu-id="f5325-139">Fügen Sie den folgenden Code am Ende der Main-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="f5325-139">Add the following code at the end of the Main method:</span></span>

```csharp
Console.WriteLine($"The list has {names.Count} people in it");
 ```

<span data-ttu-id="f5325-140">Speichern Sie die Datei, und geben Sie `dotnet run` erneut ein, um die Ergebnisse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5325-140">Save the file, and type `dotnet run` again to see the results.</span></span>

## <a name="search-and-sort-lists"></a><span data-ttu-id="f5325-141">Suchen in und Sortieren von Listen</span><span class="sxs-lookup"><span data-stu-id="f5325-141">Search and sort lists</span></span>

<span data-ttu-id="f5325-142">In unseren Beispielen werden relativ kleine Listen verwendet, aber Ihre Anwendungen erstellen möglicherweise häufig Listen mit viel mehr Elementen, die manchmal in die Tausende gehen.</span><span class="sxs-lookup"><span data-stu-id="f5325-142">Our samples use relatively small lists, but your applications may often create lists with many more elements, sometimes numbering in the thousands.</span></span> <span data-ttu-id="f5325-143">Um in diesen größeren Sammlungen Elemente zu finden, müssen Sie die Liste nach verschiedenen Elementen durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="f5325-143">To find elements in these larger collections, you need to search the list for different items.</span></span> <span data-ttu-id="f5325-144">Die <xref:System.Collections.Generic.List%601.IndexOf%2A>-Methode sucht nach einem Element und gibt den Index des Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="f5325-144">The <xref:System.Collections.Generic.List%601.IndexOf%2A> method searches for an item and returns the index of the item.</span></span> <span data-ttu-id="f5325-145">Fügen Sie diesen Code am Ende der `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="f5325-145">Add this code to the bottom of your `Main` method:</span></span>

```csharp
var index = names.IndexOf("Felipe");
if (index == -1)
{
    Console.WriteLine($"When an item is not found, IndexOf returns {index}");
} else
{
    Console.WriteLine($"The name {names[index]} is at index {index}");
}

index = names.IndexOf("Not Found");
if (index == -1)
{
    Console.WriteLine($"When an item is not found, IndexOf returns {index}");
} else
{
    Console.WriteLine($"The name {names[index]} is at index {index}");

}
```

<span data-ttu-id="f5325-146">Die Elemente in der Liste können auch sortiert werden.</span><span class="sxs-lookup"><span data-stu-id="f5325-146">The items in your list can be sorted as well.</span></span> <span data-ttu-id="f5325-147">Die <xref:System.Collections.Generic.List%601.Sort%2A>-Methode sortiert alle Elemente in der Liste in der normalen Reihenfolge (Zeichenfolgen alphabetisch).</span><span class="sxs-lookup"><span data-stu-id="f5325-147">The <xref:System.Collections.Generic.List%601.Sort%2A> method sorts all the items in the list in their normal order (alphabetically in the case of strings).</span></span> <span data-ttu-id="f5325-148">Fügen Sie diesen Code am Ende der `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="f5325-148">Add this code to the bottom of our `Main` method:</span></span>

```csharp
names.Sort();
foreach (var name in names)
{
    Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

<span data-ttu-id="f5325-149">Speichern Sie die Datei, und geben Sie `dotnet run` ein, um diese neueste Version zu testen.</span><span class="sxs-lookup"><span data-stu-id="f5325-149">Save the file and type `dotnet run` to try this latest version.</span></span>

<span data-ttu-id="f5325-150">Bevor Sie mit dem nächsten Abschnitt beginnen, verschieben wir den aktuellen Code in eine separate Methode.</span><span class="sxs-lookup"><span data-stu-id="f5325-150">Before you start the next section, let's move the current code into a separate method.</span></span> <span data-ttu-id="f5325-151">Dies erleichtert das Arbeiten mit einem neuen Beispiel.</span><span class="sxs-lookup"><span data-stu-id="f5325-151">That makes it easier to start working with a new example.</span></span> <span data-ttu-id="f5325-152">Benennen Sie Ihre `Main` -Methode in `WorkingWithStrings` um, und schreiben Sie eine neue `Main`-Methode, die `WorkingWithStrings` aufruft.</span><span class="sxs-lookup"><span data-stu-id="f5325-152">Rename your `Main` method to `WorkingWithStrings` and write a new `Main` method that calls `WorkingWithStrings`.</span></span> <span data-ttu-id="f5325-153">Anschließend sollte der Code wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="f5325-153">When you have finished, your code should look like this:</span></span>

```csharp
using System;
using System.Collections.Generic;

namespace list_tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkingWithStrings();
        }

        public static void WorkingWithStrings()
        {
            var names = new List<string> { "<name>", "Ana", "Felipe" };
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }

            Console.WriteLine();
            names.Add("Maria");
            names.Add("Bill");
            names.Remove("Ana");
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }

            Console.WriteLine($"My name is {names[0]}");
            Console.WriteLine($"I've added {names[2]} and {names[3]} to the list");

            Console.WriteLine($"The list has {names.Count} people in it");

            var index = names.IndexOf("Felipe");
            Console.WriteLine($"The name {names[index]} is at index {index}");

            var notFound = names.IndexOf("Not Found");
            Console.WriteLine($"When an item is not found, IndexOf returns {notFound}");

            names.Sort();
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }
        }
    }
}
```

## <a name="lists-of-other-types"></a><span data-ttu-id="f5325-154">Listen mit anderen Typen</span><span class="sxs-lookup"><span data-stu-id="f5325-154">Lists of other types</span></span>

<span data-ttu-id="f5325-155">Sie haben bisher den `string`-Typ in Listen verwendet.</span><span class="sxs-lookup"><span data-stu-id="f5325-155">You've been using the `string` type in lists so far.</span></span> <span data-ttu-id="f5325-156">Nun erstellen wir eine <xref:System.Collections.Generic.List%601> mithilfe eines anderen Typs.</span><span class="sxs-lookup"><span data-stu-id="f5325-156">Let's make a <xref:System.Collections.Generic.List%601> using a different type.</span></span> <span data-ttu-id="f5325-157">Zunächst erstellen wir einen Satz von Zahlen.</span><span class="sxs-lookup"><span data-stu-id="f5325-157">Let's build a set of numbers.</span></span>

<span data-ttu-id="f5325-158">Fügen Sie Folgendes am Ende Ihrer neuen `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="f5325-158">Add the following to the bottom of your new `Main` method:</span></span>

```csharp
var fibonacciNumbers = new List<int> {1, 1};
```

<span data-ttu-id="f5325-159">Damit wird eine Liste von Ganzzahlen erstellt und für die ersten beiden Ganzzahlen der Wert 1 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="f5325-159">That creates a list of integers, and sets the first two integers to the value 1.</span></span> <span data-ttu-id="f5325-160">Dies sind die ersten beiden Werte einer *Fibonacci-Sequenz* – einer Sequenz von Zahlen.</span><span class="sxs-lookup"><span data-stu-id="f5325-160">These are the first two values of a *Fibonacci Sequence*, a sequence of numbers.</span></span> <span data-ttu-id="f5325-161">Jede nächste Fibonacci-Zahl wird ermittelt, indem die Summe der beiden vorherigen Zahlen gebildet wird.</span><span class="sxs-lookup"><span data-stu-id="f5325-161">Each next Fibonacci number is found by taking the sum of the previous two numbers.</span></span> <span data-ttu-id="f5325-162">Fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="f5325-162">Add this code:</span></span>

```csharp
var previous = fibonacciNumbers[fibonacciNumbers.Count - 1];
var previous2 = fibonacciNumbers[fibonacciNumbers.Count - 2];

fibonacciNumbers.Add(previous + previous2);

foreach(var item in fibonacciNumbers)
    Console.WriteLine(item);
```

<span data-ttu-id="f5325-163">Speichern Sie die Datei, und geben Sie `dotnet run` ein, um die Ergebnisse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5325-163">Save the file and type `dotnet run` to see the results.</span></span>

> [!TIP]
> <span data-ttu-id="f5325-164">Um sich genau auf diesen Abschnitt zu konzentrieren, können Sie den Code auskommentieren, der `WorkingWithStrings();` aufruft.</span><span class="sxs-lookup"><span data-stu-id="f5325-164">To concentrate on just this section, you can comment out the code that calls `WorkingWithStrings();`.</span></span> <span data-ttu-id="f5325-165">Setzen Sie einfach zwei `/`-Zeichen wie folgt vor den Aufruf: `// WorkingWithStrings();`.</span><span class="sxs-lookup"><span data-stu-id="f5325-165">Just put two `/` characters in front of the call like this:  `// WorkingWithStrings();`.</span></span>

## <a name="challenge"></a><span data-ttu-id="f5325-166">Herausforderung</span><span class="sxs-lookup"><span data-stu-id="f5325-166">Challenge</span></span>

<span data-ttu-id="f5325-167">Versuchen Sie, einige dieser Konzepte aus dieser Lektion und früheren Lektionen in einen Zusammenhang zu bringen.</span><span class="sxs-lookup"><span data-stu-id="f5325-167">See if you can put together some of the concepts from this and earlier lessons.</span></span> <span data-ttu-id="f5325-168">Erweitern Sie das, was Sie bisher bezüglich Fibonacci-Zahlen erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f5325-168">Expand on what you've built so far with Fibonacci Numbers.</span></span> <span data-ttu-id="f5325-169">Schreiben Sie den Code zum Generieren der ersten 20 Zahlen der Sequenz.</span><span class="sxs-lookup"><span data-stu-id="f5325-169">Try to write the code to generate the first 20 numbers in the sequence.</span></span> <span data-ttu-id="f5325-170">(Hinweis: Die 20. Fibonacci-Zahl lautet 6765.)</span><span class="sxs-lookup"><span data-stu-id="f5325-170">(As a hint, the 20th Fibonacci number is 6765.)</span></span>

## <a name="complete-challenge"></a><span data-ttu-id="f5325-171">Übung abgeschlossen</span><span class="sxs-lookup"><span data-stu-id="f5325-171">Complete challenge</span></span>

<span data-ttu-id="f5325-172">Als Beispiel für eine Lösung finden Sie [fertig gestellten Code auf GitHub](https://github.com/dotnet/samples/tree/master/csharp/list-quickstart/Program.cs#L13-L23).</span><span class="sxs-lookup"><span data-stu-id="f5325-172">You can see an example solution by [looking at the finished sample code on GitHub](https://github.com/dotnet/samples/tree/master/csharp/list-quickstart/Program.cs#L13-L23)</span></span>

<span data-ttu-id="f5325-173">Mit jeder Iteration der Schleife werden die letzten beiden Ganzzahlen in der Liste summiert, und dieser Wert wird der Liste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f5325-173">With each iteration of the loop, you're taking the last two integers in the list, summing them, and adding that value to the list.</span></span> <span data-ttu-id="f5325-174">Die Schleife wird wiederholt, bis der Liste 20 Elemente hinzugefügt worden sind.</span><span class="sxs-lookup"><span data-stu-id="f5325-174">The loop repeats until you've added 20 items to the list.</span></span>

<span data-ttu-id="f5325-175">Herzlichen Glückwunsch, Sie haben das Listentutorial abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="f5325-175">Congratulations, you've completed the list tutorial.</span></span> <span data-ttu-id="f5325-176">Sie können mit dem Tutorial [Einführung in Klassen](introduction-to-classes.md) in Ihrer eigenen Entwicklungsumgebung fortfahren.</span><span class="sxs-lookup"><span data-stu-id="f5325-176">You can continue with the [Introduction to classes](introduction-to-classes.md) tutorial in your own development environment.</span></span>

<span data-ttu-id="f5325-177">Weitere Informationen zum Arbeiten mit dem `List`-Typ finden Sie im [Leitfaden für .NET](../../../standard/index.md) im Thema [Sammlungen](../../../standard/collections/index.md).</span><span class="sxs-lookup"><span data-stu-id="f5325-177">You can learn more about working with the `List` type in the [.NET Guide](../../../standard/index.md) topic on [collections](../../../standard/collections/index.md).</span></span> <span data-ttu-id="f5325-178">Sie werden auch viele andere Sammlungstypen kennenlernen.</span><span class="sxs-lookup"><span data-stu-id="f5325-178">You'll also learn about many other collection types.</span></span>