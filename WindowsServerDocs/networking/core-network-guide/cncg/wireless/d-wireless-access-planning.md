---
title: Accesso senza fili pianificazione della distribuzione
description: Questo argomento fa parte della Guida "Distribuzione basata su Password 802.1 X Authenticated Wireless Access" rete di Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1aa7d9fa66c480988ec7e3a97447157bd3eab9c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-planning"></a>Accesso senza fili pianificazione della distribuzione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Prima di distribuire l'accesso wireless, è necessario pianificare gli elementi seguenti:

- Installazione di accesso wireless punti \(APs\) sulla rete

- Accesso e la configurazione di client wireless

Le sezioni seguenti forniscono informazioni dettagliate su questi passaggi di pianificazione.

## <a name="planning-wireless-ap-installations"></a>Pianificazione delle installazioni di AP senza fili
Quando si progetta la soluzione di accesso di rete wireless, è necessario eseguire le operazioni seguenti:

1. Determinare quali standard devono supportare punti di accesso wireless
2. Determinare le aree di copertura in cui si desidera fornire servizi senza fili
3. Determinare dove si desidera individuare i punti di accesso wireless

Inoltre, è necessario pianificare una combinazione di indirizzo IP per il punto di accesso wireless e i client wireless. Vedere la sezione **pianificare la configurazione di AP senza fili in Criteri di rete** seguito informazioni per correlati.

### <a name="verify-wireless-ap-support-for-standards"></a>Verificare il supporto di AP senza fili per gli standard
Ai fini della coerenza e facilità di distribuzione e gestione di punti di accesso, è consigliabile distribuire accesso senza fili della stessa marca e modello.

I punti di accesso wireless che distribuiscono deve supportare le operazioni seguenti:

- **IEEE 802.1 X**

- **Autenticazione RADIUS**

- **Autenticazione wireless e livello di codifica.** Elencati in ordine della maggior parte di almeno preferito:

    1.  WPA2\-Enterprise con AES

    2.  WPA2\-Enterprise con TKIP

    3.  WPA\-Enterprise con AES

    4.  WPA\-Enterprise con TKIP

>[!NOTE]
>Per distribuire WPA2, è necessario utilizzare schede di rete wireless e punti di accesso wireless che supportano WPA2. In caso contrario, utilizzare WPA\ Enterprise.

Inoltre, per fornire protezione avanzata per la rete, i punti di accesso wireless deve supportare le opzioni di sicurezza seguenti:

- **Filtri DHCP.** Il punto di accesso wireless deve filtrare porte IP per impedire la trasmissione dei messaggi trasmessi DHCP nei casi in cui il client wireless è configurato come server DHCP. Il punto di accesso wireless debba bloccare il client da inviare pacchetti IP dalla porta UDP 68 alla rete.

- **Il filtro dei DNS.** Il punto di accesso wireless deve filtrare porte IP per impedire l'esecuzione come server DNS di un client. Il punto di accesso wireless debba bloccare il client di inviare pacchetti IP provenienti dalle TCP o UDP porta 53 alla rete.

- **Isolamento dei client** se il punto di accesso wireless fornisce ai client le funzionalità di isolamento, è necessario abilitare la funzionalità impedire possibili Address Resolution Protocol \(ARP\) gli attacchi di spoofing.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificare le aree di copertura per gli utenti wireless
Utilizzare disegni architettonici di ogni computer per ogni compilazione per identificare le aree in cui si desidera fornire una copertura wireless. Ad esempio, identificare l'uffici appropriati, sale conferenze, atrii, caffetterie o dei cortili.

Nei disegni, indicare i dispositivi che interferiscono con i segnali wireless, ad esempio apparecchiature medicali, videocamere senza fili, telefoni che operano in 2.4 tramite 2.5 GHz industriale, scientifico e medici \(ISM\) intervallo e i dispositivi abilitati per Bluetooth\.

