---
ms.openlocfilehash: 07527c247e6ccd53d2a77793946ffc796c3e1cbb
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643916"
---
### <a name="switchsystemwindowsformsuselegacyimages-compatibility-switch-not-supported"></a><span data-ttu-id="b1789-101">Kompatibilitätsswitch „Switch.System.Windows.Forms.UseLegacyImages“ wird nicht unterstützt</span><span class="sxs-lookup"><span data-stu-id="b1789-101">Switch.System.Windows.Forms.UseLegacyImages compatibility switch not supported</span></span>

<span data-ttu-id="b1789-102">Der mit .NET Framework 4.8 eingeführte Kompatibilitätsswitch `Switch.System.Windows.Forms.UseLegacyImages` wird in Windows Forms unter .NET Core 3.0 nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b1789-102">The `Switch.System.Windows.Forms.UseLegacyImages` compatibility switch, which was introduced in .NET Framework 4.8, is not supported in Windows Forms on .NET Core 3.0.</span></span>

#### <a name="change-description"></a><span data-ttu-id="b1789-103">Änderungsbeschreibung</span><span class="sxs-lookup"><span data-stu-id="b1789-103">Change description</span></span>

<span data-ttu-id="b1789-104">Ab .NET Framework 4.8 werden mit dem Kompatibilitätsswitch `Switch.System.Windows.Forms.UseLegacyImages` mögliche Bildskalierungsprobleme in ClickOnce-Szenarios in Umgebungen mit hohem DPI-Wert behoben.</span><span class="sxs-lookup"><span data-stu-id="b1789-104">Starting with the .NET Framework 4.8, the `Switch.System.Windows.Forms.UseLegacyImages` compatibility switch addressed possible image scaling issues in ClickOnce scenarios in high DPI environments.</span></span> <span data-ttu-id="b1789-105">Wenn der Switch auf `true` festgelegt wird, kann der Benutzer die Legacybildskalierung in Anzeigen mit hohem DPI-Wert wiederherstellen, deren Skalierung auf mehr als 100 % festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="b1789-105">When set to `true`, the switch allows the user to restore legacy image scaling on high DPI displays whose scale is set to greater than 100%.</span></span> <span data-ttu-id="b1789-106">Weitere Informationen finden Sie unter [Versionshinweise zu .NET Framework 4.8](https://github.com/microsoft/dotnet/blob/master/releases/net48/dotnet48-changes.md#clickonce) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="b1789-106">For more information, see [.NET Framework 4.8 Release Notes](https://github.com/microsoft/dotnet/blob/master/releases/net48/dotnet48-changes.md#clickonce) on GitHub.</span></span>

<span data-ttu-id="b1789-107">In .NET Core wird der Switch `Switch.System.Windows.Forms.UseLegacyImages` nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b1789-107">In .NET Core, the `Switch.System.Windows.Forms.UseLegacyImages` switch is not supported.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="b1789-108">Eingeführt in Version</span><span class="sxs-lookup"><span data-stu-id="b1789-108">Version introduced</span></span>

<span data-ttu-id="b1789-109">3.0 Vorschau 9</span><span class="sxs-lookup"><span data-stu-id="b1789-109">3.0 Preview 9</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="b1789-110">Empfohlene Maßnahme</span><span class="sxs-lookup"><span data-stu-id="b1789-110">Recommended action</span></span>

<span data-ttu-id="b1789-111">Entfernen Sie den Switch.</span><span class="sxs-lookup"><span data-stu-id="b1789-111">Remove the switch.</span></span> <span data-ttu-id="b1789-112">Der Switch wird nicht unterstützt, und es ist keine alternative Funktionalität verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b1789-112">The switch is not supported, and no alternative functionality is available.</span></span>

#### <a name="category"></a><span data-ttu-id="b1789-113">Kategorie</span><span class="sxs-lookup"><span data-stu-id="b1789-113">Category</span></span>

<span data-ttu-id="b1789-114">Windows Forms</span><span class="sxs-lookup"><span data-stu-id="b1789-114">Windows Forms</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="b1789-115">Betroffene APIs</span><span class="sxs-lookup"><span data-stu-id="b1789-115">Affected APIs</span></span>

- <span data-ttu-id="b1789-116">Keine</span><span class="sxs-lookup"><span data-stu-id="b1789-116">None</span></span>

<!-- 

### Affected APIs

- Not detectable via API analysis

-->