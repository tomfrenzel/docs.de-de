---
title: 'Vorgehensweise: Konfigurieren der Nachverfolgung mit WorkflowServiceHost'
ms.date: 03/30/2017
ms.assetid: ed1485fe-7529-4351-bca3-8bb915260b17
ms.openlocfilehash: 5781878270272f5ef894c68dc23b9433029e1d41
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69968490"
---
# <a name="how-to-configure-tracking-with-workflowservicehost"></a>Vorgehensweise: Konfigurieren der Nachverfolgung mit WorkflowServiceHost
In diesem Thema wird erläutert, wie Sie die Nachverfolgung für einen unter [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] gehosteten <xref:System.ServiceModel.Activities.WorkflowServiceHost>-Workflow konfigurieren. Es wird mithilfe einer Web.config-Datei konfiguriert, indem ein Dienstverhalten angegeben wird.  
  
### <a name="configure-tracking-in-configuration"></a>Konfigurieren der Nachverfolgung in einer Konfigurationsdatei  
  
1. `behavior`Fügen Sie <xref:System.Activities.Tracking.EtwTrackingParticipant> mithilfe des < >-Elements in einer Konfigurationsdatei hinzu, wie im folgenden Beispiel gezeigt.  
  
    ```xml  
    <behaviors>  
       <serviceBehaviors>  
         <behavior>  
           <etwTracking profileName="Sample Tracking Profile" />  
         </behavior>              
       </serviceBehaviors>  
    <behaviors>  
    ```  
  
    > [!NOTE]
    > Im vorangehenden Konfigurationsbeispiel wird die vereinfachte Konfiguration verwendet. Weitere Informationen finden Sie unter [vereinfachte Konfiguration](../../../../docs/framework/wcf/simplified-configuration.md).  
  
     Im vorangehenden Konfigurationsbeispiel wird ein <xref:System.Activities.Tracking.EtwTrackingParticipant>-Objekt hinzugefügt und ein Nachverfolgungsprofilname angegeben. Überwachungs Profile werden in einem <`trackingProfile`>-Element innerhalb eines <`tracking`> Element erstellt. Das Überwachungsprofil enthält Nachverfolgungsabfragen, mit denen ein Überwachungsteilnehmer Workflowereignisse abonnieren kann. Diese werden ausgegeben, wenn sich der Zustand einer Workflowinstanz zur Laufzeit ändert. Im folgenden Beispiel wird das Erstellen eines Nachverfolgungsprofils veranschaulicht.  
  
    ```xml  
    <system.serviceModel>  
        <tracking>   
         <trackingProfile name="Sample Tracking Profile">  
            <workflow activityDefinitionId="*">  
               <workflowInstanceQueries>  
                 <workflowInstanceQuery>  
                    <states>  
                       <state name="Started"/>  
                       <state name="Completed"/>  
                    </states>  
                </workflowInstanceQuery>  
             </workflowInstanceQueries>  
           </workflow>  
         </trackingProfile>   
       </tracking>  
    </system.serviceModel>  
    ```  
  
     Weitere Informationen zu Überwachungs Profilen finden Sie unter nach [Verfolgungs profile](../../../../docs/framework/windows-workflow-foundation/tracking-profiles.md).  
  
     Weitere Informationen zur Nachverfolgung im Allgemeinen finden Sie unter [Workflow Verfolgung und Ablauf Verfolgung](../../../../docs/framework/windows-workflow-foundation/workflow-tracking-and-tracing.md).  
  
### <a name="configure-tracking-in-code"></a>Konfigurieren der Nachverfolgung in Code  
  
1. Fügen Sie den <xref:System.Activities.Tracking.EtwTrackingParticipant> mit dem <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior>-Verhalten in Code hinzu, wie im folgenden Beispiel gezeigt.  
  
    ```csharp  
    host.Description.Behaviors.Add(new EtwTrackingBehavior { ProfileName = "Sample Tracking Profile" });  
    ```  
  
     Im vorangehenden Codebeispiel wird ein <xref:System.Activities.Tracking.EtwTrackingParticipant>-Objekt hinzugefügt und ein Nachverfolgungsprofilname angegeben. Überwachungs Profile werden in einem <`trackingProfile`>-Element innerhalb eines <`tracking`>-Elements erstellt, wie im vorherigen Abschnitt gezeigt.  
  
     Weitere Informationen zu Überwachungs Profilen finden Sie unter nach [Verfolgungs profile](../../../../docs/framework/windows-workflow-foundation/tracking-profiles.md).  
  
     Weitere Informationen zur Nachverfolgung im Allgemeinen finden Sie unter [Workflow Verfolgung und Ablauf Verfolgung](../../../../docs/framework/windows-workflow-foundation/workflow-tracking-and-tracing.md). Ein Beispiel für die programmgesteuerte Konfiguration der Überwachung finden Sie unter [Konfigurieren der Nachverfolgung für einen Workflow](../../../../docs/framework/windows-workflow-foundation/configuring-tracking-for-a-workflow.md).  
  
## <a name="see-also"></a>Siehe auch

- [Vereinfachte Konfiguration für WCF-Dienste](../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)
- [Workflowdienste](../../../../docs/framework/wcf/feature-details/workflow-services.md)
- [Überwachungsprofile](../../../../docs/framework/windows-workflow-foundation/tracking-profiles.md)
