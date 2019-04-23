---
title: Pianificazione della distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "Distribuisci con 802.1x basato su Password X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2571f509fbbca8384e626ad3c8c13f1c0a50400
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855462"
---
# <a name="wireless-access-deployment-planning"></a>Pianificazione della distribuzione dell'accesso wireless

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Prima di distribuire l'accesso wireless, è necessario pianificare gli elementi seguenti:

- Installazione di punti di accesso wireless \(APs\) sulla rete

- Accesso e la configurazione di client senza fili

Le sezioni seguenti forniscono informazioni dettagliate su questi passaggi di pianificazione.

## <a name="planning-wireless-ap-installations"></a>Pianificazione delle installazioni di AP senza fili
Quando si progetta la soluzione di accesso di rete wireless, è necessario eseguire le operazioni seguenti:

1. Determinare quali standard devono supportare l'accesso senza fili
2. Determinare le aree di copertura in cui si desidera fornire servizi senza fili
3. Determinare dove si desidera individuare i punti di accesso wireless

Inoltre, è necessario pianificare una combinazione di indirizzo IP per il punto di accesso wireless e i client wireless. Vedere la sezione **pianificare la configurazione di AP senza fili in Criteri di RETE** seguito per informazioni correlate.

### <a name="verify-wireless-ap-support-for-standards"></a>Verificare il supporto di AP senza fili per gli standard
Ai fini della coerenza e facilità di distribuzione e gestione di punti di accesso, si consiglia di distribuire le AP senza fili della stessa marca e modello.

Punti di accesso wireless che si distribuisce deve supportare quanto segue:

- **IEEE 802.1X**

- **Autenticazione RADIUS**

- **L'autenticazione wireless e pacchetti di crittografia.** Elencate in ordine della maggior parte di meno preferito:

    1.  WPA2\-Enterprise con AES

    2.  WPA2\-Enterprise con TKIP

    3.  WPA\-Enterprise con AES

    4.  WPA\-Enterprise con TKIP

>[!NOTE]
>Per distribuire WPA2, è necessario utilizzare schede di rete wireless e punti di accesso wireless che supportano WPA2. In caso contrario, utilizzare WPA\-Enterprise.

Inoltre, per fornire protezione avanzata per la rete, punti di accesso wireless deve supportare le opzioni di sicurezza seguenti:

- **Filtri DHCP.** Il punto di accesso wireless deve filtrare porte IP per impedire la trasmissione di messaggi trasmessi DHCP nei casi in cui il client wireless è configurato come server DHCP. Punto di accesso wireless deve impedire al client l'invio di pacchetti IP dalla porta UDP 68 per la rete.

- **DNS di filtro.** Punto di accesso wireless deve filtrare porte IP per impedire l'esecuzione come server DNS di un client. Punto di accesso wireless deve impedire al client l'invio di pacchetti IP provenienti dalle TCP o UDP porta 53 alla rete.

- **Isolamento dei client** Se il punto di accesso wireless fornisce ai client le funzionalità di isolamento, è necessario abilitare la funzionalità impedire possibili Address Resolution Protocol \(ARP\) gli attacchi di spoofing.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificare le aree di code coverage per gli utenti wireless
Per identificare le aree in cui si desidera fornire una copertura wireless, utilizzare disegni architettonici di ogni limite minimo per ogni edificio. Ad esempio, identificare l'uffici appropriati, sale conferenze, Hall, caffetterie o dei cortili.

Nei disegni, indicare tutti i dispositivi che interferiscono con i segnali wireless, ad esempio apparecchiature medicali, videocamere senza fili, telefoni che operano in 2.4 tramite 2.5 GHz industriale, scientifico e medici \(ISM\) intervallo e Bluetooth\-dispositivi.

Nel disegno, contrassegnare gli aspetti dell'edificio che potrebbero interferire con i segnali wireless; oggetti metalli utilizzati nella costruzione di un edificio possono influenzare il segnale wireless. Ad esempio, i seguenti oggetti comuni possono interferire con propagazione di segnale: Ascensori, riscaldamento e aria\-condizionata condotte e concreto supporto girders.

Consultare il produttore del punto di accesso per le informazioni sulle origini che potrebbero causare un'attenuazione di radiofrequenza AP wireless. La maggior parte dei punti di accesso forniscono test software che è possibile adottare per controllare la potenza del segnale, frequenza degli errori e velocità effettiva dei dati.

### <a name="determine-where-to-install-wireless-aps"></a>Determinare dove installare punti di accesso wireless
Nei disegni dell'architettura, individuare AP senza fili chiudere sufficientemente insieme per fornire una copertura wireless ampiamente ma sufficientemente distanti tra loro che non interferiscano tra loro.

