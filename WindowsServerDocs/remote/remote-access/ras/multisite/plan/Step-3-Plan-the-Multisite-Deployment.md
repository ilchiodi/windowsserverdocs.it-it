---
title: Passaggio 3 pianificare la distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e85998138f3aa3627b5e212766d491cd3fc8305f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313866"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Passaggio 3 pianificare la distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura multisito, pianificare eventuali requisiti aggiuntivi per i certificati, il modo in cui i computer client selezionano i punti di ingresso e gli indirizzi IPv6 assegnati nella distribuzione.  

Nelle sezioni seguenti vengono fornite informazioni dettagliate sulla pianificazione.
  
## <a name="31-plan-ip-https-certificates"></a><a name="bkmk_3_1_IPHTTPS"></a>3,1 pianificare i certificati IP-HTTPS  
Quando si configurano i punti di ingresso, è necessario configurare ogni punto di ingresso con un indirizzo ConnectTo specifico. Il certificato IP-HTTPS per ogni punto di ingresso deve corrispondere all'indirizzo ConnectTo. Quando si ottiene il certificato, tenere presente quanto segue:  
  
-   In una distribuzione multisito non è possibile utilizzare certificati autofirmati.  
  
-   L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
-   Nel campo soggetto specificare l'indirizzo IPv4 della scheda esterna del server di accesso remoto (se l'indirizzo ConnectTo è stato specificato come indirizzo IP e non come nome DNS) oppure il nome di dominio completo dell'URL IP-HTTPS.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito Web IP-HTTPS. È supportato anche l'uso di un URL con caratteri jolly che corrisponde al nome DNS ConnectTo.  
  
-   I certificati IP-HTTPS possono usare caratteri jolly nel nome del soggetto. Lo stesso certificato con caratteri jolly può essere utilizzato per tutti i punti di ingresso.  
  
-   Per il campo Utilizzo chiavi avanzato usare l'identificatore di oggetto (OID) di autenticazione server.  
  
-   Se si supportano i computer client che eseguono Windows 7 nella distribuzione multisito, nel campo punti di distribuzione CRL specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet. Questa operazione non è necessaria per i client che eseguono Windows 8 (per impostazione predefinita, il controllo delle revoche CRL è disabilitato per IP-HTTPS su questi client).  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale del computer e non in quello dell'utente.  
  
## <a name="32-plan-the-network-location-server"></a><a name="bkmk_3_2_NLS"></a>3,2 pianificare il server dei percorsi di rete  
Il sito Web del server dei percorsi di rete può essere ospitato nel server di accesso remoto o in un altro server dell'organizzazione. Se si ospita il server dei percorsi di rete nel server di accesso remoto, il sito Web viene creato automaticamente quando si distribuisce accesso remoto. Se si ospita il server dei percorsi di rete in un altro server che esegue un sistema operativo Windows nell'organizzazione, è necessario assicurarsi che Internet Information Services (IIS) sia installato per creare il sito Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisiti dei certificati per il server dei percorsi di rete  
Verificare che il sito Web del server dei percorsi di rete soddisfi i requisiti seguenti per la distribuzione dei certificati:  
  
-   Richiede un certificato server HTTPS.  
  
-   Se il server dei percorsi di rete si trova sul server di accesso remoto e si è scelto di utilizzare un certificato autofirmato durante la distribuzione del singolo server di accesso remoto, è necessario riconfigurare la distribuzione a server singolo per l'utilizzo di un certificato emesso da una CA interna.  
  
-   I computer client DirectAccess devono considerare attendibile la CA che ha emesso il certificato del server nel sito Web del server dei percorsi di rete.  
  
-   I computer client DirectAccess nella rete interna devono essere in grado di risolvere il nome del sito Web del server dei percorsi di rete.  
  
-   Il sito Web del server dei percorsi di rete deve essere a disponibilità elevata per i computer della rete interna.  
  
-   Il server dei percorsi di rete non deve essere accessibile dai computer client DirectAccess in Internet.  
  
