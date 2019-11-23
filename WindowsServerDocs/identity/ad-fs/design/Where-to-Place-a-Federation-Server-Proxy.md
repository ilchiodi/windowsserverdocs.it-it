---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Dove posizionare un proxy server federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 73e68d03e4f2f76dbaf4a497da551640476d0438
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407823"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Dove posizionare un proxy server federativo

È possibile inserire Active Directory Federation Services \(AD FS proxy server federativi\)in una rete perimetrale per fornire un livello di protezione contro utenti malintenzionati che potrebbero essere provenienti da Internet. I proxy server federativi sono ideali per l'ambiente della rete perimetrale perché non hanno accesso alle chiavi private usate per creare i token. Tuttavia, i proxy server federativi possono instradare in modo efficiente le richieste in ingresso ai server federativi autorizzati a produrre tali token.  
  
Non è necessario inserire un proxy server federativo all'interno della rete aziendale per il partner account o il partner risorse perché i computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo. In questo scenario, il server federativo fornisce anche la funzionalità proxy server federativo per i computer client provenienti dalla rete aziendale.  
  
Come avviene in genere con le reti perimetrali, viene stabilito un firewall\-per la rete perimetrale tra la rete perimetrale e la rete aziendale e viene spesso stabilito un firewall con connessione Internet\-tra la rete perimetrale e Internet. In questo scenario, il proxy server federativo si trova tra questi firewall nella rete perimetrale.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurazione dei server del firewall per un proxy server federativo  
Affinché il processo di reindirizzamento del proxy server federativo abbia esito positivo, tutti i server del firewall devono essere configurati in modo da consentire Hypertext Transfer Protocol protetti \(il traffico\) HTTPS. L'uso di HTTPS è richiesto perché i server del firewall devono pubblicare il proxy server federativo, usando la porta 443, in modo che il proxy server federativo nella rete perimetrale possa accedere al server federativo nella rete aziendale.  
  
> [!NOTE]  
> Tutte le comunicazioni tra computer client avvengono tramite HTTPS.  
  
Inoltre, Internet\-rivolte al server firewall, ad esempio un computer che esegue Microsoft Internet Security and Acceleration \(ISA\) server, usa un processo noto come pubblicazione del server per distribuire le richieste client Internet ai server di rete perimetrale e aziendale appropriati, ad esempio proxy server federativi o server federativi.  
  
Le regole di pubblicazione server determinano il funzionamento della pubblicazione server, in sostanza filtrano tutte le richieste in ingresso e in uscita tramite il computer ISA Server. Le regole di pubblicazione server eseguono il mapping delle richieste client ai server appropriati dietro il computer ISA Server. Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola di pubblicazione Web sicura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Nel mondo federato di AD FS, queste richieste client vengono in genere effettuate a un URL specifico, ad esempio un URL dell'identificatore del server federativo, ad esempio http:\//fs.fabrikam.com. Dal momento che queste richieste client arrivano da Internet, il server del firewall con connessione Internet\-deve essere configurato per la pubblicazione dell'URL dell'identificatore del server federativo per ogni proxy server federativo distribuito nella rete perimetrale.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurazione di ISA Server per consentire SSL  
Per semplificare le comunicazioni AD FS protette, è necessario configurare ISA Server in modo da consentire Secure Sockets Layer \(SSL\) comunicazioni tra le seguenti:  
  
-   **Server federativi e proxy server federativi.** È necessario un canale SSL per tutte le comunicazioni tra i server federativi e i proxy server federativi. Pertanto, è necessario configurare ISA Server per consentire una connessione SSL tra la rete aziendale e la rete perimetrale.  
  
-   **Computer client, server federativi e proxy server federativi.** Per consentire le comunicazioni tra i computer client e i server federativi o tra i computer client e i proxy server federativi, è possibile inserire un computer che esegue ISA Server davanti al server federativo o proxy server federativo.  
  
    Se l'organizzazione esegue l'autenticazione client SSL sul server federativo o sul proxy server federativo, quando si inserisce un computer che esegue ISA Server davanti al server federativo o al proxy server federativo, il server deve essere configurato per passare\-tramite la connessione SSL perché la connessione SSL deve terminare con il server federativo o il proxy server federativo.  
  
    Se l'organizzazione non esegue l'autenticazione client SSL sul server federativo o sul proxy server federativo, un'altra opzione consiste nel terminare la connessione SSL nel computer che esegue ISA Server e quindi ri\-stabilire una connessione SSL al server federativo o al proxy server federativo.  
  
> [!NOTE]  
> Il server federativo o il proxy server federativo richiede che la connessione sia protetta da SSL per proteggere il contenuto del token di sicurezza.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
