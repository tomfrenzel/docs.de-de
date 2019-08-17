---
title: COM Callable Wrapper (CCW)
ms.date: 10/23/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CCW
- COM interop, COM wrappers
- COM wrappers
- callable wrappers
- interoperation with unmanaged code, COM wrappers
- COM callable wrappers
ms.assetid: d04be3b5-27b9-4f5b-8469-a44149fabf78
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 601f9a216bc2e11ccb34f1f3b3df267002efb01f
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68631464"
---
# <a name="com-callable-wrapper"></a><span data-ttu-id="165bb-102">COM Callable Wrapper (CCW)</span><span class="sxs-lookup"><span data-stu-id="165bb-102">COM Callable Wrapper</span></span>

<span data-ttu-id="165bb-103">Wenn ein COM-Client ein .NET-Objekt aufruft, erstellt Common Language Runtime das verwaltete Objekt sowie einen CCW (COM Callable Wrapper) für dieses Objekt.</span><span class="sxs-lookup"><span data-stu-id="165bb-103">When a COM client calls a .NET object, the common language runtime creates the managed object and a COM callable wrapper (CCW) for the object.</span></span> <span data-ttu-id="165bb-104">COM-Clients verwenden den CCW als Proxy für das verwaltete Objekt, da sie nicht direkt auf ein .NET-Objekt verweisen können.</span><span class="sxs-lookup"><span data-stu-id="165bb-104">Unable to reference a .NET object directly, COM clients use the CCW as a proxy for the managed object.</span></span>

<span data-ttu-id="165bb-105">Common Language Runtime erstellt genau einen CCW für ein verwaltetes Objekt, unabhängig von der Anzahl der COM-Clients, die dort Dienste anfordern.</span><span class="sxs-lookup"><span data-stu-id="165bb-105">The runtime creates exactly one CCW for a managed object, regardless of the number of COM clients requesting its services.</span></span> <span data-ttu-id="165bb-106">Wie die folgende Abbildung zeigt, können mehrere COM-Clients über einen Verweis auf den CCW verfügen, der die Schnittstelle INew verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="165bb-106">As the following illustration shows, multiple COM clients can hold a reference to the CCW that exposes the INew interface.</span></span> <span data-ttu-id="165bb-107">Der CCW verfügt seinerseits über einen einzelnen Verweis auf das verwaltete Objekt, das die Schnittstelle implementiert und für das eine Garbage Collection durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="165bb-107">The CCW, in turn, holds a single reference to the managed object that implements the interface and is garbage collected.</span></span> <span data-ttu-id="165bb-108">Sowohl COM-Clients als auch .NET-Clients können gleichzeitig Anfragen an dasselbe verwaltete Objekt richten.</span><span class="sxs-lookup"><span data-stu-id="165bb-108">Both COM and .NET clients can make requests on the same managed object simultaneously.</span></span>

![Mehrere COM-Clients, die einen Verweis auf das CCW besitzen, das INew verfügbar macht.](./media/com-callable-wrapper/com-callable-wrapper-clients.gif)

<span data-ttu-id="165bb-110">CCWs können von anderen Klassen, die innerhalb der .NET-Runtime ausgeführt werden, nicht erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-110">COM callable wrappers are invisible to other classes running within the .NET runtime.</span></span> <span data-ttu-id="165bb-111">Ihr Hauptzweck besteht im Marshallen von Aufrufen zwischen verwaltetem und nicht verwaltetem Code. CCWs verwalten zusätzlich die Identität und Lebensdauer der von ihnen umschlossenen Objekte.</span><span class="sxs-lookup"><span data-stu-id="165bb-111">Their primary purpose is to marshal calls between managed and unmanaged code; however, CCWs also manage the object identity and object lifetime of the managed objects they wrap.</span></span>

## <a name="object-identity"></a><span data-ttu-id="165bb-112">Objektidentität</span><span class="sxs-lookup"><span data-stu-id="165bb-112">Object Identity</span></span>

<span data-ttu-id="165bb-113">Common Language Runtime belegt für das .NET-Objekt Speicher, der durch Garbage Collection gewonnen wird. Dadurch kann das Objekt bei Bedarf innerhalb des Speichers verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-113">The runtime allocates memory for the .NET object from its garbage-collected heap, which enables the runtime to move the object around in memory as necessary.</span></span> <span data-ttu-id="165bb-114">Im Gegensatz dazu belegt die Common Language Runtime Speicherplatz für den CCW aus einem nicht gesammelten Heap. Dies hat zur Folge, dass COM-Clients direkt auf den Wrapper verweisen können.</span><span class="sxs-lookup"><span data-stu-id="165bb-114">In contrast, the runtime allocates memory for the CCW from a noncollected heap, making it possible for COM clients to reference the wrapper directly.</span></span>

## <a name="object-lifetime"></a><span data-ttu-id="165bb-115">Lebensdauer eines Objekts</span><span class="sxs-lookup"><span data-stu-id="165bb-115">Object Lifetime</span></span>

