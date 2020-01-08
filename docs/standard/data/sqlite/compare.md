---
title: Vergleich mit System. Data. sqlite
ms.date: 12/13/2019
description: Beschreibt einige der Unterschiede zwischen den Bibliotheken Microsoft. Data. sqlite und System. Data. sqlite.
ms.openlocfilehash: dee90c132b108f2c876c0d8becc1b02035a47b61
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450279"
---
# <a name="comparison-to-systemdatasqlite"></a><span data-ttu-id="76858-103">Vergleich mit System. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="76858-103">Comparison to System.Data.SQLite</span></span>

<span data-ttu-id="76858-104">In 2005 erstellte Robert Simpson System. Data. sqlite, einen SQLite-Anbieter für ADO.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="76858-104">In 2005, Robert Simpson created System.Data.SQLite, a SQLite provider for ADO.NET 2.0.</span></span> <span data-ttu-id="76858-105">In 2010 hat das SQLite-Team die Wartung und Entwicklung des Projekts übernommen.</span><span class="sxs-lookup"><span data-stu-id="76858-105">In 2010, the SQLite team took over maintenance and development of the project.</span></span> <span data-ttu-id="76858-106">Beachten Sie auch, dass das Mono-Team den Code in 2007 als Mono. Data. sqlite verzweigt hat.</span><span class="sxs-lookup"><span data-stu-id="76858-106">It's also worth noting that the Mono team forked the code in 2007 as Mono.Data.Sqlite.</span></span> <span data-ttu-id="76858-107">"System. Data. sqlite" hat einen langen Verlauf und hat sich zu einem stabilen und voll funktionsfähigen ADO.NET-Anbieter mit Visual Studio-Tools entwickelt.</span><span class="sxs-lookup"><span data-stu-id="76858-107">System.Data.SQLite has a long history and has evolved into a stable and full-featured ADO.NET provider complete with Visual Studio tooling.</span></span> <span data-ttu-id="76858-108">Neue Releases liefern weiterhin Assemblys, die mit jeder Version von .NET Framework zurück zu Version 2,0 und sogar .NET Compact Framework 3,5 kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="76858-108">New releases continue to ship assemblies compatible with every version of .NET Framework back to version 2.0 and even .NET Compact Framework 3.5.</span></span>

<span data-ttu-id="76858-109">Die erste Version von .net Core (veröffentlicht in 2016) war eine einfache, einfache, moderne und plattformübergreifende Implementierung von .net.</span><span class="sxs-lookup"><span data-stu-id="76858-109">The first version of .NET Core (released in 2016) was a single, lightweight, modern, and cross-platform implementation of .NET.</span></span> <span data-ttu-id="76858-110">Veraltete APIs und APIs mit moderneren Alternativen wurden absichtlich entfernt.</span><span class="sxs-lookup"><span data-stu-id="76858-110">Obsolete APIs and APIs with more modern alternatives were intentionally removed.</span></span> <span data-ttu-id="76858-111">ADO.net enthielt keine der DataSet-APIs (einschließlich databel und DataAdapter).</span><span class="sxs-lookup"><span data-stu-id="76858-111">ADO.NET didn't include any of the DataSet APIs (including DataTable and DataAdapter).</span></span>

