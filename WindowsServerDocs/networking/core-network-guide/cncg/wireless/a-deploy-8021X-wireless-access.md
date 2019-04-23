---
title: Distribuire l'accesso wireless autenticato 802.1 basato su password
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "Distribuisci con 802.1x basato su Password X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f34580f4a0fd92c6f059d19d09a6926fc2775960
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840012"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Distribuire Password\-basato su accesso Wireless autenticato 802.1 X

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Si tratta di una guida complementare a Windows Server&reg; Guida alla rete Core 2016. Guida alla rete Core fornisce istruzioni per pianificare e distribuire i componenti necessari per una rete completamente funzionante e un nuovo dominio Active Directory® in una nuova foresta.

In questa guida viene illustrato come si basano su una rete core fornendo istruzioni su come distribuire Institute of Electrical and Electronics Engineers \(IEEE\) 802.1 X\-accesso wireless IEEE 802.11 mediante Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 autenticato \(PEAP\-MS\-CHAP v2\).

Poiché il protocollo PEAP\-MS\-CHAP v2 richiede l'immissione di password\-credenziali in base al posto di un certificato durante il processo di autenticazione, è in genere più semplice e meno costoso da distribuire EAP\-TLS o PEAP\-TLS.

>[!NOTE]
>In questa Guida, IEEE 802.1 X accesso Wireless autenticato con PEAP\-MS\-CHAP v2 è abbreviato in "accesso senza fili" e "Accesso Wi-Fi".

## <a name="about-this-guide"></a>Informazioni sulla guida
Questa Guida, in combinazione con le guide dei prerequisiti descritti di seguito, vengono fornite istruzioni su come distribuire l'infrastruttura di accesso Wi-Fi seguente.  

- Uno o più 802.1 X\-punti di accesso wireless 802.11 \(APs\).

- Servizi di dominio Active Directory \(Active Directory Domain Services\) utenti e computer.

- Gestione Criteri di gruppo

- Uno o più Server dei criteri di rete \(NPS\) server.

- Certificati del server per i computer che eseguono Server dei criteri di rete.

- Computer client che eseguono Windows wireless&reg; 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dipendenze per questa Guida

Per distribuire correttamente wireless autenticato con questa Guida, è necessario disporre di un ambiente di rete e di dominio con tutte le tecnologie necessarie distribuite. È necessario disporre anche i certificati del server distribuiti per l'autenticazione NPSs.

Nelle sezioni seguenti vengono forniscono collegamenti alla documentazione che illustra come distribuire queste tecnologie.

#### <a name="network-and-domain-environment-dependencies"></a>Dipendenze di ambiente di rete e di dominio

Questa guida è progettata per gli amministratori di rete e di sistema e che hanno seguito le istruzioni in Windows Server 2016 **Guida alla rete Core** per distribuire una rete core, o per coloro che in precedenza hanno distribuito le tecnologie di base incluso nella rete core, tra cui Active Directory, Domain Name System \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP e dei criteri di rete Windows Internet Name Service \(WINS\).  

Guida alla rete Core è disponibile nei percorsi seguenti:

- Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) è disponibile nella libreria tecnica di Windows Server 2016. 