<span data-ttu-id="165bb-116">Beim CCW erfolgt die Verweiszählung nach Art des herkömmlichen COM, anders als beim .NET-Client, der von ihm umschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="165bb-116">Unlike the .NET client it wraps, the CCW is reference-counted in traditional COM fashion.</span></span> <span data-ttu-id="165bb-117">Wenn die Verweiszählung für den CCW 0 erreicht hat, gibt der Wrapper seinen Verweis auf das verwaltete Objekt frei.</span><span class="sxs-lookup"><span data-stu-id="165bb-117">When the reference count on the CCW reaches zero, the wrapper releases its reference on the managed object.</span></span> <span data-ttu-id="165bb-118">Beim nächsten Garbage Collection-Zyklus werden die verwalteten Objekte ohne verbleibende Verweise eingesammelt.</span><span class="sxs-lookup"><span data-stu-id="165bb-118">A managed object with no remaining references is collected during the next garbage-collection cycle.</span></span>

## <a name="simulating-com-interfaces"></a><span data-ttu-id="165bb-119">Simulieren von COM-Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="165bb-119">Simulating COM interfaces</span></span>

<span data-ttu-id="165bb-120">Mit dem CCW werden alle öffentlichen, für COM sichtbaren Schnittstellen, Datentypen und Rückgabewerte für COM-Clients in einer Weise angezeigt, die mit der COM-Durchsetzung von schnittstellenbasierten Interaktionen konsistent ist.</span><span class="sxs-lookup"><span data-stu-id="165bb-120">CCW exposes all public, COM-visible interfaces, data types, and return values to COM clients in a manner that is consistent with COM's enforcement of interface-based interaction.</span></span> <span data-ttu-id="165bb-121">Für einen COM-Client ist das Aufrufen von Methoden für ein .NET-Objekt identisch wie das Aufrufen von Methoden für ein COM-Objekt.</span><span class="sxs-lookup"><span data-stu-id="165bb-121">For a COM client, invoking methods on a .NET object is identical to invoking methods on a COM object.</span></span>

<span data-ttu-id="165bb-122">Zur Unterstützung dieses nahtlosen Ansatzes erstellt der CCW traditionelle COM-Schnittstellen wie **IUnknown** und **IDispatch**.</span><span class="sxs-lookup"><span data-stu-id="165bb-122">To create this seamless approach, the CCW manufactures traditional COM interfaces, such as **IUnknown** and **IDispatch**.</span></span> <span data-ttu-id="165bb-123">Wie die folgende Abbildung zeigt, unterhält der CCW einen einzigen Verweis auf das .NET-Objekt, den er einschließt.</span><span class="sxs-lookup"><span data-stu-id="165bb-123">As the following illustration shows, the CCW maintains a single reference on the .NET object that it wraps.</span></span> <span data-ttu-id="165bb-124">Sowohl der COM-Client als auch das .NET-Objekt interagieren über den Proxy und die Stubkonstruktion des CCWs miteinander.</span><span class="sxs-lookup"><span data-stu-id="165bb-124">Both the COM client and the .NET object interact with each other through the proxy and stub construction of the CCW.</span></span>

![Diagramm, das zeigt, wie CCW COM-Schnittstellen herstellt.](./media/com-callable-wrapper/com-callable-wrapper-interfaces.gif)

<span data-ttu-id="165bb-126">Neben der Offenlegung von Schnittstellen, die explizit mit einer Klasse in der verwalteten Umgebung implementiert wird, stellt die .NET-Runtime für das Objekt Implementierungen der COM-Schnittstellen bereit, die in der folgenden Tabelle aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="165bb-126">In addition to exposing the interfaces that are explicitly implemented by a class in the managed environment, the .NET runtime supplies implementations of the COM interfaces listed in the following table on behalf of the object.</span></span> <span data-ttu-id="165bb-127">Eine .NET-Klasse kann das Standardverhalten überschreiben, indem sie eigene Implementierungen dieser Schnittstellen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="165bb-127">A .NET class can override the default behavior by providing its own implementation of these interfaces.</span></span> <span data-ttu-id="165bb-128">Zur Laufzeit stehen jedoch immer die Implementierungen der **IUnknown**- und **IDispatch**-Schnittstellen bereit.</span><span class="sxs-lookup"><span data-stu-id="165bb-128">However, the runtime always provides the implementation for the **IUnknown** and **IDispatch** interfaces.</span></span>

