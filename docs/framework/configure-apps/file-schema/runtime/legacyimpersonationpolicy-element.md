---
title: <legacyImpersonationPolicy>-Element
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#legacyImpersonationPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/legacyImpersonationPolicy
helpviewer_keywords:
- <legacyImpersonationPolicy> element
- legacyImpersonationPolicy element
ms.assetid: 6e00af10-42f3-4235-8415-1bb2db78394e
ms.openlocfilehash: 18a027bc09f2400a10a06efdc4c5355686bcb56d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73116540"
---
# <a name="legacyimpersonationpolicy-element"></a>\<legacyidentitäts-> Element
Gibt an, dass die Windows-Identität nicht über asynchrone Punkte verläuft, unabhängig von den Floweinstellungen für den Ausführungskontext im aktuellen Thread.  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<runtime >** ](runtime-element.md) \
&nbsp;&nbsp;&nbsp;&nbsp; **\<legacyidentität** >  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<legacyImpersonationPolicy    
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|  
|---------------|-----------------|  
|`enabled`|Erforderliches Attribut.<br /><br /> Gibt an, dass der <xref:System.Security.Principal.WindowsIdentity> nicht über asynchrone Punkte fließt, unabhängig von den <xref:System.Threading.ExecutionContext> Fluss Einstellungen im aktuellen Thread.|  
  
## <a name="enabled-attribute"></a>Enabled-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|`false`|<xref:System.Security.Principal.WindowsIdentity> in Abhängigkeit von den <xref:System.Threading.ExecutionContext> Fluss Einstellungen für den aktuellen Thread über asynchrone Punkte hinweg. Dies ist die Standardeinstellung.|  
|`true`|<xref:System.Security.Principal.WindowsIdentity> erfolgt nicht über asynchrone Punkte, unabhängig von den <xref:System.Threading.ExecutionContext> Fluss Einstellungen im aktuellen Thread.|  
  
### <a name="child-elements"></a>Untergeordnete Elemente  
 Keine  
  
### <a name="parent-elements"></a>Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|-----------------|  
|`configuration`|Das Stammelement in jeder von den Common Language Runtime- und .NET Framework-Anwendungen verwendeten Konfigurationsdatei.|  
|`runtime`|Enthält Informationen über die Assemblybindung und die Garbage Collection.|  
  
## <a name="remarks"></a>Hinweise  
 In den .NET Framework Versionen 1,0 und 1,1 fließt der <xref:System.Security.Principal.WindowsIdentity> nicht über benutzerdefinierte asynchrone Punkte hinweg. Beginnend mit der .NET Framework Version 2,0 gibt es ein <xref:System.Threading.ExecutionContext> Objekt, das Informationen über den aktuell ausgeführten Thread enthält und über asynchrone Punkte innerhalb einer Anwendungsdomäne fließt. Der <xref:System.Security.Principal.WindowsIdentity> ist in diesem Ausführungs Kontext enthalten und verläuft daher auch über die asynchronen Punkte. Dies bedeutet, dass, wenn ein Identitätswechsel Kontext vorhanden ist, auch ein Flow ausgeführt wird.  
  
 Beginnend mit dem .NET Framework 2,0 können Sie das `<legacyImpersonationPolicy>`-Element verwenden, um anzugeben, dass <xref:System.Security.Principal.WindowsIdentity> nicht über asynchrone Punkte fließt.  
  
