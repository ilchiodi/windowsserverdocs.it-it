---
title: Pianificazione della distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "distribuire l'accesso wireless autenticato con 802.1 X basato su password"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4606e1cb418426623aca9e199ddde575a826a064
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865327"
---
# <a name="wireless-access-deployment-planning"></a>Pianificazione della distribuzione dell'accesso wireless

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

Prima di distribuire l'accesso wireless, è necessario pianificare gli elementi seguenti:

- Installazione di punti di accesso wireless \(APs\) sulla rete

- Configurazione e accesso del client wireless

Le sezioni seguenti forniscono informazioni dettagliate su questi passaggi di pianificazione.

## <a name="planning-wireless-ap-installations"></a>Pianificazione delle installazioni di AP wireless
Quando si progetta la soluzione di accesso di rete wireless, è necessario eseguire le operazioni seguenti:

1. Determinare quali standard devono supportare l'accesso senza fili
2. Determinare le aree di copertura in cui si desidera fornire servizi senza fili
3. Determinare dove si desidera individuare i punti di accesso wireless

Inoltre, è necessario pianificare uno schema di indirizzi IP per i client wireless e del punto di accesso wireless. Per informazioni correlate, vedere la sezione **pianificare la configurazione del punto di accesso wireless nel server dei criteri di rete** .

### <a name="verify-wireless-ap-support-for-standards"></a>Verificare il supporto di AP wireless per gli standard
Ai fini della coerenza e della facilità di distribuzione e gestione dei punti di accesso, è consigliabile distribuire punti di accesso wireless con lo stesso marchio e modello.

I punti di accesso wireless distribuiti devono supportare quanto segue:

- **IEEE 802.1 X**

- **Autenticazione RADIUS**

- **Autenticazione e crittografia wireless.** Elencato in ordine più o meno preferibile:

    1.  WPA2\-Enterprise con AES

    2.  WPA2\-Enterprise con TKIP

    3.  WPA\-Enterprise con AES

    4.  WPA\-Enterprise con TKIP

>[!NOTE]
>Per distribuire WPA2, è necessario utilizzare schede di rete wireless e punti di accesso wireless che supportano WPA2. In caso contrario, utilizzare WPA\-Enterprise.

Inoltre, per fornire protezione avanzata per la rete, punti di accesso wireless deve supportare le opzioni di sicurezza seguenti:

- **Filtro DHCP.** Il punto di accesso wireless deve filtrare porte IP per impedire la trasmissione di messaggi trasmessi DHCP nei casi in cui il client wireless è configurato come server DHCP. Il punto di accesso wireless deve impedire al client di inviare pacchetti IP dalla porta UDP 68 alla rete.

- **Filtro DNS.** Il punto di accesso wireless deve filtrare sulle porte IP per impedire l'esecuzione di un client come server DNS. Il punto di accesso wireless deve impedire al client di inviare pacchetti IP dalla porta TCP o UDP 53 alla rete.

- **Isolamento dei client** Se il punto di accesso wireless fornisce ai client le funzionalità di isolamento, è necessario abilitare la funzionalità impedire possibili Address Resolution Protocol \(ARP\) gli attacchi di spoofing.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificare le aree di copertura per gli utenti wireless
Usare i disegni architettonici di ogni piano per ogni edificio per identificare le aree in cui si vuole fornire la copertura wireless. Ad esempio, identificare gli uffici, le sale conferenze, le lobby, le caffetterie o i Courtyard appropriati.

Nei disegni, indicare tutti i dispositivi che interferiscono con i segnali wireless, ad esempio apparecchiature medicali, videocamere senza fili, telefoni che operano in 2.4 tramite 2.5 GHz industriale, scientifico e medici \(ISM\) intervallo e Bluetooth\-dispositivi.