<span data-ttu-id="76858-112">Das Entity Framework Team war mit der System. Data. sqlite-CodeBase etwas vertraut.</span><span class="sxs-lookup"><span data-stu-id="76858-112">The Entity Framework team was somewhat familiar with the System.Data.SQLite codebase.</span></span> <span data-ttu-id="76858-113">Brice Lambson, ein Mitglied des EF-Teams, hatte das SQLite-Team zuvor beim Hinzufügen von Unterstützung für die Entity Framework-Versionen 5 und 6 unterstützt.</span><span class="sxs-lookup"><span data-stu-id="76858-113">Brice Lambson, a member of the EF team, had previously helped the SQLite team add support for Entity Framework versions 5 and 6.</span></span> <span data-ttu-id="76858-114">Brice hat auch mit seiner eigenen Implementierung eines SQLite-ADO.NET-Anbieters experimentieren, der ungefähr zur gleichen Zeit wie .net Core geplant wurde.</span><span class="sxs-lookup"><span data-stu-id="76858-114">Brice was also experimenting with his own implementation of a SQLite ADO.NET provider around the same time that .NET Core was being planned.</span></span> <span data-ttu-id="76858-115">Nach einer langen Diskussion entschied sich das Entity Framework Team, Microsoft. Data. sqlite basierend auf dem Prototyp der Brice zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="76858-115">After a long discussion, the Entity Framework team decided to create Microsoft.Data.Sqlite based on Brice's prototype.</span></span> <span data-ttu-id="76858-116">Dies ermöglicht es Ihnen, eine neue, leichte und moderne Implementierung zu erstellen, die an den Zielen von .net Core ausgerichtet ist.</span><span class="sxs-lookup"><span data-stu-id="76858-116">This would allow them to create a new lightweight and modern implementation that would align with the goals of .NET Core.</span></span>

<span data-ttu-id="76858-117">Im folgenden finden Sie ein Beispiel für eine modernere Bedeutung von Code zum Erstellen einer [benutzerdefinierten Funktion](user-defined-functions.md) in "System. Data. sqlite" und "Microsoft. Data. sqlite".</span><span class="sxs-lookup"><span data-stu-id="76858-117">As an example of what we mean by more modern, here is code to create a [user-defined function](user-defined-functions.md) in both System.Data.SQLite and Microsoft.Data.Sqlite.</span></span>

```csharp
// System.Data.SQLite
connection.BindFunction(
    new SQLiteFunctionAttribute("ceiling", 1, FunctionType.Scalar),
    (Func<object[], object>)((object[] args) => Math.Ceiling((double)((object[])args[1])[0])),
    null);

// Microsoft.Data.Sqlite
connection.CreateFunction(
    "ceiling",
    (double arg) => Math.Ceiling(arg));
```

