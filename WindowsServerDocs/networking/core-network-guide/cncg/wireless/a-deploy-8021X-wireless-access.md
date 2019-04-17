---
title: Distribuzione basata su Password accesso autenticato 802.1 X Wireless
description: Questo argomento fa parte della Guida "Distribuzione basata su Password 802.1 X Authenticated Wireless Access" rete di Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Distribuzione basata su Password\ accesso autenticato 802.1 X Wireless

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Si tratta di una guida complementare a Windows Server&reg; Guida alla rete Core 2016. Guida alla rete Core vengono fornite istruzioni per pianificare e distribuire i componenti necessari per una rete completamente funzionante e un nuovo dominio di Active in una nuova foresta.

Questa guida viene illustrato come si basano su una rete core fornendo istruzioni su come distribuire Institute of Electrical and Electronics Engineers \(IEEE\) 802.1X\-accesso wireless IEEE 802.11 con Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 autenticato \ (v2\ PEAP\-MS\-CHAP).

Poiché PEAP\-MS\-CHAP v2 richiede che gli utenti forniscano le credenziali basate su password\ invece di un certificato durante il processo di autenticazione, è in genere più semplice e meno costoso da distribuire EAP\ TLS o PEAP\-TLS.

>[!NOTE]
>In questa Guida, IEEE 802.1 X accesso Wireless autenticato con PEAP\-MS\-CHAP v2 è abbreviato in "accesso senza fili" e "Accesso Wi-Fi".

## <a name="about-this-guide"></a>Informazioni sulla Guida
Questa Guida, in combinazione con le guide dei prerequisiti descritti di seguito, vengono fornite istruzioni su come distribuire l'infrastruttura di accesso Wi-Fi seguente.  

- Uno o più 802.1X\-in grado di supportare le \(APs\) punti accesso wireless 802.11.

- \(AD DS\) utenti e computer di servizi di dominio Active Directory.

- Gestione criteri di gruppo.

- Uno o più server \(NPS\) Server dei criteri di rete.

- Certificati server per i computer dei criteri di rete.

- Computer client che eseguono Windows wireless&reg; 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dipendenze per questa Guida

Per distribuire correttamente wireless autenticato con questa Guida, è necessario disporre di un ambiente di rete e di dominio con tutte le tecnologie necessarie distribuite. È inoltre certificati server distribuiti ai server dei criteri di rete che esegue l'autenticazione.

Le sezioni seguenti forniscono collegamenti alla documentazione che illustra come distribuire queste tecnologie.

#### <a name="network-and-domain-environment-dependencies"></a>Dipendenze di ambiente di rete e di dominio

Questa guida è progettata per gli amministratori di rete e di sistema che hanno seguito le istruzioni in Windows Server 2016 **Guida alla rete Core** per distribuire una rete core, o per coloro che hanno distribuito in precedenza le tecnologie principali incluse nella rete principale, tra cui servizi di dominio Active Directory, Domain Name System \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP/IP, dei criteri di rete e Windows Internet Name Service \(WINS\).  

Guida alla rete Core è disponibile nei percorsi seguenti:

- Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) è disponibile nella libreria tecnica di Windows Server 2016. 