Nel disegno contrassegnare gli aspetti della compilazione che potrebbero interferire con i segnali wireless; gli oggetti metal usati nella costruzione di un edificio possono influenzare il segnale wireless. Ad esempio, gli oggetti comuni seguenti possono interferire con la propagazione del segnale: Ascensori, riscaldamento e condizionamento dell'aria\-e travi di supporto concrete.

Per informazioni sulle origini che potrebbero causare un'attenuazione della frequenza radio AP wireless, vedere il produttore del punto di accesso. La maggior parte degli APs fornisce software di testing che è possibile usare per verificare la potenza del segnale, la frequenza degli errori e la velocità effettiva dei dati.

### <a name="determine-where-to-install-wireless-aps"></a>Determinare il percorso di installazione dei punti di accesso wireless
Nei disegni dell'architettura individuare i punti di accesso wireless sufficientemente vicini per offrire una copertura senza fili elevata, ma piuttosto che non interferiscono tra loro.

La distanza tra punti di accesso necessaria dipende dal tipo di punto di accesso e antenna PA, gli aspetti dell'edificio che bloccano wireless segnali e altre fonti di interferenze. È possibile contrassegnare posizioni AP senza fili in modo che ogni punto di accesso wireless non più di 300 piedi da qualsiasi punto di accesso wireless adiacente. Vedere la documentazione del produttore del punto di accesso wireless per le specifiche AP e le linee guida per la selezione host.

Installare temporaneamente i punti di accesso wireless nei percorsi specificati nei disegni dell'architettura. Quindi, utilizza un computer portatile dotato di una scheda di rete wireless 802.11 e il software di sondaggio del sito che in genere viene fornito con schede di rete wireless, determinare la qualità del segnale all'interno di ogni area di copertura.

Nelle aree di copertura in cui la potenza del segnale è bassa, posizionare il punto di accesso per migliorare la potenza del segnale per l'area di copertura, installare ulteriori punti di accesso wireless per fornire una copertura necessaria, spostare o rimuovere le origini di interferenze del segnale.  

Aggiornare i disegni dell'architettura per indicare la posizione finale di tutti i punti di accesso wireless. La presenza di una mappa di selezione host AP corretta sarà utile in seguito durante le operazioni di risoluzione dei problemi o quando si desidera aggiornare o sostituire i punti di accesso.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Pianificare la configurazione di punti di accesso e il Client RADIUS dei criteri di RETE wireless
È possibile utilizzare criteri di RETE per configurare i punti di accesso wireless singolarmente o in gruppi.

Se si distribuisce una rete wireless di grandi dimensioni che include numerosi punti di accesso, è molto più semplice configurare i punti di accesso in gruppi. Per aggiungere i punti di accesso come gruppi di client RADIUS in Criteri di RETE, è necessario configurare i punti di accesso con queste proprietà.

- Punti di accesso wireless configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

- Tutti i punti di accesso wireless sono configurati con lo stesso segreto condiviso.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Pianificare l'utilizzo della riconnessione rapida PEAP
In un'infrastruttura 802.1 X, i punti di accesso wireless sono configurati come client RADIUS per i server RADIUS. Quando viene distribuita la riconnessione rapida PEAP, non è necessario che un client wireless che esegue il roaming tra due o più punti di accesso sia autenticato con ogni nuova associazione.

La riconnessione rapida PEAP riduce il tempo di risposta per l'autenticazione tra client e autenticatore perché la richiesta di autenticazione viene trasmessa dal nuovo punto di accesso al server dei criteri di accesso che ha eseguito originariamente l'autenticazione e l'autorizzazione per il client richiesta di connessione.

Poiché sia il client PEAP sia il server dei criteri di gruppo \(utilizzano entrambi in \(precedenza memorizzati nella cache Transport Layer Security proprietà di connessione TLS\)\) la cui raccolta è denominata handle TLS, il server dei criteri di gruppo può determinare rapidamente il client è autorizzato per una riconnessione.