-   Il certificato del server deve essere verificato in base a un elenco di revoche di certificati (CRL).  
  
-   I certificati con caratteri jolly non sono supportati quando il server dei percorsi di rete è ospitato nel server di accesso remoto.  
  
Quando si ottiene il certificato del sito Web da utilizzare per il server dei percorsi di rete, tenere presente quanto segue:  
  
1.  Nel campo Soggetto, specificare l'indirizzo IP dell'interfaccia Intranet del server dei percorsi di rete o il nome di dominio completo dell'URL del percorso di rete. Si noti che non è necessario specificare un indirizzo IP se il server dei percorsi di rete è ospitato nel server di accesso remoto. Questo perché il server dei percorsi di rete deve usare lo stesso nome soggetto per tutti i punti di ingresso e non tutti i punti di ingresso hanno lo stesso indirizzo IP.  
  
2.  Nel campo Utilizzo chiavi avanzato, usare l'OID di autenticazione server.  
  
3.  Per il campo punti di distribuzione CRL, utilizzare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla Intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2 DNS per il server dei percorsi di rete  
Se si ospita il server dei percorsi di rete nel server di accesso remoto, è necessario aggiungere una voce DNS per il sito Web del server dei percorsi di rete per ogni punto di ingresso nella distribuzione. Tenere presente quanto segue:  
  
-   Il nome soggetto del primo certificato server dei percorsi di rete nella distribuzione multisito viene usato come URL del server dei percorsi di rete per tutti i punti di ingresso, quindi il nome del soggetto e l'URL del server dei percorsi di rete non possono corrispondere al nome del computer del primo server di accesso remoto nella distribuzione. Deve essere un FQDN dedicato per il server dei percorsi di rete.  
  
-   Il servizio fornito dal traffico del server dei percorsi di rete è bilanciato tra i punti di ingresso tramite DNS e pertanto deve essere presente una voce DNS con lo stesso URL per ogni punto di ingresso, configurato con l'indirizzo IP interno del punto di ingresso.  
  