> [!NOTE]
> Der Common Language Runtime (CLR) kennt Identitätswechsel Vorgänge, die nur mit verwaltetem Code ausgeführt werden, nicht den Identitätswechsel außerhalb von verwaltetem Code, wie z. b. durch Platt Form Aufrufe zu nicht verwaltetem Code oder durch direkte Aufrufe von Win32-Funktionen. Nur verwaltete <xref:System.Security.Principal.WindowsIdentity> Objekte können über asynchrone Punkte hinweg fließen, es sei denn, das `alwaysFlowImpersonationPolicy`-Element wurde auf true (`<alwaysFlowImpersonationPolicy enabled="true"/>`) festgelegt. Wenn das `alwaysFlowImpersonationPolicy`-Element auf true festgelegt wird, wird angegeben, dass die Windows-Identität immer asynchrone Punkte verläuft, unabhängig davon, wie der Identitätswechsel durchgeführt wurde Weitere Informationen zum Übertragen von nicht verwaltetem Identitätswechsel über asynchrone Punkte hinweg finden Sie unter [\<alwaysFlowImpersonationPolicy >-Element](alwaysflowimpersonationpolicy-element.md).  
  
 Sie können dieses Standardverhalten auf zwei verschiedene Arten ändern:  
  
1. In verwaltetem Code auf Thread Basis.  
  
     Sie können den Flow Thread bezogen unterdrücken, indem Sie die <xref:System.Threading.ExecutionContext>-und <xref:System.Security.SecurityContext> Einstellungen mithilfe der <xref:System.Threading.ExecutionContext.SuppressFlow%2A?displayProperty=nameWithType>-, <xref:System.Security.SecurityContext.SuppressFlowWindowsIdentity%2A?displayProperty=nameWithType>-oder <xref:System.Security.SecurityContext.SuppressFlow%2A?displayProperty=nameWithType>-Methode ändern.  
  
2. Im aufzurufenden Befehl an die nicht verwaltete Hostingschnittstelle zum Laden der Common Language Runtime (CLR).  
  
     Wenn eine nicht verwaltete Hostingschnittstelle (anstelle einer einfachen verwalteten ausführbaren Datei) zum Laden der CLR verwendet wird, können Sie ein spezielles Flag im Aufrufen der [CorBindToRuntimeEx-Funktions](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md) Funktion angeben. Um den Kompatibilitätsmodus für den gesamten Prozess zu aktivieren, legen Sie den `flags`-Parameter für die [CorBindToRuntimeEx-Funktion](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md) auf STARTUP_LEGACY_IMPERSONATION fest.  
  
 Weitere Informationen finden Sie unter [\<alwaysFlowImpersonationPolicy >-Element](alwaysflowimpersonationpolicy-element.md).  
  
## <a name="configuration-file"></a>Konfigurationsdatei  
 In einer .NET Framework Anwendung kann dieses Element nur in der Anwendungs Konfigurationsdatei verwendet werden.  
  
 Bei einer ASP.NET-Anwendung kann der Identitätswechsel in der ASPNET. config-Datei konfiguriert werden, die sich im \<Windows-Ordner > \Microsoft.NET\Framework\vx.x.xxxx-Verzeichnis befindet.  
  
 ASP.net deaktiviert den Identitätswechsel in der Datei Aspnet. config standardmäßig mithilfe der folgenden Konfigurationseinstellungen:  
  
``` xml
<configuration>  
   <runtime>  
      <legacyImpersonationPolicy enabled="true"/>  
      <alwaysFlowImpersonationPolicy enabled="false"/>  
   </runtime>  
</configuration>  
```  
  
 Wenn Sie in ASP.NET den Fluss des Identitäts Wechsels zulassen möchten, müssen Sie explizit die folgenden Konfigurationseinstellungen verwenden:  
  
```xml  
<configuration>  
   <runtime>  
      <legacyImpersonationPolicy enabled="false"/>  
      <alwaysFlowImpersonationPolicy enabled="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird gezeigt, wie das Legacy Verhalten angegeben wird, das die Windows-Identität nicht über asynchrone Punkte hinweg fließt.  
  
```xml  
<configuration>  
   <runtime>  
      <legacyImpersonationPolicy enabled="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Siehe auch

- [Schema für Laufzeiteinstellungen](index.md)
- [Konfigurationsdateischema](../index.md)
- [\<alwaysFlowImpersonationPolicy >-Element](alwaysflowimpersonationpolicy-element.md)
