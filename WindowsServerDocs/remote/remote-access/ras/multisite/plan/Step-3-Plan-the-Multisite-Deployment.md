---
title: Passaggio 3 piano la distribuzione Multisita
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 16a2dcdc573fac2631b5a9890ee04f2efb08d90a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282533"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Passaggio 3 piano la distribuzione Multisita

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura multisito, pianificare i requisiti di qualsiasi certificato aggiuntivo, come i computer client Seleziona punti di ingresso e gli indirizzi IPv6 assegnati nella distribuzione.  

Le sezioni seguenti riportano informazioni dettagliate sulla pianificazione.
  
## <a name="bkmk_3_1_IPHTTPS"></a>Certificati IP-HTTPS prevede 3.1  
Quando si configurano i punti di ingresso, si configura ogni punto di ingresso con uno specifico indirizzo ConnectTo. Il certificato IP-HTTPS per ogni punto di ingresso deve corrispondere all'indirizzo ConnectTo. Durante il recupero del certificato, tenere presente quanto segue:  
  
-   In una distribuzione multisito non è possibile utilizzare certificati autofirmati.  
  
-   L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
-   Nel campo soggetto, specificare l'indirizzo IPv4 della scheda esterna del server di accesso remoto (se l'indirizzo ConnectTo è stato specificato come un indirizzo IP e non un nome DNS), o il FQDN dell'URL IP-HTTPS.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito Web IP-HTTPS. È inoltre supportato l'utilizzo di un URL con caratteri jolly che corrisponde al nome DNS ConnectTo.  
  
-   I certificati IP-HTTPS possono usare caratteri jolly nel nome del soggetto. Lo stesso certificato con caratteri jolly può essere utilizzato per tutti i punti di ingresso.  
  
-   Per il campo Utilizzo chiavi avanzato usare l'identificatore di oggetto (OID) di autenticazione server.  
  
-   Se si prevede di supportare computer client che eseguono Windows 7 nella distribuzione multisito, nel campo punti di distribuzione CRL, specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet. Ciò non è necessario per i client che eseguono Windows 8 (per impostazione predefinita che la verifica delle revoche CRL viene disabilitata per IP-HTTPS in questi client).  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale del computer e non dell'utente.  
  
## <a name="bkmk_3_2_NLS"></a>3.2 piano server dei percorsi di rete  
Il sito Web server percorsi di rete può essere ospitato nel server di accesso remoto o in un altro server all'interno dell'organizzazione. Se si ospita il server dei percorsi rete sul server di accesso remoto, il sito Web viene creato automaticamente quando si distribuisce accesso remoto. Se si ospita il server dei percorsi rete in un altro server che eseguono un sistema operativo Windows nell'organizzazione, è necessario assicurarsi che sia installato Internet Information Services (IIS) per creare il sito Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisiti dei certificati per server dei percorsi di rete  
Assicurarsi che il sito Web server percorsi di rete soddisfi i requisiti seguenti per la distribuzione del certificato:  
  
-   Richiede un certificato server HTTPS.  
  
-   Se il server dei percorsi di rete si trova nel server di accesso remoto e si è scelto di usare un certificato autofirmato quando è stato distribuito il server di accesso remoto singolo, è necessario riconfigurare la distribuzione a server singolo per utilizzare un certificato emesso da un'autorità di certificazione interna.  
  
-   I computer client DirectAccess devono considerare attendibile la CA che ha emesso il certificato del server nel sito Web del server dei percorsi di rete.  
  
-   I computer client DirectAccess nella rete interna devono essere in grado di risolvere il nome del server dei percorsi di rete.  
  
-   Il sito Web server percorsi di rete deve essere a disponibilità elevata per i computer nella rete interna.  
  
-   Il server dei percorsi di rete non deve essere accessibile dai computer client DirectAccess in Internet.  
  
-   Il certificato del server deve essere archiviato in un elenco di revoche certificati (CRL).  
  
-   Certificati con caratteri jolly non sono supportati quando il server dei percorsi rete è ospitato nel server di accesso remoto.  
  
Quando si ottiene il certificato del sito Web da utilizzare per il server dei percorsi di rete, tenere presente quanto segue:  
  
1.  Nel campo Soggetto, specificare l'indirizzo IP dell'interfaccia Intranet del server dei percorsi di rete o il nome di dominio completo dell'URL del percorso di rete. Si noti che è necessario specificare un indirizzo IP non se il server dei percorsi rete è ospitato nel server di accesso remoto. Infatti, il server dei percorsi di rete devono usare lo stesso nome di soggetto per tutti i punti di ingresso e non tutti i punti di ingresso hanno lo stesso indirizzo IP.  
  