-   Tutti i punti di ingresso devono essere configurati con un certificato del server dei percorsi di rete con lo stesso nome soggetto (che corrisponde all'URL del server dei percorsi di rete).  
  
-   L'infrastruttura del server dei percorsi di rete (impostazioni DNS e certificato) per un punto di ingresso deve essere creata prima di aggiungere il punto di ingresso.  
  
## <a name="33-plan-the-ipsec-root-certificate-for-all-remote-access-servers"></a><a name="bkmk_3_3_IPsec"></a>3,3 pianificare il certificato radice IPsec per tutti i server di accesso remoto  
Quando si pianifica l'autenticazione del client IPsec in una distribuzione multisito, tenere presente quanto segue:  
  
1.  Se si è scelto di usare il proxy Kerberos predefinito per l'autenticazione del computer quando si configura il singolo server di accesso remoto, è necessario modificare l'impostazione in modo da usare i certificati del computer rilasciati da una CA interna, perché il proxy Kerberos non è supportato per un multisito distribuzione.  
  
2.  Se è stato usato un certificato autofirmato, è necessario riconfigurare la distribuzione a server singolo per usare un certificato emesso da una CA interna.  
  
3.  Affinché l'autenticazione IPsec venga completata durante l'autenticazione del client, tutti i server di accesso remoto devono disporre di un certificato emesso dalla radice IPsec o CA intermedia e con l'OID di autenticazione client per l'utilizzo chiavi avanzato.  
  
4.  Lo stesso certificato radice o intermedio IPsec deve essere installato in tutti i server di accesso remoto nella distribuzione multisito.  
  
## <a name="34-plan-global-server-load-balancing"></a><a name="bkmk_3_4_GSLB"></a>3,4 pianificare il bilanciamento del carico globale del server  
In una distribuzione multisito è inoltre possibile configurare un servizio di bilanciamento del carico globale del server. Un servizio di bilanciamento del carico globale del server può essere utile per l'organizzazione se la distribuzione copre una distribuzione geografica di grandi dimensioni perché può distribuire il carico del traffico tra i punti di ingresso.  Il servizio di bilanciamento del carico globale del server può essere configurato per fornire ai client DirectAccess le informazioni sul punto di ingresso del punto di ingresso più vicino. Il processo funziona nel modo seguente:  
  
1.  Nei computer client che eseguono Windows 10 o Windows 8 è presente un elenco di indirizzi IP globali del servizio di bilanciamento del carico del server, ognuno associato a un punto di ingresso.  
  
2.  Il computer client Windows 10 o Windows 8 tenta di risolvere il nome di dominio completo del servizio di bilanciamento del carico del server globale nel DNS pubblico in un indirizzo IP. Se l'indirizzo IP risolto è elencato come indirizzo IP globale del servizio di bilanciamento del carico del server di un punto di ingresso, il computer client seleziona automaticamente tale punto di ingresso e si connette al relativo URL IP-HTTPS (indirizzo ConnectTo) o all'indirizzo IP del server Teredo. Si noti che l'indirizzo IP del servizio di bilanciamento del carico del server globale non deve essere identico all'indirizzo ConnectTo o all'indirizzo del server Teredo del punto di ingresso, poiché i computer client non tentano mai di connettersi all'indirizzo IP del servizio di bilanciamento del carico globale del server.  
  
3.  Se il computer client si trova dietro un proxy Web (e non può usare la risoluzione DNS) o se il nome FQDN del servizio di bilanciamento del carico globale del server non viene risolto in un indirizzo IP del servizio di bilanciamento del carico server globale configurato, verrà selezionato automaticamente un punto di ingresso usando un probe HTTPS per URL IP-HTTPS di tutti i punti di ingresso. Il client si connetterà al server che risponde per primo.  
  
Per un elenco dei dispositivi globali di bilanciamento del carico del server che supportano l'accesso remoto, passare alla pagina trova un partner in [Microsoft Server and Cloud Platform](https://www.microsoft.com/server-cloud/).  
  
## <a name="35-plan-directaccess-client-entry-point-selection"></a><a name="bkmk_3_5_EP_Selection"></a>3,5 pianificare la selezione del punto di ingresso del client DirectAccess  
Quando si configura una distribuzione multisito, per impostazione predefinita i computer client Windows 10 e Windows 8 sono configurati con le informazioni necessarie per connettersi a tutti i punti di ingresso nella distribuzione e per connettersi automaticamente a un singolo punto di ingresso in base a una selezione algoritmo. È anche possibile configurare la distribuzione per consentire ai computer client Windows 10 e Windows 8 di selezionare manualmente il punto di ingresso a cui si connetteranno. Se un computer client Windows 10 o Windows 8 è attualmente connesso al punto di ingresso Stati Uniti e la selezione automatica dei punti di ingresso è abilitata, se il punto di ingresso Stati Uniti diventa irraggiungibile, dopo alcuni minuti il computer client tenterà di connettersi tramite il punto di ingresso dell'Europa. È consigliabile usare la selezione automatica del punto di ingresso. Tuttavia, consentire la selezione del punto di ingresso manuale consente agli utenti finali di connettersi a un punto di ingresso diverso in base alle condizioni di rete correnti. Ad esempio, se un computer è connesso al punto di ingresso Stati Uniti e la connessione alla rete interna diventa molto più lenta del previsto. In questa situazione, l'utente finale può selezionare manualmente per connettersi al punto di ingresso dell'Europa per migliorare la connessione alla rete interna.  
  
> [!NOTE]  
> Quando un utente finale seleziona un punto di ingresso manualmente, il computer client non eseguirà il ripristino della selezione automatica dei punti di ingresso. Ovvero, se il punto di ingresso selezionato manualmente diventa irraggiungibile, l'utente finale deve ripristinare la selezione automatica dei punti di ingresso oppure selezionare manualmente un altro punto di ingresso.  
  
 I computer client Windows 7 sono configurati con le informazioni necessarie per connettersi a un singolo punto di ingresso nella distribuzione multisito. Non possono archiviare contemporaneamente le informazioni per più punti di ingresso. Un computer client Windows 7, ad esempio, può essere configurato per connettersi al punto di ingresso del Stati Uniti, ma non al punto di ingresso dell'Europa. Se il punto di ingresso del Stati Uniti non è raggiungibile, il computer client Windows 7 perderà la connettività alla rete interna finché il punto di ingresso non sarà raggiungibile. L'utente finale non può apportare alcuna modifica per tentare di connettersi al punto di ingresso dell'Europa.  
  
## <a name="36-plan-prefixes-and-routing"></a><a name="bkmk_3_6_IPv6"></a>3,6 prefissi e routing del piano  
  
### <a name="internal-ipv6-prefix"></a>Prefisso IPv6 interno  
Durante la distribuzione del singolo server di accesso remoto pianificato i prefissi IPv6 della rete interna tenere presente quanto segue in una distribuzione multisito:  
  
1.  Se sono stati inclusi tutti i siti di Active Directory quando è stata configurata la distribuzione di accesso remoto a server singolo, i prefissi IPv6 della rete interna saranno già definiti nella console di gestione accesso remoto.  
  
2.  Se si creano ulteriori siti Active Directory per la distribuzione multisito, è necessario pianificare nuovi prefissi IPv6 per i siti aggiuntivi e definirli in accesso remoto. Si noti che i prefissi IPv6 possono essere configurati solo tramite la console di gestione accesso remoto o i cmdlet di PowerShell se IPv6 viene distribuito nella rete aziendale interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefisso IPv6 per i computer client DirectAccess (prefisso IP-HTTPS)  
  
1.  Se IPv6 viene distribuito nella rete aziendale interna, è necessario pianificare un prefisso IPv6 da assegnare ai computer client DirectAccess in tutti i punti di ingresso aggiuntivi della distribuzione.  
  
2.  Verificare che i prefissi IPv6 da assegnare ai computer client DirectAccess in ogni punto di ingresso siano distinti e che non vi siano sovrapposizioni nei prefissi IPv6.  
  
3.  Se IPv6 non è distribuito nella rete aziendale, quando si aggiunge il punto di ingresso verrà selezionato automaticamente un prefisso IP-HTTPS per ogni punto di ingresso.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefisso IPv6 per client VPN  
Se è stata distribuita una VPN nel server di accesso remoto singolo, tenere presente quanto segue:  
  
1.  L'aggiunta di un prefisso VPN IPv6 a un punto di ingresso è necessaria solo se si vuole consentire la connettività IPv6 del client VPN alla rete aziendale.  
  
2.  Il prefisso VPN può essere configurato solo su un punto di ingresso usando la console di gestione accesso remoto o il cmdlet di PowerShell se IPv6 viene distribuito nella rete aziendale interna e se la VPN è abilitata nel punto di ingresso.  
  
3.  Il prefisso VPN deve essere univoco in ogni punto di ingresso e non deve sovrapporsi ad altri prefissi VPN o IP-HTTPS.  
  
4.  Se IPv6 non è distribuito nella rete aziendale, ai client VPN che si connettono al punto di ingresso non verrà assegnato un indirizzo IPv6.  
  
### <a name="routing"></a>Routing  
In una distribuzione multisito simmetrica il routing viene applicato usando Teredo e IP-HTTPS. Quando IPv6 viene distribuito nella rete aziendale, tenere presente quanto segue:  
  
1. I prefissi Teredo e IP-HTTPS di ogni punto di ingresso devono essere instradabili attraverso la rete aziendale al server di accesso remoto associato.  
  
2. Le route devono essere configurate nell'infrastruttura di routing della rete aziendale.  
  
3. Per ogni punto di ingresso devono essere presenti da una a tre Route nella rete interna:  
  
   1. Prefisso IP-HTTPS. questo prefisso viene scelto dall'amministratore nella procedura guidata Aggiungi punto di ingresso.  
  
   2. Prefisso IPv6 VPN (facoltativo). Questo prefisso può essere scelto dopo l'abilitazione della VPN per un punto di ingresso  
  
   3. Prefisso Teredo (facoltativo). Questo prefisso è pertinente solo se il server di accesso remoto è configurato con due indirizzi IPv4 pubblici consecutivi nella scheda esterna. Il prefisso è basato sul primo indirizzo IPv4 pubblico della coppia di indirizzi. Se, ad esempio, gli indirizzi esterni sono:  
  
      1. www\.xxx. yyy. zzz  
  
      2. www\.xxx. yyy. zzz + 1  
  
      Quindi il prefisso Teredo da configurare è 2001:0: WWXX: YYZZ::/64, dove WWXX: YYZZ è la rappresentazione esadecimale dell'indirizzo IPv4 www\.xxx. yyy. zzz.  
  
      Si noti che è possibile usare lo script seguente per calcolare il prefisso Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Tutte le route precedenti devono essere indirizzate all'indirizzo IPv6 nella scheda interna del server di accesso remoto (o all'indirizzo IP virtuale (VIP) interno per un punto di ingresso con carico bilanciato).  
  