|<span data-ttu-id="165bb-129">Interface</span><span class="sxs-lookup"><span data-stu-id="165bb-129">Interface</span></span>|<span data-ttu-id="165bb-130">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="165bb-130">Description</span></span>|
|---------------|-----------------|
|<span data-ttu-id="165bb-131">**IDispatch**</span><span class="sxs-lookup"><span data-stu-id="165bb-131">**IDispatch**</span></span>|<span data-ttu-id="165bb-132">Stellt einen Mechanismus für die späte Bindung an den Typ bereit.</span><span class="sxs-lookup"><span data-stu-id="165bb-132">Provides a mechanism for late binding to type.</span></span>|
|<span data-ttu-id="165bb-133">**IErrorInfo**</span><span class="sxs-lookup"><span data-stu-id="165bb-133">**IErrorInfo**</span></span>|<span data-ttu-id="165bb-134">Stellt eine Textbeschreibung des Fehlers und der Fehlerquelle, eine Hilfedatei, den Hilfekontext und die GUID der Schnittstelle bereit, die den Fehler definiert hat (bei .NET-Klassen immer **GUID_NULL**).</span><span class="sxs-lookup"><span data-stu-id="165bb-134">Provides a textual description of the error, its source, a Help file, Help context, and the GUID of the interface that defined the error (always **GUID_NULL** for .NET classes).</span></span>|
|<span data-ttu-id="165bb-135">**IProvideClassInfo**</span><span class="sxs-lookup"><span data-stu-id="165bb-135">**IProvideClassInfo**</span></span>|<span data-ttu-id="165bb-136">Ermöglicht es COM-Clients, auf die **ITypeInfo**-Schnittstelle zuzugreifen, die von einer verwalteten Klasse implementiert wurde.</span><span class="sxs-lookup"><span data-stu-id="165bb-136">Enables COM clients to gain access to the **ITypeInfo** interface implemented by a managed class.</span></span> <span data-ttu-id="165bb-137">Gibt `COR_E_NOTSUPPORTED` in .NET Core für die Typen zurück, die nicht von COM importiert wurden.</span><span class="sxs-lookup"><span data-stu-id="165bb-137">Returns `COR_E_NOTSUPPORTED` on .NET Core for types not imported from COM.</span></span> |
|<span data-ttu-id="165bb-138">**ISupportErrorInfo**</span><span class="sxs-lookup"><span data-stu-id="165bb-138">**ISupportErrorInfo**</span></span>|<span data-ttu-id="165bb-139">Ermöglicht es einem COM-Client festzustellen, ob das verwaltete Objekt die **IErrorInfo**-Schnittstelle unterstützt.</span><span class="sxs-lookup"><span data-stu-id="165bb-139">Enables a COM client to determine whether the managed object supports the **IErrorInfo** interface.</span></span> <span data-ttu-id="165bb-140">Falls ja, ist der Client in der Lage einen Zeiger auf das letzte Ausnahmeobjekt abzurufen.</span><span class="sxs-lookup"><span data-stu-id="165bb-140">If so, enables the client to obtain a pointer to the latest exception object.</span></span> <span data-ttu-id="165bb-141">Alle verwalteten Typen unterstützen die **IErrorInfo**-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="165bb-141">All managed types support the **IErrorInfo** interface.</span></span>|
|<span data-ttu-id="165bb-142">**ITypeInfo** (nur .NET Framework)</span><span class="sxs-lookup"><span data-stu-id="165bb-142">**ITypeInfo** (.NET Framework Only)</span></span>|<span data-ttu-id="165bb-143">Stellt Typinformationen für eine Klasse bereit, die exakt den von Tlbexp.exe erstellten Typinformationen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="165bb-143">Provides type information for a class that is exactly the same as the type information produced by Tlbexp.exe.</span></span>|
|<span data-ttu-id="165bb-144">**IUnknown**</span><span class="sxs-lookup"><span data-stu-id="165bb-144">**IUnknown**</span></span>|<span data-ttu-id="165bb-145">Stellt die Standardimplementierung der **IUnknown**-Schnittstelle bereit, über die der COM-Client die Lebensdauer des CCWs verwalten und die Typkonvertierung erzwingt.</span><span class="sxs-lookup"><span data-stu-id="165bb-145">Provides the standard implementation of the **IUnknown** interface with which the COM client manages the lifetime of the CCW and provides type coercion.</span></span>|

 <span data-ttu-id="165bb-146">Eine verwaltete Klasse kann zudem die COM-Schnittstellen bereitstellen, die in der folgenden Tabelle beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-146">A managed class can also provide the COM interfaces described in the following table.</span></span>

