---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: Server Farm federativa con database interno di Windows
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bb4e5f88f3d62511b185a2b4317416169717c860
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>Server Farm federativa con database interno di Windows

>Si applica a: Windows Server 2012

La topologia predefinita per Active Directory Federation Services \(AD FS\) è una server farm federativa utilizzando \(WID\) il Database interno di Windows, che è costituito da un massimo di cinque server federativi ospitano servizio federativo dell'organizzazione. In questa topologia, ADFS utilizza WID come archivio per il database di configurazione di ADFS per tutti i server federativi che sono connessi alla farm. La farm replica e mantiene i dati del servizio federativo nel database di configurazione di tutti i server della farm.  
  
L'atto di creare il primo server federativo in una farm crea anche un nuovo servizio federativo. Quando si usa database interno di Windows per il database di configurazione di ADFS, il primo server federativo che crea la farm viene definito come il *server federativo primario*. Ciò significa che il computer è configurato con una copia di lettura/scrittura del database di configurazione di ADFS.  
  
Tutti gli altri server federativi configurati per la farm vengono dette *server federativi secondari* poiché devono replicare le modifiche apportate nel server federativo primario per le copie di sola lettura del database di configurazione di ADFS che memorizzano localmente.  
  
> [!NOTE]  
> È consigliabile utilizzare almeno due server federativi in una configurazione bilanciato Load.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione di  
In questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario fornire agli utenti interni \ (connesso al computer connessi fisicamente all'isolamento di rete aziendale) con una sola di accesso \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che vogliono fornire agli utenti interni accesso SSO a Microsoft Online Services o Microsoft Office 365  
  
-   Organizzazioni di piccole dimensioni che richiedono servizi scalabili ridondanti  
  
> [!NOTE]  
> Le organizzazioni con database di dimensioni superiori considerare l'uso di [Federation Server Farm utilizzando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia di distribuzione, verrà illustrata più avanti in questa sezione. Le organizzazioni con utenti che accedono dall'esterno della rete considerare utilizzando il [Federation Server Farm utilizzando WID e proxy](Federation-Server-Farm-Using-WID-and-Proxies.md) topologia o [Federation Server Farm utilizzando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'uso di questa topologia?  
  
-   Fornisce l'accesso SSO per gli utenti interni  
  
-   Ridondanza dei dati e del servizio federativo \ (ogni server federativo replica le modifiche in altri server federativi nella stessa farm\)  
  
-   La farm può essere ampliata aggiungendo fino a cinque server federativi  
  
-   Database interno di Windows è incluso in Windows. Pertanto, non è necessario per l'acquisto di SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono i limiti di utilizzo di questa topologia?  
  
-   Una farm WID ha un limite di cinque server federativi. Per ulteriori informazioni, vedere [considerazioni sulla topologia di distribuzione ADFS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Una farm database interno di Windows non supporta la risoluzione di rilevamento o elemento riproduzione token \ (in parte il \(SAML\) protocol\ Security Assertion Markup Language).  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Quando si è pronti per iniziare a distribuire la topologia della rete, è necessario pianificare la collocazione di tutti i server federativi della rete aziendale dietro un host di bilanciamento carico di rete \(NLB\) che può essere configurato per un cluster di bilanciamento carico di rete con un nome del cluster dedicato Domain Name System \(DNS\) e l'indirizzo IP del cluster.  
  
> [!NOTE]  
> Questo nome DNS del cluster deve corrispondere al nome servizio federativo, ad esempio, fs.fabrikam.com.  
  
L'host di bilanciamento carico di rete può utilizzare le impostazioni definite in questo cluster NLB per allocare le richieste client per i server federativi singoli. Nella figura seguente mostra come la società fittizia Fabrikam, Inc., imposta la prima fase della relativa distribuzione tramite una server farm federativa forma computer \(fs1 and fs2\) con WID e il collocamento di un server DNS e un singolo host di bilanciamento carico di rete che è collegato alla rete aziendale.  
  
![Server farm con database interno di Windows](media/FarmWID.gif)  
  
> [!NOTE]  
> Se si verifica un errore in questo host NLB singolo, gli utenti non saranno in grado di accedere alle applicazioni federate o servizi. Se i requisiti aziendali non consentono un singolo punto di errore, aggiungere altri host di bilanciamento carico di rete.  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) nella Guida alla progettazione di ADFS.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