- Il [Guida alla rete Core](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) è disponibile in formato Word nel Raccolta TechNet, anche in [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dipendenze del certificato server
Sono disponibili due opzioni disponibili per registrare i server di autenticazione con certificati server per l'utilizzo con l'autenticazione 802.1 X - distribuire la propria infrastruttura a chiave pubblica utilizzando Servizi certificati Active Directory \(AD CS\) o utilizzare i certificati server registrati da un'autorità di certificazione pubblica \(CA\).

##### <a name="ad-cs"></a>SERVIZI CERTIFICATI ACTIVE DIRECTORY
Gli amministratori di rete e di sistema distribuzione wireless autenticato devono seguire le istruzioni di Windows Server 2016 Core guida complementare alla rete, **distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X**. Questa guida viene illustrato come distribuire e utilizzare Servizi certificati Active Directory per registrare automaticamente i certificati server nei computer dei criteri di rete.

Questa guida è disponibile nel percorso seguente.

- La Guida complementare alla Core rete di Windows Server 2016 [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) in formato HTML nella raccolta di documentazione tecnica. 

##### <a name="public-ca"></a>CA pubblica
È possibile acquistare i certificati del server da una CA pubblica, ad esempio VeriSign, che i computer client è già attendibile. 

Un computer client considera attendibile un'autorità di certificazione quando viene installato il certificato della CA nell'archivio certificati Autorità di certificazione radice attendibili. Per impostazione predefinita, i computer che eseguono Windows dispongano di più certificati di autorità di certificazione pubblici installati nel certificato autorità di certificazione radice attendibili archiviare.  

È consigliabile consultare le guide alla progettazione e distribuzione per ognuna delle tecnologie utilizzate in questo scenario di distribuzione. Queste guide consentono di determinare se questo scenario di distribuzione fornisce i servizi e le impostazioni di configurazione per la rete dell'organizzazione.

### <a name="requirements"></a>Requisiti
Di seguito sono i requisiti per la distribuzione di un'infrastruttura di accesso wireless con lo scenario descritto in questa guida:

- Prima di distribuire questo scenario, è necessario acquistare 802.1X\-punti di accesso wireless in grado di fornire una copertura wireless nelle posizioni desiderate nel sito. La sezione pianificazione di questa Guida consente di determinare le funzionalità che dei punti di accesso deve supportare.

- Active Directory Domain Services \(AD DS\) è installato, come altre tecnologie di rete necessarie, seguendo le istruzioni nella Guida alla rete Windows Server 2016 Core.  

- Servizi certificati Active Directory viene distribuito e certificati server registrati nei server dei criteri di rete. Questi certificati sono necessari quando si distribuisce il metodo di autenticazione basata su certificate\ v2 PEAP\-MS\-CHAP che viene utilizzato in questa Guida.

- Un membro dell'organizzazione abbia familiarità con gli standard IEEE 802.11 supportati da punti di accesso wireless e le schede di rete wireless che vengono installate nei computer client e i dispositivi della rete. Ad esempio, un utente dell'organizzazione abbia familiarità con i tipi frequenza radio, autenticazione wireless 802.11 \ (WPA2 o WPA\) e crittografie \(AES or TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Cosa non fornisce questa Guida  
Ecco alcuni elementi in questa Guida non fornisce:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Indicazioni per la selezione 802.1X\-punti di accesso wireless in grado di supportare

Poiché esistono molte differenze tra marche e modelli di 802.1X\-punti di accesso wireless in grado di supportare questa Guida non fornisce informazioni dettagliate su:  

- Determinare quali marca o modello di AP senza fili è migliore adatta alle proprie esigenze.

- La distribuzione fisica dei punti di accesso wireless nella rete.

- Avanzata configurazione AP senza fili, ad esempio per wireless \(VLANs\) reti locali virtuali.

- Istruzioni su come configurare gli attributi di vendor\ specifici AP senza fili in Criteri di rete.

Inoltre, terminologia e i nomi delle impostazioni variano tra i modelli e i marchi AP senza fili e potrebbero non corrispondere i nomi delle impostazioni generiche che vengono utilizzati in questa Guida. Per i dettagli di configurazione AP senza fili, è necessario esaminare la documentazione fornita dal produttore di punti di accesso wireless.

### <a name="instructions-for-deploying-nps-server-certificates"></a>Istruzioni per la distribuzione dei certificati del server dei criteri di rete
  
Esistono due alternative per la distribuzione di certificati del server dei criteri di rete. Questa Guida non fornisce indicazioni per aiutarti a determinare quali alternativa più adatta a soddisfare le esigenze. In generale, tuttavia, le opzioni che trovano ad affrontare sono:

- Acquisto di certificati da una CA pubblica, ad esempio VeriSign, che sono già considerato attendibile dai client basati su Windows. In genere, questa opzione è consigliata per le reti di dimensioni inferiori.

- Distribuzione \(PKI\) un'infrastruttura a chiave pubblica nella rete tramite Servizi certificati Active Directory. Questa opzione è consigliata per la maggior parte delle reti e le istruzioni per distribuire i certificati server con Servizi certificati Active Directory sono disponibili nella Guida alla distribuzione menzionati in precedenza.

### <a name="nps-network-policies-and-other-nps-settings"></a>Criteri di criteri di rete e altre impostazioni dei criteri di rete

Fatta eccezione per le impostazioni di configurazione effettuate quando si eseguono i **configurazione 802.1 X** procedura guidata, come descritto in questa Guida, questa Guida non fornisce informazioni dettagliate per configurare manualmente le condizioni dei criteri di rete, i vincoli o altre impostazioni dei criteri di rete.

### <a name="dhcp"></a>DHCP

Questa Guida alla distribuzione non fornisce informazioni sulla progettazione o distribuzione subnet DHCP per LAN wireless.

## <a name="technology-overviews"></a>Panoramiche delle tecnologie
Di seguito sono panoramiche sulla tecnologia per la distribuzione di accesso wireless:

### <a name="ieee-8021x"></a>IEEE 802.1 X
Di standard IEEE 802.1 X definisce il controllo di accesso di rete basata su port\ che viene utilizzato per fornire l'accesso autenticato alle reti Ethernet. Questo controllo di accesso di rete basata su port\ Usa le caratteristiche fisiche dell'infrastruttura LAN commutata per autenticare i dispositivi collegati a una porta LAN. L'accesso alla porta può essere negato se il processo di autenticazione non riesce. Nonostante questo standard sia stato progettato per le reti Ethernet cablate, è stato adattato per l'uso sulle LAN wireless 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-\(APs\) i punti di accesso wireless in grado di supportare
Questo scenario richiede la distribuzione di uno o più 802.1X\-in grado di supportare punti di accesso wireless che sono compatibili con il protocollo \(RADIUS\) Remote Authentication Dial\-In User Service.

802.1 X e i punti di accesso RADIUS\ conforme, quando vengono distribuiti in un'infrastruttura RADIUS con un server RADIUS, ad esempio un server dei criteri di rete, sono denominati *client RADIUS*.

### <a name="wireless-clients"></a>Client wireless
Questa guida fornisce i dettagli di configurazione completa per fornire l'accesso autenticato per gli utenti membri di domain \ che si connettono alla rete con i computer client wireless che eseguono Windows 10, Windows 8.1 e Windows 8 802.1 X. I computer devono essere aggiunti al dominio per stabilire l'accesso autenticato.

>[!NOTE]
>È inoltre possibile utilizzare i computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come computer client wireless.

### <a name="support-for-ieee-80211-standards"></a>Supporto per IEEE 802.11 standard
Sistemi operativi Windows e Windows Server supportati supportano predefinita per la rete wireless 802.11. In questi sistemi operativi, una scheda di rete senza fili 802.11 installata viene visualizzata come una connessione di rete wireless in Centro rete e condivisione. 

Anche se non esiste supporto predefinita per la rete senza fili 802.11, i componenti senza fili di Windows dipendono le operazioni seguenti:

- Le funzionalità della scheda di rete wireless. La scheda di rete wireless installata deve supportare la LAN wireless o standard di sicurezza wireless che è necessario. Ad esempio, se la scheda di rete senza fili non supporta \(WPA\) Wi\-Fi Protected Access, non può abilitare o configurare le opzioni di protezione WPA.

- Le funzionalità del driver della scheda di rete wireless. Per consentire di configurare le opzioni di rete wireless, il driver per scheda di rete wireless deve supportare la segnalazione di tutte le funzionalità a Windows. Verificare che il driver per la scheda di rete è stato scritto per le funzionalità del sistema operativo. Assicurarsi inoltre che il driver è la versione più recente controllando Microsoft Update o il sito Web del fornitore scheda rete wireless.

La tabella seguente mostra le velocità di trasmissione e le frequenze per standard wireless IEEE 802.11 comuni.

|Standard|Frequenze|Velocità di trasmissione in bit|Utilizzo|
|-------------|---------------|--------------------------|---------|  
|802.11|Intervallo di frequenza \(ISM\) S\ banda industriale, scientifico e medici \ (2.4 a 2,5 GHz\)|2 megabit al secondo \(Mbps\)|Obsoleto. Non è in genere utilizzato.|  
|802.11 b|S\ banda ISM|11 Mbps|In genere utilizzato.|  
|802.11|C \ banda ISM \ (5,725 a 5.875 GHz\)|54 Mbps|Generalmente non usato a causa di spesa e un intervallo limitato.|  
|802.11g|S\ banda ISM|54 Mbps|Ampiamente usata. 802.11g dispositivi sono compatibili con 802.11 b dispositivi.|  
|802.11 n \2.4 e 5,0 GHz|C \ fuori banda e S\ banda ISM|250 Mbps|Dispositivi sulla base di pre-ratifica IEEE 802.11 n standard è diventato disponibili nell'agosto 2007. 802.11 n molti dispositivi sono compatibili con 802.11 a, b e c dispositivi.|
|802.11 AC |Da 5 GHz |6,93 GB/s |802.11 AC, approvato da IEEE 2014, è più scalabile e più veloce rispetto a 802.11 n e viene distribuito in punti di accesso e i client wireless supportano.|

### <a name="wireless-network-security-methods"></a>Metodi di sicurezza di rete wireless
**Metodi di sicurezza di rete wireless** è un raggruppamento informale di autenticazione wireless \ (talvolta detta security\ wireless) e la crittografia di sicurezza wireless. Per impedire agli utenti non autorizzati di accedere alla rete wireless e per proteggere le trasmissioni wireless, crittografia e autenticazione wireless vengono utilizzati in coppia. 

Quando si configurano le impostazioni di protezione wireless in Criteri di gruppo di criteri di reti Wireless, sono presenti più combinazioni da selezionare. Tuttavia, solo il WPA2\ Enterprise WPA\-Enterprise e aperta con 802.1 X standard di autenticazione sono supportati per 802.1 X autenticato le distribuzioni wireless.

>[!NOTE]
>Durante la configurazione dei criteri di rete Wireless, è necessario selezionare **WPA2\ Enterprise**, **WPA\ Enterprise**, o **aperta con 802.1 X** per ottenere l'accesso alle impostazioni EAP sono obbligatori per le distribuzioni wireless con autenticazione 802.1 X.  

#### <a name="wireless-authentication"></a>Autenticazione wireless
Questa guida si consiglia che l'uso dei seguenti standard di autenticazione wireless per 802.1 X autenticato distribuzioni wireless.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)** WPA è uno standard provvisorio sviluppato da Wi-Fi Alliance per rispettare il protocollo di sicurezza wireless 802.11. Il protocollo WPA è stato sviluppato in risposta a un numero di problemi gravi che sono stati individuati nel precedente Wired Equivalent Privacy \(WEP\) protocollo.

WPA\ Enterprise offre una maggiore sicurezza tramite WEP da:  

1. Richiesta di autenticazione che utilizza 802.1 X EAP framework come parte dell'infrastruttura che garantisce l'autenticazione reciproca centralizzata e gestione delle chiavi dinamico  

2. Miglioramento \(ICV\) il valore di controllo di integrità con un \(MIC\) Message Integrity Check, per proteggere l'intestazione e il payload  

3. Implementazione di un contatore di frame per impedire gli attacchi di riproduzione  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)** come la WPA\-Enterprise standard, WPA2\ Enterprise utilizza 802.1 X ed EAP framework. WPA2\ Enterprise fornisce maggiore protezione dei dati per più utenti e reti gestite di grandi dimensioni. WPA2\ Enterprise è un protocollo affidabile che è progettato per evitare accessi non autorizzati alla rete verificando gli utenti di rete tramite un server di autenticazione.  

#### <a name="wireless-security-encryption"></a>Crittografia di sicurezza wireless
Crittografia di sicurezza wireless viene usata per proteggere le trasmissioni wireless che vengono inviate tra il client wireless e il punto di accesso wireless. Crittografia di sicurezza wireless viene utilizzata in combinazione con il metodo di autenticazione di sicurezza di rete selezionata. Per impostazione predefinita, i computer che eseguono Windows 10, Windows 8.1 e Windows 8 supportano due standard di crittografia:

1. **Protocollo di integrità chiave temporale** \(TKIP\) è un protocollo di crittografia precedente che è stato originariamente progettato per fornire crittografia wireless più sicura rispetto a quello fornito dal debole Wired Equivalent Privacy \(WEP\) protocollo. TKIP è stato progettato per IEEE 802.11 i gruppo e la Wi\-Fi Alliance per sostituire WEP senza richiedere la sostituzione di hardware legacy di attività. TKIP è una suite di algoritmi che incapsula il payload WEP e consente agli utenti di apparecchiature Wi-Fi legacy per l'aggiornamento a TKIP senza sostituzione di hardware. Come WEP, TKIP Usa l'algoritmo di crittografia RC4 flusso come base. Il nuovo protocollo, tuttavia, crittografa ogni pacchetto di dati con una chiave di crittografia univoca e le chiavi sono molto più sicuro rispetto a quelle da WEP. Anche se è utile per l'aggiornamento di sicurezza nei dispositivi meno recenti che sono stati progettati per usare solo WEP TKIP, non risolvere tutti i problemi di sicurezza affiancate LAN wireless e nella maggior parte dei casi non è sufficientemente potente per proteggere sensibili statale o trasmissione dei dati aziendali.  

2. **Advanced Encryption Standard** \(AES\) è il protocollo di crittografia preferito per la crittografia dei dati di settore commerciale sia governativo. AES offre un livello di sicurezza wireless trasmissione superiore rispetto a TKIP o WEP. A differenza TKIP e WEP, AES richiede hardware wireless che supporta lo standard AES. AES è una crittografia a chiave symmetric\ standard che utilizza tre blocchi, AES\-128, 192-AES\ e AES\-256.

In Windows Server 2016, i metodi di crittografia wireless base AES\ seguenti sono disponibili per la configurazione nelle proprietà del profilo wireless quando si seleziona un metodo di autenticazione di WPA2\-Enterprise, è consigliabile.

1. **AES\-CCMP**. Contatore \(CCMP\) modalità crittografia blocco concatenamento messaggio codice protocollo di autenticazione implementa standard 802.11 i è progettato per la crittografia di sicurezza superiore rispetto a quelle fornite da WEP e utilizza le chiavi di crittografia AES a 128 bit.
2. **AES\ GCMP**. Protocollo di modalità contatore Galois \(GCMP\) è supportata da 802.11 AC, è più efficiente AES\-CCMP e fornisce prestazioni migliori per i client wireless. GCMP utilizza chiavi di crittografia AES a 256 bit.

> [!IMPORTANT]
> Wired Equivalency Privacy \(WEP\) stato originale di sicurezza wireless standard che è stato utilizzato per crittografare il traffico di rete. Non è necessario distribuire WEP sulla rete perché esistono vulnerabilità noto in questa forma di protezione non aggiornata.

### <a name="active-directory-doman-services-ad-ds"></a>\(AD DS\) di servizi di dominio Active Directory
Servizi di dominio Active Directory fornisce un database distribuito che consente di archiviare e gestione informazioni sulle risorse di rete e dati specifici provenienti da applicazioni abilitate directory\. Gli amministratori possono utilizzare servizi di dominio Active Directory per organizzare gli elementi di una rete, ad esempio utenti, computer e altri dispositivi, in una struttura di contenimento gerarchica. La struttura di contenimento gerarchica include la foresta di Active Directory, domini nella foresta e \(OUs\) unità organizzative in ogni dominio. Un server che esegue servizi di dominio Active Directory viene definito un *controller di dominio*.  

Active Directory contiene gli account utente, account computer e proprietà dell'account necessari per IEEE 802.1 X e PEAP\-MS\-CHAP v2 per autenticare le credenziali dell'utente e per valutare l'autorizzazione per le connessioni wireless.

### <a name="active-directory-users-and-computers"></a>Utenti e computer active Directory
Active Directory Users and Computers è un componente di dominio Active Directory che contiene gli account che rappresentano entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Un *gruppo di sicurezza* è un insieme di account utente o computer che gli amministratori possono gestire come singola unità. Account utente e computer appartenenti a un determinato gruppo vengono dette *i membri del gruppo*.  

### <a name="group-policy-management"></a>Gestione criteri di gruppo
Gestione criteri di gruppo consente directory\ gestione basate su modifiche e della configurazione delle impostazioni utente e computer, incluse le informazioni di sicurezza e utente. Utilizzare criteri di gruppo per definire le configurazioni per i gruppi di utenti e computer. Con criteri di gruppo, è possibile specificare le impostazioni per le voci del Registro di sistema, sicurezza, installazione software, script, reindirizzamento cartelle, servizi di installazione remota e manutenzione di Internet Explorer. I criteri di gruppo impostazioni creati sono contenute nei criteri di gruppo dell'oggetto \(GPO\). Associando un oggetto Criteri di gruppo a contenitori di sistema di Active Directory selezionati, siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo a utenti e computer presenti in tali contenitori Active Directory. Per gestire oggetti Criteri di gruppo in un'organizzazione, è possibile utilizzare il \(MMC\) Editor Gestione criteri di gruppo di Microsoft Management Console.  

Questa guida fornisce istruzioni dettagliate su come specificare le impostazioni della rete senza fili \ (IEEE 802.11\) estensione criteri di gestione criteri di gruppo. La rete senza fili \ (IEEE 802.11\) criteri configurare computer client wireless membri di domain \ con la necessaria connettività e le impostazioni wireless per 802.1 X accesso wireless autenticato.

### <a name="server-certificates"></a>Certificati del server
Questo scenario di distribuzione richiede certificati del server per ogni server dei criteri di rete che esegue l'autenticazione 802.1 X.  

Un certificato server è un documento digitale comunemente utilizzato per l'autenticazione e la protezione delle informazioni sulle reti aperte. Un certificato associa in modo sicuro una chiave pubblica all'entità che contiene la chiave privata corrispondente. I certificati firmati digitalmente dall'autorità di certificazione emittente, e che possono essere emessi per un utente, un computer o un servizio.  

Un'autorità di certificazione \(CA\) è un'entità responsabile dello stabilire e garantire l'autenticità delle chiavi pubbliche appartenenti ai soggetti \ (in genere utenti o computer) o ad altre CA. Le attività di un'autorità di certificazione possono includere l'associazione di chiavi pubbliche a nomi distinti attraverso certificati firmati, la gestione di numeri di serie di certificato e la revoca dei certificati.  

Active Directory certificato servizi \(AD CS\) è un ruolo del server che rilascia certificati come CA di rete. Un Servizi certificati Active Directory certificati di infrastruttura, noto anche come un *infrastruttura a chiave pubblica \(PKI\)*, offre servizi personalizzabili per l'emissione e la gestione dei certificati per l'azienda.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP e PEAP PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) estende Point\-da destra-Point Protocol \(PPP\) consentendo altri metodi di autenticazione che utilizzano informazioni e credenziali scambi di lunghezza arbitraria. Con l'autenticazione EAP sia nella rete accesso client e l'autenticatore \ (ad esempio, il server dei criteri di rete) deve supportare lo stesso tipo EAP per l'autenticazione corretta esecuzione. Windows Server 2016 include un'infrastruttura EAP, supporta due tipi EAP e la possibilità di inviare i messaggi EAP al server dei criteri di rete. Utilizzando EAP, è possibile supportare schemi di autenticazione aggiuntivi, noti come *tipi EAP*. I tipi EAP sono supportati da Windows Server 2016 sono:  