> [!NOTE]  
> Quando IPv6 viene distribuito nella rete aziendale e l'amministrazione del server di accesso remoto viene eseguita in modalità remota su DirectAccess, le route per i prefissi IP-HTTPS e Teredo di tutti gli altri punti di ingresso devono essere aggiunte a ogni server di accesso remoto, in modo che il traffico venga da inviare alla rete interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Prefissi IPv6 specifici del sito Active Directory  
Quando un computer client che esegue Windows 10 o Windows 8 è connesso a un punto di ingresso, il computer client viene immediatamente associato al sito Active Directory del punto di ingresso e viene configurato con i prefissi IPv6 associati al punto di ingresso. La preferenza è che i computer client si connettano alle risorse usando questi prefissi IPv6, perché sono configurati in modo dinamico nella tabella dei criteri di prefisso IPv6 con precedenza maggiore durante la connessione a un punto di ingresso.  
  
Se l'organizzazione usa una topologia Active Directory con prefissi IPv6 specifici del sito, ad esempio un FQDN di risorsa interna app.corp.com è ospitato sia in America del Nord che in Europa con un indirizzo IP specifico del sito in ogni posizione, questo non è configurato da per impostazione predefinita viene utilizzata la console di accesso remoto e i prefissi IPv6 specifici del sito non sono configurati per ogni punto di ingresso. Se si vuole abilitare questo scenario facoltativo, è necessario configurare ogni punto di ingresso con i prefissi IPv6 specifici che devono essere preferiti dai computer client che si connettono a un punto di ingresso specifico. Procedere come segue:  
  