<span data-ttu-id="76858-118">In 2017 hat .net Core 2,0 eine Änderung der Strategie erlebt.</span><span class="sxs-lookup"><span data-stu-id="76858-118">In 2017, .NET Core 2.0 experienced a change in strategy.</span></span> <span data-ttu-id="76858-119">Es wurde entschieden, dass die Kompatibilität mit .NET Framework für den Erfolg von .net Core entscheidend war.</span><span class="sxs-lookup"><span data-stu-id="76858-119">It was decided that compatibility with .NET Framework was vital to the success of .NET Core.</span></span> <span data-ttu-id="76858-120">Viele der entfernten APIs, einschließlich der DataSet-APIs, wurden wieder hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="76858-120">Many of the removed APIs, including the DataSet APIs, were added back.</span></span> <span data-ttu-id="76858-121">Wie auch für viele andere, wurde die Blockierung von System. Data. sqlite aufgehoben, sodass Sie auch nach .net Core portiert werden konnte.</span><span class="sxs-lookup"><span data-stu-id="76858-121">Like it did for many others, this unblocked System.Data.SQLite allowing it to also be ported to .NET Core.</span></span> <span data-ttu-id="76858-122">Das ursprüngliche Ziel von Microsoft. Data. sqlite ist, dass es einfach und modern ist, bleibt jedoch erhalten.</span><span class="sxs-lookup"><span data-stu-id="76858-122">The original goal of Microsoft.Data.Sqlite to be lightweight and modern, however, still remains.</span></span> <span data-ttu-id="76858-123">Weitere Informationen zu ADO.NET-APIs, die nicht von Microsoft. Data. sqlite implementiert werden, finden Sie unter [ADO.net](adonet-limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="76858-123">See [ADO.NET limitations](adonet-limitations.md) for details about ADO.NET APIs not implemented by Microsoft.Data.Sqlite.</span></span>

<span data-ttu-id="76858-124">Wenn "Microsoft. Data. sqlite" neue Funktionen hinzugefügt werden, wird der Entwurf von "System. Data. sqlite" berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="76858-124">When new features are added to Microsoft.Data.Sqlite, the design of System.Data.SQLite is taken into account.</span></span> <span data-ttu-id="76858-125">Wir versuchen, nach Möglichkeit die Änderungen zwischen den beiden zu minimieren, um den Übergang zwischen diesen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="76858-125">We try, when possible, to minimize changes between the two to ease transitioning between them.</span></span>

## <a name="data-types"></a><span data-ttu-id="76858-126">Datentypen</span><span class="sxs-lookup"><span data-stu-id="76858-126">Data types</span></span>

<span data-ttu-id="76858-127">Der größte Unterschied zwischen Microsoft. Data. sqlite und System. Data. sqlite besteht darin, wie Datentypen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="76858-127">The biggest difference between Microsoft.Data.Sqlite and System.Data.SQLite is how data types are handled.</span></span> <span data-ttu-id="76858-128">Wie in den [Datentypen](types.md)beschrieben, versucht Microsoft. Data. sqlite nicht, den zugrunde liegenden quirkiness von SQLite auszublenden, wodurch jede beliebige Zeichenfolge als Spaltentyp angegeben werden kann und nur vier primitive Typen aufweist: Integer, Real, Text und BLOB.</span><span class="sxs-lookup"><span data-stu-id="76858-128">As described in [Data types](types.md), Microsoft.Data.Sqlite doesn't try to hide the underlying quirkiness of SQLite, which allows any arbitrary string to be specified as the column type, and only has four primitive types: INTEGER, REAL, TEXT, and BLOB.</span></span>

<span data-ttu-id="76858-129">System. Data. sqlite wendet zusätzliche Semantik auf Spaltentypen an, die Sie direkt zu .NET-Typen hinzu ordnen.</span><span class="sxs-lookup"><span data-stu-id="76858-129">System.Data.SQLite applies additional semantics to column types mapping them directly to .NET types.</span></span> <span data-ttu-id="76858-130">Dadurch erhält der Anbieter ein stärker typisiertes Gefühl, aber er hat einige grobe Ränder.</span><span class="sxs-lookup"><span data-stu-id="76858-130">This gives the provider a more strongly typed feel, but it has some rough edges.</span></span> <span data-ttu-id="76858-131">Sie mussten z. b. eine neue SQL-Anweisung (-Typen) einführen, um die Spaltentypen von Ausdrücken in SELECT-Anweisungen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="76858-131">For example, they had to introduce a new SQL statement (TYPES) to specify the column types of expressions in SELECT statements.</span></span>

## <a name="connection-strings"></a><span data-ttu-id="76858-132">Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="76858-132">Connection strings</span></span>

<span data-ttu-id="76858-133">"Microsoft. Data. sqlite" hat viel weniger Schlüsselwörter für [Verbindungs Zeichenfolgen](connection-strings.md) .</span><span class="sxs-lookup"><span data-stu-id="76858-133">Microsoft.Data.Sqlite has a lot fewer [connection string](connection-strings.md) keywords.</span></span> <span data-ttu-id="76858-134">In der folgenden Tabelle werden Alternativen angezeigt, die stattdessen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="76858-134">The following table shows alternatives that can be used instead.</span></span>

| <span data-ttu-id="76858-135">Stichwort</span><span class="sxs-lookup"><span data-stu-id="76858-135">Keyword</span></span>          | <span data-ttu-id="76858-136">Alternative</span><span class="sxs-lookup"><span data-stu-id="76858-136">Alternative</span></span>                                         |
| ---------------- | --------------------------------------------------- |
| <span data-ttu-id="76858-137">Cachegröße</span><span class="sxs-lookup"><span data-stu-id="76858-137">Cache Size</span></span>       | <span data-ttu-id="76858-138">Sende `PRAGMA cache_size = <pages>`</span><span class="sxs-lookup"><span data-stu-id="76858-138">Send `PRAGMA cache_size = <pages>`</span></span>                  |
| <span data-ttu-id="76858-139">Standardzeitlimit</span><span class="sxs-lookup"><span data-stu-id="76858-139">Default Timeout</span></span>  | <span data-ttu-id="76858-140">Verwenden der defaultTimeout-Eigenschaft für sqliteconnection</span><span class="sxs-lookup"><span data-stu-id="76858-140">Use the DefaultTimeout property on SqliteConnection</span></span> |
| <span data-ttu-id="76858-141">FailIf Missing</span><span class="sxs-lookup"><span data-stu-id="76858-141">FailIfMissing</span></span>    | <span data-ttu-id="76858-142">Verwenden Sie `Mode=ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="76858-142">Use `Mode=ReadWrite`</span></span>                                |
| <span data-ttu-id="76858-143">FullUri</span><span class="sxs-lookup"><span data-stu-id="76858-143">FullUri</span></span>          | <span data-ttu-id="76858-144">Verwenden des Schlüssel Worts für die Datenquelle</span><span class="sxs-lookup"><span data-stu-id="76858-144">Use the Data Source keyword</span></span>                         |
| <span data-ttu-id="76858-145">Journal Modus</span><span class="sxs-lookup"><span data-stu-id="76858-145">Journal Mode</span></span>     | <span data-ttu-id="76858-146">Sende `PRAGMA journal_mode = <mode>`</span><span class="sxs-lookup"><span data-stu-id="76858-146">Send `PRAGMA journal_mode = <mode>`</span></span>                 |
| <span data-ttu-id="76858-147">Legacy Format</span><span class="sxs-lookup"><span data-stu-id="76858-147">Legacy Format</span></span>    | <span data-ttu-id="76858-148">Sende `PRAGMA legacy_file_format = 1`</span><span class="sxs-lookup"><span data-stu-id="76858-148">Send `PRAGMA legacy_file_format = 1`</span></span>                |
| <span data-ttu-id="76858-149">Maximale Seitenanzahl</span><span class="sxs-lookup"><span data-stu-id="76858-149">Max Page Count</span></span>   | <span data-ttu-id="76858-150">Sende `PRAGMA max_page_count = <pages>`</span><span class="sxs-lookup"><span data-stu-id="76858-150">Send `PRAGMA max_page_count = <pages>`</span></span>              |
| <span data-ttu-id="76858-151">Seitengröße</span><span class="sxs-lookup"><span data-stu-id="76858-151">Page Size</span></span>        | <span data-ttu-id="76858-152">Sende `PRAGMA page_size = <bytes>`</span><span class="sxs-lookup"><span data-stu-id="76858-152">Send `PRAGMA page_size = <bytes>`</span></span>                   |
| <span data-ttu-id="76858-153">Schreibgeschützt</span><span class="sxs-lookup"><span data-stu-id="76858-153">Read Only</span></span>        | <span data-ttu-id="76858-154">Verwenden Sie `Mode=ReadOnly`</span><span class="sxs-lookup"><span data-stu-id="76858-154">Use `Mode=ReadOnly`</span></span>                                 |
| <span data-ttu-id="76858-155">Synchronous</span><span class="sxs-lookup"><span data-stu-id="76858-155">Synchronous</span></span>      | <span data-ttu-id="76858-156">Sende `PRAGMA synchronous = <mode>`</span><span class="sxs-lookup"><span data-stu-id="76858-156">Send `PRAGMA synchronous = <mode>`</span></span>                  |
| <span data-ttu-id="76858-157">URI</span><span class="sxs-lookup"><span data-stu-id="76858-157">Uri</span></span>              | <span data-ttu-id="76858-158">Verwenden des Schlüssel Worts für die Datenquelle</span><span class="sxs-lookup"><span data-stu-id="76858-158">Use the Data Source keyword</span></span>                         |
| <span data-ttu-id="76858-159">UseUTF16Encoding</span><span class="sxs-lookup"><span data-stu-id="76858-159">UseUTF16Encoding</span></span> | <span data-ttu-id="76858-160">Sende `PRAGMA encoding = 'UTF-16'`</span><span class="sxs-lookup"><span data-stu-id="76858-160">Send `PRAGMA encoding = 'UTF-16'`</span></span>                   |

## <a name="authorization"></a><span data-ttu-id="76858-161">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="76858-161">Authorization</span></span>

<span data-ttu-id="76858-162">Microsoft. Data. sqlite verfügt über keine API, die den Autorisierungs Rückruf von SQLite verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="76858-162">Microsoft.Data.Sqlite doesn't have any API exposing SQLite's authorization callback.</span></span> <span data-ttu-id="76858-163">Verwenden Sie Problem [#13835](https://github.com/aspnet/EntityFrameworkCore/issues/13835) , um Feedback zu diesem Feature bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="76858-163">Use issue [#13835](https://github.com/aspnet/EntityFrameworkCore/issues/13835) to provide feedback about this feature.</span></span>

## <a name="data-change-notifications"></a><span data-ttu-id="76858-164">Daten Änderungs Benachrichtigungen</span><span class="sxs-lookup"><span data-stu-id="76858-164">Data change notifications</span></span>

<span data-ttu-id="76858-165">Microsoft. Data. sqlite verfügt über keine API, die SQLite-Daten Änderungs Benachrichtigungen verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="76858-165">Microsoft.Data.Sqlite doesn't have any API exposing SQLite's data change notifications.</span></span> <span data-ttu-id="76858-166">Verwenden Sie Problem [#13827](https://github.com/aspnet/EntityFrameworkCore/issues/13827) , um Feedback zu diesem Feature bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="76858-166">Use issue [#13827](https://github.com/aspnet/EntityFrameworkCore/issues/13827) to provide feedback about this feature.</span></span>

## <a name="virtual-table-modules"></a><span data-ttu-id="76858-167">Module für virtuelle Tabellen</span><span class="sxs-lookup"><span data-stu-id="76858-167">Virtual table modules</span></span>

<span data-ttu-id="76858-168">Microsoft. Data. sqlite verfügt über keine API zum Erstellen von virtuellen Tabellen Modulen.</span><span class="sxs-lookup"><span data-stu-id="76858-168">Microsoft.Data.Sqlite doesn't have any API for creating virtual table modules.</span></span> <span data-ttu-id="76858-169">Verwenden Sie Problem [#13823](https://github.com/aspnet/EntityFrameworkCore/issues/13823) , um Feedback zu diesem Feature bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="76858-169">Use issue [#13823](https://github.com/aspnet/EntityFrameworkCore/issues/13823) to provide feedback about this feature.</span></span>

## <a name="see-also"></a><span data-ttu-id="76858-170">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="76858-170">See also</span></span>

* [<span data-ttu-id="76858-171">Datentypen</span><span class="sxs-lookup"><span data-stu-id="76858-171">Data types</span></span>](types.md)
* [<span data-ttu-id="76858-172">Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="76858-172">Connection strings</span></span>](connection-strings.md)
* [<span data-ttu-id="76858-173">Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="76858-173">Encryption</span></span>](encryption.md)
* [<span data-ttu-id="76858-174">ADO.net Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="76858-174">ADO.NET limitations</span></span>](adonet-limitations.md)
* [<span data-ttu-id="76858-175">Dappereinschränkungen</span><span class="sxs-lookup"><span data-stu-id="76858-175">Dapper limitations</span></span>](dapper-limitations.md)