- Transport Layer Security \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol versione 2 \ (v2\ MS\-CHAP)

>[!IMPORTANT]
>I tipi EAP avanzati \ (ad esempio quelli basati su certificates\) offrono una migliore protezione contro attacchi di forza brute\, attacchi con dizionario e attacchi rispetto ai protocolli di autenticazione basata su password\ di individuazione delle password \ (ad esempio, CHAP o MS\-CHAP versione 1 \).

Protected EAP \(PEAP\) utilizza TLS per creare un canale crittografato tra un client esegue l'autenticazione PEAP, ad esempio un computer wireless e un autenticatore PEAP, ad esempio un server dei criteri di rete o da altri server RADIUS. PEAP non specifica un metodo di autenticazione, ma fornisce protezione aggiuntiva per altri protocolli di autenticazione EAP \ (ad esempio v2\ EAP\-MS\-CHAP) che possono funzionare attraverso il canale crittografato TLS fornito da PEAP. PEAP viene utilizzato come un metodo di autenticazione per i client di accesso che si connettono alla rete dell'organizzazione tramite i seguenti tipi di rete accesso server \(NASs\):

- 802.1X\-punti di accesso wireless in grado di supportare

- 802.1X\-in grado di supportare commutatori di autenticazione

- Computer che eseguono Windows Server 2016 e \(RAS\) il servizio di accesso remoto che sono configurati come private virtuali rete \(VPN\) Server, i server DirectAccess o entrambi