1.  Per ogni oggetto Criteri di gruppo usato per i computer client Windows 10 o Windows 8, eseguire il cmdlet di PowerShell set-DAEntryPointTableItem  
  
2.  Impostare il parametro EntryPointRange per il cmdlet con i prefissi IPv6 specifici del sito. Ad esempio, per aggiungere i prefissi specifici del sito 2001: DB8:1: 1::/64 e 2001: DB: 1:2::/64 a un punto di ingresso denominato Europe, eseguire il comando seguente  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Quando si modifica il parametro EntryPointRange, assicurarsi di non rimuovere i prefissi a 128 bit esistenti che appartengono agli endpoint del tunnel IPsec e all'indirizzo DNS64.  
  
## <a name="37-plan-the-transition-to-ipv6-when-multisite-remote-access-is-deployed"></a><a name="bkmk_3_7_TransitionIPv6"></a>3,7 pianificare la transizione a IPv6 quando si distribuisce l'accesso remoto multisito  
Molte organizzazioni usano il protocollo IPv4 nella rete aziendale. Con l'esaurimento dei prefissi IPv4 disponibili, molte organizzazioni stanno effettuando la transizione da solo IPv4 alle reti solo IPv6.  
  
Questa transizione è probabilmente eseguita in due fasi:  
  
1.  Da un solo IPv4 a una rete aziendale IPv6 + IPv4.  
  
2.  Da un IPv6 + IPv4 a una rete aziendale solo IPv6.  
  