|<span data-ttu-id="165bb-147">Interface</span><span class="sxs-lookup"><span data-stu-id="165bb-147">Interface</span></span>|<span data-ttu-id="165bb-148">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="165bb-148">Description</span></span>|
|---------------|-----------------|
|<span data-ttu-id="165bb-149">Die Klassenschnittstelle (\_*Klassenname*)</span><span class="sxs-lookup"><span data-stu-id="165bb-149">The (\_*classname*) class interface</span></span>|<span data-ttu-id="165bb-150">Schnittstelle, die von der Laufzeit offengelegt wird und nicht explizit definiert wird; legt alle öffentlichen Schnittstellen, Methoden, Eigenschaften und Felder offen, die für ein verwaltetes Objekt explizit offengelegt werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-150">Interface, exposed by the runtime and not explicitly defined, that exposes all public interfaces, methods, properties, and fields that are explicitly exposed on a managed object.</span></span>|
|<span data-ttu-id="165bb-151">**IConnectionPoint** und **IConnectionPointContainer**</span><span class="sxs-lookup"><span data-stu-id="165bb-151">**IConnectionPoint** and **IConnectionPointContainer**</span></span>|<span data-ttu-id="165bb-152">Schnittstelle für Objekte, die delegatbasierende Ereignisse bedienen (eine Schnittstelle für die Registrierung von Ereignisabonnenten).</span><span class="sxs-lookup"><span data-stu-id="165bb-152">Interface for objects that source delegate-based events (an interface for registering event subscribers).</span></span>|
|<span data-ttu-id="165bb-153">**IDispatchEx** (nur .NET Framework)</span><span class="sxs-lookup"><span data-stu-id="165bb-153">**IDispatchEx** (.NET Framework Only)</span></span>|<span data-ttu-id="165bb-154">Schnittstelle, die von der Laufzeit bereitgestellt wird, wenn die Klasse **IExpando** implementiert.</span><span class="sxs-lookup"><span data-stu-id="165bb-154">Interface supplied by the runtime if the class implements **IExpando**.</span></span> <span data-ttu-id="165bb-155">Die **IDispatchEx**-Schnittstelle ist eine Erweiterung der **IDispatch**-Schnittstelle. Im Gegensatz zu **IDispatch** ermöglicht sie das Aufzählen, Hinzufügen, Löschen und Aufrufen von Membern unter Berücksichtigung von Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="165bb-155">The **IDispatchEx** interface is an extension of the **IDispatch** interface that, unlike **IDispatch**, enables enumeration, addition, deletion, and case-sensitive calling of members.</span></span>|
|<span data-ttu-id="165bb-156">**IEnumVARIANT**</span><span class="sxs-lookup"><span data-stu-id="165bb-156">**IEnumVARIANT**</span></span>|<span data-ttu-id="165bb-157">Schnittstelle für Klassen von Typ „Auflistung“, die die Objekte in der Auflistung enumeriert, wenn die Klasse **IEnumerable** implementiert.</span><span class="sxs-lookup"><span data-stu-id="165bb-157">Interface for collection-type classes, which enumerates the objects in the collection if the class implements **IEnumerable**.</span></span>|

## <a name="introducing-the-class-interface"></a><span data-ttu-id="165bb-158">Einführung in die Klassenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="165bb-158">Introducing the class interface</span></span>

<span data-ttu-id="165bb-159">Die Klassenschnittstelle, in in verwaltetem Code nicht explizit definiert ist, ist eine Schnittstelle, die alle öffentlichen Methoden, Eigenschaften, Felder und Ereignisse offenlegt, die explizit für das .NET-Objekt verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-159">The class interface, which is not explicitly defined in managed code, is an interface that exposes all public methods, properties, fields, and events that are explicitly exposed on the .NET object.</span></span> <span data-ttu-id="165bb-160">Bei dieser Schnittstelle kann es sich um eine duale oder um eine nur für Dispatch vorgesehene Schnittstelle handeln.</span><span class="sxs-lookup"><span data-stu-id="165bb-160">This interface can be a dual or dispatch-only interface.</span></span> <span data-ttu-id="165bb-161">Die Klassenschnittstelle erhält den Namen von der .NET-Klasse selbst, mit vorangestelltem Unterstrich.</span><span class="sxs-lookup"><span data-stu-id="165bb-161">The class interface receives the name of the .NET class itself, preceded by an underscore.</span></span> <span data-ttu-id="165bb-162">Für die Klasse „Mammal“ heißt die Klassenschnittstelle beispielsweise „\_Mammal“.</span><span class="sxs-lookup"><span data-stu-id="165bb-162">For example, for class Mammal, the class interface is \_Mammal.</span></span>

<span data-ttu-id="165bb-163">Bei abgeleiteten Klassen legt die Klassenschnittstelle zudem alle öffentlichen Methoden, Eigenschaften und Felder der Basisklasse offen.</span><span class="sxs-lookup"><span data-stu-id="165bb-163">For derived classes, the class interface also exposes all public methods, properties, and fields of the base class.</span></span> <span data-ttu-id="165bb-164">Die abgeleitete Klasse macht zudem auch eine Klassenschnittstelle für jede Basisklasse verfügbar.</span><span class="sxs-lookup"><span data-stu-id="165bb-164">The derived class also exposes a class interface for each base class.</span></span> <span data-ttu-id="165bb-165">Wenn die Klasse „Mammal“ beispielsweise die Klasse „MammalSuperclass“ erweitert, die ihrerseits System.Object erweitert, stellt das .NET-Objekt für COM-Clients drei Klassenschnittstellen mit Namen „\_Mammal“, „\_MammalSuperclass“ und „\_Object“ zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="165bb-165">For example, if class Mammal extends class MammalSuperclass, which itself extends System.Object, the .NET object exposes to COM clients three class interfaces named \_Mammal, \_MammalSuperclass, and \_Object.</span></span>