- Computer che eseguono Windows Server 2016 e Servizi Desktop remoto

PEAP\-MS\-CHAP v2 è più facile da distribuire rispetto a EAP\ TLS perché l'autenticazione utente viene eseguita con le credenziali basate su password\ \ (nome utente e password\), anziché certificati o smart card. Solo i criteri di rete o altri server RADIUS devono disporre di un certificato. Il certificato del server dei criteri di rete viene utilizzato dal server dei criteri di rete durante il processo di autenticazione per dimostrare la propria identità ai client PEAP.

Questa guida fornisce istruzioni per configurare i client wireless e le server\(s\) dei criteri di rete per l'utilizzo PEAP\-MS\-CHAP v2 per l'accesso autenticato 802.1 X.

### <a name="network-policy-server"></a>Server dei criteri di rete
Rete Server dei criteri \(NPS\) consente di configurare e gestire criteri di rete utilizzando Remote Authentication Dial\-In utente Service \(RADIUS\) server e proxy RADIUS centralmente. Criteri di rete è necessario quando si distribuisce l'accesso wireless 802.1 X.

Quando si configurano i punti di accesso wireless 802.1 X come client RADIUS in Criteri di rete, dei criteri di rete elabora le richieste di connessione inviate dai punti di accesso. Durante l'elaborazione della richiesta di connessione, dei criteri di rete esegue l'autenticazione e autorizzazione. Autenticazione determina se il client ha presentato credenziali valide. Se i criteri di rete esegue l'autenticazione client richiedente, dei criteri di rete determina se il client è autorizzato per effettuare la connessione a richiesta, e consente o rifiuta la connessione. Questo aspetto verrà spiegato in dettaglio come indicato di seguito:

#### <a name="authentication"></a>Autenticazione

Ha esito positivo PEAP\-MS\-CHAP v2 l'autenticazione reciproca ha due parti principali:

1.  Il client autentica il server dei criteri di rete.  Durante questa fase di autenticazione reciproca, il server dei criteri di rete invia il certificato del server per il computer client in modo che il client può verificare l'identità del server dei criteri di rete con il certificato. Per autenticare correttamente il server dei criteri di rete, il computer client deve considerare attendibile la CA che ha emesso il certificato del server dei criteri di rete. Quando il certificato della CA è presente nell'archivio certificati Autorità di certificazione radice attendibili nel computer client, il client considera attendibile la CA.

    Se si distribuisce la CA privata, il certificato della CA viene installato automaticamente nell'archivio certificati Autorità di certificazione radice attendibili per l'utente corrente e per il Computer locale all'aggiornamento di criteri di gruppo nel computer client membro di dominio. Se si decide di distribuire i certificati server da una CA pubblica, assicurarsi che il certificato della CA pubblico è già nell'archivio certificati Autorità di certificazione radice attendibili.  

2.  Il server dei criteri di rete autentica l'utente. Dopo che il client viene autenticato correttamente il server dei criteri di rete, il client invia le credenziali dell'utente in base password\ al server dei criteri di rete, che consente di verificare le credenziali dell'utente nel database di account utente in servizi di dominio Active Directory \(AD DS\).