- Il [Guida alla rete Core](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) è disponibile in formato Word nel Raccolta TechNet, anche in [ https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683 ](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dipendenze del certificato server
Sono disponibili due opzioni disponibili per registrare i server di autenticazione con certificati server per l'utilizzo con l'autenticazione 802.1 X - distribuire la propria infrastruttura a chiave pubblica utilizzando Servizi certificati Active Directory \(Servizi certificati Active Directory\) o utilizzare i certificati server registrati da un'autorità di certificazione pubblica \(CA\).

##### <a name="ad-cs"></a>SERVIZI CERTIFICATI ACTIVE DIRECTORY
Gli amministratori di rete e di sistema la distribuzione wireless autenticato devono seguire le istruzioni di Windows Server 2016 Core guida complementare alla rete, **distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X**. Questa guida illustra come distribuire e utilizzare Servizi certificati Active Directory per registrare automaticamente i certificati server nei computer che eseguono criteri di rete.

In questa guida è disponibile nel percorso seguente.

- La Guida complementare alla Core rete di Windows Server 2016 [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) in formato HTML nella raccolta di documentazione tecnica. 

##### <a name="public-ca"></a>CA pubblica
È possibile acquistare i certificati del server da una CA pubblica, ad esempio VeriSign, che i computer client attendibile. 

Un computer client considera attendibile un'autorità di certificazione quando viene installato il certificato della CA nell'archivio certificati Autorità di certificazione radice attendibili. Per impostazione predefinita, i computer che eseguono Windows hanno più certificati di autorità di certificazione pubblici installati nel proprio certificato di autorità di certificazione radice attendibili archiviare.  

È consigliabile vedere le guide alla progettazione e alla distribuzione per ognuna delle tecnologie usate in questo scenario di distribuzione. Queste guide consentono di determinare se questo scenario di distribuzione fornisce la configurazione e i servizi necessari per la rete dell'organizzazione.

### <a name="requirements"></a>Requisiti
Di seguito sono i requisiti per la distribuzione di un'infrastruttura di accesso wireless tramite lo scenario descritto in questa guida:

- Prima di distribuire questo scenario, è necessario acquistare 802.1 X\-punti di accesso wireless in grado di fornire una copertura wireless nelle posizioni desiderate nel sito. La sezione pianificazione di questa Guida consente di determinare le funzionalità che dei punti di accesso deve supportare.

- Servizi di dominio Active Directory \(Active Directory Domain Services\) è installato, come le altre tecnologie di rete necessarie, seguendo le istruzioni nella Guida alla rete Windows Server 2016 Core.  

- Servizi certificati Active Directory viene distribuito e i certificati server registrati nei NPSs. Questi certificati sono necessari quando si distribuisce PEAP\-MS\-CHAP v2 certificato\-basato su metodo di autenticazione che viene utilizzato in questa Guida.

- Un membro dell'organizzazione abbia familiarità con gli standard IEEE 802.11 supportati da punti di accesso wireless e le schede di rete wireless che vengono installate nei computer client e dispositivi nella rete. Ad esempio, un utente dell'organizzazione abbia familiarità con i tipi frequenza radio, l'autenticazione wireless 802.11 \(WPA o WPA2\), e crittografie \(AES o TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Informazioni non contenute in questa guida  
Ecco alcuni elementi in questa Guida non fornisce:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Indicazioni per la selezione di 802.1 X\-punti di accesso wireless in grado di supportare

Poiché esistono molte differenze tra marche e modelli di 802.1 X\-punti di accesso wireless in grado di supportare questa Guida non fornisce informazioni dettagliate su:  

- Determinare quali marca o modello di AP senza fili è preferibile adatto alle proprie esigenze.

- La distribuzione fisica di AP senza fili nella rete.

- Avanzata configurazione AP senza fili, ad esempio per wireless virtual Local Area Network \(VLAN\).

- Istruzioni su come configurare fornitore AP senza fili\-attributi specifici in Criteri di RETE.

Inoltre, terminologia e i nomi delle impostazioni variano tra i modelli e i marchi AP senza fili e potrebbero non corrispondere i nomi delle impostazioni generiche che vengono utilizzati in questa Guida. Per informazioni sulla configurazione AP senza fili, è necessario esaminare la documentazione fornita dal produttore di punti di accesso wireless.

### <a name="instructions-for-deploying-nps-certificates"></a>Istruzioni per la distribuzione dei certificati dei criteri di rete
  
Esistono due alternative per la distribuzione dei certificati dei criteri di rete. Questa Guida non fornisce indicazioni complete che consentono di determinare quale alternativa più adatta a soddisfare le proprie esigenze. In generale, tuttavia, le scelte che devi affrontare sono:

- Acquisto di certificati da una CA pubblica, ad esempio VeriSign, che sono già considerato attendibile da Windows\-i client basati su. In genere, questa opzione è consigliata per le reti più piccole.

- Distribuzione di un'infrastruttura a chiave pubblica \(PKI\) sulla rete utilizzando Servizi certificati Active Directory. È consigliato per la maggior parte delle reti e le istruzioni per distribuire i certificati server con Servizi certificati Active Directory sono disponibili nella Guida alla distribuzione menzionati in precedenza.

### <a name="nps-network-policies-and-other-nps-settings"></a>Criteri di criteri di rete e altre impostazioni dei criteri di rete

Fatta eccezione per le impostazioni di configurazione effettuate quando si eseguono i **Configurazione 802.1 X** procedura guidata, come descritto in questa Guida, questa Guida non fornisce informazioni dettagliate per configurare manualmente le condizioni dei criteri di RETE, i vincoli o altre impostazioni dei criteri di RETE.

### <a name="dhcp"></a>DHCP

Questa Guida alla distribuzione non fornisce informazioni sulla progettazione o distribuzione subnet DHCP per LAN wireless.

## <a name="technology-overviews"></a>Panoramiche delle tecnologie
Di seguito sono panoramiche sulla tecnologia per la distribuzione di accesso wireless:

### <a name="ieee-8021x"></a>IEEE 802.1X
Lo standard IEEE 802.1 X definisce la porta\-basato su controllo di accesso alla rete che viene utilizzato per fornire l'accesso autenticato alle reti Ethernet. Questa porta\-controllo accesso alla rete in base utilizza le caratteristiche fisiche dell'infrastruttura LAN commutata per autenticare i dispositivi collegati a una porta LAN. L'accesso alla porta può essere negato se il processo di autenticazione non riesce. Nonostante questo standard è stato progettato per le reti Ethernet cablate, è stato adattato per l'uso sulle LAN wireless 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1x\-punti di accesso wireless in grado di supportare \(APs\)
Questo scenario richiede la distribuzione di uno o più 802.1 X\-in grado di supportare punti di accesso wireless che sono compatibili con Remote Authentication Dial\-In User Service \(RADIUS\) protocollo.

802.1X e RADIUS\-APs conformi, se distribuite in un'infrastruttura RADIUS con un server RADIUS, ad esempio un criteri di rete, vengono chiamati *client RADIUS*.

### <a name="wireless-clients"></a>Computer client wireless
In questa guida fornisce i dettagli di configurazione completa per fornire l'accesso autenticato per dominio 802.1 X\-gli utenti che si connettono alla rete con i computer client wireless Windows 10, Windows 8.1 e Windows 8. I computer devono essere aggiunti al dominio per stabilire correttamente l'accesso autenticato.

>[!NOTE]
>È inoltre possibile utilizzare i computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come computer client wireless.

### <a name="support-for-ieee-80211-standards"></a>Supporto per IEEE 802.11 standard
Sistemi operativi Windows e Windows Server forniscono compilato\-il supporto per 802.11 wireless networking. In questi sistemi operativi, una scheda di rete senza fili 802.11 installata viene visualizzata come una connessione di rete wireless in Centro rete e condivisione. 

Anche se è compilato\-il supporto per reti senza fili 802.11, i componenti senza fili di Windows dipendono le operazioni seguenti:

- Le funzionalità della scheda di rete wireless. La scheda di rete senza fili installata deve supportare la LAN wireless o standard di sicurezza wireless che è necessario. Ad esempio, se la scheda di rete senza fili non supporta Wi\-Fi Protected Access \(WPA\), non è possibile abilitare o configurare le opzioni di protezione WPA.

- Le funzionalità del driver della scheda di rete wireless. Per consentire di configurare le opzioni di rete wireless, il driver per scheda di rete wireless deve supportare la creazione di report di tutte le funzionalità di Windows. Verificare che il driver per la scheda di rete è stato scritto per le funzionalità del sistema operativo. Assicurarsi inoltre che il driver è la versione più recente mediante il controllo di Microsoft Update o il sito Web del fornitore scheda rete wireless.

Nella tabella seguente mostra la velocità di trasmissione e le frequenze per comuni standard wireless IEEE 802.11.

|Standard|Frequenze|Trasmissione velocità in bit|Uso|
|-------------|---------------|--------------------------|---------|  
|802.11|S\-banda industriale, scientifico e medici \(ISM\) intervallo di frequenza \(2.4 a 2,5 GHz\)|2 megabit al secondo \(Mbps\)|Obsoleta. Generalmente non utilizzato.|  
|802.11 b|S\-banda ISM|11 Mbps|Comunemente utilizzati.|  
|802.11|C\-banda ISM \(5,725 a 5.875 GHz\)|54 Mbps|Non è in genere usato a causa della nota spese e intervallo limitato.|  
|g 802.11|S\-banda ISM|54 Mbps|Ampiamente usati. dispositivi 802.11g sono compatibili con 802.11b dispositivi.|  
|802.11 n \2.4 e 5,0 GHz|C\-banda e S\-banda ISM|250 Mbps|Dispositivi in base alle fasi pre\-ratifica IEEE 802.11 n standard è diventato disponibile nell'agosto 2007. 802.11n molti dispositivi compatibili con 802.11 a, b e c dispositivi.|
|802.11ac |5 GHz |6,93 Gbps |802.11 AC, approvato da IEEE 2014, è più scalabile e più veloce rispetto a 802.11 n e viene distribuito in cui i punti di accesso e i computer client wireless supportano.|

### <a name="wireless-network-security-methods"></a>Metodi di sicurezza di rete wireless
**Metodi di sicurezza di rete wireless** è un raggruppamento informale di autenticazione wireless \(talvolta detta protezione wireless\) e la crittografia di sicurezza wireless. Crittografia e l'autenticazione wireless vengono utilizzate insieme per impedire che utenti non autorizzati l'accesso alla rete wireless e per proteggere le trasmissioni wireless. 

Quando si configurano le impostazioni di protezione wireless in Criteri di gruppo di criteri di reti Wireless, sono presenti più combinazioni da selezionare. Tuttavia, solo il WPA2\-Enterprise, WPA\-Enterprise e aperta con 802.1 X standard di autenticazione sono supportati per 802.1 X autenticato le distribuzioni wireless.

>[!NOTE]
>Durante la configurazione dei criteri di rete Wireless, è necessario selezionare **WPA2\-Enterprise**, **WPA\-Enterprise**, o **aperta con 802.1 X** per ottenere l'accesso alle impostazioni EAP sono obbligatori per le distribuzioni wireless con autenticazione 802.1 X.  

#### <a name="wireless-authentication"></a>Autenticazione wireless
In questa guida si consiglia che l'utilizzo di ai seguenti standard di autenticazione wireless per 802.1 X autenticato distribuzioni wireless.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)** WPA è uno standard provvisorio sviluppato da Wi-Fi Alliance per rispettare il protocollo di sicurezza wireless 802.11. Il protocollo WPA è stato sviluppato in risposta a un numero di problemi gravi che sono stati individuati nel precedente Wired Equivalent Privacy \(WEP\) protocollo.

WPA\-Enterprise offre una maggiore sicurezza tramite WEP da:  

1. La richiesta di autenticazione che usa il framework X EAP 802.1x come parte dell'infrastruttura che assicura l'autenticazione reciproca centralizzata e gestione delle chiavi dinamica  

2. Migliorare il valore di controllo di integrità \(ICV\) con Message Integrity Check \(MIC\), per proteggere l'intestazione e il payload  

3. Implementazione di un contatore di frame per impedire gli attacchi di riproduzione  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)** come il WPA\-standard Enterprise, WPA2\-Enterprise utilizza 802.1 X ed EAP framework. WPA2\-Enterprise fornisce maggiore protezione dei dati per gli utenti più grandi e di reti gestite. WPA2\-Enterprise è un protocollo affidabile che è progettato per impedire accessi non autorizzati alla rete verificando gli utenti di rete tramite un server di autenticazione.  

