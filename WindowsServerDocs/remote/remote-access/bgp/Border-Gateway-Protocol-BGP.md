---
title: Protocollo BGP (Border Gateway Protocol)
description: È possibile usare questo argomento per comprendere Border Gateway Protocol (BGP) in Windows Server 2016, incluse le topologie di distribuzione supportate da BGP e le caratteristiche e funzionalità BGP.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ae6fddce1564e44ad72a5630c6abb16cdb6735d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388977"
---
# <a name="border-gateway-protocol-bgp"></a>Protocollo BGP (Border Gateway Protocol)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento contiene informazioni sul protocollo BGP (Border Gateway Protocol), incluse le topologie di distribuzione supportate da BGP e le caratteristiche e le funzionalità BGP.  
  
> [!NOTE]  
> Oltre a questo argomento, la seguente documentazione BGP è disponibile.  
>   
> -   [Riferimento per i comandi di Windows PowerShell per BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Topologie di distribuzione supportate da BGP](#bkmk_top)  
  
-   [Funzionalità BGP](#bkmk_features)  
  
Quando è configurato in un servizio di accesso remoto Windows Server 2016 \(RAS\) Gateway in modalità multi-tenant, Border Gateway Protocol (BGP) offre la possibilità di gestire il routing del traffico di rete tra reti VM dei tenant e i rispettivi siti remoti. È inoltre possibile utilizzare BGP per le distribuzioni di Gateway RAS single-tenant e quando si distribuisce accesso remoto come una rete locale \(LAN\) router.  
  
BGP, essendo un protocollo di routing dinamico che apprende automaticamente le route tra i siti connessi da connessioni VPN da sito a sito, riduce la necessità di configurare manualmente le route sui router.  
  
Per utilizzare il routing BGP, è necessario installare il **Remote Access Service \(RAS\)** e/o **Routing** servizio ruolo del ruolo del server di accesso remoto in un computer o di una macchina virtuale \(VM\) -il tipo di sistema in uso dipende da se si dispone di una distribuzione multi-tenant:  
  
-   Per una distribuzione multi-tenant, è consigliabile installare il Gateway RAS in uno o più macchine virtuali. Utilizzo di più macchine virtuali offre un'elevata disponibilità. Il Gateway RAS è in grado di gestire più connessioni da più tenant e costituito da un host Hyper-V e una macchina Virtuale che viene configurata come gateway. Questo gateway è configurato con le connessioni VPN site-to-site come router BGP multi-tenant per tenant di exchange e Provider di servizi Cloud \(CSP\) le route di subnet.  
  
-   Per una distribuzione di gateway edge single-tenant o una distribuzione di router LAN, è possibile installare il Gateway RAS su un computer fisico o una macchina Virtuale.  
  
> [!IMPORTANT]  
> Quando si installa un Gateway RAS, è necessario specificare se BGP è abilitato per ogni tenant utilizzando il **Enable RemoteAccessRoutingDomain** comando Windows PowerShell con il **tipo** valore del parametro di **tutti**. Per installare accesso remoto come router LAN BGP abilitato senza funzionalità multi-tenant, è possibile utilizzare il comando **RemoteAccess Install - VpnType RoutingOnly**.  
>   
> Esempio di codice seguente viene illustrato come installare il servizio RAS in modalità multi-tenancy con tutte le funzionalità RAS (point-to-site VPN site-to-site VPN e BGP routing) abilitata per due tenant, Contoso e Fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bkmk_top"></a>Topologie di distribuzione supportate da BGP  
Di seguito sono elencate le topologie di distribuzione supportate in cui i siti aziendali sono connessi a un data center di provider di servizi cloud (CSP, Cloud Service Provider).  
  
In tutti gli scenari, il gateway CSP è un Gateway di Windows Server 2016 RAS in corrispondenza del bordo. Il Gateway RAS, che è in grado di gestire più connessioni da più tenant, è costituito da un host Hyper-V e una macchina Virtuale che viene configurata come gateway. Questo gateway perimetrale viene configurato con connessioni VPN da sito a sito come router BGP multi-tenant per scambiare le route della subnet aziendale e CSP.  
  
I tenant si connettono alle risorse nel data center CSP usando una connessione VPN da sito a sito (S2S). Inoltre il protocollo di routing BGP viene distribuito per lo scambio di informazioni di routing dinamico tra i gateway aziendali e CSP.  
  
Sono supportate le topologie di distribuzione seguenti.  
  
-   [Gateway da sito a sito VPN RAS con BGP presso il sito aziendale perimetrale](#bkmk_top1)  
  
-   [Gateway di terze parti con BGP presso il sito aziendale Edge](#bkmk_top2)  
  
-   [Più siti aziendali con gateway di terze parti](#bkmk_top3)  
  
-   [Punti di terminazione distinti per BGP e VPN](#bkmk_top4)  
  
Le sezioni seguenti contengono informazioni aggiuntive su ogni topologia BGP supportata.  
  
### <a name="bkmk_top1"></a>Gateway da sito a sito VPN RAS con BGP presso il sito aziendale perimetrale  
Questa topologia illustra un sito aziendale connesso a un provider CSP. La topologia di routing Enterprise include un router interno, un Gateway di Windows Server 2016 RAS configurato per le connessioni site-to-site VPN con il CSP e un dispositivo firewall perimetrale. Il Gateway RAS termina le connessioni VPN S2S e BGP.  
  
![Gateway to-Site VPN RAS](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Entrambi i siti vengono connessi usando il protocollo eBGP (External Border Gateway Protocol), che può trasmettere informazioni tra i router abilitati per BGP in sistemi autonomi distinti. A questo scopo è necessario che sia la rete aziendale sia il provider CSP abbiano un numero di sistema autonomo (un parametro integrato nel protocollo BGP) distinto.  
  
In questo scenario BGP funziona nel modo seguente.  
  
-   Il dispositivo perimetrale del sito aziendale apprende le route della subnet virtualizzate (10.2.1.0/24) ospitate nel cloud usando BGP. Inoltre, questo dispositivo annuncia le route di subnet locale (10.1.1.0/24) al Gateway multi-tenant RAS CSP.  
  
-   Il router perimetrale del cliente apprende le route interne locali con uno dei meccanismi seguenti:  
  
    -   Il dispositivo perimetrale esegue BGP con un router interno e apprende le route interne (in questo esempio, 10.1.1.0/24). Intanto il router interno apprende le route esterne (ad esempio, 10.2.1.0/24) dal dispositivo perimetrale e il router interno deve distribuire queste route agli altri router locali usando un protocollo IGP (Interior Gateway Protocol), ad esempio OSPF (Open Shortest Path First) o RIP (Routing Information Protocol).  
  
    -   Il dispositivo perimetrale può essere configurato con route statiche o interfacce per selezionare le route per l'annuncio usando BGP. Il dispositivo perimetrale distribuisce anche le route esterne agli altri router locali usando un protocollo IGP.  
  
### <a name="bkmk_top2"></a>Gateway di terze parti con BGP presso il sito aziendale Edge  
Questa topologia illustra un sito aziendale che usa un router perimetrale di terze parti per connettersi a un provider CSP. Il router perimetrale funge anche da gateway VPN da sito a sito.  
  
![Gateway di terze parti con BGP a livello di sito aziendale](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
Il router perimetrale aziendale apprende le route interne locali con uno dei meccanismi seguenti:  
  
-   Il dispositivo perimetrale esegue BGP con un router interno e apprende le route interne (in questo caso, 10.1.1.0/24).  
  
-   Il dispositivo perimetrale implementa un protocollo IGP e partecipa direttamente al routing interno.  
  
### <a name="bkmk_top3"></a>Più siti aziendali che si connettono a CSP Cloud Datacenter  
Questa topologia illustra più siti aziendali che usano gateway di terze parti per connettersi a un provider CSP. I dispositivi perimetrali di terze parti fungono da gateway VPN da sito a sito e da router BGP.  
  
![Più siti aziendali connessi a CSP cloud datacenter](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
I router perimetrali dei clienti apprendono le route interne locali con uno dei meccanismi seguenti:  
  
-   Il dispositivo perimetrale esegue BGP con un router interno e apprende le route interne (in questo caso, 10.1.1.0/24).  
  
-   Il dispositivo perimetrale implementa un protocollo IGP e partecipa direttamente al routing interno.  
  
Ogni sito aziendale apprende le route dall'altro sito tramite la connettività eBGP diretta.  
  
Ogni sito aziendale apprende le route di rete ospitate direttamente e usando l'altro sito aziendale, ma seleziona la route migliore in base al costo della route.  
  
Se il router BGP Enterprise siti 1 non può connettersi con il router Enterprise sito 2 BGP perché la connettività non è riuscita, il router BGP 1 del sito in modo dinamico inizierà a imparare le route alla rete aziendale sito 2 da Router BGP CSP e il traffico viene reindirizzato facilmente dal sito 1 a 2 del sito tramite il Router Windows Server BGP CSP.  
  
### <a name="bkmk_top4"></a>Punti di terminazione distinti per BGP e VPN  
Questa topologia illustra una rete aziendale che usa due diversi router come endpoint BGP e VPN da sito a sito. Site-to-site VPN è terminata in Windows Server 2016 RAS Gateway, mentre BGP è terminato su un router interno. Sul lato CSP delle connessioni, il CSP termina le connessioni VPN sia BGP con il Gateway RAS. Con questa configurazione, l'hardware del router di terze parti interno deve supportare la ridistribuzione delle route IGP in BGP, ma anche la ridistribuzione delle route BGP in IGP.  
  
![Punti di terminazione distinti per BGP e VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
Il router interno apprende le route aziendali con uno dei meccanismi seguenti:  
  
-   BGP  
  
-   Un protocollo IGP, ad esempio OSPF o RIP.  
  
-   Configurazione delle route statiche  
  
Quando un protocollo IGP viene usato nel sito aziendale, il router interno deve ridistribuire le route IGP in BGP, ma anche ridistribuire le route BGP nelle route IGP, per mantenere la connettività della subnet tra le reti virtuali CSP e le subnet aziendali locali.  
  
Questa distribuzione, il Gateway RAS Enterprise con una connessione VPN site-to-site con il Gateway RAS CSP, che fornisce il Gateway RAS aziendale con le route per il gateway CSP. Il router interno Enterprise apprende quindi questa route per il gateway CSP utilizzando iBGP con il Gateway aziendale di RAS. Per questo motivo, il router interno dell'organizzazione è quindi in grado di stabilire una sessione di peering con il Router BGP Gateway di CSP RAS.  
  
Da questo punto in poi, il router interno dell'organizzazione e il Gateway di RAS CSP scambio di informazioni di routine. E il router BGP RAS Enterprise apprende il CSP route e le route di Enterprise fisicamente instradare pacchetti tra le reti.  
  
## <a name="bkmk_features"></a>Funzionalità BGP  
Di seguito sono le funzionalità del Router BGP Gateway RAS.  
  
**Routing BGP come un servizio ruolo di accesso remoto**. È ora possibile installare il **Routing** servizio ruolo del ruolo del server di accesso remoto senza installare il **servizio di accesso remoto (RAS)** servizio ruolo quando si desidera utilizzare l'accesso remoto come router LAN BGP.  Ciò riduce il footprint di memoria router BGP e installa solo i componenti necessari per il routing dinamico BGP. Il servizio ruolo di Routing è utile quando solo una macchina Virtuale Router BGP è obbligatorio e non sia richiesto l'utilizzo di DirectAccess o VPN. Inoltre, tramite accesso remoto come router LAN con il protocollo BGP offre i vantaggi di routing dinamici di BGP nella rete interna.  
  
**Statistiche BGP (contatori di messaggi, contatori di route)** . Il router BGP supporta la visualizzazione delle statistiche relative a messaggi e route, se necessario, usando il comando **Get-BgpStatistics** di Windows PowerShell.  
  
**Supporto ECMP (Equal Cost Multi Path Routing)** . Il router BGP supporta ECMP e può avere più di una route con uguale costo di cui è stato eseguito il plumbing nello stack e nella tabella di routing BGP. La selezione da parte del router BGP della route per trasmettere pacchetti di dati è casuale con ECMP abilitato.  
  
**Configurazione HoldTime**. Il router BGP supporta la configurazione del valore HoldTimer in base ai requisiti della rete. Questo timer può essere modificato dinamicamente per agevolare l'interoperabilità con dispositivi di terze parti o per mantenere un tempo massimo specifico per il timeout della sessione di peering BGP.  
  
**Supporto di Internal BGP ed External BGP**. Il router BGP supporta il peering sia iBGP che eBGP. Per configurarne uno, è necessario assicurarsi che ai router BGP locali e remoti vengano assegnati i numeri di sistema autonomo appropriati. Tutte e quattro le topologie di distribuzione BGP adottano l'uso del peering eBGP. La quarta topologia usa anche il peering iBGP.  
  
**Interoperabilità con soluzioni di terze parti**. Il router BGP si basa sull'ultima specifica della versione 4 del protocollo BGP e ne è stata testata l'interoperabilità con la maggior parte dei principali dispositivi di routing BGP di terze parti. Per altre informazioni, vedere la specifica RFC (Request for Comments) 4271, [A Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Supporto del peering di trasporto IPv4 e IPv6**. Il router BGP supporta il peering sia IPv4 che IPv6. Tuttavia, è necessario configurare l'identificatore BGP come indirizzo IPv4 del router BGP. Per tutte le topologie di distribuzione di router BGP, è possibile usare uno dei due tipi di peering (IPV4/IPv6).  
  
**Funzionalità di apprendimento e annuncio delle route unicast IPv4 e IPv6 (NLRI [Network Layer Reachability Information] multiprotocollo)** . Indipendentemente dal trasporto usato, il router BGP può scambiare le route IPv4 e IPv6 se viene annunciata la capacità appropriata dagli altri router BGP mentre viene stabilita la sessione. Per configurare il routing IPv6, il parametro IPv6Routing deve essere abilitato e un indirizzo IPv6 globale locale deve essere configurato a livello di router.  
  
**Peering in modalità mista e in modalità passiva**. È possibile configurare le sessioni di peering BGP in modalità mista, in cui il router BGP funge iniziatore e risponditore - o la modalità passiva, in cui il router BGP non avviare peering, ma rispondere alle richieste in ingresso. La modalità mista è quella predefinita ed è consigliata per il peering BGP, a meno che non si voglia usare la modalità passiva a scopo di debug o di diagnostica. Per tutte le topologie di distribuzione di router BGP, il peering in modalità mista è necessario per abilitare i riavvii automatici in caso di eventi di tipo errore.  
  
**Funzionalità di riscrittura degli attributi delle route**. È possibile aggiungere, modificare o rimuovere gli attributi seguenti dagli annunci delle route in ingresso e in uscita dei router BGP usando i criteri di routing BGP: Next-Hop, MED, Local-Pref e Community.  
  
**Filtro delle route**. Il router BGP supporta il filtro degli annunci delle route in ingresso o in uscita in base a più attributi delle route, ad esempio Prefix, ASN-Range, Community e Next-Hop.  
  
**Route Reflector (RR) e record client**. Il Router BGP può fungere da un riflettore di Route e un client di record di risorse. Ciò è utile in topologie complesse in cui RR consente di semplificare la rete mediante record di risorse cluster.  
  
**Supporto dell'aggiornamento della route**. Il router BGP supporta l'aggiornamento della route e annuncia questa capacità nel peering per impostazione predefinita. È in grado di inviare una nuova serie di aggiornamenti delle route quando richiesto da un peer tramite messaggio di aggiornamento di route, come l'invio di un aggiornamento Route per aggiornare la tabella di Routing di eventi quali le modifiche dei criteri di Routing di un peer. In questo modo lo scenario di modifica o l'aggiornamento di criteri di Routing BGP in Windows Server 2016 senza la necessità di riavviare il peering.  
  
**Configurazione delle route statiche**. È possibile configurate route o interfacce statiche sul router BGP usando il comando **Add-BgpCustomRoute** di Windows PowerShell. Le route statiche configurate possono essere i prefissi o il nome delle interfacce da cui devono essere scelte le route. Tuttavia, solo delle route con passaggi successivi risolvibili viene eseguito il plumbing nelle tabelle di routing BGP e ne viene dato l'annuncio ai peer.  
  
**Supporto del routing di transito**. Il Router BGP supporta il routing di transito per iBGP le connessioni iBGP, iBGP per connessioni eBGP nonché eBGP eBGP le connessioni.  
  
**Indirizzare ali attenuazione**. Route ali attenuazione al Routing BGP in Windows Server 2016 fornisce supporto per l'attenuazione ali Route. Ad esempio, quando una route è costantemente pubblicizzata e ritirati, rendendo instabile, la tabella di routing è possibile configurare il Router BGP per assegnare un peso di blocco per la route e verificare la presenza di flap - esclusione di conseguenza o annullamento-eliminarlo in base alle esigenze. Ciò consente di gestire una tabella di routing stabile ed elaborazione da parte del Router BGP.  
  
**Indirizzare aggregazione**. Aggregazione di route per il Router BGP offre la possibilità di configurare le route di aggregazione e di sostituire gli annunci di route più granulari con le route di riepilogo o aggregazione di peer. Ciò comporta un minore numero di messaggi di annuncio route trasmessi in rete.  
  
> [!NOTE]  
> In System Center, il Gateway RAS è denominato Gateway Windows Server.  
  

  