<span data-ttu-id="165bb-166">Betrachten Sie beispielsweise die folgende .NET-Klasse:</span><span class="sxs-lookup"><span data-stu-id="165bb-166">For example, consider the following .NET class:</span></span>

```vb
' Applies the ClassInterfaceAttribute to set the interface to dual.
<ClassInterface(ClassInterfaceType.AutoDual)> _
' Implicitly extends System.Object.
Public Class Mammal
    Sub Eat()
    Sub Breathe()
    Sub Sleep()
End Class
```

```csharp
// Applies the ClassInterfaceAttribute to set the interface to dual.
[ClassInterface(ClassInterfaceType.AutoDual)]
// Implicitly extends System.Object.
public class Mammal
{
    public void Eat() {}
    public void Breathe() {}
    public void Sleep() {}
}
```

<span data-ttu-id="165bb-167">Der COM-Client kann einen Zeiger auf eine Klassenschnittstelle mit dem Namen `_Mammal` abrufen.</span><span class="sxs-lookup"><span data-stu-id="165bb-167">The COM client can obtain a pointer to a class interface named `_Mammal`.</span></span> <span data-ttu-id="165bb-168">In .NET Framework können Sie das [Type Library Exporter-Tool (Tlbexp.exe)](../../framework/tools/tlbexp-exe-type-library-exporter.md) verwenden, um eine Typbibliothek zu erstellen, die die `_Mammal`-Schnittstellendefinition enthält.</span><span class="sxs-lookup"><span data-stu-id="165bb-168">On .NET Framework, you can use the [Type Library Exporter (Tlbexp.exe)](../../framework/tools/tlbexp-exe-type-library-exporter.md) tool to generate a type library containing the `_Mammal` interface definition.</span></span> <span data-ttu-id="165bb-169">Das Type Library Exporter-Tool wird unter .NET Core nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="165bb-169">The Type Library Exporter is not supported on .NET Core.</span></span> <span data-ttu-id="165bb-170">Wenn die `Mammal`-Klasse eine oder mehrere Schnittstellen implementiert hat, werden die Schnittstellen unter der Co-Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="165bb-170">If the `Mammal` class implemented one or more interfaces, the interfaces would appear under the coclass.</span></span>

```
[odl, uuid(…), hidden, dual, nonextensible, oleautomation]
interface _Mammal : IDispatch
{
    [id(0x00000000), propget] HRESULT ToString([out, retval] BSTR*
        pRetVal);
    [id(0x60020001)] HRESULT Equals([in] VARIANT obj, [out, retval]
        VARIANT_BOOL* pRetVal);
    [id(0x60020002)] HRESULT GetHashCode([out, retval] short* pRetVal);
    [id(0x60020003)] HRESULT GetType([out, retval] _Type** pRetVal);
    [id(0x6002000d)] HRESULT Eat();
    [id(0x6002000e)] HRESULT Breathe();
    [id(0x6002000f)] HRESULT Sleep();
}
[uuid(…)]
coclass Mammal
{
    [default] interface _Mammal;
}
```

<span data-ttu-id="165bb-171">Das Generieren der Klassenschnittstelle ist optional.</span><span class="sxs-lookup"><span data-stu-id="165bb-171">Generating the class interface is optional.</span></span> <span data-ttu-id="165bb-172">Standardmäßig generiert COM-Interop eine auf Dispatch beschränkte Schnittstelle für jede Klasse, die Sie in eine Typbibliothek exportieren.</span><span class="sxs-lookup"><span data-stu-id="165bb-172">By default, COM interop generates a dispatch-only interface for each class you export to a type library.</span></span> <span data-ttu-id="165bb-173">Sie können die automatische Erstellen dieser Schnittstelle verhindern oder ändern, indem Sie Ihrer Klasse das <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> zuweisen.</span><span class="sxs-lookup"><span data-stu-id="165bb-173">You can prevent or modify the automatic creation of this interface by applying the <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> to your class.</span></span> <span data-ttu-id="165bb-174">Obwohl die Klassenschnittstelle die Offenlegung verwalteter Klassen für COM vereinfachen kann, sind die Verwendungsmöglichkeiten begrenzt.</span><span class="sxs-lookup"><span data-stu-id="165bb-174">Although the class interface can ease the task of exposing managed classes to COM, its uses are limited.</span></span>

> [!CAUTION]
> <span data-ttu-id="165bb-175">Bei Verwendung der Klassenschnittstelle anstatt explizit eine eigene Schnittstelle zu definieren, kann die zukünftige Versionsverwaltung Ihrer verwalteten Klasse erschweren.</span><span class="sxs-lookup"><span data-stu-id="165bb-175">Using the class interface, instead of explicitly defining your own, can complicate the future versioning of your managed class.</span></span> <span data-ttu-id="165bb-176">Lesen Sie bitte die folgenden Richtlinien, bevor Sie die Klassenschnittstelle verwenden.</span><span class="sxs-lookup"><span data-stu-id="165bb-176">Please read the following guidelines before using the class interface.</span></span>