#### <a name="wireless-security-encryption"></a>Crittografia di sicurezza wireless
Crittografia di sicurezza wireless viene usata per proteggere le trasmissioni wireless che vengono inviate tra il client wireless e il punto di accesso wireless. Crittografia di sicurezza wireless viene usata in combinazione con il metodo di autenticazione di sicurezza di rete selezionata. Per impostazione predefinita, i computer che eseguono Windows 10, Windows 8.1 e Windows 8 supportano due standard di crittografia:

1. **Protocollo di integrità chiave temporale** \(TKIP\) è un protocollo di crittografia precedente che è stato originariamente progettato per fornire crittografia wireless più sicura rispetto a quello fornito dal debole Wired Equivalent Privacy \(WEP\) protocollo. TKIP è stato progettato per IEEE 802.11 i gruppo e il Wi attività\-Fi Alliance per sostituire WEP senza richiedere la sostituzione di hardware legacy. TKIP è una suite di algoritmi che incapsula il payload WEP e consente agli utenti delle apparecchiature Wi-Fi legacy di eseguire l'aggiornamento a TKIP senza sostituzione di hardware. Ad esempio, WEP TKIP Usa l'algoritmo di crittografia RC4 flusso come base. Il nuovo protocollo, tuttavia, crittografa ogni pacchetto di dati con una chiave di crittografia univoco e le chiavi sono molto più avanzate rispetto a quelle da WEP. TKIP è molto utile per l'aggiornamento di sicurezza nei dispositivi meno recenti che sono state progettate per l'uso solo WEP, non viene risolto tutti i problemi di sicurezza con connessione LAN wireless e nella maggior parte dei casi non è sufficientemente potente per la protezione per enti pubblici riservati o dati aziendali trasmissioni.  

