---
ms.openlocfilehash: a56c5fc32b5fd274d5da0d9b295918f5ea3b345e
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394469"
---
### <a name="signalr-hubconnection-resetsendping-and-resettimeout-methods-removed"></a><span data-ttu-id="a2d56-101">SignalR: Methoden ResetSendPing und ResetTimeout wurden aus HubConnection entfernt.</span><span class="sxs-lookup"><span data-stu-id="a2d56-101">SignalR: HubConnection ResetSendPing and ResetTimeout methods removed</span></span>

<span data-ttu-id="a2d56-102">Die Methoden `ResetSendPing` und `ResetTimeout` wurden aus der SignalR-API `HubConnection` entfernt.</span><span class="sxs-lookup"><span data-stu-id="a2d56-102">The `ResetSendPing` and `ResetTimeout` methods were removed from the SignalR `HubConnection` API.</span></span> <span data-ttu-id="a2d56-103">Diese Methoden waren ursprünglich nur für die interne Verwendung vorgesehen, wurden aber in ASP.NET Core 2.2 öffentlich („public“) gemacht.</span><span class="sxs-lookup"><span data-stu-id="a2d56-103">These methods were originally intended only for internal use but were made public in ASP.NET Core 2.2.</span></span> <span data-ttu-id="a2d56-104">Diese Methoden sind ab dem Release ASP.NET Core 3.0 Preview 4 nicht mehr verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a2d56-104">These methods won't be available starting in the ASP.NET Core 3.0 Preview 4 release.</span></span> <span data-ttu-id="a2d56-105">Weitere Informationen finden Sie unter [aspnet/AspNetCore#8543](https://github.com/aspnet/AspNetCore/issues/8543).</span><span class="sxs-lookup"><span data-stu-id="a2d56-105">For discussion, see [aspnet/AspNetCore#8543](https://github.com/aspnet/AspNetCore/issues/8543).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="a2d56-106">Eingeführt in Version</span><span class="sxs-lookup"><span data-stu-id="a2d56-106">Version introduced</span></span>

<span data-ttu-id="a2d56-107">3.0</span><span class="sxs-lookup"><span data-stu-id="a2d56-107">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="a2d56-108">Altes Verhalten</span><span class="sxs-lookup"><span data-stu-id="a2d56-108">Old behavior</span></span>

<span data-ttu-id="a2d56-109">Die APIs waren verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a2d56-109">APIs were available.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="a2d56-110">Neues Verhalten</span><span class="sxs-lookup"><span data-stu-id="a2d56-110">New behavior</span></span>

<span data-ttu-id="a2d56-111">Die APIs wurden entfernt.</span><span class="sxs-lookup"><span data-stu-id="a2d56-111">APIs are removed.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="a2d56-112">Grund für die Änderung</span><span class="sxs-lookup"><span data-stu-id="a2d56-112">Reason for change</span></span>

<span data-ttu-id="a2d56-113">Diese Methoden waren ursprünglich nur für die interne Verwendung vorgesehen, wurden aber in ASP.NET Core 2.2 öffentlich („public“) gemacht.</span><span class="sxs-lookup"><span data-stu-id="a2d56-113">These methods were originally intended only for internal use but were made public in ASP.NET Core 2.2.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="a2d56-114">Empfohlene Maßnahme</span><span class="sxs-lookup"><span data-stu-id="a2d56-114">Recommended action</span></span>

<span data-ttu-id="a2d56-115">Verwenden Sie diese Methoden nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="a2d56-115">Don't use these methods.</span></span>

#### <a name="category"></a><span data-ttu-id="a2d56-116">Kategorie</span><span class="sxs-lookup"><span data-stu-id="a2d56-116">Category</span></span>

<span data-ttu-id="a2d56-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d56-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="a2d56-118">Betroffene APIs</span><span class="sxs-lookup"><span data-stu-id="a2d56-118">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing`
- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout`

-->