### <a name="define-an-explicit-interface-for-com-clients-to-use-rather-than-generating-the-class-interface"></a><span data-ttu-id="165bb-177">Definieren Sie eine explizite Schnittstelle für COM-Clients anstatt die Klassenschnittstelle zu generieren.</span><span class="sxs-lookup"><span data-stu-id="165bb-177">Define an explicit interface for COM clients to use rather than generating the class interface.</span></span>

<span data-ttu-id="165bb-178">Da die COM-Interop automatisch eine Klassenschnittstelle generiert, können nachträgliche Änderungen an Ihrer Klasse das Layout der Klassenschnittstelle ändern, de von der Common Language Runtime verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="165bb-178">Because COM interop generates a class interface automatically, post-version changes to your class can alter the layout of the class interface exposed by the common language runtime.</span></span> <span data-ttu-id="165bb-179">Da COM-Clients in der Regel nicht für die Behandlung von Änderungen am Layout einer Schnittstelle ausgelegt sind, werden sie unterbrochen, wenn Sie das Memberlayout der Klasse ändern.</span><span class="sxs-lookup"><span data-stu-id="165bb-179">Since COM clients are typically unprepared to handle changes in the layout of an interface, they break if you change the member layout of the class.</span></span>

<span data-ttu-id="165bb-180">Diese Richtlinien unterstreichen den Eindruck, dass Schnittstellen, die für COM-Clients verfügbar gemacht werden, nicht verändert werden dürfen.</span><span class="sxs-lookup"><span data-stu-id="165bb-180">This guideline reinforces the notion that interfaces exposed to COM clients must remain unchangeable.</span></span> <span data-ttu-id="165bb-181">Um das Risiko der Unterbrechung von COM-Clients zu verringern, das mit einer versehentlichen Neuordnung des Schnittstellenlayouts einhergeht, sollten Sie alle Änderungen an der Klasse vom Schnittstellenlayout trennen, indem Sie explizit Schnittstellen definieren.</span><span class="sxs-lookup"><span data-stu-id="165bb-181">To reduce the risk of breaking COM clients by inadvertently reordering the interface layout, isolate all changes to the class from the interface layout by explicitly defining interfaces.</span></span>

<span data-ttu-id="165bb-182">Verwenden Sie das **ClassInterfaceAttribute**, um die automatische Erzeugung der Klassenschnittstelle zu deaktivieren, und implementieren Sie eine explizite Schnittstelle für die Klasse, wie im folgenden Codefragment dargestellt:</span><span class="sxs-lookup"><span data-stu-id="165bb-182">Use the **ClassInterfaceAttribute** to disengage the automatic generation of the class interface and implement an explicit interface for the class, as the following code fragment shows:</span></span>

```vb
<ClassInterface(ClassInterfaceType.None)>Public Class LoanApp
    Implements IExplicit
    Sub M() Implements IExplicit.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.None)]
public class LoanApp : IExplicit
{
    int IExplicit.M() { return 0; }
}
```

<span data-ttu-id="165bb-183">Der Wert **ClassInterfaceType.None** verhindert, dass die Klassenschnittstelle generiert wird, wenn die Metadaten der Klassen in eine Typbibliothek exportiert werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-183">The **ClassInterfaceType.None** value prevents the class interface from being generated when the class metadata is exported to a type library.</span></span> <span data-ttu-id="165bb-184">Im vorstehenden Beispiel können COM-Clients nur über die `IExplicit`-Schnittstelle auf die `LoanApp`-Klasse zugreifen.</span><span class="sxs-lookup"><span data-stu-id="165bb-184">In the preceding example, COM clients can access the `LoanApp` class only through the `IExplicit` interface.</span></span>

### <a name="avoid-caching-dispatch-identifiers-dispids"></a><span data-ttu-id="165bb-185">Vermeiden Sie das Zwischenspeichern von Dispatch-IDs (DispIds).</span><span class="sxs-lookup"><span data-stu-id="165bb-185">Avoid caching dispatch identifiers (DispIds)</span></span>

<span data-ttu-id="165bb-186">Die Verwendung der Klassenschnittstelle ist eine akzeptable Option für skriptgesteuerte Clients, Microsoft Visual Basic 6.0-Clients oder alle spät gebundenen Clients, die die DispIds von Schnittstellenmembern nicht zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="165bb-186">Using the class interface is an acceptable option for scripted clients, Microsoft Visual Basic 6.0 clients, or any late-bound client that does not cache the DispIds of interface members.</span></span> <span data-ttu-id="165bb-187">Mit DispIds werden Schnittstellenmember identifiziert, um ein spätes Binden zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="165bb-187">DispIds identify interface members to enable late binding.</span></span>