2. **Advanced Encryption Standard** \(AES\) è il protocollo di crittografia preferito per la crittografia dei dati commerciali e governative. AES offre un livello superiore di protezione della trasmissione wireless rispetto a TKIP o WEP. A differenza TKIP e WEP, AES richiede hardware senza fili che supporta lo standard AES. AES è un\-crittografia della chiave standard che utilizza tre blocchi, AES\-128, AES\-192 e AES\-256.

In Windows Server 2016, AES seguente\-metodi di crittografia wireless base sono disponibili per la configurazione nelle proprietà del profilo wireless quando si seleziona un metodo di autenticazione di WPA2\-Enterprise, è consigliabile.

1. **AES\-CCMP**. Modalità di crittografia blocco concatenamento messaggio codice protocollo di autenticazione del contatore \(CCMP\) implementa standard 802.11 i è progettato per la crittografia di sicurezza superiore rispetto a quelle fornite da WEP e utilizza le chiavi di crittografia AES a 128 bit.
2. **AES\-GCMP**. Protocollo di modalità contatore Galois \(GCMP\) è supportata da 802.11 AC, è più efficiente AES\-CCMP e fornisce prestazioni migliori per i computer client wireless. GCMP utilizza chiavi di crittografia AES a 256 bit.

> [!IMPORTANT]
> Wired Equivalency Privacy \(WEP\) è lo standard di protezione wireless originale utilizzato per crittografare il traffico di rete. Non è necessario distribuire WEP sulla rete perché esistono ben\-causa di vulnerabilità note in questa forma di protezione non aggiornata.