In ogni parte, la transizione può essere eseguita in fasi. In ogni fase è possibile che una sola subnet della rete venga modificata nella nuova configurazione di rete. Pertanto, è necessaria una distribuzione multisito di DirectAccess per supportare una distribuzione ibrida in cui, ad esempio, alcuni dei punti di ingresso appartengono a una subnet solo IPv4 e altri appartengono a una subnet IPv6 + IPv4. Inoltre, le modifiche alla configurazione durante i processi di transizione non devono interrompere la connettività client tramite DirectAccess.  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6ipv4-corporate-network"></a><a name="TransitionIPv4toMixed"></a>Transizione da un solo IPv4 a una rete aziendale IPv6 + IPv4  
Quando si aggiungono indirizzi IPv6 a una rete aziendale solo IPv4, potrebbe essere necessario aggiungere un indirizzo IPv6 a un server DirectAccess già distribuito. Inoltre, è possibile aggiungere un punto di ingresso o un nodo a un cluster con carico bilanciato con indirizzi sia IPv4 che IPv6 alla distribuzione di DirectAccess.  
  
Accesso remoto consente di aggiungere i server con indirizzi IPv4 e IPv6 a una distribuzione originariamente configurata con solo indirizzi IPv4. Questi server vengono aggiunti come server solo IPv4 e i relativi indirizzi IPv6 vengono ignorati da DirectAccess. di conseguenza, l'organizzazione non può sfruttare i vantaggi della connettività IPv6 nativa su questi nuovi server.  
  
Per trasformare la distribuzione in una distribuzione IPv6 + IPv4 e sfruttare le funzionalità IPv6 native, è necessario reinstallare DirectAccess. Per mantenere la connettività client durante la reinstallazione, vedere la pagina relativa alla transizione da un solo IPv4 a una distribuzione solo IPv6 usando le distribuzioni Dual DirectAccess.  
  
> [!NOTE]  
> Come per una rete solo IPv4, in una rete mista IPv4 + IPv6, l'indirizzo del server DNS usato per risolvere le richieste DNS client deve essere configurato con il DNS64 distribuito nei server di accesso remoto e non con un DNS aziendale.  
  
### <a name="transition-from-an-ipv6ipv4-to-an-ipv6-only-corporate-network"></a><a name="TransitionMixedtoIPv6"></a>Transizione da un IPv6 + IPv4 a una rete aziendale solo IPv6  
DirectAccess consente di aggiungere punti di ingresso solo IPv6 solo se il primo server di accesso remoto nella distribuzione aveva originariamente indirizzi sia IPv4 che IPv6 o solo un indirizzo IPv6. Ovvero non è possibile eseguire la transizione da una rete solo IPv4 a una rete solo IPv6 in un unico passaggio senza reinstallare DirectAccess. Per passare direttamente da una rete solo IPv4 a una rete solo IPv6, vedere transizione da una rete solo IPv4 a una distribuzione solo IPv6 usando le distribuzioni di DirectAccess duali.  
  
Dopo aver completato la transizione da una distribuzione solo IPv4 a una distribuzione IPv6 + IPv4, è possibile passare a una rete solo IPv6. Durante e dopo la transizione, tenere presente quanto segue:  
  
-   Se tutti i server back-end solo IPv4 rimangono nella rete aziendale, non saranno raggiungibili dai client che si connettono tramite i punti di ingresso solo IPv6.  
  
-   Quando si aggiungono punti di ingresso solo IPv6 a una distribuzione IPv4 + IPv6, DNS64 e NAT64 non verranno abilitati nei nuovi server. I client che si connettono a questi punti di ingresso verranno configurati automaticamente per l'utilizzo dei server DNS aziendali.  
  
-   Se è necessario eliminare indirizzi IPv4 da un server distribuito, è necessario rimuovere il server dalla distribuzione DirectAccess, rimuovere l'indirizzo di rete aziendale IPv4 e aggiungerlo di nuovo alla distribuzione.  
  
