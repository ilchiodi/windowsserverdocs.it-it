---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Dove posizionare un proxy server federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842982"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Dove posizionare un proxy server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile inserire Active Directory Federation Services \(ADFS\)proxy server federativi in una rete perimetrale per fornire un livello di protezione contro gli utenti malintenzionati che potrebbero essere provenienti da Internet. I proxy server federativi sono ideali per l'ambiente della rete perimetrale perché non hanno accesso alle chiavi private usate per creare i token. Tuttavia, proxy server federativi possono indirizzare in modo corretto le richieste in ingresso per i server federativi che sono autorizzati a produrre tali token.  
  
Non è necessaria a posizionare un proxy server federativo nella rete aziendale per il partner account o partner risorse perché i computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo. In questo scenario, il server federativo fornisce anche funzionalità proxy server federativo per i computer client provenienti dalla rete aziendale.  
  
Come è tipico nelle reti perimetrali, una rete intranet\-rivolta firewall viene stabilita tra la rete perimetrale e la rete aziendale e Internet\-rivolta firewall spesso viene stabilito tra la rete perimetrale e il Internet. In questo scenario, il proxy server federativo si colloca tra questi firewall sulla rete perimetrale.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurazione dei server del firewall per un proxy server federativo  
Per il federazione server proxy processo di reindirizzamento abbia esito positivo, tutti i server del firewall devono essere configurati per consentire Secure Hypertext Transfer Protocol \(HTTPS\) il traffico. L'uso di HTTPS è obbligatorio perché il server del firewall devono pubblicare il proxy server federativo, usando la porta 443, in modo che il proxy server federativo nella rete perimetrale possa accedere al server federativo nella rete aziendale.  
  
> [!NOTE]  
> Tutte le comunicazioni tra computer client avvengono tramite HTTPS.  
  
Inoltre, Internet\-rivolto verso server del firewall, ad esempio un computer che esegue Microsoft Internet Security and Acceleration \(ISA\) Server, Usa un processo noto come pubblicazione del server per distribuire client Internet richieste al perimetrale appropriato e il server di rete aziendale, ad esempio server federativi o server federativi.  
  
Le regole di pubblicazione server determinano il funzionamento della pubblicazione server, in sostanza filtrano tutte le richieste in ingresso e in uscita tramite il computer ISA Server. Le regole di pubblicazione server eseguono il mapping delle richieste client ai server appropriati dietro il computer ISA Server. Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola pubblicazione Web sicura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Nel mondo federativo di AD FS, queste richieste client vengono in genere inviate a un URL specifico, ad esempio, un URL di identificatore del server federativo, ad esempio http://fs.fabrikam.com. Poiché queste richieste client hanno origine in da Internet, Internet\-rivolto verso server del firewall deve essere configurato per pubblicare l'URL identificatore del server federativo per ogni proxy server federativo che viene distribuito nella rete perimetrale.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurazione di ISA Server per consentire SSL  
Per agevolare le comunicazioni sicure di ADFS, è necessario configurare ISA Server per consentire di Secure Sockets Layer \(SSL\) le comunicazioni tra le operazioni seguenti:  
  
-   **I server federativi e proxy server federativo.** Un canale SSL è obbligatorio per tutte le comunicazioni tra server federativi e proxy server federativo. Pertanto, è necessario configurare ISA Server per consentire una connessione SSL tra la rete aziendale e la rete perimetrale.  
  
-   **I computer client, server federativi e proxy server federativo.** In modo che le comunicazioni possono verificarsi tra i computer client e server federativi o tra computer client e server federativi, è possibile inserire un computer che esegue ISA Server davanti il server federativo o proxy server federativo.  
  
    Se l'organizzazione esegue l'autenticazione client SSL nel server federativo o proxy server federativo, quando si inserisce un computer che esegue ISA Server davanti il server federativo o proxy server federativo, il server deve essere configurato per pass\-tramite la connessione SSL perché la connessione SSL deve terminare con il server federativo o proxy server federativo.  
  
    Se l'organizzazione non esegue l'autenticazione client SSL nel server federativo o proxy server federativo, un'altra opzione consiste nel terminare la connessione SSL nel computer che esegue ISA Server e successivamente reinstallarli\-stabilire una connessione SSL il server federativo o proxy server federativo.  
  
> [!NOTE]  
> Il server federativo o proxy server federativo è necessario che la connessione sia protetta da SSL per proteggere il contenuto del token di sicurezza.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