### <a name="active-directorydoman-services-adds"></a>Servizi di dominio Active Directory \(Active Directory Domain Services\)
Active Directory fornisce un database distribuito che archivia e gestisce le informazioni sulle risorse di rete e applicazione\-dati specifici provenienti dalla directory\-applicazioni abilitate. Gli amministratori possono utilizzare Servizi di dominio Active Directory per organizzare gli elementi di una rete, ad esempio utenti, computer e altri dispositivi, in una struttura di contenimento gerarchica. La struttura di contenimento gerarchica include la foresta di Active Directory, domini nella foresta e le unità organizzative \(unità organizzative\) in ogni dominio. Un server che esegue Active Directory viene definito un *controller di dominio*.  

AD DS contiene gli account utente, gli account computer e proprietà dell'account necessari per IEEE 802.1 X e PEAP\-MS\-CHAP v2 per autenticare le credenziali dell'utente e per valutare l'autorizzazione per le connessioni wireless.

### <a name="active-directory-users-and-computers"></a>Utenti e computer di Active Directory
Active Directory Users and Computers è un componente di dominio Active Directory che contiene gli account che rappresentano entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Oggetto *gruppo di sicurezza* è una raccolta di account utente o computer che gli amministratori possono gestire come singola unità. Account utente e computer appartenenti a un determinato gruppo vengono dette *i membri del gruppo*.  

### <a name="group-policy-management"></a>Gestione Criteri di gruppo
Gestione criteri di gruppo consente di directory\-basato su gestione delle modifiche e configurazione delle impostazioni utente e computer, incluse le informazioni di sicurezza e utente. Si usano criteri di gruppo per definire le configurazioni per i gruppi di utenti e computer. Con criteri di gruppo, è possibile specificare le impostazioni per le voci del Registro di sistema, sicurezza, installazione del software, script, reindirizzamento cartelle, servizi di installazione remota e la manutenzione di Internet Explorer. Le impostazioni di criteri di gruppo creati dall'utente sono contenute in un oggetto Criteri di gruppo \(oggetto Criteri di gruppo\). Associando un oggetto Criteri di gruppo di contenitori di sistema di Active Directory selezionati, siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo a utenti e computer presenti in tali contenitori Active Directory. Per gestire gli oggetti Criteri di gruppo in un'organizzazione, è possibile utilizzare Microsoft Management Console Editor Gestione criteri di gruppo \(MMC\).  