La distanza tra punti di accesso necessaria dipende dal tipo di punto di accesso e antenna PA, gli aspetti dell'edificio che bloccano wireless segnali e altre fonti di interferenze. È possibile contrassegnare posizioni AP senza fili in modo che ogni punto di accesso wireless non più di 300 piedi da qualsiasi punto di accesso wireless adiacente. Vedere la documentazione del produttore AP senza fili alle specifiche del punto di accesso e le linee guida per la selezione host.

Installare punti di accesso wireless temporaneamente nei percorsi specificati nei disegni dell'architettura. Quindi, utilizza un computer portatile dotato di una scheda di rete wireless 802.11 e il software di sondaggio del sito che in genere viene fornito con schede di rete wireless, determinare la qualità del segnale all'interno di ogni area di copertura.

Nelle aree di copertura in cui la potenza del segnale è bassa, posizionare il punto di accesso per migliorare la potenza del segnale per l'area di copertura, installare ulteriori punti di accesso wireless per fornire una copertura necessaria, spostare o rimuovere le origini di interferenze del segnale.  

Aggiornare i disegni architetturali per indicare la posizione finale di tutti i punti di accesso wireless. Presenza di una mappa di selezione host AP accurata fornirà in seguito durante la risoluzione dei problemi relativi a operazioni o quando si desidera aggiornare o sostituire i punti di accesso.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Pianificare la configurazione di punti di accesso e il Client RADIUS dei criteri di RETE wireless
È possibile utilizzare criteri di RETE per configurare i punti di accesso wireless singolarmente o in gruppi.

Se si distribuisce una rete wireless di grandi dimensioni che include numerosi punti di accesso, è molto più semplice configurare i punti di accesso in gruppi. Per aggiungere i punti di accesso come gruppi di client RADIUS in Criteri di RETE, è necessario configurare i punti di accesso con queste proprietà.

- Punti di accesso wireless configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

- Tutti i punti di accesso wireless sono configurati con lo stesso segreto condiviso.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Pianificare l'uso di riconnessione rapida PEAP
In un'infrastruttura 802.1 X, punti di accesso wireless configurati come client RADIUS al server RADIUS. Quando la riconnessione rapida PEAP viene distribuito, un client wireless si sposta tra due o più punti di accesso non è necessario essere autenticati con ogni nuova associazione.

La riconnessione rapida PEAP riduce il tempo di risposta per l'autenticazione tra client e un autenticatore perché la richiesta di autenticazione verrà inoltrata dal nuovo punto di accesso ai criteri di rete che ha originariamente eseguito l'autenticazione e autorizzazione per il client richiesta di connessione.