Il disegno, contrassegnare gli aspetti dell'edificio che potrebbero interferire con i segnali wireless; oggetti di metallo utilizzati per la costruzione di un edificio possono influenzare il segnale wireless. Ad esempio, i seguenti oggetti comuni possono interferire con la propagazione di segnale: ascensori, riscaldamento e air\ condizionata condotte e concreto supporto girders.

Fare riferimento al produttore del punto di accesso per informazioni sulle origini che potrebbero causare attenuazione frequenza radio di AP senza fili. La maggior parte dei punti di accesso forniscono software test che è possibile utilizzare per controllare per la potenza del segnale, tasso di errore e velocità effettiva di dati.

### <a name="determine-where-to-install-wireless-aps"></a>Determinare dove installare i punti di accesso wireless
Nei disegni dell'architettura, individuare AP senza fili Chiudi abbastanza insieme per fornire una copertura wireless ampia ma con estrema sufficiente tra di loro che non interferiscano tra loro.

La distanza tra punti di accesso necessaria dipende dal tipo di punto di accesso e antenna PA, gli aspetti dell'edificio che bloccano wireless segnali e altre fonti di interferenze. È possibile contrassegnare posizioni AP senza fili in modo che ogni punto di accesso wireless non più di 300 piedi da qualsiasi punto di accesso wireless adiacente. Vedere la documentazione del produttore AP wireless per le specifiche del punto di accesso e linee guida per la selezione host.

Installare temporaneamente i punti di accesso wireless nei percorsi specificati in disegni dell'architettura. Quindi, utilizza un computer portatile dotato di una scheda wireless 802.11 e il software di sondaggio del sito che in genere viene fornito con schede di rete wireless, determinare la potenza del segnale all'interno di ogni area di copertura.

In aree di copertura in cui la potenza del segnale è bassa, posizionare il punto di accesso per migliorare la potenza del segnale per l'area di copertura, installare ulteriori punti di accesso wireless per fornire una copertura necessaria, spostare o rimuovere le origini di interferenze del segnale.  

Aggiornare i disegni dell'architettura per indicare la posizione finale di tutti i punti di accesso wireless. Con una mappa di selezione AP accurata fornirà assistenza in un secondo momento durante le operazioni di risoluzione dei problemi o quando si desidera aggiornare o sostituire i punti di accesso.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Pianificare la configurazione di Client RADIUS dei criteri di rete e di punto di accesso wireless
È possibile utilizzare criteri di rete per configurare i punti di accesso wireless singolarmente o in gruppi.

Se si distribuisce una rete wireless di grandi dimensioni che include numerosi punti di accesso, è molto più semplice configurare i punti di accesso in gruppi. Per aggiungere i punti di accesso come gruppi di client RADIUS in Criteri di rete, è necessario configurare i punti di accesso con queste proprietà.

- I punti di accesso wireless configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

- Tutti i punti di accesso wireless sono configurati con lo stesso segreto condiviso.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Pianificare l'utilizzo di riconnessione rapida PEAP
In un'infrastruttura 802.1 X, punti di accesso wireless sono configurati come client RADIUS in server RADIUS. Quando la riconnessione rapida PEAP viene distribuita, un client wireless che si sposta tra due o più punti di accesso non è richiesto l'autenticazione con ogni nuova associazione.

Riconnessione rapida PEAP riduce il tempo di risposta per l'autenticazione tra client e autenticatore perché la richiesta di autenticazione viene inoltrata dal nuovo punto di accesso al server dei criteri di rete che originariamente eseguito l'autenticazione e autorizzazione per la richiesta di connessione client.

Perché sia il client PEAP server dei criteri di rete utilizzano precedentemente memorizzati nella cache le proprietà di connessione \(TLS\) Transport Layer Security \ (la raccolta di cui è denominata il handle\ TLS), il server dei criteri di rete può determinare rapidamente che il client è autorizzato per una riconnessione.

