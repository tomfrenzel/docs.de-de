---
title: Service Mesh-Kommunikationsinfrastruktur
description: Erfahren Sie, wie Service Mesh-Technologien die cloudbasierte mikroservicekommunikation optimieren.
author: robvet
ms.date: 09/10/2019
ms.openlocfilehash: a9192bf9f5827d05b2453c796c72e11782f9f911
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "73841282"
---
# <a name="service-mesh-communication-infrastructure"></a><span data-ttu-id="2357b-103">Service Mesh-Kommunikationsinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="2357b-103">Service Mesh communication infrastructure</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="2357b-104">In diesem Kapitel haben wir die Herausforderungen der Kommunikation mit dem Microsoft-Dienst untersucht.</span><span class="sxs-lookup"><span data-stu-id="2357b-104">Throughout this chapter, we've explored the challenges of microservice communication.</span></span> <span data-ttu-id="2357b-105">Wir haben gesagt, dass Entwicklungsteams sensibel sein müssen, wie Back-End-Dienste miteinander kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="2357b-105">We said that development teams need to be sensitive to how back-end services communicate with each other.</span></span> <span data-ttu-id="2357b-106">Im Idealfall umso besser ist die Kommunikation zwischen Diensten.</span><span class="sxs-lookup"><span data-stu-id="2357b-106">Ideally, the less inter-service communication, the better.</span></span> <span data-ttu-id="2357b-107">Die Vermeidung ist jedoch nicht immer möglich, da Back-End-Dienste oft aufeinander aufbauen, um Vorgänge abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="2357b-107">However, avoidance isn't always possible as back-end services often rely on one another to complete operations.</span></span>

<span data-ttu-id="2357b-108">Wir haben unterschiedliche Ansätze für die Implementierung von synchroner HTTP-Kommunikation und asynchronem Messaging untersucht.</span><span class="sxs-lookup"><span data-stu-id="2357b-108">We explored different approaches for implementing synchronous HTTP communication and asynchronous messaging.</span></span> <span data-ttu-id="2357b-109">In jedem Fall ist der Entwickler mit der Implementierung von Kommunikations Code belastet.</span><span class="sxs-lookup"><span data-stu-id="2357b-109">In each of the cases, the developer is burdened with implementing communication code.</span></span> <span data-ttu-id="2357b-110">Der Kommunikations Code ist komplex und Zeit intensiv.</span><span class="sxs-lookup"><span data-stu-id="2357b-110">Communication code is complex and time intensive.</span></span> <span data-ttu-id="2357b-111">Falsche Entscheidungen können zu erheblichen Leistungsproblemen führen.</span><span class="sxs-lookup"><span data-stu-id="2357b-111">Incorrect decisions can lead to significant performance issues.</span></span>