In questa guida vengono fornite istruzioni dettagliate su come specificare le impostazioni della rete senza fili \(IEEE 802.11\) estensione criteri di gestione criteri di gruppo. La rete senza fili \(IEEE 802.11\) criteri configurare dominio\-accesso wireless autenticato con computer client wireless membri con la necessaria connettività e le impostazioni wireless per 802.1 X.

### <a name="server-certificates"></a>Certificati del server
Questo scenario di distribuzione richiede certificati del server per ogni dei criteri di rete che esegue l'autenticazione 802.1x.  

Un certificato del server è un documento digitale comunemente usato per l'autenticazione e la protezione delle informazioni sulle reti aperte. Un certificato associa in modo sicuro una chiave pubblica all'entità che contiene la corrispondente chiave privata. I certificati sono firmati digitalmente dall'autorità di certificazione emittente, e che possono essere emessi per un utente, un computer o un servizio.  

Un'autorità di certificazione \(CA\) è un'entità responsabile per stabilire e garantire l'autenticità delle chiavi pubbliche appartenenti ai soggetti di \(in genere utenti o computer\) o altre autorità di certificazione. Le attività di un'autorità di certificazione includono l'associazione di chiavi pubbliche a nomi distinti attraverso certificati firmati, la gestione di numeri di serie del certificato e la revoca dei certificati.  

Servizi certificati Active Directory \(Servizi certificati Active Directory\) è un ruolo del server che rilascia certificati come CA di rete. Un Servizi certificati Active Directory certificati di infrastruttura, noto anche come un *infrastruttura a chiave pubblica \(PKI\)*, offre servizi personalizzabili per l'emissione e la gestione dei certificati per le aziende.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>PEAP, EAP e PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) estende punto\-a\-protocollo Point \(PPP\) consentendo altri metodi di autenticazione che utilizzano informazioni e credenziali scambi di lunghezza arbitraria. Con l'autenticazione EAP sia nella rete accesso client e l'autenticatore \(, ad esempio i criteri di rete\) deve supportare lo stesso tipo EAP per l'autenticazione ha esito positivo da eseguire. Windows Server 2016 include un'infrastruttura EAP, supporta due tipi EAP e la possibilità di inviare i messaggi EAP al NPSs. Utilizzando EAP, è possibile supportare schemi di autenticazione aggiuntivi, noti come *tipi EAP*. I tipi EAP sono supportati da Windows Server 2016 sono:  

- Transport Layer Security \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>I tipi EAP avanzati \(ad esempio quelle basate sui certificati\) offrono una migliore protezione contro bruta\-forzare informatico di password, attacchi con dizionario e attacchi\-basato su protocolli di autenticazione \(ad esempio il protocollo CHAP o MS\-CHAP versione 1\).

Protected EAP \(PEAP\) utilizza TLS per creare un canale crittografato tra un client che esegue l'autenticazione PEAP, ad esempio un computer wireless e un autenticatore PEAP, ad esempio un criteri di rete o altri server RADIUS. PEAP non specifica un metodo di autenticazione, ma fornisce protezione aggiuntiva per altri protocolli di autenticazione EAP \(ad esempio EAP\-MS\-CHAP v2\) che possono funzionare attraverso il canale crittografato TLS fornito da PEAP. PEAP viene utilizzato come un metodo di autenticazione per i client di accesso che si connettono alla rete dell'organizzazione tramite i seguenti tipi di server di accesso alla rete \(NAS\):

- 802.1 X\-punti di accesso wireless in grado di supportare

- 802.1 X\-in grado di supportare commutatori di autenticazione

- Computer che eseguono Windows Server 2016 e il servizio di accesso remoto \(RAS\) che sono configurati come rete privata virtuale \(VPN\) server, i server DirectAccess o entrambi

- Computer che eseguono Servizi Desktop remoto e Windows Server 2016