Se le credenziali sono valide e l'autenticazione ha esito positivo, il server dei criteri di rete viene avviata la fase di autorizzazione dell'elaborazione della richiesta di connessione. Se le credenziali non sono valide e l'autenticazione non riesce, dei criteri di rete invia un messaggio di rifiutare di accesso e la richiesta di connessione viene negata.  

#### <a name="authorization"></a>Autorizzazione

Il server dei criteri di rete esegue l'autorizzazione come indicato di seguito:  

1. Criteri di rete controlla per le restrizioni di utente o computer dial\ proprietà dell'account di dominio Active Directory. Per ogni account utente e computer in Active Directory Users and Computers include più proprietà, inclusi quelli nel **Dial\ aggiuntivo** scheda. In questa scheda in **autorizzazione di accesso alla rete**, se il valore è **consentire l'accesso**, l'utente o il computer è autorizzato a connettersi alla rete. Se il valore è **negare l'accesso**, l'utente o il computer non è autorizzato a connettersi alla rete. Se il valore è **controllare l'accesso tramite criteri di rete NPS**, dei criteri di rete valuta i criteri di rete configurate per determinare se l'utente o il computer è autorizzato a connettersi alla rete. 

2. Quindi, dei criteri di rete elabora i criteri di rete per trovare un criterio che corrisponde alla richiesta di connessione. Se viene trovato un criterio corrispondente, dei criteri di rete concede o rifiuta la connessione in base alla configurazione del criterio.  