2.  Nel campo Utilizzo chiavi avanzato, usare l'OID di autenticazione server.  
  
3.  Per il campo punti di distribuzione CRL, usare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2DNS per il server dei percorsi di rete  
Se si ospita il server dei percorsi rete sul server di accesso remoto, è necessario aggiungere una voce DNS per il sito Web server percorsi di rete per ogni punto di ingresso nella distribuzione. Tenere presente quanto segue:  
  
-   Il nome del soggetto del certificato del server percorso rete prima della distribuzione multisito è utilizzato come URL del server di percorso di rete per tutti i punti di ingresso, pertanto il nome del soggetto e l'URL di server percorsi di rete non può essere lo stesso come il nome del computer il primo server di accesso remoto nella distribuzione. Deve essere un FQDN dedicato per il server dei percorsi di rete.  
  
-   Il servizio fornito da traffico di rete percorso server viene bilanciato tra i punti di ingresso tramite DNS e pertanto deve essere presente una voce DNS con lo stesso URL per ogni punto di ingresso, configurato con l'indirizzo IP interno del punto di ingresso.  
  
-   Tutti i punti di ingresso devono essere configurati con un certificato server percorsi di rete con lo stesso nome soggetto (che corrisponde all'URL di server di percorso di rete).  
  
-   Prima di aggiungere il punto di ingresso, è necessario creare l'infrastruttura di server di percorso di rete (impostazioni di DNS e il certificato) per un punto di ingresso.  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 piano il certificato radice IPsec per tutti i server di accesso remoto  
Durante la pianificazione per l'autenticazione client IPsec in una distribuzione multisito, tenere presente quanto segue:  
  
1.  Se si è scelto di usare il proxy Kerberos incorporato per l'autenticazione del computer quando si configura il server di accesso remoto singolo, è necessario modificare l'impostazione per usare i certificati computer emessi da un'autorità di certificazione interna, poiché il proxy Kerberos non è supportato per un distribuzione multisito distribuzione.  
  
2.  Se è stato usato un certificato autofirmato, è necessario riconfigurare la distribuzione a server singolo per utilizzare un certificato emesso da un'autorità di certificazione interna.  
  
3.  Per l'autenticazione IPsec avere successo durante l'autenticazione client, tutti i server di accesso remoto devono avere un certificato emesso dalla radice IPsec o autorità di certificazione intermedia e con l'OID di autenticazione Client per l'utilizzo chiavi avanzato.  
  
4.  La stessa radice IPsec o un certificato intermedio deve essere installato su tutti i server di accesso remoto nella distribuzione multisito.  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 pianificare il bilanciamento del carico server globale  
In una distribuzione multisito, è inoltre possibile configurare un bilanciamento del carico globale di server. Se la distribuzione include una vasta distribuzione geografica perché è possibile distribuire il carico del traffico tra i punti di ingresso, può essere utile all'organizzazione un bilanciamento del carico globale di server.  Il bilanciamento del carico globale di server può essere configurato per fornire i client DirectAccess con le informazioni sul punto di ingresso del punto di ingresso più vicino. Il processo funziona nel modo seguente:  
  
1.  Computer client che eseguono Windows 10 o Windows 8 dispone di un elenco di indirizzi IP del servizio di bilanciamento del carico globale server, ognuno associato a un punto di ingresso.  
  
2.  Il computer client Windows 10 o Windows 8 tenta di risolvere il FQDN del servizio di bilanciamento del carico globale di server nel DNS pubblico per un indirizzo IP. Se l'indirizzo IP risolto è elencato come indirizzo IP del servizio di bilanciamento carico globale di server del punto di ingresso, consente di selezionare tale punto di ingresso automaticamente il computer client e si connette al relativo URL IP-HTTPS (indirizzo ConnectTo) o al relativo indirizzo IP del server Teredo. Si noti che l'indirizzo IP del bilanciamento del carico globale di server non devono essere identici all'indirizzo ConnectTo o all'indirizzo del server Teredo del punto di ingresso, poiché i computer client non provano mai a connettersi all'indirizzo IP del servizio di bilanciamento carico globale di server.  
  
3.  Se il computer client si trova dietro un proxy web (e non è possibile usare la risoluzione DNS) o se il bilanciamento del carico globale server FQDN non viene risolto in qualsiasi configurato global server load balancer indirizzo IP, quindi un punto di ingresso verrà selezionato automaticamente Usa un probe HTTPS a gli URL IP-HTTPS di tutti i punti di ingresso. Il client si connetterà al server che risponde per primo.  
  
Per un elenco di dispositivi che supportano l'accesso remoto di bilanciamento del carico globale di server, passare alla scheda Cerca una pagina a Partner [Microsoft Server and Cloud Platform](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 pianificare la selezione del punto di ingresso client DirectAccess  
Quando si configura una distribuzione multisito, per impostazione predefinita, Windows 10 e Windows 8 client computer sono configurati con le informazioni necessarie per connettersi a tutti i punti di ingresso nella distribuzione e per connettere automaticamente a un singolo punto di ingresso basato su una selezione algoritmo. È anche possibile configurare la distribuzione per consentire a Windows 10 e i computer client Windows 8 e selezionare manualmente la voce del punto da cui si connetteranno. Se un computer client Windows 10 o Windows 8 è attualmente connesso al punto di ingresso di Stati Uniti e selezione del punto di ingresso automatico è abilitata, se il punto di ingresso di Stati Uniti è raggiungibile, dopo alcuni minuti il client tenterà di connettersi tramite il punto di ingresso Europa. È consigliabile usare la selezione del punto di ingresso automatica; Tuttavia, consentendo il punto di ingresso manuale selezione consente agli utenti di connettersi a un punto di ingresso diversi in base alle condizioni di rete corrente. Ad esempio, se un computer è connesso al punto di ingresso di Stati Uniti e la connessione alla rete interna diventa molto più lento del previsto. In questo caso, l'utente finale può selezionare manualmente per la connessione al punto di ingresso Europa per migliorare la connessione alla rete interna.  
  
> [!NOTE]  
> Dopo che un utente finale seleziona manualmente un punto di ingresso, il computer client non disporrà di selezione del punto di ingresso automatica. Vale a dire, se il punto di ingresso selezionato manualmente è raggiungibile, l'utente finale deve ripristinare la selezione del punto di ingresso automatico o manualmente selezionare un altro punto di ingresso.  
  
 I computer client Windows 7 sono configurati con le informazioni necessarie per connettersi a un singolo punto di ingresso nella distribuzione multisito. Non possono archiviare le informazioni per più punti di ingresso contemporaneamente. Ad esempio, un computer client Windows 7 possa essere configurato per connettersi al punto di ingresso di Stati Uniti, ma non il punto di ingresso Europa. Se il punto di ingresso di Stati Uniti è raggiungibile, il computer client Windows 7 perderanno la connettività alla rete interna fino a quando il punto di ingresso è raggiungibile. L'utente finale non è possibile apportare le modifiche per tentare di connettersi al punto di ingresso Europa.  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 pianificare i prefissi e routing  
  
### <a name="internal-ipv6-prefix"></a>Prefisso IPv6 interno  
Durante la distribuzione di accesso remoto singolo server è pianificato i prefissi IPv6 della rete interna, tenere presente quanto segue in una distribuzione multisito:  
  
1.  Se è stato incluso tutti i siti di Active Directory durante la configurazione del singolo server di distribuzione di accesso remoto, i prefissi IPv6 della rete interna già essere definiti nella console di gestione accesso remoto.  
  
2.  Se si creano altri siti di Active Directory per la distribuzione multisito, è necessario pianificare nuovi prefissi IPv6 per gli altri siti e modelli di accesso remoto. Si noti che i prefissi IPv6 possono essere configurati solo tramite la console di gestione accesso remoto o i cmdlet di PowerShell se IPv6 viene distribuito nella rete aziendale interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefisso IPv6 per i computer client DirectAccess (prefisso IP-HTTPS)  
  
1.  Se IPv6 viene distribuito nella rete aziendale interna, è necessario pianificare un prefisso IPv6 da assegnare ai computer client DirectAccess in alcun punto di ingresso aggiuntive nella distribuzione.  
  
2.  Assicurarsi che i prefissi IPv6 da assegnare ai computer client DirectAccess in ogni punto di ingresso sono distinti e che non vi sia alcuna sovrapposizione tra i prefissi IPv6.  
  
3.  Se IPv6 non viene distribuito nella rete aziendale, un prefisso IP-HTTPS per ogni punto di ingresso verrà selezionato automaticamente quando si aggiunge il punto di ingresso.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefisso IPv6 per i client VPN  
Se VPN è stato distribuito nel server di accesso remoto singolo, tenere presente quanto segue:  
  
1.  Aggiunta di una VPN IPv6 prefisso da un punto di ingresso è solo necessario se si desidera consentire a client VPN IPv6 connettività alla rete aziendale.  
  
2.  Il prefisso VPN può essere configurato solo in un punto di ingresso usando la console di gestione accesso remoto o i cmdlet di PowerShell se IPv6 viene distribuito nella rete aziendale interna e se è abilitata la VPN sul punto di ingresso.  
  
3.  Il prefisso VPN deve essere univoco in ogni punto di ingresso e non deve sovrapporsi con altri prefissi IP-HTTPS o VPN.  
  
4.  Se IPv6 non viene distribuito nella rete aziendale, client VPN che si connettono al punto di ingresso non riceveranno un indirizzo IPv6.  
  
### <a name="routing"></a>Routing  
In una distribuzione multisito routing simmetrica viene applicato tramite Teredo e IP-HTTPS. Quando IPv6 viene distribuito nella rete aziendale tenere presente quanto segue:  
  
1. I prefissi IP-HTTPS e Teredo di ogni punto di ingresso devono essere instradabili attraverso la rete azienda per i server di accesso remoto associato.  
  
2. Le route devono essere configurate nell'infrastruttura di routing di rete aziendale.  
  
3. Per ogni punto di ingresso deve essere uno a tre route nella rete interna:  
  
   1. Prefisso ciò prefisso IP-HTTPS viene scelta dall'amministratore nella finestra Aggiungi una creazione guidata punto di ingresso.  
  
   2. Prefisso IPv6 VPN (facoltativo). È possibile scegliere questo prefisso dopo avere abilitato la VPN per un punto di ingresso  
  
   3. Prefisso di Teredo (facoltativo). Questo prefisso è rilevante solo se il server di accesso remoto è configurato con due indirizzi IPv4 pubblici consecutivi nella scheda esterna. Il prefisso si basa sul primo indirizzo IPv4 pubblico della coppia di indirizzo. Se, ad esempio gli indirizzi esterni sono:  
  
      1. www.xxx.yyy.zzz  
  
      2. www.xxx.yyy.zzz+1  
  
      Il prefisso di Teredo da configurare è 2001:0:WWXX:YYZZ::/ 64 in cui WWXX: YYZZ è la rappresentazione esadecimale di www.xxx.yyy.zzz l'indirizzo IPv4.  
  
      Si noti che è possibile usare lo script seguente per calcolare il prefisso di Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Tutte le route precedente devono essere indirizzate all'indirizzo IPv6 della scheda interna del server di accesso remoto o per l'indirizzo IP (VIP) virtuale interno per un carico bilanciato punto di ingresso.  
  
> [!NOTE]  
> Quando IPv6 viene distribuito nella rete azienda e amministrazione del server Accesso remoto viene eseguito in remoto tramite DirectAccess, le route per il Teredo e i prefissi IP-HTTPS di tutti gli altri punti di ingresso devono essere aggiunto a ogni server di accesso remoto in modo che il traffico sarà inoltrato alla rete interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Prefissi IPv6 di specifiche del sito di Active Directory  
Quando un computer client che eseguono Windows 10 o Windows 8 è connesso a un punto di ingresso, il computer client è immediatamente associati al sito di Active Directory del punto di ingresso e viene configurato con i prefissi IPv6 associati con il punto di ingresso. La preferenza è per i computer client per connettersi alle risorse usando i prefissi IPv6, poiché vengono configurate in modo dinamico la tabella dei criteri di prefisso IPv6 con priorità più alta quando ci si connette a un punto di ingresso.  
  
Se l'organizzazione Usa una topologia di Active Directory con prefissi IPv6 specifico del sito (ad esempio app.corp.com un FQDN risorsa interna viene ospitato in sia America del Nord ed Europa con un indirizzo IP specifico del sito in ogni posizione) non configurato per predefinito utilizzando la console di accesso remoto e i prefissi IPv6 specifico del sito non è configurata per ogni punto di ingresso. Se si desidera abilitare questo scenario facoltativo, è necessario configurare ogni punto di ingresso con i prefissi IPv6 specifici che devono essere Preferiti dal computer client che si connettono a un punto di ingresso specifico. Procedere come segue:  
  
1.  Per ogni oggetto Criteri di gruppo usato per i computer client di Windows 8 o Windows 10, eseguire il cmdlet di PowerShell Set-DAEntryPointTableItem  
  
2.  Impostare il parametro EntryPointRange per il cmdlet con i prefissi IPv6 specifico del sito. Ad esempio, per aggiungere le specifiche del sito prefissi 2001:db8:1:1::/ 64 e 2001:db:1:2:: / 64 per un punto di ingresso denominato Europa, eseguire il comando seguente  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Quando si modifica il parametro EntryPointRange, assicurarsi di non rimuovere i prefissi di 128 bit esistenti che appartengono all'endpoint del tunnel IPsec e l'indirizzo DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 alla pianificazione della transizione a IPv6 nelle distribuzioni di accesso remoto multisito  
Molte organizzazioni usano il protocollo IPv4 nella rete aziendale. Con l'esaurimento dei prefissi IPv4 disponibili, molte organizzazioni sono in corso la transizione da solo IPv4 a reti solo IPv6.  
  
Questa transizione è molto probabile che si verificano in due fasi:  
  
1.  Da un solo IPv4 a una rete aziendale IPv6 + IPv4.  
  
2.  Da un IPv6 + IPv4 a una rete aziendale solo IPv6.  
  
In ogni parte, è possibile eseguire la transizione in fasi. In ogni fase può essere modificata una sola subnet della rete per la nuova configurazione di rete. Pertanto, una distribuzione multisito di DirectAccess è necessaria per supportare una distribuzione ibrida in cui, ad esempio, alcuni dei punti di ingresso appartengono a una subnet solo IPv4 e ad altri utenti appartengono a una subnet IPv6 + IPv4. Inoltre, le modifiche alla configurazione durante i processi di transizione non deve interrompere la connettività dei client tramite DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>Eseguire la transizione da un solo IPv4 a una rete aziendale IPv6 + IPv4  
Quando si aggiungono gli indirizzi IPv6 a una rete aziendale solo IPv4, è possibile aggiungere un indirizzo IPv6 a un server DirectAccess già distribuito. Inoltre, è possibile aggiungere un nodo o un punto di ingresso a un cluster con bilanciamento di carico con indirizzi IPv4 e IPv6 per la distribuzione di DirectAccess.  
  
Accesso remoto consente di aggiungere i server con indirizzi IPv4 e IPv6 in una distribuzione in cui è stato originariamente configurata solo con indirizzi IPv4. Questi server vengono aggiunti come server solo IPv4 e i relativi indirizzi IPv6 vengono ignorati da DirectAccess; di conseguenza, l'organizzazione non è possibile sfruttare i vantaggi della connettività IPv6 nativa in questi nuovi server.  
  
Per trasformare la distribuzione a una distribuzione di IPv6 + IPv4 e sfruttare i vantaggi delle funzionalità di IPv6 nativo, è necessario reinstallare DirectAccess. Per mantenere la connettività dei client in tutta la reinstallazione vedere transizione da un solo IPv4 a una distribuzione solo IPv6 usando due distribuzioni di DirectAccess.  
  
> [!NOTE]  
> Come con una rete solo IPv4, in una rete IPv4 + IPv6 mista, l'indirizzo del server DNS che viene usato per risolvere le richieste di client DNS deve essere configurato con DNS64 che viene distribuito nel server di accesso remoto e non con un DNS aziendale.  
  
### <a name="TransitionMixedtoIPv6"></a>Eseguire la transizione da IPv6 + IPv4 a una rete aziendale solo IPv6  
DirectAccess consente di aggiungere punti di ingresso solo IPv6 solo se il primo server di accesso remoto nella distribuzione conteneva in origine sia IPv4 e gli indirizzi IPv6 o solo un indirizzo IPv6. Vale a dire, è Impossibile eseguire la transizione da una rete solo IPv4 a una rete solo IPv6 in un unico passaggio senza reinstallare DirectAccess. Per passare direttamente da una rete solo IPv4 a una rete solo IPv6, vedere transizione da un solo IPv4 a una distribuzione solo IPv6 usando due distribuzioni di DirectAccess.  
  
Dopo aver completato la transizione da una distribuzione solo IPv4 a una distribuzione di IPv6 + IPv4, è possibile eseguire la transizione a una rete solo IPv6. Durante e dopo la transizione, tenere presente quanto segue:  
  
-   Se tutti i server back-end solo IPv4 rimangano nella rete aziendale, non saranno raggiungibili ai client che si connettono tramite i punti di ingresso solo IPv6.  
  
-   Quando si aggiungono i punti di ingresso solo IPv6 a una distribuzione di IPv6 + IPv4, NAT64 e DNS64 non verrà abilitato nei server di nuovo. I client che si connettono a questi punti di ingresso verranno automaticamente configurati per usare i server DNS aziendali.  
  
-   Se è necessario eliminare gli indirizzi IPv4 da un server distribuito, è necessario rimuovere il server dalla distribuzione di DirectAccess, rimuovere l'indirizzo di rete aziendale IPv4 e aggiungerlo nuovamente alla distribuzione.  
  
Per supportare la connettività client alla rete aziendale, è necessario assicurarsi che il server dei percorsi di rete può essere risolto da DNS aziendale per l'indirizzo IPv6. Un indirizzo IPv4 aggiuntivo può essere impostato anche, ma non è obbligatorio.  
  
### <a name="DualDeployment"></a>Eseguire la transizione da un solo IPv4 a una distribuzione solo IPv6 usando due distribuzioni di DirectAccess  
La transizione da un solo IPv4 a una rete aziendale solo IPv6 non può essere eseguita senza reinstallare la distribuzione di DirectAccess. Per mantenere la connettività dei client durante la transizione, è possibile usare un'altra distribuzione di DirectAccess. Distribuzione dual è obbligatoria quando la prima fase di transizione termina (rete solo IPv4 aggiornato a IPv4 + IPv6) e si intende preparare per una transizione futura per un solo IPv6 rete aziendale orto usufruire dei vantaggi della connettività IPv6 nativi. La distribuzione dual è descritto nei passaggi generali seguenti:  
  
1.  Installare una seconda distribuzione di DirectAccess. È possibile installare DirectAccess nei nuovi server, o rimuovere server dalla distribuzione del primo e usarli per la seconda distribuzione.  
  
    > [!NOTE]  
    > Quando si installa una distribuzione di DirectAccess aggiuntiva insieme ad uno corrente, assicurarsi che nessuna due punti di ingresso condividono lo stesso prefisso di client.  
    >   
    > Se si installa DirectAccess con attività iniziali guidate o con il cmdlet `Install-RemoteAccess`, l'accesso remoto imposta automaticamente il prefisso del client del primo punto di ingresso nella distribuzione di un valore predefinito di < subnet IPv6\_prefisso >: 1000::/ / 64. Se necessario, è necessario modificare il prefisso.  
  
2.  Rimuovere i gruppi di sicurezza client scelto dalla prima distribuzione.  
  
3.  Aggiungere i gruppi di sicurezza di client per la seconda distribuzione.  
  
    > [!IMPORTANT]  
    > Per mantenere la connettività client nel corso del processo, è necessario aggiungere i gruppi di sicurezza per la seconda distribuzione immediatamente dopo la loro rimozione dalla prima. Ciò garantisce che i client non verranno aggiornati oggetti Criteri di gruppo DirectAccess di due o zero. I client verranno avviata con la seconda distribuzione una volta possano recuperare e aggiornare i client oggetto Criteri di gruppo.  
  
4.  Facoltativo: Rimuovere i punti di ingresso DirectAccess dalla prima distribuzione e aggiungere tali server come nuovi punti di ingresso al secondo.  
  
Dopo aver completato la transizione, è possibile disinstallare la prima distribuzione di DirectAccess. Quando si disinstalla, potrebbero verificarsi i problemi seguenti:  
  
-   Se la distribuzione è stata configurata per supportare solo i client su computer portatili, verrà eliminato il filtro WMI. Se i gruppi di sicurezza di client della distribuzione del secondo includono computer desktop, il gruppo di client DirectAccess non filtrerà i computer desktop e può causare problemi su di essi. Se è necessario un filtro ai computer portatili, ricrearlo seguendo le istruzioni disponibili [creare i filtri WMI per l'oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Se entrambe le distribuzioni sono stati originariamente create nello stesso dominio Active Directory, DNS probe voce che punta a localhost verrà eliminate e potrebbe causare problemi di connettività client. Ad esempio, i client possono connettersi mediante IP-HTTPS anziché Teredo o passare tra i punti di ingresso multisito di DirectAccess. In questo caso, è necessario aggiungere la voce DNS seguente al DNS aziendale:  
  
    -   Zona: nome di dominio  
  
    -   Name: directaccess-corpConnectivityHost  
  
    -   Indirizzo IP::: 1  
  
    -   Digitare:  AAAA  
  
  
  