<span data-ttu-id="165bb-188">Bei der Klassenschnittstelle basiert die Generierung von DispIDs auf der Position des Members in der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="165bb-188">For the class interface, generation of DispIds is based on the position of the member in the interface.</span></span> <span data-ttu-id="165bb-189">Wenn Sie die Reihenfolge der Member ändern und die Klasse in eine Typbibliothek exportieren, ändern Sie die von der Klassenschnittstelle generierten DispIds.</span><span class="sxs-lookup"><span data-stu-id="165bb-189">If you change the order of the member and export the class to a type library, you will alter the DispIds generated in the class interface.</span></span>

<span data-ttu-id="165bb-190">Um eine Unterbrechung spät gebundener COM-Clients bei Verwendung der Klassenschnittstelle zu vermeiden, wenden Sie das **ClassInterfaceAttribute** mit dem Wert **ClassInterfaceType.AutoDispatch** an.</span><span class="sxs-lookup"><span data-stu-id="165bb-190">To avoid breaking late-bound COM clients when using the class interface, apply the **ClassInterfaceAttribute** with the **ClassInterfaceType.AutoDispatch** value.</span></span> <span data-ttu-id="165bb-191">Dieser Wert implementiert eine nur auf Dispatch ausgelegte Klassenschnittstelle, lässt aber die Schnittstellenbeschreibung aus der Typbibliothek wegfallen.</span><span class="sxs-lookup"><span data-stu-id="165bb-191">This value implements a dispatch-only class interface, but omits the interface description from the type library.</span></span> <span data-ttu-id="165bb-192">Ohne eine Schnittstellenbeschreibung sind Clients nicht in der Lage, während der Kompilierung DispIds zwischenzuspeichern.</span><span class="sxs-lookup"><span data-stu-id="165bb-192">Without an interface description, clients are unable to cache DispIds at compile time.</span></span> <span data-ttu-id="165bb-193">Obwohl es sich hierbei um den Standardschnittstellentyp für die Klassenschnittstelle handelt, können Sie den Attributwert explizit anwenden.</span><span class="sxs-lookup"><span data-stu-id="165bb-193">Although this is the default interface type for the class interface, you can apply the attribute value explicitly.</span></span>

```vb
<ClassInterface(ClassInterfaceType.AutoDispatch)> Public Class LoanApp
    Implements IAnother
    Sub M() Implements IAnother.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.AutoDispatch)]
public class LoanApp
{
    public int M() { return 0; }
}
```

<span data-ttu-id="165bb-194">Um die DispId eines Schnittstellenmembers zur Laufzeit abzurufen, können COM-Clients **IDispatch.GetIdsOfNames** aufrufen.</span><span class="sxs-lookup"><span data-stu-id="165bb-194">To get the DispId of an interface member at run time, COM clients can call **IDispatch.GetIdsOfNames**.</span></span> <span data-ttu-id="165bb-195">Zum Aufrufen einer Methode auf der Schnittstelle, übergeben Sie die zurückgegebene DispId als Argument an **IDispatch.Invoke**.</span><span class="sxs-lookup"><span data-stu-id="165bb-195">To invoke a method on the interface, pass the returned DispId as an argument to **IDispatch.Invoke**.</span></span>

### <a name="restrict-using-the-dual-interface-option-for-the-class-interface"></a><span data-ttu-id="165bb-196">Beschränken Sie die Nutzung der dualen Schnittstellenoption für die Klassenschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="165bb-196">Restrict using the dual interface option for the class interface.</span></span>

<span data-ttu-id="165bb-197">Duale Schnittstellen ermöglichen ein frühes und spätes Binden an Schnittstellenmember durch COM-Clients.</span><span class="sxs-lookup"><span data-stu-id="165bb-197">Dual interfaces enable early and late binding to interface members by COM clients.</span></span> <span data-ttu-id="165bb-198">Während der Entwurfszeit und beim Testen finden Sie es möglicherweise hilfreich, die Klassenschnittstelle auf dual festzulegen.</span><span class="sxs-lookup"><span data-stu-id="165bb-198">At design time and during testing, you might find it useful to set the class interface to dual.</span></span> <span data-ttu-id="165bb-199">Bei einer verwalteten Klasse (und deren Basisklassen), die nie geändert wird, ist diese Option ebenfalls akzeptabel.</span><span class="sxs-lookup"><span data-stu-id="165bb-199">For a managed class (and its base classes) that will never be modified, this option is also acceptable.</span></span> <span data-ttu-id="165bb-200">In allen anderen Fällen sollten Sie vermeiden, die Klassenschnittstelle auf dual festzulegen.</span><span class="sxs-lookup"><span data-stu-id="165bb-200">In all other cases, avoid setting the class interface to dual.</span></span>