PEAP\-MS\-CHAP v2 è più facile da distribuire rispetto a EAP\-TLS perché l'autenticazione utente viene eseguita tramite password\-credenziali basate su \(nome utente e password\), anziché certificati o smart card. Solo i criteri di rete o altri server RADIUS deve essere un certificato. Il certificato dei criteri di rete viene usato per i criteri di rete durante il processo di autenticazione per dimostrare la propria identità ai client PEAP.

Questa guida fornisce istruzioni su come configurare i client wireless e i criteri di rete\(s\) utilizzare PEAP\-MS\-CHAP v2 per 802.1 X accesso autenticato.

### <a name="network-policy-server"></a>Server dei criteri di rete
Server dei criteri di rete \(NPS\) consente di configurare e gestire i criteri di rete utilizzando Remote Authentication Dial in modo centralizzato\-In User Service \(RADIUS\) server e proxy RADIUS. Criteri di RETE è necessaria quando si distribuisce l'accesso wireless 802.1 X.

Quando si configurano i punti di accesso wireless 802.1 X come client RADIUS in Criteri di rete, criteri di rete elabora le richieste di connessione inviate dai punti di accesso. Durante l'elaborazione della richiesta di connessione, i criteri di rete esegue l'autenticazione e autorizzazione. L'autenticazione determina se il client ha presentato le credenziali valide. Se Criteri di rete viene autenticato correttamente il client richiedente, NPS determina se il client è autorizzato a effettuare la connessione richiesta e una consente o Nega la connessione. Ciò è illustrato più dettagliatamente come indicato di seguito:

#### <a name="authentication"></a>Autenticazione

PEAP reciproco corretta\-MS\-autenticazione CHAP v2 costituita da due parti principali:

1.  Il client autentica i criteri di rete.  Durante questa fase di autenticazione reciproca, i criteri di rete invia il certificato del server nel computer client in modo che il client possa verificare l'identità del NPS con il certificato. Per autenticare correttamente i criteri di rete, il computer client deve considerare attendibile la CA che ha emesso il certificato di criteri di rete. Quando il certificato della CA è presente nell'archivio certificati Autorità di certificazione radice attendibili nel computer client, il client considera attendibile la CA.

    Se si distribuisce la CA privata, il certificato della CA viene installato automaticamente nell'archivio certificati Autorità di certificazione radice attendibili per l'utente corrente e per il Computer locale quando viene aggiornato criteri di gruppo nel computer client membri del dominio. Se si decide di distribuire i certificati del server da una CA pubblica, assicurarsi che il certificato di autorità di certificazione pubblica è già nell'archivio certificati Autorità di certificazione radice attendibili.  

2.  I criteri di rete autentica l'utente. Il client viene autenticato correttamente i criteri di rete, il client invia la password dell'utente\-credenziali ai criteri di rete, che consente di verificare le credenziali dell'utente nel database degli account utente in servizi di dominio Active Directory basate su \(AD DS\).

Se le credenziali siano valide e l'autenticazione ha esito positivo, i criteri di rete inizia la fase di autorizzazione dell'elaborazione della richiesta di connessione. Se le credenziali non sono valide e l'autenticazione non riesce, dei criteri di rete invia un messaggio di rifiuto di accesso e la richiesta di connessione viene rifiutata.  

#### <a name="authorization"></a>Authorization

Il server dei criteri di rete esegue l'autorizzazione nel modo seguente:  

1. Verifica dei criteri di rete per le restrizioni di connessione di account utente o computer\-nelle proprietà di dominio Active Directory. Per ogni account utente e computer in Active Directory Users and Computers include più proprietà, inclusi quelli nel **connessione\-in** scheda. In questa scheda in dell'autorizzazione di accesso di rete**, se il valore è **consentire l'accesso**, l'utente o il computer è autorizzato a connettersi alla rete. Se il valore è **negare l'accesso**, l'utente o il computer non è autorizzato a connettersi alla rete. Se il valore è **Controlla accesso tramite criteri di rete NPS**, dei criteri di RETE valuta i criteri di rete configurate per determinare se l'utente o il computer è autorizzato a connettersi alla rete. 

2. Criteri di rete elabora quindi i relativi criteri di rete per trovare un criterio che corrisponde alla richiesta di connessione. Se viene trovato un criterio corrispondente, NPS concede o Nega la connessione in base alla configurazione del criterio.  

