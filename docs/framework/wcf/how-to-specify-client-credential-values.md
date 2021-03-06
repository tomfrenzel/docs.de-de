---
title: 'Gewusst wie: Angeben der Clientanmeldeinformationswerte'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 82293d7f-471a-4549-8f19-0be890e7b074
ms.openlocfilehash: 2417c2dd16224d6cbf00d3f1f4a8958420830b6c
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319860"
---
# <a name="how-to-specify-client-credential-values"></a>Gewusst wie: Angeben der Clientanmeldeinformationswerte

Mithilfe von Windows Communication Foundation (WCF) kann der Dienst angeben, wie ein Client für den Dienst authentifiziert wird. Ein Dienst kann beispielsweise festlegen, dass der Client mit einem Zertifikat authentifiziert wird.

## <a name="to-determine-the-client-credential-type"></a>So bestimmen Sie den Typ der Clientanmeldeinformationen

1. Rufen Sie die Metadaten aus dem Metadatenendpunkt des Diensts ab. Die Metadaten umfassen in der Regel zwei Dateien: den Clientcode in der Programmiersprache Ihrer Wahl (die Standardeinstellung ist Visual C#) und eine XML-Konfigurationsdatei. Eine Möglichkeit zum Abrufen der Metadaten ist, das Tool "Svcutil.exe" zu verwenden, um den Clientcode und die Clientkonfiguration zurückzugeben. Weitere Informationen finden Sie unter [Abrufen von Metadaten](./feature-details/retrieving-metadata.md) und [Service Model Metadata Utility Tool (Svcutil. exe)](servicemodel-metadata-utility-tool-svcutil-exe.md).

2. Öffnen Sie die XML-Konfigurationsdatei. Wenn Sie das Tool "Svcutil.exe" verwenden, lautet der Standardname der Datei "Output.config".

3. Suchen Sie das **\<Security->** Element mit dem **Mode** -Attribut ( **\<Security Mode =** `MessageOrTransport` **>** , wobei `MessageOrTransport` auf einen der Sicherheitsmodi festgelegt ist.

4. Suchen Sie nach dem untergeordneten Element, das dem Moduswert entspricht. Wenn der Modus z. b. auf **Message**festgelegt ist, suchen Sie das >-Element **\<message** , das im **\<Security->** Element enthalten ist.

5. Beachten Sie den Wert, der dem **clientkredentialtype** -Attribut zugewiesen ist. Der tatsächliche Wert hängt davon ab, welcher Modus verwendet wird: Transport oder Nachricht.

Der folgende XML-Code veranschaulicht die Konfiguration für einen Client, der die Nachrichtensicherheit verwendet und ein Zertifikat zum Authentifizieren des Clients erfordert.

```xml
<security mode="Message">
    <transport clientCredentialType="Windows" proxyCredentialType="None"
        realm="" />
     <message clientCredentialType="Certificate" negotiateServiceCredential="true"
    algorithmSuite="Default" establishSecurityContext="true" />
</security>
```

## <a name="example-tcp-transport-mode-with-certificate-as-client-credential"></a>Beispiel: TCP-Transportmodus mit einem Zertifikat als Clientanmeldeinformationen

In diesem Beispiel wird der Sicherheitsmodus auf "Transport" und der Clientanmeldeinformationswert auf ein X.509-Zertifikat festgelegt. Die folgenden Vorgänge zeigen, wie Sie den Clientanmeldeinformationswert für den Client im Code und in der Konfiguration festlegen können. Dabei wird davon ausgegangen, dass Sie das [Service Model Metadata Utility-Tool (Svcutil. exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) verwendet haben, um die Metadaten (Code und Konfiguration) vom Dienst zurückzugeben. Weitere Informationen finden Sie unter Gewusst [wie: Erstellen eines Clients](how-to-create-a-wcf-client.md).

### <a name="to-specify-the-client-credential-value-on-the-client-in-code"></a>So legen Sie den Clientanmeldeinformationswert für den Client im Code fest

1. Verwenden Sie das [Service Model Metadata Utility-Tool (Svcutil. exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) , um Code und Konfiguration aus dem Dienst zu generieren.

2. Erstellen Sie eine Instanz des WCF-Clients mithilfe des generierten Codes.

3. Legen Sie für die Clientklasse die <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>-Eigenschaft der <xref:System.ServiceModel.ClientBase%601>-Klasse auf einen geeigneten Wert fest. In diesem Beispiel wird die Eigenschaft auf ein X.509-Zertifikat mit der <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>-Methode der <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>-Klasse festgelegt.

     [!code-csharp[c_TcpService#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpservice/cs/source.cs#4)]
     [!code-vb[c_TcpService#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpservice/vb/source.vb#4)]

     Sie können jede beliebige Enumeration der <xref:System.Security.Cryptography.X509Certificates.X509FindType>-Klasse verwenden. Der Betreffname wird hier verwendet, für den Fall, dass das Zertifikat (aufgrund des Ablaufdatums) geändert wird. Anhand des Betreffnamens kann die Infrastruktur das Zertifikat wieder suchen.

### <a name="to-specify-the-client-credential-value-on-the-client-in-configuration"></a>So legen Sie den Clientanmeldeinformationswert für den Client in der Konfiguration fest

1. Fügen Sie dem [\<behavior->](../configure-apps/file-schema/wcf/behaviors.md) Element ein >-Element vom Typ " [\<behavior](../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) " hinzu.

2. Fügen Sie dem [\<verhalten>](../configure-apps/file-schema/wcf/behaviors.md) Element ein [>-Element \<clientanmelde](../configure-apps/file-schema/wcf/clientcredentials.md) Informationen hinzu. Achten Sie darauf, für das erforderliche `name`-Attribut einen geeigneten Wert festzulegen.

3. Fügen Sie dem [> Element \<clientanmelde](../configure-apps/file-schema/wcf/clientcredentials.md) Informationen ein [>-Element \<clientcertificate](../configure-apps/file-schema/wcf/clientcertificate-of-servicecredentials.md) hinzu.

4. Legen Sie für die folgenden Attribute geeignete Werte fest: `storeLocation`, `storeName`, `x509FindType` und `findValue`, wie im folgenden Code dargestellt. Weitere Informationen zu Zertifikaten finden Sie unter [Arbeiten mit Zertifikaten](./feature-details/working-with-certificates.md).

    ```xml
    <behaviors>
       <endpointBehaviors>
          <behavior name="endpointCredentialBehavior">
            <clientCredentials>
              <clientCertificate findValue="Contoso.com"
                                 storeLocation="LocalMachine"
                                 storeName="TrustedPeople"
                                 x509FindType="FindBySubjectName" />
            </clientCredentials>
          </behavior>
       </endpointBehaviors>
    </behaviors>
    ```

5. Wenn Sie den Client konfigurieren, legen Sie das Verhalten fest, indem Sie das `behaviorConfiguration`-Attribut des `<endpoint>`-Elements festlegen, wie im folgenden Code dargestellt. Das Endpunkt Element ist ein untergeordnetes Element des [>-Elements \<client](../configure-apps/file-schema/wcf/client.md) . Geben Sie auch den Namen der Bindungskonfiguration an, indem Sie das `bindingConfiguration`-Attribut auf die Bindung für den Client festlegen. Wenn Sie eine generierte Konfigurationsdatei verwenden, wird der Name der Bindung automatisch erstellt. In diesem Beispiel lautet der Name `"tcpBindingWithCredential"`.

    ```xml
    <client>
      <endpoint name =""
                address="net.tcp://contoso.com:8036/aloha"
                binding="netTcpBinding"
                bindingConfiguration="tcpBindingWithCredential"
                behaviorConfiguration="endpointCredentialBehavior" />
    </client>
    ```

## <a name="see-also"></a>Siehe auch

- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A>
- <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>
- <xref:System.ServiceModel.ClientBase%601>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>
- [Programmieren der WCF-Sicherheit](./feature-details/programming-wcf-security.md)
- [Ausählen eines Anmeldeinformationentyps](./feature-details/selecting-a-credential-type.md)
- [ServiceModel Metadata Utility-Tool (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [Arbeiten mit Zertifikaten](./feature-details/working-with-certificates.md)
- [Vorgehensweise: Erstellen eines Clients](how-to-create-a-wcf-client.md)
- [\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md)
- [\<Security >](../configure-apps/file-schema/wcf/security-of-nettcpbinding.md)
- [\<message >](../configure-apps/file-schema/wcf/message-element-of-nettcpbinding.md)
- [\<-Verhalten >](../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)
- [\<-Verhalten >](../configure-apps/file-schema/wcf/behaviors.md)
- [\<clientcertificate >](../configure-apps/file-schema/wcf/clientcertificate-of-servicecredentials.md)
- [\<clientanmelde Informationen >](../configure-apps/file-schema/wcf/clientcredentials.md)