Se ha esito positivo sia l'autenticazione e autorizzazione, se il criterio di rete corrispondente concede l'accesso, dei criteri di rete consente l'accesso alla rete e l'utente e computer possono connettersi alle risorse di rete per cui dispongono delle autorizzazioni.  

>[!NOTE]
>Per distribuire accesso senza fili, è necessario configurare i criteri di criteri di rete. Questa guida fornisce istruzioni per l'uso di **configurazione 802.1 X wizard** in Criteri di rete per creare criteri di criteri di accesso wireless autenticato con 802.1 X.  

### <a name="bootstrap-profiles"></a>Profili di bootstrap
In 802.1X\-autenticato reti wireless, i client wireless devono fornire le credenziali di sicurezza vengono autenticate da un server RADIUS per potersi connettere alla rete. Per PEAP \[PEAP\]\-Microsoft Challenge Handshake Authentication Protocol versione 2 \[MS\-CHAP v2\], le credenziali di sicurezza sono un nome utente e password. Per \[TLS\ EAP\ Transport Layer Security] o PEAP\-TLS, le credenziali di sicurezza sono certificati, ad esempio i certificati utente e computer client o smart card.

Durante la connessione a una rete che è configurata per eseguire PEAP\-MS\-CHAP v2, PEAP\ TLS o autenticazione EAP\-TLS, per impostazione predefinita, i client wireless Windows inoltre necessario convalidare un certificato del computer che viene inviato dal server RADIUS. Il certificato del computer che viene inviato dal server RADIUS per ogni sessione di autenticazione è comunemente noti come certificato server.