>[!IMPORTANT]
>Per ottenere rapidamente riconnettersi per funzionare correttamente, i punti di accesso devono essere configurati come client RADIUS del server dei criteri di rete stesso.

Se il server dei criteri di rete originale diventa non disponibile o se il client viene spostato in un punto di accesso che è configurato come client RADIUS a un altro server RADIUS, è necessario che l'autenticazione completo tra il client e il nuovo autenticatore.

### <a name="wireless-ap-configuration"></a>Configurazione di AP senza fili
L'elenco seguente riepiloga gli elementi in genere configurati in 802.1X\-punti di accesso wireless in grado di supportare:

> [!NOTE]
> I nomi degli elementi possono variare in base alla marca e modello e potrebbero essere diverse da quelle nell'elenco seguente. Vedere la documentazione di AP senza fili per i dettagli di configurazione specifiche.

- **Identificatore del set di \(SSID\)**. Questo è il nome della rete wireless \ (ad esempio, ExampleWlan\) e il nome che verrà annunciato ai client senza fili. Per evitare confusione, l'identificatore SSID che si sceglie di annunciare non deve corrispondere il SSID che verrà trasmessi per reti wireless che si trovano all'interno di intervallo di ricezione della rete senza fili.

    Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con lo stesso SSID. Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con lo stesso SSID.  

    Nei casi in cui è necessario distribuire diverse reti senza fili per soddisfare specifiche esigenze aziendali, l'AP senza fili in una rete deve trasmettere un SSID diverso rispetto il SSID l'altra network\(s\). Ad esempio, se è necessaria una rete wireless separata per i dipendenti e Guest, è possibile configurare punti di accesso wireless per la rete aziendale con l'identificatore SSID impostato per la trasmissione **ExampleWLAN**. Per la rete guest, è quindi possibile impostare SSID di ogni AP senza fili per la trasmissione **GuestWLAN**. In questo modo i dipendenti e Guest possono connettersi alla rete desiderata senza confusione non necessaria.  

    > [!TIP]  
    > Alcuni AP senza fili hanno la possibilità di trasmettere più SSID per contenere le distribuzioni di rete di più. AP senza fili che può trasmettere più SSID può ridurre i costi di manutenzione operativa e distribuzione.  

- **Wireless l'autenticazione e crittografia**.

    Autenticazione wireless è l'autenticazione di sicurezza che viene utilizzato quando il client senza fili associa a un punto di accesso wireless.  

    Crittografia wireless è la crittografia di crittografia di sicurezza che viene utilizzata per proteggere le comunicazioni che vengono inviate tra il punto di accesso wireless e il client wireless con autenticazione wireless.  