<span data-ttu-id="165bb-201">Eine automatisch generierte duale Schnittstelle kann in seltenen Fällen die geeignete Lösung sein, aber häufiger werden hiermit versionsbezogene Komplexitäten erzeugt.</span><span class="sxs-lookup"><span data-stu-id="165bb-201">An automatically generated dual interface might be appropriate in rare cases; however, more often it creates version-related complexity.</span></span> <span data-ttu-id="165bb-202">So können COM-Clients, die die Klassenschnittstelle einer abgeleiteten Klasse verwenden, beispielsweise schnell unterbrochen werden, wenn Änderungen an der Basisklasse vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="165bb-202">For example, COM clients using the class interface of a derived class can easily break with changes to the base class.</span></span> <span data-ttu-id="165bb-203">Wenn die Basisklasse von einem Drittanbieter bereitgestellt wird, liegt das Layout der Klassenschnittstelle außerhalb Ihrer Kontrolle.</span><span class="sxs-lookup"><span data-stu-id="165bb-203">When a third party provides the base class, the layout of the class interface is out of your control.</span></span> <span data-ttu-id="165bb-204">Darüber hinaus stellt eine duale Schnittstelle (**ClassInterfaceType.AutoDual**) im Gegensatz zu einer auf Dispatch beschränkten Schnittstelle eine Beschreibung der Klassenschnittstelle in der exportierten Typbibliothek bereit.</span><span class="sxs-lookup"><span data-stu-id="165bb-204">Further, unlike a dispatch-only interface, a dual interface (**ClassInterfaceType.AutoDual**) provides a description of the class interface in the exported type library.</span></span> <span data-ttu-id="165bb-205">Eine solche Beschreibung kann dafür sorgen, dass spät gebundene Clients DispIds zur Kompilierungszeit zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="165bb-205">Such a description encourages late-bound clients to cache DispIds at compile time.</span></span>

### <a name="ensure-that-all-com-event-notifications-are-late-bound"></a><span data-ttu-id="165bb-206">Stellen Sie sicher, dass alle COM-Ereignisbenachrichtigungen spät gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="165bb-206">Ensure that all COM event notifications are late-bound.</span></span>

<span data-ttu-id="165bb-207">Standardmäßig werden COM-Typinformationen direkt in verwaltete Assemblys eingebettet, sodass primäre Interopassemblys (PIAs) überflüssig sind.</span><span class="sxs-lookup"><span data-stu-id="165bb-207">By default, COM type information is embedded directly into managed assemblies, which eliminates the need for primary interop assemblies (PIAs).</span></span> <span data-ttu-id="165bb-208">Eine der Einschränkungen von eingebetteten Typinformationen ist jedoch, dass sie nicht die Bereitstellung von COM-Ereignisbenachrichtigungen durch früh gebundene vtable-Aufrufe unterstützen, sondern nur spät gebundene `IDispatch::Invoke`-Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="165bb-208">However, one of the limitations of embedded type information is that it does not support delivery of COM event notifications by early-bound vtable calls, but only supports late-bound `IDispatch::Invoke` calls.</span></span>

<span data-ttu-id="165bb-209">Wenn Ihre Anwendung früh gebundene Aufrufe von COM-Ereignisschnittstellenmethoden erfordert, können Sie für die Eigenschaft **Interoptypen einbetten** in Visual Studio `true` festlegen, oder schließen Sie das folgende Element in Ihre Projektdatei ein:</span><span class="sxs-lookup"><span data-stu-id="165bb-209">If your application requires early-bound calls to COM event interface methods, you can set the **Embed Interop Types** property in Visual Studio to `true`, or include the following element in your project file:</span></span>

```xml
<EmbedInteropTypes>True</EmbedInteropTypes>
```

## <a name="see-also"></a><span data-ttu-id="165bb-210">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="165bb-210">See also</span></span>

- <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>
- [<span data-ttu-id="165bb-211">COM-Wrapper</span><span class="sxs-lookup"><span data-stu-id="165bb-211">COM Wrappers</span></span>](com-wrappers.md)
- [<span data-ttu-id="165bb-212">Verfügbarmachen von .NET Framework-Komponenten in COM</span><span class="sxs-lookup"><span data-stu-id="165bb-212">Exposing .NET Framework Components to COM</span></span>](../../framework/interop/exposing-dotnet-components-to-com.md)
- [<span data-ttu-id="165bb-213">Verfügbarmachen von .NET Core-Komponenten in COM</span><span class="sxs-lookup"><span data-stu-id="165bb-213">Exposing .NET Core Components to COM</span></span>](../../core/native-interop/expose-components-to-com.md)
- [<span data-ttu-id="165bb-214">Qualifizieren von .NET-Typen für die Interoperation</span><span class="sxs-lookup"><span data-stu-id="165bb-214">Qualifying .NET Types for Interoperation</span></span>](qualify-net-types-for-interoperation.md)
- [<span data-ttu-id="165bb-215">Runtime Callable Wrapper (RCW)</span><span class="sxs-lookup"><span data-stu-id="165bb-215">Runtime Callable Wrapper</span></span>](runtime-callable-wrapper.md)