Per supportare la connettività client alla rete aziendale, è necessario assicurarsi che il server dei percorsi di rete possa essere risolto dal DNS aziendale al relativo indirizzo IPv6. È anche possibile impostare un indirizzo IPv4 aggiuntivo, ma non è obbligatorio.  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6-only-deployment-using-dual-directaccess-deployments"></a><a name="DualDeployment"></a>Transizione da un solo IPv4 a una distribuzione solo IPv6 usando distribuzioni Dual DirectAccess  
Non è possibile eseguire la transizione da una rete aziendale solo IPv4 a una rete aziendale solo IPv6 senza reinstallare la distribuzione di DirectAccess. Per mantenere la connettività del client durante la transizione, è possibile usare un'altra distribuzione di DirectAccess. Quando viene completata la prima fase di transizione (rete solo IPv4 aggiornata a IPv4 + IPv6), è necessaria una distribuzione doppia e si intende prepararsi per una transizione futura a una rete aziendale solo IPv6, sfruttando i vantaggi della connettività IPv6 nativa. La doppia distribuzione viene descritta nei passaggi generali seguenti:  
  
1.  Installare una seconda distribuzione di DirectAccess. È possibile installare DirectAccess nei nuovi server o rimuovere i server dalla prima distribuzione e usarli per la seconda distribuzione.  
  
    > [!NOTE]  
    > Quando si installa una distribuzione di DirectAccess aggiuntiva insieme a una corrente, assicurarsi che due punti di ingresso non condividano lo stesso prefisso client.  
    >   
    > Se si installa DirectAccess tramite la procedura guidata di Introduzione o con il cmdlet `Install-RemoteAccess`, accesso remoto imposta automaticamente il prefisso client del primo punto di ingresso nella distribuzione su un valore predefinito di < prefisso\_subnet IPv6 >: 1000::/64. Se necessario, è necessario modificare il prefisso.  
  
2.  Rimuovere i gruppi di sicurezza client scelti dalla prima distribuzione.  
  
3.  Aggiungere i gruppi di sicurezza client alla seconda distribuzione.  
  
    > [!IMPORTANT]  
    > Per mantenere la connettività client durante il processo, è necessario aggiungere i gruppi di sicurezza alla seconda distribuzione immediatamente dopo averli rimossi dalla prima. Ciò garantisce che i client non verranno aggiornati con due o zero oggetti Criteri di gruppo DirectAccess. I client inizieranno a usare la seconda distribuzione dopo il recupero e l'aggiornamento dell'oggetto Criteri di gruppo del client.  
  
4.  Facoltativo: rimuovere i punti di ingresso di DirectAccess dalla prima distribuzione e aggiungere i server come nuovi punti di ingresso nel secondo.  
  
Al termine della transizione, è possibile disinstallare la prima distribuzione di DirectAccess. Quando si disinstalla, è possibile che si verifichino i problemi seguenti:  
  
-   Se la distribuzione è stata configurata in modo da supportare solo i client sui computer portatili, il filtro WMI verrà eliminato. Se i gruppi di sicurezza client della seconda distribuzione includono computer desktop, l'oggetto Criteri di gruppo del client DirectAccess non consentirà di filtrare i computer desktop e potrebbe causare problemi. Se è necessario un filtro per computer portatili, ricrearlo seguendo le istruzioni riportate in [creare filtri WMI per l'oggetto Criteri](https://technet.microsoft.com/library/cc947846.aspx)di gruppo.  
  
-   Se entrambe le distribuzioni sono state create originariamente nello stesso dominio Active Directory, la voce del probe DNS che punta a localhost verrà eliminata e potrebbe causare problemi di connettività dei client. È ad esempio possibile che i client si connettano utilizzando IP-HTTPS anziché Teredo oppure passano tra i punti di ingresso multisito di DirectAccess. In questo caso, è necessario aggiungere la voce DNS seguente al DNS aziendale:  
  
    -   Zona: nome di dominio  
  
    -   Nome: DirectAccess-corpConnectivityHost  
  
    -   Indirizzo IP::: 1  
  
    -   Tipo: AAAA  
  
  
  