Poiché il client PEAP e dei criteri di rete utilizzano precedentemente memorizzati nella cache Transport Layer Security \(TLS\) le proprietà di connessione \(la raccolta di cui è denominata l'handle TLS\), i criteri di rete possono determinare rapidamente che il client è autorizzato per una riconnessione.

>[!IMPORTANT]
>Per ottenere rapidamente riconnettersi per funzionare correttamente, i punti di accesso devono essere configurati come client RADIUS del NPS stesso.

Se i criteri di rete originale diventa non disponibile o se il client passa a un punto di accesso è configurato come client RADIUS in un altro server RADIUS, un'autenticazione completa deve verificarsi tra il client e l'autenticatore di nuovo.

### <a name="wireless-ap-configuration"></a>Configurazione AP senza fili
Nell'elenco seguente sono riepilogati gli elementi in genere configurati sullo standard 802.1 X\-punti di accesso wireless in grado di supportare:

> [!NOTE]
> I nomi degli elementi può variare per marca e modello e potrebbe essere diversi da quelli elencati di seguito. Vedere la documentazione di AP wireless per la configurazione\-dettagli specifici.

- **Identificatore del set di \(SSID\)**. Questo è il nome della rete wireless \(ad esempio, ExampleWlan\), e il nome che verrà annunciato ai client senza fili. Per ridurre la confusione, l'identificatore SSID che si desidera annunciare non deve corrispondere l'identificatore SSID viene trasmesso da eventuali reti wireless che rientrano nell'intervallo di ricezione della rete wireless.

    Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con l'identificatore SSID stesso. Nei casi in cui vengono distribuiti più punti di accesso wireless come parte della stessa rete wireless, configurare ogni punto di accesso wireless con l'identificatore SSID stesso.  

    Nei casi in cui è necessario distribuire diverse reti senza fili per soddisfare specifiche esigenze aziendali, l'AP senza fili in una rete deve trasmettere un SSID diverso rispetto al SSID l'altra rete\(s\). Ad esempio, se è necessaria una rete wireless separata per i dipendenti e Guest, è possibile configurare punti di accesso wireless per la rete aziendale con l'identificatore SSID impostato per la trasmissione di **ExampleWLAN**. Per la rete guest, è quindi possibile impostare SSID di ogni punto di accesso wireless per trasmettere **GuestWLAN**. In questo modo i dipendenti e Guest può connettersi la rete senza confusione non necessaria.  

    > [!TIP]  
    > Alcuni AP senza fili hanno la possibilità di trasmettere più SSID per contenere più\-distribuzione della rete. AP senza fili che possono trasmettere più SSID può ridurre i costi di manutenzione e distribuzione.  

- **Wireless l'autenticazione e crittografia**.

    L'autenticazione wireless è l'autenticazione di sicurezza che viene usato quando il client senza fili associa a un punto di accesso wireless.  

    Crittografia wireless è l'algoritmo di crittografia di sicurezza che viene usato con l'autenticazione wireless per proteggere le comunicazioni inviate tra il punto di accesso wireless e i computer client wireless.  

- **Indirizzo IP AP wireless \(statico\)**. In ogni punto di accesso wireless, configurare un indirizzo IP statico. Se la subnet viene gestita da un server DHCP, assicurarsi che tutti gli indirizzi IP PA rientrano in un intervallo di esclusione DHCP in modo che il server DHCP non tenta di eseguire lo stesso indirizzo IP a un altro computer o dispositivo. Gli intervalli di esclusione sono documentati nella procedura "per creare e attivare un nuovo ambito DHCP" nel [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Se si prevede di configurare i punti di accesso come client RADIUS dal gruppo di criteri di RETE, ogni punto di accesso del gruppo deve avere un indirizzo IP dallo stesso intervallo di indirizzi IP.

- **Nome DNS**. Alcuni AP senza fili può essere configurato con un nome DNS. Configurare ogni AP senza fili con un nome univoco. Ad esempio, se si dispone di un accesso senza fili distribuito in un più\-storia di compilazione, è possibile denominare i primi tre punti di accesso wireless che vengono distribuite sul pavimento terzo AP3\-01, AP3\-02 e AP3\-03.

- **Maschera di subnet AP wireless**. Configurazione della maschera per designare la parte di IP indirizzo è l'ID di rete e la parte dell'indirizzo IP è l'host.

- **Servizio DHCP AP**. Se il punto di accesso wireless è integrato\-nel servizio DHCP, disabilitarla.

- **Segreto condiviso RADIUS**. Utilizzare un RAGGIO univoco segreto condiviso per ogni punto di accesso wireless a meno che non si prevede di configurare client RADIUS NPS in gruppi, in quali casi è necessario configurare tutti i punti di accesso nel gruppo con lo stesso segreto condiviso. I segreti condivisi devono essere una sequenza casuale di almeno 22 caratteri, con lettere maiuscole e minuscole, numeri e segni di punteggiatura. Per assicurare la casualità, è possibile utilizzare un programma di generazione casuale di caratteri per creare i segreti condivisi. Si consiglia di registrare il segreto condiviso per ogni punto di accesso wireless e archiviarlo in un luogo sicuro, ad esempio un ufficio sicuro. Quando si configurano i client RADIUS nella console Criteri di rete si creerà una versione virtuale dell'ogni punto di accesso. Il segreto condiviso configurato in ogni PA in Criteri di rete virtuale deve corrispondere il segreto condiviso sul punto di accesso effettivo, fisica.

- **Indirizzo IP del server RADIUS**. Digitare l'indirizzo IP di NPS che si desidera usare per autenticare e autorizzare le richieste di connessione a questo punto di accesso.

- **La porta UDP\(s\)**. Per impostazione predefinita dei criteri di rete Usa le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS. È consigliabile che non modificare le impostazioni di porte UDP raggio predefinito.

- **Gli attributi VSA**. Alcuni punti di accesso wireless richiedono fornitore\-attributi specifici \(VSA\) per fornire funzionalità AP wireless completo.

- **Filtri DHCP**. Configurare in modo da bloccare i computer client wireless dall'invio di pacchetti IP dalla porta UDP 68 per la rete senza fili. Vedere la documentazione per il punto di accesso wireless configurare il filtro di DHCP.

- **Il filtro dei DNS**. Configurare in modo da bloccare i computer client wireless dall'invio di pacchetti IP dalla porta TCP o UDP 53 alla rete senza fili. Vedere la documentazione per il punto di accesso wireless configurare il filtro dei DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Pianificazione della configurazione di client wireless e accesso

Quando si pianifica la distribuzione di 802.1 X\-autenticato accesso wireless, è necessario considerare diversi client\-fattori specifici:

- **Pianificazione di supporto per gli standard più**.

    Determinare se tutti i computer wireless utilizzando la stessa versione di Windows o se sono una combinazione di computer che eseguono sistemi operativi diversi. Se sono diversi, assicurarsi di comprendere le differenze nella standard supportati dai sistemi operativi.

    Determinare se tutte le schede di rete wireless in tutti i computer client wireless supportano gli stessi standard wireless, o se è necessario supportare vari standard. Ad esempio, determinare se alcuni driver dell'hardware di schede di rete supportano WPA2\-Enterprise e AES, mentre altri ne supportano solo WPA\-Enterprise e TKIP.

- **Pianificazione della modalità di autenticazione client**. Modalità di autenticazione consente di definire il modo in cui i client Windows elaborano le credenziali di dominio. È possibile selezionare dalla modalità di autenticazione tre rete seguenti nei criteri di rete wireless.  

    1. **Utente re\-autenticazione**. Questa modalità consente di specificare che l'autenticazione viene sempre eseguita utilizzando le credenziali di sicurezza in base allo stato corrente del computer. Quando gli utenti non sono connessi al computer, l'autenticazione viene eseguita usando le credenziali del computer. Quando un utente è connesso al computer, l'autenticazione viene sempre eseguita utilizzando le credenziali dell'utente.  

    2. **Solo computer**. Computer solo modalità specifica che l'autenticazione viene sempre eseguita utilizzando solo le credenziali del computer.  

    3.  **Autenticazione utente**. Modalità di autenticazione utente specifica che l'autenticazione viene eseguita solo quando l'utente è connesso al computer. Quando non sono presenti nessun utente connesso al computer, non vengono eseguiti tentativi di autenticazione.  

- **Pianificazione restrizioni wireless**. Determinare se si desidera fornire tutti gli utenti wireless con lo stesso livello di accesso alla rete wireless o se si desidera limitare l'accesso per alcuni utenti wireless. È possibile applicare restrizioni in Criteri di rete in base a specifici gruppi di utenti wireless.  Ad esempio, è possibile definire specifiche giorni e ore che alcuni gruppi sono autorizzati l'accesso alla rete wireless.  

- **Pianificazione di metodi per l'aggiunta di nuovi computer wireless**. Per il wireless\-in grado di supportare computer appartenenti al dominio prima di distribuire la rete wireless, se il computer è connesso a un segmento di rete cablata che non è protetta da 802.1 X, le impostazioni di configurazione senza fili vengono applicate automaticamente dopo la configurazione della rete senza fili \(IEEE 802.11\) criteri sul controller di dominio e dopo l'aggiornamento di criteri di gruppo sul client senza fili.  

    Per i computer che non sono già aggiunti al dominio, tuttavia, è necessario pianificare un metodo per applicare le impostazioni necessarie per 802.1 X\-l'accesso autenticato. Ad esempio, determinare se si desidera aggiungere il computer al dominio utilizzando uno dei metodi seguenti.

    1.  Connettere il computer a un segmento di rete cablata che non è protetta da 802.1 X, quindi aggiungere il computer al dominio.

    2.  Fornire agli utenti wireless con le impostazioni necessarie per aggiungere il proprio profilo bootstrap wireless, che consente di aggiungere il computer al dominio e i passaggi.

    3.  Assegnare IL personale IT per aggiungere i computer client wireless al dominio.

### <a name="planning-support-for-multiple-standards"></a>Pianificazione di supporto per più standard

La rete senza fili \(IEEE 802.11\) estensione di criteri in Criteri di gruppo offre un'ampia gamma di opzioni di configurazione per supportare un'ampia gamma di opzioni di distribuzione.

È possibile distribuire i punti di accesso wireless configurati con gli standard che si desidera supportare e quindi configurare i profili wireless più nella rete senza fili \(IEEE 802.11\) con ogni profilo di un set di standard che è necessario specificare i criteri.

Ad esempio, se la rete include computer wireless che supportano WPA2\-Enterprise e AES, altri computer che supportano WPA\-Enterprise e AES e altri computer che supportano solo WPA\-Enterprise e TKIP, è necessario determinare se si desidera:

- Configurare un unico profilo per supportare tutti i computer wireless utilizzando il metodo di crittografia più debole che tutti i computer supportano - in questo caso, WPA\-Enterprise e TKIP.  
- Configurare i profili di due per garantire la miglior protezione possibile supportato da ogni computer wireless. In questa istanza è necessario configurare un profilo che specifica la crittografia \(WPA2\-Enterprise e AES\), e un profilo che usa WPA debole\-Enterprise e TKIP crittografia. In questo esempio, è essenziale inserire il profilo che utilizza WPA2\-Enterprise e AES più alta nell'ordine di preferenza. Computer che non sono in grado di utilizzare WPA2\-Enterprise e AES verrà automaticamente andare al profilo successivo nell'ordine di preferenza ed elaborare il profilo che specifica WPA\-Enterprise e TKIP.

> [!IMPORTANT]
> È necessario inserire il profilo con gli standard più sicuri superiore nell'elenco ordinato dei profili, poiché connessione dei computer utilizza il primo profilo che sono in grado di utilizzare.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Accesso limitato alla rete wireless di pianificazione

In molti casi, si potrebbe voler fornire agli utenti wireless con vari livelli di accesso alla rete wireless. È possibile, ad esempio consentire accesso illimitato alcuni utenti, tutte le ore del giorno, ogni giorno della settimana. Per altri utenti, è possibile solo consentire l'accesso durante l'orario principale, dal lunedì al venerdì, sabato e domenica gli accessi.

Questa guida fornisce istruzioni per creare un ambiente di accesso che inserisce tutti gli utenti wireless in un gruppo con accesso comune per le risorse wireless. Si crea un gruppo di sicurezza wireless agli utenti in Active Directory Users e computer blocca\-in e quindi verificare tutti gli utenti per cui si desidera concedere l'accesso wireless un membro di tale gruppo.

Quando si configurano i criteri di rete dei criteri di rete, specificare il gruppo di sicurezza utenti wireless come l'oggetto che elabora i criteri di rete durante la determinazione di autorizzazione.

Tuttavia, la distribuzione richiede il supporto per vari livelli di accesso è necessario solo eseguire le operazioni seguenti:  

1. Creare più di un gruppo di sicurezza utenti Wireless per la creazione di gruppi di sicurezza wireless aggiuntive in Active Directory Users and Computers. Ad esempio, è possibile creare un gruppo che contiene gli utenti che hanno accesso completo, un gruppo per gli utenti che dispongono di accesso durante il regolare orario di lavoro e altri gruppi che soddisfano gli altri criteri che corrispondono ai propri requisiti.

2. Aggiungere utenti a gruppi di protezione appropriati che è stato creato.

3. Configurare i criteri di rete dei criteri di RETE aggiuntivi per ogni gruppo di sicurezza wireless aggiuntive e configurare i criteri per applicare le condizioni e i vincoli necessari per ogni gruppo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Metodi di pianificazione per l'aggiunta di nuovi computer wireless

Il metodo preferito per aggiungere nuovi computer wireless al dominio e quindi accedere al dominio consiste nell'usare una connessione cablata a un segmento di LAN che ha accesso ai controller di dominio e non è protetta da un commutatore Ethernet di autenticazione 802.1x.

In alcuni casi, tuttavia, potrebbe risultare poco pratico da utilizzare una connessione cablata per aggiungere computer al dominio o, per un utente di utilizzare una connessione cablata per il primo tentativo di accesso con computer che sono già connessi al dominio.

Per aggiungere un computer al dominio tramite una connessione wireless o agli utenti di accedere al dominio la prima volta con un dominio\-computer aggiunto e una connessione wireless, i client wireless devono innanzitutto stabilire una connessione alla rete wireless in un segmento che ha accesso ai controller di dominio di rete utilizzando uno dei metodi seguenti.

1. **Un membro del personale IT aggiunge un computer wireless al dominio e quindi configura un profilo Single Sign On bootstrap wireless.** Con questo metodo, un amministratore IT si connette il computer wireless alla rete Ethernet cablata e quindi aggiunge il computer al dominio. L'amministratore distribuisce quindi il computer per l'utente. Quando l'utente avvia il computer, le credenziali di dominio che si specificano manualmente per il processo di accesso utente consentono di stabilire una connessione alla rete wireless sia accedere al dominio.  

2. **L'utente consente di configurare manualmente i computer wireless con profilo wireless bootstrap e quindi viene aggiunto al dominio.** Con questo metodo, gli utenti configurare manualmente i computer wireless con un profilo wireless bootstrap sulla base delle istruzioni da un amministratore IT. Il profilo wireless bootstrap consente agli utenti di stabilire una connessione wireless e quindi aggiungere il computer al dominio. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può accedere al dominio tramite una connessione wireless e le credenziali di account di dominio.

Per distribuire accesso senza fili, vedere [distribuzione di accesso Wireless](e-wireless-access-deployment.md).