>[!IMPORTANT]
>Per il corretto funzionamento della riconnessione rapida, è necessario configurare gli APs come client RADIUS dello stesso server dei criteri di server.

Se il server dei criteri di accesso originale non è più disponibile o se il client passa a un punto di accesso configurato come client RADIUS in un altro server RADIUS, deve essere eseguita l'autenticazione completa tra il client e il nuovo autenticatore.

### <a name="wireless-ap-configuration"></a>Configurazione del punto di accesso wireless
Nell'elenco seguente sono riepilogati gli elementi in genere configurati sullo standard 802.1 X\-punti di accesso wireless in grado di supportare:

> [!NOTE]
> I nomi degli elementi possono variare in base al marchio e al modello e potrebbero essere diversi da quelli elencati nell'elenco seguente. Vedere la documentazione di AP wireless per la configurazione\-dettagli specifici.

- **Identificatore del set di \(SSID\)** . Questo è il nome della rete wireless \(ad esempio, ExampleWlan\), e il nome che verrà annunciato ai client senza fili. Per ridurre la confusione, il SSID scelto per annunciare non deve corrispondere al SSID trasmesso da tutte le reti wireless che rientrano nell'intervallo di ricezione della rete wireless.

    Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con lo stesso SSID. Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con lo stesso SSID.  

    Nei casi in cui è necessario distribuire diverse reti wireless per soddisfare specifiche esigenze aziendali, il punto di accesso wireless in una rete deve trasmettere un SSID diverso rispetto all'SSID dell'altra rete\(.\) Ad esempio, se è necessaria una rete wireless separata per i dipendenti e Guest, è possibile configurare punti di accesso wireless per la rete aziendale con l'identificatore SSID impostato per la trasmissione di **ExampleWLAN**. Per la rete Guest è quindi possibile impostare il SSID di ogni punto di accesso wireless per la trasmissione di **GuestWLAN**. In questo modo i dipendenti e i guest possono connettersi alla rete desiderata senza inutili confusioni.  

    > [!TIP]  
    > Alcuni AP wireless hanno la possibilità di trasmettere più SSID per supportare\-distribuzioni di più reti. Il punto di accesso wireless in grado di trasmettere più SSID può ridurre i costi di distribuzione e manutenzione operativa.  

- **Wireless l'autenticazione e crittografia**.

    L'autenticazione wireless è l'autenticazione di sicurezza usata quando il client wireless associa a un punto di accesso wireless.  

    La crittografia senza fili è la crittografia di crittografia di sicurezza usata con l'autenticazione wireless per proteggere le comunicazioni inviate tra il punto di accesso wireless e il client wireless.  

