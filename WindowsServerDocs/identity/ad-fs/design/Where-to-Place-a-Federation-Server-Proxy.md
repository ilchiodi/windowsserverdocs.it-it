---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Dove posizionare un Proxy Server federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>Dove posizionare un Proxy Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile distribuire Active Directory Federation Services \(AD FS\) i proxy server federativi in una rete perimetrale per fornire un livello di protezione da utenti malintenzionati che potrebbero essere provenienti da Internet. I proxy server federativi sono ideali per l'ambiente di rete perimetrale perché non hanno accesso alle chiavi private che vengono utilizzati per creare i token. Tuttavia, i proxy server federativi in modo efficiente inoltrare le richieste in ingresso ai server federativi che sono autorizzati a produrre tali token.  
  
Non è necessario inserire un proxy server federativo nella rete aziendale per il partner account o partner risorse perché i computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo. In questo scenario, il server federativo offre inoltre funzionalità di proxy server federativo per i computer client provenienti dalla rete aziendale.  
  
Come nelle reti perimetrali tipiche, viene stabilito un firewall sulla intranet\ tra la rete perimetrale e la rete aziendale e spesso viene stabilito un firewall di con connessione Internet tra la rete perimetrale e Internet. In questo scenario, il proxy server federativo si trova tra questi firewall nella rete perimetrale.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurazione dei server del firewall per un proxy server federativo  
Per il federazione server proxy processo di reindirizzamento abbia esito positivo, tutti i server firewall devono essere configurati per consentire il traffico \(HTTPS\) Secure Hypertext Transfer Protocol. L'utilizzo di HTTPS è richiesto perché i server del firewall devono pubblicare il proxy server federativo, tramite la porta 443, in modo che il proxy server federativo nella rete perimetrale può accedere al server federativo nella rete aziendale.  
  
> [!NOTE]  
> Tutte le comunicazioni tra i computer client avvengono tramite HTTPS.  
  
Inoltre, con il connessione Internet firewall server, ad esempio un computer che esegue Microsoft Internet Security and Acceleration \(ISA\) Server, Usa un processo noto come pubblicazione del server per distribuire le richieste client Internet per il perimetro appropriato e un server di rete aziendale, ad esempio i proxy server federativi o server federativi.  
  
Regole di pubblicazione server determinano il funzionamento di pubblicazione del server, sostanza filtrano tutte le richieste in ingresso e in uscita tramite il computer ISA Server. Regole di pubblicazione server eseguire il mapping delle richieste client in ingresso ai server appropriati dietro il computer ISA Server. Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola pubblicazione Web Secure](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Nel mondo federativo di ADFS, queste richieste client vengono in genere inviate a un URL specifico, ad esempio, un federazione server URL dell'identificatore, ad esempio http://fs.fabrikam.com. Poiché queste richieste client hanno origine in da Internet, il server del firewall con connessione Internet deve essere configurato per pubblicare l'URL dell'identificatore server federativo per ogni proxy server federativo che viene distribuito nella rete perimetrale.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurazione di ISA Server per consentire SSL  
Per semplificare le comunicazioni protette ADFS, è necessario configurare ISA Server per consentire la comunicazione Secure Sockets Layer \(SSL\) tra gli elementi seguenti:  
  
-   **I server federativi e proxy server federativi.** Un canale SSL è necessario per tutte le comunicazioni tra server federativi e proxy server federativi. Pertanto, è necessario configurare ISA Server per consentire una connessione SSL tra la rete aziendale e la rete perimetrale.  
  
-   **I computer client, i server federativi e proxy server federativi.** In modo che le comunicazioni possono verificarsi tra i computer client e server federativi o tra i computer client e i proxy server federativi, è possibile inserire un computer che esegue ISA Server davanti il server federativo o proxy server federativo.  
  
    Se l'organizzazione esegue l'autenticazione client SSL nel server federativo o proxy server federativo, quando si inserisce un computer che esegue ISA Server davanti il server federativo o proxy server federativo, il server deve essere configurato per pass\-through della connessione SSL perché la connessione SSL deve terminare con il server federativo o proxy server federativo.  
  
    Se l'organizzazione non esegue l'autenticazione client SSL nel server federativo o proxy server federativo, un'altra opzione è per terminare la connessione SSL nel computer che esegue ISA Server e quindi nuovamente-stabilire una connessione SSL per il server federativo o proxy server federativo.  
  
> [!NOTE]  
> Il server federativo o proxy server federativo è necessario che la connessione sia protetta da SSL per proteggere il contenuto del token di sicurezza.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