<span data-ttu-id="2357b-112">Ein modernerer Ansatz für die Kommunikation zwischen den microservices ist eine neue und schnell weiterentwickelnde Technologie mit dem *Service Mesh*.</span><span class="sxs-lookup"><span data-stu-id="2357b-112">A more modern approach to microservice communication centers around a new and rapidly evolving technology entitled *Service Mesh*.</span></span> <span data-ttu-id="2357b-113">Ein [Dienst Netz](https://www.nginx.com/blog/what-is-a-service-mesh/) ist eine konfigurierbare Infrastruktur Schicht mit integrierten Funktionen für die Kommunikation zwischen Dienst und Dienst, Resilienz und viele übergreifende Aspekte.</span><span class="sxs-lookup"><span data-stu-id="2357b-113">A [service mesh](https://www.nginx.com/blog/what-is-a-service-mesh/) is a configurable infrastructure layer with built-in capabilities to handle service-to-service communication, resiliency, and many cross-cutting concerns.</span></span> <span data-ttu-id="2357b-114">Dadurch wird die Verantwortung für diese Belange von den-und in der Service Mesh-Schicht verlagert.</span><span class="sxs-lookup"><span data-stu-id="2357b-114">It moves the responsibility for these concerns out of the microservices and into service mesh layer.</span></span> <span data-ttu-id="2357b-115">Die Kommunikation wird von Ihren-Diensten abstrahiert.</span><span class="sxs-lookup"><span data-stu-id="2357b-115">Communication is abstracted away from your microservices.</span></span>

<span data-ttu-id="2357b-116">Eine Schlüsselkomponente eines Dienst Netzes ist ein Proxy.</span><span class="sxs-lookup"><span data-stu-id="2357b-116">A key component of a service mesh is a proxy.</span></span> <span data-ttu-id="2357b-117">In einer cloudanwendung wird in der Regel eine Instanz eines Proxys mit jedem-mikrodienst zusammengestellt.</span><span class="sxs-lookup"><span data-stu-id="2357b-117">In a cloud-native application, an instance of a proxy is typically colocated with each microservice.</span></span> <span data-ttu-id="2357b-118">Während Sie in separaten Prozessen ausgeführt werden, sind die beiden eng miteinander verknüpft und haben denselben Lebenszyklus gemeinsam.</span><span class="sxs-lookup"><span data-stu-id="2357b-118">While they execute in separate processes, the two are closely linked and share the same lifecycle.</span></span> <span data-ttu-id="2357b-119">Dieses Muster, das als [Sidecar-Muster](https://docs.microsoft.com/azure/architecture/patterns/sidecar)bezeichnet wird, ist in Abbildung 4-23 dargestellt.</span><span class="sxs-lookup"><span data-stu-id="2357b-119">This pattern, known as the [Sidecar pattern](https://docs.microsoft.com/azure/architecture/patterns/sidecar), and is shown in Figure 4-23.</span></span>

![Dienst Netz mit einem seitigen Fahrzeug](./media/service-mesh-with-side-car.png)

<span data-ttu-id="2357b-121">**Abbildung 4-23.**</span><span class="sxs-lookup"><span data-stu-id="2357b-121">**Figure 4-23**.</span></span> <span data-ttu-id="2357b-122">Dienst Netz mit einem seitigen Fahrzeug</span><span class="sxs-lookup"><span data-stu-id="2357b-122">Service mesh with a side car</span></span>

<span data-ttu-id="2357b-123">Beachten Sie in der obigen Abbildung, wie Nachrichten von einem Proxy abgefangen werden, der neben den einzelnen mikrodiensten ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2357b-123">Note in the previous figure how messages are intercepted by a proxy that runs alongside each microservice.</span></span> <span data-ttu-id="2357b-124">Jeder Proxy kann mit Datenverkehrs Regeln konfiguriert werden, die für den-Dienst spezifisch sind.</span><span class="sxs-lookup"><span data-stu-id="2357b-124">Each proxy can be configured with traffic rules specific to the microservice.</span></span> <span data-ttu-id="2357b-125">Sie versteht Nachrichten und kann Sie über ihre Dienste und die Außenwelt hinweg weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="2357b-125">It understands messages and can route them across your services and the outside world.</span></span>

<span data-ttu-id="2357b-126">Zusammen mit der Verwaltung der Dienst-zu-Dienst-Kommunikation bietet das Dienst Netz Unterstützung für Dienst Ermittlung und Lastenausgleich.</span><span class="sxs-lookup"><span data-stu-id="2357b-126">Along with managing service-to-service communication, the Service Mesh provides support for service discovery and load balancing.</span></span>

<span data-ttu-id="2357b-127">Nach der Konfiguration ist ein Dienst Mesh hoch funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="2357b-127">Once configured, a service mesh is highly functional.</span></span> <span data-ttu-id="2357b-128">Das Mesh Ruft einen entsprechenden Pool von Instanzen von einem Dienst Ermittlungs Endpunkt ab.</span><span class="sxs-lookup"><span data-stu-id="2357b-128">The mesh retrieves a corresponding pool of instances from a service discovery endpoint.</span></span> <span data-ttu-id="2357b-129">Er sendet eine Anforderung an eine bestimmte Dienst Instanz und zeichnet die Latenz und den Antworttyp des Ergebnisses auf.</span><span class="sxs-lookup"><span data-stu-id="2357b-129">It sends a request to a specific service instance, recording the latency and response type of the result.</span></span> <span data-ttu-id="2357b-130">Es wählt die Instanz aus, die höchstwahrscheinlich eine schnelle Antwort auf der Grundlage verschiedener Faktoren zurückgibt, einschließlich der beobachteten Latenzzeit für aktuelle Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="2357b-130">It chooses the instance most likely to return a fast response based on different factors, including the observed latency for recent requests.</span></span>

<span data-ttu-id="2357b-131">Ein Dienst Netz verwaltet den Datenverkehr, die Kommunikation und Netzwerkprobleme auf Anwendungsebene.</span><span class="sxs-lookup"><span data-stu-id="2357b-131">A service mesh manages traffic, communication, and networking concerns at the application level.</span></span> <span data-ttu-id="2357b-132">Sie versteht Nachrichten und Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="2357b-132">It understands messages and requests.</span></span> <span data-ttu-id="2357b-133">Ein Dienst Netz ist in der Regel in einen containerorchestrator integriert.</span><span class="sxs-lookup"><span data-stu-id="2357b-133">A service mesh typically integrates with a container orchestrator.</span></span> <span data-ttu-id="2357b-134">Kubernetes unterstützt eine erweiterbare Architektur, in der ein Dienst Netz hinzugefügt werden kann.</span><span class="sxs-lookup"><span data-stu-id="2357b-134">Kubernetes supports an extensible architecture in which a service mesh can be added.</span></span>

<span data-ttu-id="2357b-135">In Kapitel 6 werden die Service Mesh-Technologien ausführlich erläutert, einschließlich einer Erörterung der Architektur und der verfügbaren Open Source-Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="2357b-135">In chapter 6, we deep-dive into Service Mesh technologies including a discussion on its architecture and available open-source implementations.</span></span>

## <a name="summary"></a><span data-ttu-id="2357b-136">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2357b-136">Summary</span></span>

<span data-ttu-id="2357b-137">In diesem Kapitel haben wir die cloudbasierten Kommunikationsmuster erläutert.</span><span class="sxs-lookup"><span data-stu-id="2357b-137">In this chapter, we discussed cloud-native communication patterns.</span></span> <span data-ttu-id="2357b-138">Wir haben damit begonnen, die Kommunikation von Front-End-Clients mit Back-End-Webdiensten zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="2357b-138">We started by examining how front-end clients communicate with back-end microservices.</span></span> <span data-ttu-id="2357b-139">Gleichzeitig haben wir uns mit API-gatewayplattformen und Echtzeit-Kommunikation beschäftigt.</span><span class="sxs-lookup"><span data-stu-id="2357b-139">Along the way, we talked about API Gateway platforms and real-time communication.</span></span> <span data-ttu-id="2357b-140">Anschließend haben wir uns mit der Kommunikation zwischen den Webdiensten und anderen Back-End-Diensten beschäftigt.</span><span class="sxs-lookup"><span data-stu-id="2357b-140">We then looked at how microservices communicate with other back-end services.</span></span> <span data-ttu-id="2357b-141">Wir haben sowohl die synchrone HTTP-Kommunikation als auch das asynchrone Messaging über Dienste hinweg betrachtet.</span><span class="sxs-lookup"><span data-stu-id="2357b-141">We looked at both synchronous HTTP communication and asynchronous messaging across services.</span></span> <span data-ttu-id="2357b-142">Wir haben GrpC behandelt, eine bevorstehende Technologie in der Cloud-Native Welt.</span><span class="sxs-lookup"><span data-stu-id="2357b-142">We covered gRPC, an upcoming technology in the cloud-native world.</span></span> <span data-ttu-id="2357b-143">Schließlich haben wir eine neue und schnell entwickelnde Technologie mit dem Service Mesh eingeführt, mit der die Kommunikation zwischen den Diensten optimiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="2357b-143">Finally, we introduced a new and rapidly evolving technology entitled Service Mesh that can streamline microservice communication.</span></span>

<span data-ttu-id="2357b-144">Besonderes Augenmerk lag auf verwalteten Azure-Diensten, die bei der Implementierung von Kommunikation in cloudbasierten Systemen helfen können:</span><span class="sxs-lookup"><span data-stu-id="2357b-144">Special emphasis was on managed Azure services that can help implement communication in cloud-native systems:</span></span>

- [<span data-ttu-id="2357b-145">Azure-Anwendung Gateway</span><span class="sxs-lookup"><span data-stu-id="2357b-145">Azure Application Gateway</span></span>](https://docs.microsoft.com/azure/application-gateway/overview)
- [<span data-ttu-id="2357b-146">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2357b-146">Azure API Management</span></span>](https://azure.microsoft.com/services/api-management/)
- [<span data-ttu-id="2357b-147">Azure SignalR-Dienst</span><span class="sxs-lookup"><span data-stu-id="2357b-147">Azure SignalR Service</span></span>](https://azure.microsoft.com/services/signalr-service/)
- [<span data-ttu-id="2357b-148">Azure Storage Warteschlangen</span><span class="sxs-lookup"><span data-stu-id="2357b-148">Azure Storage Queues</span></span>](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction)
- [<span data-ttu-id="2357b-149">Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="2357b-149">Azure Service Bus</span></span>](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview)
- [<span data-ttu-id="2357b-150">Azure Event Grid</span><span class="sxs-lookup"><span data-stu-id="2357b-150">Azure Event Grid</span></span>](https://docs.microsoft.com/azure/event-grid/overview)
- [<span data-ttu-id="2357b-151">Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="2357b-151">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

<span data-ttu-id="2357b-152">Wir wechseln als nächstes zu verteilten Daten in cloudbasierten Systemen und den Vorteilen und Herausforderungen, die es bietet.</span><span class="sxs-lookup"><span data-stu-id="2357b-152">We next move to distributed data in cloud-native systems and the benefits and challenges that it presents.</span></span>

### <a name="references"></a><span data-ttu-id="2357b-153">Verweise</span><span class="sxs-lookup"><span data-stu-id="2357b-153">References</span></span>

- [<span data-ttu-id="2357b-154">.Net-microservices: Architektur für .NET-Container Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2357b-154">.NET Microservices: Architecture for Containerized .NET applications</span></span>](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [<span data-ttu-id="2357b-155">Entwerfen der Kommunikation zwischen Diensten für-Dienste</span><span class="sxs-lookup"><span data-stu-id="2357b-155">Designing Interservice Communication for Microservices</span></span>](https://docs.microsoft.com/azure/architecture/microservices/design/interservice-communication)

- [<span data-ttu-id="2357b-156">Azure signalr Service, ein vollständig verwalteter Dienst zum Hinzufügen von Echtzeitfunktionen</span><span class="sxs-lookup"><span data-stu-id="2357b-156">Azure SignalR Service, a fully managed service to add real-time functionality</span></span>](https://azure.microsoft.com/blog/azure-signalr-service-a-fully-managed-service-to-add-real-time-functionality/)

- [<span data-ttu-id="2357b-157">Azure-API-Gateway-Eingangs Controller</span><span class="sxs-lookup"><span data-stu-id="2357b-157">Azure API Gateway Ingress Controller</span></span>](https://azure.github.io/application-gateway-kubernetes-ingress/)

- [<span data-ttu-id="2357b-158">Informationen zum Einzug in Azure Kubernetes Service (AKS)</span><span class="sxs-lookup"><span data-stu-id="2357b-158">About Ingress in Azure Kubernetes Service (AKS)</span></span>](https://vincentlauzon.com/2018/10/10/about-ingress-in-azure-kubernetes-service-aks/)

- [<span data-ttu-id="2357b-159">Praktischer GrpC</span><span class="sxs-lookup"><span data-stu-id="2357b-159">Practical gRPC</span></span>](https://www.worldcat.org/title/practical-grpc/oclc/1042342319)

- [<span data-ttu-id="2357b-160">Dokumentation zu GrpC</span><span class="sxs-lookup"><span data-stu-id="2357b-160">gRPC Documentation</span></span>](https://grpc.io/docs/guides/)

- <span data-ttu-id="2357b-161">[GrpC for WCF-Entwickler](https://bing.com) [Mark es GrpC Book]</span><span class="sxs-lookup"><span data-stu-id="2357b-161">[gRPC for WCF Developers](https://bing.com) [Mark's gRPC book]</span></span>

- [<span data-ttu-id="2357b-162">Vergleichen von GrpC-Diensten mit http-APIs</span><span class="sxs-lookup"><span data-stu-id="2357b-162">Comparing gRPC Services with HTTP APIs</span></span>](https://docs.microsoft.com/aspnet/core/grpc/comparison?view=aspnetcore-3.0)

>[!div class="step-by-step"]
><span data-ttu-id="2357b-163">[Zurück](rest-grpc.md)
>[Weiter](distributed-data.md)</span><span class="sxs-lookup"><span data-stu-id="2357b-163">[Previous](rest-grpc.md)
[Next](distributed-data.md)</span></span>