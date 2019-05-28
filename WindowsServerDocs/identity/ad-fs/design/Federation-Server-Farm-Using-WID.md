---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Server farm federativa che usa Database interno di Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 067461b90ed5ce03d9470a450917dcbb93cf653a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191306"
---
# <a name="federation-server-farm-using-wid"></a>Server farm federativa che usa Database interno di Windows

La topologia predefinita per Active Directory Federation Services \(ADFS\) è una server farm federativa, utilizzare Database interno di Windows \(WID\). In questa topologia, ADFS utilizza WID come archivio per il database di configurazione di ADFS per tutti i server federativi che sono connessi alla farm. La farm replica e mantiene i dati del servizio federativo nel database di configurazione di tutti i server della farm. ADFS in Windows Server 2012 R2 consente alle organizzazioni con un massimo di 100 trust della relying party per configurare federation server farm con database interno di Windows server fino a 30.  
  
La creazione del primo server federativo in una farm crea anche un nuovo servizio federativo. Quando si usa database interno di Windows per il database di configurazione di ADFS, il primo server federativo che si crea la farm è detto il *server federativo primario*. Ciò significa che il computer è configurato con un'operazione di lettura\/scrivere copia del database di configurazione di AD FS.  
  
Tutti gli altri server di federazione configurato per questa farm vengono dette *i server federativi secondari* poiché devono replicare tutte le modifiche apportate nel server federativo primario per la lettura\-solo copie del database di configurazione di ADFS che memorizzano localmente.  
  
> [!IMPORTANT]  
> È consigliabile utilizzare almeno due server federativi in un carico\-configurazione bilanciato.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario fornire agli utenti interni \(connesso al computer connessi fisicamente alla rete aziendale\) con single sign-\-in \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che desiderano offrire agli utenti interni di accesso SSO a Microsoft Online Services o Microsoft Office 365  
  
-   Organizzazioni di piccole dimensioni che richiedono servizi scalabili ridondanti  
  
> [!NOTE]  
> Le organizzazioni con database di dimensioni superiori considerare l'uso di [Federation Server Farm utilizzando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia di distribuzione. Devono prendere in considerazione le organizzazioni con utenti che accedono dall'esterno della rete utilizzando il [Federation Server Farm utilizzando WID e proxy](Federation-Server-Farm-Using-WID-and-Proxies.md) topologia o [Federation Server Farm utilizzando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Fornisce l'accesso SSO per gli utenti interni  
  
-   La ridondanza dei dati e del servizio federativo \(ciascun server federativo replica le modifiche in altri server federativi nella stessa farm\)  
  
-   Database interno di Windows è incluso in Windows; Pertanto, non è necessario per l'acquisto di SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.  
  
-   Una farm database interno di Windows non supporta la risoluzione di rilevamento o elemento riproduzione token \(fa parte di Security Assertion Markup Language \(SAML\) protocollo\).  
  
Nella tabella seguente fornisce un riepilogo di utilizzo di una farm database interno di Windows.  Usato per pianificare l'implementazione.  
  
|| 1 \- 100 attendibilità della relying Party | Più di 100 attendibilità della relying Party |
| --- | --- | --- |
|1 \- 30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID - SQL necessarie 
|Nodi più di 30 AD FS|Non supportato utilizzando WID - SQL necessarie|Non supportato utilizzando WID - SQL necessarie  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Si è pronti per iniziare a distribuire la topologia della rete, è necessario pianificare la collocazione di tutti i server federativi della rete aziendale dietro un bilanciamento del carico di rete \(Bilanciamento carico di RETE\) host che può essere configurato per un cluster di bilanciamento carico di RETE con un cluster dedicato Domain Name System \(DNS\) indirizzo IP del cluster e nome.  
  
> [!NOTE]  
> Questo nome DNS del cluster deve corrispondere al nome servizio federativo, ad esempio, fs.fabrikam.com.  
  
L'host di bilanciamento carico di RETE può utilizzare le impostazioni definite in questo cluster NLB per allocare le richieste client per i server federativi singoli. La figura seguente mostra come la società fittizia Fabrikam, Inc., imposta la prima fase della relativa distribuzione tramite due\-server farm federativa computer \(fs1 e fs2\) con WID e il collocamento di un server DNS e un singolo host di bilanciamento carico di RETE che è collegato alla rete aziendale.  
  
![server farm con database interno di Windows](media/FarmWID.gif)  
  
> [!NOTE]  
> Se si verifica un errore in questo host NLB singolo, gli utenti non saranno in grado di accedere alle applicazioni federate o servizi. Se i propri requisiti aziendali non consentono un singolo punto di errore, aggiungere altri host NLB.  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi, vedere la sezione requisiti di risoluzione dei nomi in [requisiti per ADFS](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