Come accennato in precedenza, è possibile emettere i server RADIUS il proprio certificato server in uno dei due modi: da una CA commerciale \ (ad esempio VeriSign, Inc., \), o da una CA privata distribuiti nella rete. Se il server RADIUS invia un certificato del computer che è stato emesso da un'autorità di certificazione commerciale che dispone già di un certificato installato nell'archivio certificati di autorità di certificazione radice attendibili del client, il client senza fili può convalidare certificato del computer del server RADIUS, indipendentemente dal fatto che il client senza fili venga aggiunto al dominio di Active Directory. In questo caso il client senza fili può connettersi alla rete wireless e quindi è possibile aggiungere il computer al dominio.

>[!NOTE]
>Il comportamento che richiedono il client convalidare il certificato del server può essere disabilitato, ma la disattivazione di convalida del certificato server non è consigliata in ambienti di produzione.

I profili di bootstrap wireless sono profili temporanei che sono configurati in modo tale da consentire agli utenti di client wireless per connettersi al 802.1X\-rete wireless autenticato prima che il computer è unito al dominio, and\ / o prima che l'utente è connesso al dominio utilizzando un determinato computer wireless per la prima volta.  Questa sezione vengono riepilogati quale problema si verifica quando si tenta di aggiungere un computer wireless al dominio o per un utente di utilizzare un computer appartenente a un domain \ wireless per la prima volta per accedere al dominio.

Per le distribuzioni in cui l'utente o un amministratore IT non può connettersi fisicamente un computer alla rete Ethernet cablata per aggiungere il computer al dominio e il computer non è necessario emittente radice certificato installato nel relativo **autorità di certificazione radice attendibili** archivio dei certificati, è possibile configurare i client senza fili con un profilo di connessione wireless temporaneo, denominato un *bootstrap profilo* , per connettersi alla rete wireless.

Un *bootstrap profilo* Elimina la necessità di convalidare il certificato di computer del server RADIUS. Questa configurazione temporanea consente all'utente wireless aggiungere il computer al dominio, a ogni rete Wireless \ (IEEE 802.11\) i criteri vengono applicati e il certificato CA radice appropriato viene automaticamente installato nel computer.

Quando viene applicato criteri di gruppo, vengono applicati uno o più profili di connessione wireless che imporre il requisito per l'autenticazione reciproca del computer. il profilo di bootstrap non è più necessario e verrà rimosso. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può usare una connessione wireless per accedere al dominio.

Per una panoramica del processo di distribuzione di accesso wireless tramite tali tecnologie, vedere [Cenni preliminari sulla distribuzione di accesso Wireless](b-wireless-access-deploy-overview.md).