- **Indirizzo IP AP wireless \(statico\)** . In ogni punto di accesso wireless, configurare un indirizzo IP statico. Se la subnet viene gestita da un server DHCP, assicurarsi che tutti gli indirizzi IP PA rientrano in un intervallo di esclusione DHCP in modo che il server DHCP non tenta di eseguire lo stesso indirizzo IP a un altro computer o dispositivo. Gli intervalli di esclusione sono documentati nella procedura "per creare e attivare un nuovo ambito DHCP" nella [Guida alla rete core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Se si prevede di configurare i punti di accesso come client RADIUS dal gruppo di criteri di RETE, ogni punto di accesso del gruppo deve avere un indirizzo IP dallo stesso intervallo di indirizzi IP.

- **Nome DNS**. Alcuni punti di accesso wireless possono essere configurati con un nome DNS. Configurare ogni punto di accesso wireless con un nome univoco. Ad esempio, se si dispone di un accesso senza fili distribuito in un più\-storia di compilazione, è possibile denominare i primi tre punti di accesso wireless che vengono distribuite sul pavimento terzo AP3\-01, AP3\-02 e AP3\-03.

- **Maschera di subnet AP wireless**. Configurazione della maschera per designare la parte di IP indirizzo è l'ID di rete e la parte dell'indirizzo IP è l'host.

- **Servizio DHCP AP**. Se il punto di accesso wireless è integrato\-nel servizio DHCP, disabilitarla.

- **Segreto condiviso RADIUS**. Utilizzare un RAGGIO univoco segreto condiviso per ogni punto di accesso wireless a meno che non si prevede di configurare client RADIUS NPS in gruppi, in quali casi è necessario configurare tutti i punti di accesso nel gruppo con lo stesso segreto condiviso. I segreti condivisi devono essere una sequenza casuale di almeno 22 caratteri, con lettere maiuscole e minuscole, numeri e segni di punteggiatura. Per assicurare la casualità, è possibile usare un programma di generazione casuale di caratteri per creare i segreti condivisi. È consigliabile registrare il segreto condiviso per ogni punto di accesso wireless e archiviarlo in una posizione sicura, ad esempio un ufficio sicuro. Quando si configurano i client RADIUS nella console server dei criteri di accesso, si creerà una versione virtuale di ogni punto di accesso. Il segreto condiviso configurato in ogni punto di accesso virtuale nel server dei criteri di server deve corrispondere al segreto condiviso nel punto di accesso fisico effettivo.

- **Indirizzo IP del server RADIUS**. Digitare l'indirizzo IP del server dei criteri di accesso che si desidera utilizzare per autenticare e autorizzare le richieste di connessione a questo punto di accesso.

- **La porta UDP\(s\)** . Per impostazione predefinita, NPS usa le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS. Si consiglia di non modificare le impostazioni predefinite delle porte UDP RADIUS.

- **Gli attributi VSA**. Alcuni punti di accesso wireless richiedono fornitore\-attributi specifici \(VSA\) per fornire funzionalità AP wireless completo.

- **Filtri DHCP**. Configurare punti di accesso wireless per impedire ai client wireless di inviare pacchetti IP dalla porta UDP 68 alla rete. Per configurare il filtro DHCP, vedere la documentazione relativa al punto di accesso wireless.

- **Il filtro dei DNS**. Configurare punti di accesso wireless per impedire ai client wireless di inviare pacchetti IP dalla porta TCP o UDP 53 alla rete. Per configurare il filtro DNS, vedere la documentazione relativa al punto di accesso wireless.

## <a name="planning-wireless-client-configuration-and-access"></a>Pianificazione dell'accesso e della configurazione del client wireless

Quando si pianifica la distribuzione di 802.1 X\-autenticato accesso wireless, è necessario considerare diversi client\-fattori specifici:

- **Pianificazione di supporto per gli standard più**.

    Determinare se tutti i computer wireless utilizzando la stessa versione di Windows o se sono una combinazione di computer che eseguono sistemi operativi diversi. Se sono diversi, assicurarsi di comprendere le differenze nella standard supportati dai sistemi operativi.

    Determinare se tutte le schede di rete wireless in tutti i computer client wireless supportano gli stessi standard wireless o se è necessario supportare standard diversi. Ad esempio, determinare se alcuni driver dell'hardware di schede di rete supportano WPA2\-Enterprise e AES, mentre altri ne supportano solo WPA\-Enterprise e TKIP.

- **Pianificazione della modalità di autenticazione client**. Le modalità di autenticazione definiscono come i client Windows elaborano le credenziali di dominio È possibile scegliere tra le seguenti tre modalità di autenticazione di rete nei criteri di rete wireless.  

    1. **Utente re\-autenticazione**. Questa modalità consente di specificare che l'autenticazione viene sempre eseguita utilizzando le credenziali di sicurezza in base allo stato corrente del computer. Quando nessun utente è connesso al computer, l'autenticazione viene eseguita utilizzando le credenziali del computer. Quando un utente è connesso al computer, l'autenticazione viene sempre eseguita utilizzando le credenziali utente.  

    2. **Solo computer**. Computer solo modalità specifica che l'autenticazione viene sempre eseguita utilizzando solo le credenziali del computer.  

    3.  **Autenticazione utente**. Modalità di autenticazione utente specifica che l'autenticazione viene eseguita solo quando l'utente è connesso al computer. Quando nessun utente è connesso al computer, i tentativi di autenticazione non vengono eseguiti.  

- **Pianificazione restrizioni wireless**. Determinare se si vuole fornire a tutti gli utenti wireless lo stesso livello di accesso alla rete wireless o se si vuole limitare l'accesso per alcuni utenti senza fili. È possibile applicare restrizioni in NPS a gruppi specifici di utenti wireless.  Ad esempio, è possibile definire specifiche giorni e ore che alcuni gruppi sono autorizzati l'accesso alla rete wireless.  

- **Pianificazione di metodi per l'aggiunta di nuovi computer wireless**. Per il wireless\-in grado di supportare computer appartenenti al dominio prima di distribuire la rete wireless, se il computer è connesso a un segmento di rete cablata che non è protetta da 802.1 X, le impostazioni di configurazione senza fili vengono applicate automaticamente dopo la configurazione della rete senza fili \(IEEE 802.11\) criteri sul controller di dominio e dopo l'aggiornamento di criteri di gruppo sul client senza fili.  

    Per i computer che non sono già aggiunti al dominio, tuttavia, è necessario pianificare un metodo per applicare le impostazioni necessarie per 802.1 X\-l'accesso autenticato. Ad esempio, determinare se si desidera aggiungere il computer al dominio utilizzando uno dei metodi seguenti.

    1.  Connettere il computer a un segmento di rete cablata che non è protetta da 802.1 X, quindi aggiungere il computer al dominio.

    2.  Fornire agli utenti wireless con le impostazioni necessarie per aggiungere il proprio profilo bootstrap wireless, che consente di aggiungere il computer al dominio e i passaggi.

    3.  Assegnare IL personale IT per aggiungere i computer client wireless al dominio.

### <a name="planning-support-for-multiple-standards"></a>Pianificazione del supporto per più standard

La rete senza fili \(IEEE 802.11\) estensione di criteri in Criteri di gruppo offre un'ampia gamma di opzioni di configurazione per supportare un'ampia gamma di opzioni di distribuzione.

È possibile distribuire i punti di accesso wireless configurati con gli standard che si desidera supportare e quindi configurare i profili wireless più nella rete senza fili \(IEEE 802.11\) con ogni profilo di un set di standard che è necessario specificare i criteri.

Ad esempio, se la rete include computer wireless che supportano WPA2\-Enterprise e AES, altri computer che supportano WPA\-Enterprise e AES e altri computer che supportano solo WPA\-Enterprise e TKIP, è necessario determinare se si desidera:

- Configurare un unico profilo per supportare tutti i computer wireless utilizzando il metodo di crittografia più debole che tutti i computer supportano - in questo caso, WPA\-Enterprise e TKIP.  
- Configurare due profili per fornire la migliore sicurezza possibile supportata da ogni computer senza fili. In questa istanza è necessario configurare un profilo che specifica la crittografia \(WPA2\-Enterprise e AES\), e un profilo che usa WPA debole\-Enterprise e TKIP crittografia. In questo esempio, è essenziale inserire il profilo che utilizza WPA2\-Enterprise e AES più alta nell'ordine di preferenza. Computer che non sono in grado di utilizzare WPA2\-Enterprise e AES verrà automaticamente andare al profilo successivo nell'ordine di preferenza ed elaborare il profilo che specifica WPA\-Enterprise e TKIP.

> [!IMPORTANT]
> È necessario inserire il profilo con gli standard più sicuri superiore nell'elenco ordinato dei profili, poiché connessione dei computer utilizza il primo profilo che sono in grado di utilizzare.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Pianificazione dell'accesso limitato alla rete wireless

In molti casi, potrebbe essere necessario fornire agli utenti wireless con diversi livelli di accesso alla rete wireless. Ad esempio, è possibile consentire ad alcuni utenti di accedere senza restrizioni, qualsiasi ora del giorno, ogni giorno della settimana. Per gli altri utenti, è possibile consentire l'accesso solo durante le ore di core, dal lunedì al venerdì e negare l'accesso a sabato e domenica.

Questa guida fornisce le istruzioni per creare un ambiente di accesso che inserisce tutti gli utenti wireless in un gruppo con accesso comune alle risorse wireless. È possibile creare un gruppo di sicurezza utenti senza fili nello snap\--in utenti e computer di Active Directory e quindi fare in modo che tutti gli utenti per i quali si desidera concedere l'accesso wireless a un membro di tale gruppo.

Quando si configurano i criteri di rete del server dei criteri di rete, si specifica il gruppo di sicurezza utenti wireless come oggetto che viene elaborato dal server dei criteri di rete

Tuttavia, se la distribuzione richiede il supporto per diversi livelli di accesso, è necessario eseguire le operazioni seguenti:  

1. Creare più di un gruppo di sicurezza utenti Wireless per la creazione di gruppi di sicurezza wireless aggiuntive in Active Directory Users and Computers. Ad esempio, è possibile creare un gruppo che contiene gli utenti che hanno accesso completo, un gruppo per gli utenti che dispongono di accesso durante il regolare orario di lavoro e altri gruppi che soddisfano gli altri criteri che corrispondono ai propri requisiti.

2. Aggiungere utenti a gruppi di protezione appropriati che è stato creato.

3. Configurare i criteri di rete dei criteri di RETE aggiuntivi per ogni gruppo di sicurezza wireless aggiuntive e configurare i criteri per applicare le condizioni e i vincoli necessari per ogni gruppo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Metodi di pianificazione per l'aggiunta di nuovi computer senza fili

Il metodo preferito per aggiungere nuovi computer wireless al dominio e quindi accedere al dominio consiste nell'utilizzare una connessione cablata a un segmento della LAN che ha accesso ai controller di dominio e non è protetto da un commutatore Ethernet 802.1 X che esegue l'autenticazione.

In alcuni casi, tuttavia, potrebbe risultare poco pratico da utilizzare una connessione cablata per aggiungere computer al dominio o, per un utente di utilizzare una connessione cablata per il primo tentativo di accesso con computer che sono già connessi al dominio.

Per aggiungere un computer al dominio tramite una connessione wireless o agli utenti di accedere al dominio la prima volta con un dominio\-computer aggiunto e una connessione wireless, i client wireless devono innanzitutto stabilire una connessione alla rete wireless in un segmento che ha accesso ai controller di dominio di rete utilizzando uno dei metodi seguenti.

1. **Un membro del personale IT aggiunge un computer wireless al dominio e quindi configura un profilo wireless bootstrap Single Sign-on.** Con questo metodo, un amministratore IT si connette il computer wireless alla rete Ethernet cablata e quindi aggiunge il computer al dominio. Quindi, l'amministratore distribuisce il computer all'utente. Quando l'utente avvia il computer, le credenziali di dominio specificate manualmente per il processo di accesso utente vengono usate per stabilire una connessione alla rete wireless e accedere al dominio.  

2. **L'utente configura manualmente il computer senza fili con un profilo wireless bootstrap, quindi aggiunge il dominio.** Con questo metodo, gli utenti configurare manualmente i computer wireless con un profilo wireless bootstrap sulla base delle istruzioni da un amministratore IT. Il profilo wireless bootstrap consente agli utenti di stabilire una connessione wireless e quindi di aggiungere il computer al dominio. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può accedere al dominio utilizzando una connessione wireless e le credenziali dell'account di dominio.

Per distribuire accesso senza fili, vedere [distribuzione di accesso Wireless](e-wireless-access-deployment.md).