- **\(Static\) indirizzo IP AP wireless**. In ogni punto di accesso wireless, configurare un indirizzo IP statico. Se la subnet viene gestita da un server DHCP, assicurarsi che tutti gli indirizzi IP PA rientrano in un intervallo di esclusione DHCP in modo che il server DHCP non tenta di eseguire lo stesso indirizzo IP a un altro computer o dispositivo. Gli intervalli di esclusione sono documentati nella procedura "per creare e attivare un nuovo ambito DHCP" nel [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Se si prevede di configurare i punti di accesso come client RADIUS dal gruppo in Criteri di rete, ogni punto di accesso del gruppo deve avere un indirizzo IP dallo stesso intervallo di indirizzi IP.

- **Nome DNS**. Alcuni AP senza fili può essere configurato con un nome DNS. Configurare ogni punto di accesso wireless con un nome univoco. Ad esempio, se si dispone di un accesso senza fili distribuito in un edificio storia di più, è possibile denominare i primi tre punti di accesso wireless che vengono distribuite sul pavimento terzo AP3\-01, AP3\-02 e AP3\-03.

- **Maschera di subnet AP senza fili**. Configurazione della maschera per designare la parte di IP indirizzo è l'ID di rete e la parte dell'indirizzo IP dell'host.

- **Servizio DHCP AP**. Se il punto di accesso wireless è un servizio DHCP predefinita, disabilitarla.

- **Segreto condiviso RADIUS**. Utilizzare un raggio univoco segreto condiviso per ogni punto di accesso wireless a meno che non si prevede di configurare client RADIUS NPS in gruppi, in quali casi è necessario configurare tutti i punti di accesso del gruppo con lo stesso segreto condiviso. I segreti condivisi devono essere una sequenza casuale di almeno 22 caratteri, con lettere maiuscole e minuscole, numeri e segni di punteggiatura. Per assicurare la casualità, è possibile utilizzare un programma di generazione casuale di caratteri per creare i segreti condivisi. È consigliabile annotare il segreto condiviso per ogni punto di accesso wireless e archiviarlo in un luogo sicuro, ad esempio una sede sicuro. Quando si configurano i client RADIUS nella console di criteri di rete si creerà una versione virtuale dell'ogni punto di accesso. Il segreto condiviso configurato in ogni punto di accesso virtuale in Criteri di rete deve corrispondere il segreto condiviso sul punto di accesso fisico, effettivo.

- **Indirizzo IP del server RADIUS**. Digitare l'indirizzo IP del server dei criteri di rete che si desidera utilizzare per autenticare e autorizzare le richieste di connessione a questo punto di accesso.

- **UDP port\(s\)**. Per impostazione predefinita, dei criteri di rete utilizza le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS. È consigliabile non modificare le impostazioni di porte UDP RADIUS predefinite.

- **VSA**. Alcuni punti di accesso wireless richiedono attributi specifici vendor\ \(VSAs\) per fornire funzionalità AP wireless completo.

- **Filtri DHCP**. Configurare i punti di accesso wireless per bloccare i client wireless inviare pacchetti IP dalla porta UDP 68 alla rete. Vedere la documentazione per il punto di accesso wireless configurare il filtro di DHCP.

- **Il filtro dei DNS**. Configurare i punti di accesso wireless per bloccare i client wireless inviare pacchetti IP dalla porta TCP o UDP 53 alla rete. Vedere la documentazione per il punto di accesso wireless configurare il filtro dei DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Pianificazione della configurazione di client wireless e accesso

Quando si pianifica la distribuzione di 802.1X\-autenticato accesso wireless, è necessario considerare diversi fattori di connessione tra client specifici:

- **Pianificazione di supporto per gli standard più**.

    Determinare se tutti i computer wireless utilizzando la stessa versione di Windows o se sono una combinazione di computer che eseguono sistemi operativi diversi. Se sono diversi, assicurarsi di comprendere le differenze nella standard supportati dai sistemi operativi.

    Determinare se tutte le schede di rete wireless in tutti i computer client wireless supportano gli standard wireless stesso, o se è necessario supportare diversi standard. Ad esempio, determinare se alcuni driver hardware di schede di rete supportano WPA2\ Enterprise e AES, mentre altri ne supportano solo WPA\-Enterprise e TKIP.

- **Pianificazione della modalità di autenticazione client**. Modalità di autenticazione consente di definire il modo in cui i client Windows elaborano le credenziali di dominio. È possibile scegliere le modalità di autenticazione seguenti tre rete nei criteri di rete wireless.  

    1. **L'autenticazione utente nuovamente**. Questa modalità specifica che l'autenticazione viene sempre eseguita utilizzando le credenziali di sicurezza in base allo stato corrente del computer. Quando nessun utente sono connessi al computer, l'autenticazione viene eseguita utilizzando le credenziali del computer. Quando un utente è connesso al computer, l'autenticazione viene sempre eseguita utilizzando le credenziali dell'utente.  

    2. **Solo computer**. Computer solo modalità specifica che l'autenticazione viene sempre eseguita utilizzando solo le credenziali del computer.  

    3.  **L'autenticazione utente**. Modalità di autenticazione utente specifica che l'autenticazione viene eseguita solo quando l'utente è connesso al computer. Quando non sono presenti utenti connessi al computer, i tentativi di autenticazione non vengono eseguiti.  

- **Pianificazione restrizioni wireless**. Determinare se si desidera fornire tutti gli utenti wireless con lo stesso livello di accesso alla rete wireless o se si desidera limitare l'accesso per alcuni utenti wireless. È possibile applicare restrizioni in Criteri di rete in base a specifici gruppi di utenti wireless.  Ad esempio, è possibile definire specifiche giorni e ore che alcuni gruppi sono autorizzati l'accesso alla rete wireless.  

- **Pianificazione di metodi per l'aggiunta di nuovi computer wireless**. Per i computer in grado di supportare wireless\ aggiunti al dominio prima di distribuire una rete wireless, se il computer è connesso a un segmento di rete cablata che non è protetta da 802.1 X, le impostazioni di configurazione senza fili vengono applicate automaticamente dopo la configurazione della rete Wireless \ (IEEE 802.11\) criteri sul controller di dominio e dopo l'aggiornamento di criteri di gruppo sul client senza fili.  

    Per i computer che non sono già aggiunti al dominio, tuttavia, è necessario pianificare un metodo per applicare le impostazioni necessarie per 802.1X\-l'accesso autenticato. Ad esempio, determinare se si desidera aggiungere il computer al dominio utilizzando uno dei metodi seguenti.

    1.  Connettere il computer a un segmento di rete cablata che non è protetta da 802.1 X, quindi aggiungere il computer al dominio.

    2.  Fornire agli utenti wireless con i passaggi e le impostazioni necessarie per aggiungere il proprio profilo bootstrap wireless, che consente di aggiungere il computer al dominio.

    3.  Assegnare il personale IT per aggiungere i computer client wireless al dominio.

### <a name="planning-support-for-multiple-standards"></a>Pianificazione di supporto per gli standard più

La rete Wireless \ (IEEE 802.11\) estensione di criteri in Criteri di gruppo offre un'ampia gamma di opzioni di configurazione per supportare un'ampia gamma di opzioni di distribuzione.

È possibile distribuire i punti di accesso wireless configurati con gli standard che si desidera supportare e quindi configurare i profili wireless più nella rete senza fili \ (IEEE 802.11\), i criteri con ogni profilo che specifica un set di standard che è necessario.

Ad esempio, se la rete include computer wireless che supportano WPA2\-Enterprise e AES, altri computer che supportano WPA\ Enterprise e AES e altri computer che supportano solo WPA\-Enterprise e TKIP, è necessario determinare se si desidera:

- Configurare un unico profilo per supportare tutti i computer wireless utilizzando il metodo di crittografia più debole che tutti i computer supportano - in questo caso, WPA\-Enterprise e TKIP.  
- Configurare due profili per fornire la massima sicurezza possibile supportato da ogni computer wireless. In questo caso è necessario configurare un profilo che specifica la crittografia \ (WPA2\-Enterprise e AES\) e un profilo che utilizza la crittografia più vulnerabile WPA\ Enterprise e TKIP. In questo esempio, è essenziale inserire il profilo che utilizza WPA2\-Enterprise e AES più alta nell'ordine di preferenza. Computer che non sono in grado di utilizzare WPA2\ Enterprise e AES verrà automaticamente andare al profilo successivo nell'ordine di preferenza ed elaborare il profilo che specifica WPA\ Enterprise e TKIP.

> [!IMPORTANT]
> È necessario inserire il profilo con gli standard più sicuri superiore nell'elenco ordinato dei profili, poiché connessione dei computer utilizza il primo profilo che sono in grado di utilizzare.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Pianificazione dell'accesso limitato alla rete wireless

In molti casi, si desidera fornire agli utenti wireless con livelli di accesso alla rete wireless. Ad esempio, si desidera consentire l'accesso senza restrizioni alcuni utenti, qualsiasi ora del giorno, ogni giorno della settimana. Per altri utenti, potrebbe essere solo vuoi consentire l'accesso durante l'orario dei componenti di base, dal lunedì al venerdì e negare l'accesso in sabato e domenica.

Questa guida fornisce istruzioni per creare un ambiente di accesso che inserisce tutti gli utenti wireless in un gruppo con comune accesso alle risorse wireless. Puoi creare un gruppo di sicurezza wireless agli utenti in Active Directory Users e computer snap-in e quindi rendere ogni utente per il quale si desidera concedere l'accesso wireless un membro di tale gruppo.

Quando si configura criteri di criteri di rete, specificare il gruppo di sicurezza utenti wireless come l'oggetto che elabora i criteri di rete quando si determina l'autorizzazione.

Tuttavia, se la distribuzione richiede il supporto per diversi livelli di accesso è necessario solo eseguire le operazioni seguenti:  

1. Creare più di un gruppo di sicurezza utenti Wireless per la creazione di gruppi di sicurezza wireless aggiuntive in Active Directory Users and Computers. Ad esempio, è possibile creare un gruppo che contiene gli utenti che hanno accesso completo, un gruppo per gli utenti che dispongono di accesso durante il regolare orario di lavoro e altri gruppi che soddisfano gli altri criteri che corrispondono ai propri requisiti.

2. Aggiungere utenti ai gruppi di protezione appropriati che è stato creato.

3. Configurare criteri di rete dei criteri di rete aggiuntivi per ogni gruppo di sicurezza wireless aggiuntive e configurare i criteri per applicare le condizioni e i vincoli necessari per ogni gruppo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Pianificazione di metodi per l'aggiunta di nuovi computer wireless

Il metodo preferito per aggiungere nuovi computer wireless al dominio e quindi accedere al dominio è tramite una connessione a un segmento di LAN che dispone dell'accesso ai controller di dominio e non è protetto da un'autenticazione 802.1 X commutatore Ethernet cablata.

In alcuni casi, tuttavia, potrebbe risultare poco pratico da utilizzare una connessione cablata per aggiungere computer al dominio o, per un utente di utilizzare una connessione cablata per il primo tentativo di accesso con computer che sono già connessi al dominio.

Per aggiungere un computer al dominio tramite una connessione wireless o agli utenti di accedere al dominio la prima volta utilizzando un computer aggiunto al domain \ e una connessione wireless, i client wireless devono innanzitutto stabilire una connessione alla rete wireless in un segmento che ha accesso ai controller di dominio di rete utilizzando uno dei metodi seguenti.

1. **Un membro del personale IT aggiunge un computer wireless al dominio e quindi configura un profilo Single Sign On bootstrap wireless.** Con questo metodo, un amministratore IT si connette il computer wireless alla rete Ethernet cablata e quindi aggiunge il computer al dominio. L'amministratore distribuisce quindi il computer per l'utente. Quando l'utente avvia il computer, le credenziali di dominio che specificano manualmente per il processo di accesso utente vengono utilizzate per stabilire una connessione alla rete wireless e accedere al dominio.  

2. **L'utente Configura manualmente i computer wireless con profilo wireless bootstrap e quindi viene aggiunto al dominio.** Con questo metodo, gli utenti configurare manualmente i computer wireless con un profilo wireless bootstrap sulla base delle istruzioni da un amministratore IT. Il profilo wireless bootstrap consente agli utenti di stabilire una connessione wireless e quindi aggiungere il computer al dominio. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può accedere al dominio utilizzando una connessione wireless e le credenziali di account di dominio.

Per distribuire accesso senza fili, vedere [distribuzione di accesso Wireless](e-wireless-access-deployment.md).