Se l'autenticazione e autorizzazione hanno esito positivo, se il criterio di rete corrispondente concede l'accesso, NPS concede l'accesso alla rete e l'utente e computer possono connettersi alle risorse di rete per cui dispongono delle autorizzazioni.  

>[!NOTE]
>Per distribuire l'accesso wireless, è necessario configurare i criteri di criteri di rete. Questa guida vengono fornite istruzioni per l'uso di **Configurazione 802.1 X wizard** in Criteri di RETE per creare criteri di criteri di RETE per l'accesso wireless autenticato 802.1 X.  

### <a name="bootstrap-profiles"></a>Profili di bootstrap
802.1 X\-autenticato reti wireless, i client wireless devono fornire credenziali di sicurezza vengono autenticate da un server RADIUS per potersi connettere alla rete. Per PEAP \[PEAP\]\-Microsoft Challenge Handshake Authentication Protocol versione 2 \[MS\-CHAP v2\], le credenziali di sicurezza sono un nome utente e password. Per l'autenticazione EAP\-Transport Layer Security \[TLS\] o PEAP\-TLS, le credenziali di sicurezza sono certificati, ad esempio le smart card o certificati utente e computer client.

Quando ci si connette a una rete che è configurata per eseguire PEAP\-MS\-CHAP v2, PEAP\-TLS o EAP\-l'autenticazione TLS, per impostazione predefinita, Windows client senza fili inoltre necessario convalidare un certificato del computer che viene inviato dal server RADIUS. Il certificato del computer che viene inviato dal server RADIUS per ogni sessione di autenticazione è noto come certificato server.

Come accennato in precedenza, è possibile eseguire i server RADIUS il proprio certificato server in uno dei due modi: da un'autorità di certificazione commerciale \(ad esempio VeriSign, Inc.,\), o da una CA privata distribuiti nella rete. Se il server RADIUS invia un certificato del computer che è stato rilasciato da un'autorità di certificazione commerciale che dispone già di un certificato installato nell'archivio certificati Autorità di certificazione radice attendibili del client, il client senza fili possa convalidare certificato del computer del server RADIUS, indipendentemente dal fatto che il client senza fili venga aggiunto al dominio di Active Directory. In questo caso il client senza fili può connettersi alla rete wireless e quindi è possibile aggiungere il computer al dominio.

>[!NOTE]
>Il comportamento di richiedere al client convalidare il certificato del server può essere disabilitato, ma la disabilitazione della convalida certificato server non è consigliata in ambienti di produzione.

I profili di bootstrap wireless sono profili temporanei che sono configurati in modo da consentire agli utenti di client wireless per connettersi a 802.1 X\-autenticazione di rete wireless prima che il computer è unito al dominio, e\/o prima che l'utente è connesso al dominio utilizzando un determinato computer wireless per la prima volta.  Questa sezione vengono riepilogati quale problema si verifica quando si tenta di aggiungere un computer wireless al dominio o per un utente di utilizzare un dominio\-unite in join i computer wireless per la prima volta accedere al dominio.

Per le distribuzioni in cui l'utente o un amministratore IT non può connettersi fisicamente un computer alla rete Ethernet cablata per aggiungere il computer al dominio e il computer non è necessario il rilascio radice certificato installato nel relativo **autorità di certificazione radice attendibili** archivio certificati, è possibile configurare i client senza fili con un profilo di connessione wireless temporaneo, denominato un *bootstrap profilo*, per connettersi alla rete wireless.

Oggetto *bootstrap profilo* Elimina la necessità di convalidare il certificato di computer del server RADIUS. Questa configurazione temporanea consente all'utente wireless aggiungere il computer al dominio, a ogni rete Wireless \(IEEE 802.11\) i criteri vengono applicati e il certificato CA radice appropriato viene automaticamente installato nel computer.

Quando viene applicato criteri di gruppo, vengono applicati uno o più profili di connessione wireless che imporre il requisito per l'autenticazione reciproca del computer. il profilo bootstrap non è più necessario e viene rimosso. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può usare una connessione wireless per l'accesso al dominio.

Per una panoramica del processo di distribuzione di accesso wireless tramite tali tecnologie, vedere [Cenni preliminari sulla distribuzione di accesso Wireless](b-wireless-access-deploy-overview.md).
