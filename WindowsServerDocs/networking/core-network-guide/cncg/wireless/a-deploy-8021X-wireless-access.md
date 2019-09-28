---
title: Distribuire l'accesso wireless autenticato 802.1 basato su password
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "distribuire l'accesso wireless autenticato con 802.1 X basato su password"
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90c2c6c4fbd0110724bc3950b11b3b2c09c5c604
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406269"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Distribuire Password\-basato su accesso Wireless autenticato 802.1 X

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Si tratta di una guida complementare a Windows Server&reg; Guida alla rete Core 2016. Nella Guida alla rete core sono disponibili istruzioni per la pianificazione e la distribuzione dei componenti necessari per una rete completamente funzionante e un nuovo Active Directory® dominio in una nuova foresta.

In questa guida viene illustrato come si basano su una rete core fornendo istruzioni su come distribuire Institute of Electrical and Electronics Engineers \(IEEE\) 802.1 X\-accesso wireless IEEE 802.11 mediante Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 autenticato \(PEAP\-MS\-CHAP v2\).

Poiché il protocollo PEAP\-MS\-CHAP v2 richiede l'immissione di password\-credenziali in base al posto di un certificato durante il processo di autenticazione, è in genere più semplice e meno costoso da distribuire EAP\-TLS o PEAP\-TLS.

>[!NOTE]
>In questa Guida, IEEE 802.1 X accesso Wireless autenticato con PEAP\-MS\-CHAP v2 è abbreviato in "accesso senza fili" e "Accesso Wi-Fi".

## <a name="about-this-guide"></a>Informazioni sulla guida
Questa Guida, in combinazione con le guide dei prerequisiti descritti di seguito, vengono fornite istruzioni su come distribuire l'infrastruttura di accesso Wi-Fi seguente.  

- Uno o più 802.1 X\-punti di accesso wireless 802.11 \(APs\).

- Active Directory Domain Services \(utenti e\) computer di servizi di dominio Active Directory.

- Gestione Criteri di gruppo

- Uno o più Server dei criteri di rete \(NPS\) server.

- Certificati del server per i computer che eseguono Server dei criteri di rete.

- Computer client che eseguono Windows wireless&reg; 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dipendenze per questa Guida

Per distribuire correttamente wireless autenticato con questa Guida, è necessario disporre di un ambiente di rete e di dominio con tutte le tecnologie necessarie distribuite. È necessario anche che i certificati server siano distribuiti nel NPSs di autenticazione.

Nelle sezioni seguenti vengono forniscono collegamenti alla documentazione che illustra come distribuire queste tecnologie.

#### <a name="network-and-domain-environment-dependencies"></a>Dipendenze di ambiente di rete e di dominio

Questa guida è destinata agli amministratori di rete e di sistema che hanno seguito le istruzioni nella **Guida alla rete core** di Windows Server 2016 per distribuire una rete core o per coloro che hanno distribuito in precedenza le tecnologie principali incluse nel core rete, inclusi servizi di dominio Active \(directory\), Domain Name System \(DNS\), Dynamic Host Configuration Protocol\/DHCP, IP TCP, server dei criteri di \(rete e Windows Internet Name Service WINS\).  

La guida alla rete core è disponibile nelle posizioni seguenti:

- Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) è disponibile nella libreria tecnica di Windows Server 2016. 

- La [Guida alla rete core](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) è disponibile anche in formato Word nella raccolta TechNet, [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)all'indirizzo.

#### <a name="server-certificate-dependencies"></a>Dipendenze del certificato server
Sono disponibili due opzioni disponibili per registrare i server di autenticazione con certificati server per l'utilizzo con l'autenticazione 802.1 X - distribuire la propria infrastruttura a chiave pubblica utilizzando Servizi certificati Active Directory \(Servizi certificati Active Directory\) o utilizzare i certificati server registrati da un'autorità di certificazione pubblica \(CA\).

##### <a name="ad-cs"></a>SERVIZI CERTIFICATI ACTIVE DIRECTORY
Gli amministratori di rete e di sistema la distribuzione wireless autenticato devono seguire le istruzioni di Windows Server 2016 Core guida complementare alla rete, **distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X**. In questa guida viene illustrato come distribuire e utilizzare Servizi certificati Active Directory per registrare automaticamente i certificati del server nei computer che eseguono NPS.

In questa guida è disponibile nel percorso seguente.

- La Guida complementare alla Core rete di Windows Server 2016 [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) in formato HTML nella raccolta di documentazione tecnica. 

##### <a name="public-ca"></a>CA pubblica
È possibile acquistare i certificati del server da una CA pubblica, ad esempio VeriSign, che i computer client attendibile. 

Un computer client considera attendibile un'autorità di certificazione quando viene installato il certificato della CA nell'archivio certificati Autorità di certificazione radice attendibili. Per impostazione predefinita, i computer che eseguono Windows dispongono di più certificati CA pubblici installati nell'archivio certificati delle autorità di certificazione radice attendibili.  

È consigliabile vedere le guide alla progettazione e alla distribuzione per ognuna delle tecnologie usate in questo scenario di distribuzione. Queste guide consentono di determinare se questo scenario di distribuzione fornisce la configurazione e i servizi necessari per la rete dell'organizzazione.

### <a name="requirements"></a>Requisiti
Di seguito sono riportati i requisiti per la distribuzione di un'infrastruttura di accesso wireless mediante lo scenario descritto in questa guida:

- Prima di distribuire questo scenario, è necessario acquistare 802.1 X\-punti di accesso wireless in grado di fornire una copertura wireless nelle posizioni desiderate nel sito. La sezione pianificazione di questa Guida consente di determinare le funzionalità che dei punti di accesso deve supportare.

- Active Directory Domain Services \(servizi di\) dominio Active Directory è installato, così come le altre tecnologie di rete necessarie, seguendo le istruzioni riportate nella Guida alla rete core di Windows Server 2016.  

- Servizi certificati Active Directory viene distribuito e i certificati del server vengono registrati in NPSs. Questi certificati sono necessari quando si distribuisce PEAP\-MS\-CHAP v2 certificato\-basato su metodo di autenticazione che viene utilizzato in questa Guida.

- Un membro dell'organizzazione abbia familiarità con gli standard IEEE 802.11 supportati da punti di accesso wireless e le schede di rete wireless che vengono installate nei computer client e dispositivi nella rete. Ad esempio, un utente dell'organizzazione abbia familiarità con i tipi frequenza radio, l'autenticazione wireless 802.11 \(WPA o WPA2\), e crittografie \(AES o TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Informazioni non contenute in questa guida  
Di seguito sono riportati alcuni elementi che non sono disponibili in questa guida:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Indicazioni per la selezione di 802.1 X\-punti di accesso wireless in grado di supportare

Poiché esistono molte differenze tra marche e modelli di 802.1 X\-punti di accesso wireless in grado di supportare questa Guida non fornisce informazioni dettagliate su:  

- Determinare quale marchio o modello di AP wireless è più adatto alle proprie esigenze.

- Distribuzione fisica dei punti di accesso wireless nella rete.

- Avanzata configurazione AP senza fili, ad esempio per wireless virtual Local Area Network \(VLAN\).

- Istruzioni su come configurare fornitore AP senza fili\-attributi specifici in Criteri di RETE.

Inoltre, terminologia e i nomi delle impostazioni variano tra i modelli e i marchi AP senza fili e potrebbero non corrispondere i nomi delle impostazioni generiche che vengono utilizzati in questa Guida. Per informazioni dettagliate sulla configurazione dell'AP wireless, è necessario esaminare la documentazione del prodotto fornita dal produttore dei punti di accesso wireless.

### <a name="instructions-for-deploying-nps-certificates"></a>Istruzioni per la distribuzione dei certificati NPS
  
Esistono due alternative per la distribuzione dei certificati NPS. Questa guida non fornisce indicazioni complete che consentono di determinare quale alternativa sarà più adatta alle proprie esigenze. In generale, tuttavia, le scelte da affrontare sono:

- Acquisto di certificati da una CA pubblica, ad esempio VeriSign, che sono già considerato attendibile da Windows\-i client basati su. Questa opzione è in genere consigliata per reti di piccole dimensioni.

- Distribuzione di un'infrastruttura \(a chiave pubblica PKI\) sulla rete tramite Servizi certificati Active Directory. È consigliato per la maggior parte delle reti e le istruzioni per distribuire i certificati server con Servizi certificati Active Directory sono disponibili nella Guida alla distribuzione menzionati in precedenza.

### <a name="nps-network-policies-and-other-nps-settings"></a>Criteri di rete NPS e altre impostazioni server dei criteri di rete

Fatta eccezione per le impostazioni di configurazione effettuate quando si eseguono i **Configurazione 802.1 X** procedura guidata, come descritto in questa Guida, questa Guida non fornisce informazioni dettagliate per configurare manualmente le condizioni dei criteri di RETE, i vincoli o altre impostazioni dei criteri di RETE.

### <a name="dhcp"></a>DHCP

Questa guida alla distribuzione non fornisce informazioni sulla progettazione o la distribuzione di subnet DHCP per le reti LAN wireless.

## <a name="technology-overviews"></a>Panoramiche delle tecnologie
Di seguito sono riportate alcune panoramiche sulla tecnologia per la distribuzione dell'accesso wireless:

### <a name="ieee-8021x"></a>IEEE 802.1X
Lo standard IEEE 802.1 X definisce la porta\-basato su controllo di accesso alla rete che viene utilizzato per fornire l'accesso autenticato alle reti Ethernet. Questa porta\-controllo accesso alla rete in base utilizza le caratteristiche fisiche dell'infrastruttura LAN commutata per autenticare i dispositivi collegati a una porta LAN. L'accesso alla porta può essere negato se il processo di autenticazione non riesce. Sebbene questo standard sia stato progettato per le reti Ethernet cablate, è stato adattato per l'uso in reti LAN wireless 802,11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1 x\-punti \(di accesso wireless in grado di supportare APS\)
Questo scenario richiede la distribuzione di uno o più 802.1 X\-in grado di supportare punti di accesso wireless che sono compatibili con Remote Authentication Dial\-In User Service \(RADIUS\) protocollo.

i punti di distribuzione\-conformi a 802.1 x e RADIUS, quando distribuiti in un'infrastruttura RADIUS con un server RADIUS, ad esempio un server dei criteri di server, sono denominati *client RADIUS*.

### <a name="wireless-clients"></a>Client wireless
In questa guida fornisce i dettagli di configurazione completa per fornire l'accesso autenticato per dominio 802.1 X\-gli utenti che si connettono alla rete con i computer client wireless Windows 10, Windows 8.1 e Windows 8. I computer devono essere aggiunti al dominio per poter stabilire correttamente l'accesso autenticato.

>[!NOTE]
>È inoltre possibile utilizzare i computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come computer client wireless.

### <a name="support-for-ieee-80211-standards"></a>Supporto per gli standard IEEE 802,11
Sistemi operativi Windows e Windows Server forniscono compilato\-il supporto per 802.11 wireless networking. In questi sistemi operativi, una scheda di rete senza fili 802.11 installata viene visualizzata come una connessione di rete wireless in Centro rete e condivisione. 

Anche se è compilato\-il supporto per reti senza fili 802.11, i componenti senza fili di Windows dipendono le operazioni seguenti:

- Funzionalità della scheda di rete wireless. La scheda di rete wireless installata deve supportare la LAN wireless o gli standard di sicurezza wireless richiesti. Ad esempio, se la scheda di rete senza fili non supporta Wi\-Fi Protected Access \(WPA\), non è possibile abilitare o configurare le opzioni di protezione WPA.

- Funzionalità del driver della scheda di rete wireless. Per consentire la configurazione delle opzioni di rete wireless, il driver della scheda di rete wireless deve supportare la creazione di report di tutte le funzionalità di Windows. Verificare che il driver per la scheda di rete è stato scritto per le funzionalità del sistema operativo. Assicurarsi inoltre che il driver è la versione più recente mediante il controllo di Microsoft Update o il sito Web del fornitore scheda rete wireless.

La tabella seguente illustra le frequenze e le frequenze di trasmissione per gli standard wireless IEEE 802,11 comuni.

|Standard|Frequenze|Velocità di trasmissione bit|Utilizzo|
|-------------|---------------|--------------------------|---------|  
|802,11|Gruppo di frequenze \(industriale,scientificoe\) medico ISM da 2,4a2,5GHz\(\-\)|2 megabit al secondo \(Mbps\)|Obsoleta. Usato in genere.|  
|802.11 b|S\-banda ISM|11 Mbps|Utilizzato comunemente.|  
|802.11 a|C\-banda ISM \(5,725 a 5,875 GHz\)|54 Mbps|Non comunemente usato a causa di spese e di un intervallo limitato.|  
|802.11 g|S\-banda ISM|54 Mbps|Ampiamente usato. i dispositivi 802.11 g sono compatibili con i dispositivi 802.11 b.|  
|802.11 n \2.4 e 5,0 GHz|C\-banda e S\-banda ISM|250 Mbps|Dispositivi in base alle fasi pre\-ratifica IEEE 802.11 n standard è diventato disponibile nell'agosto 2007. Molti dispositivi 802.11 n sono compatibili con i dispositivi 802.11 a, b e g.|
|802.11ac |5 GHz |6,93 Gbps |802.11 AC, approvato da IEEE 2014, è più scalabile e più veloce rispetto a 802.11 n e viene distribuito in cui i punti di accesso e i computer client wireless supportano.|

### <a name="wireless-network-security-methods"></a>Metodi di sicurezza della rete wireless
**Metodi di sicurezza di rete wireless** è un raggruppamento informale di autenticazione wireless \(talvolta detta protezione wireless\) e la crittografia di sicurezza wireless. L'autenticazione e la crittografia wireless vengono utilizzate in coppie per impedire a utenti non autorizzati di accedere alla rete wireless e proteggere le trasmissioni wireless. 

Quando si configurano le impostazioni di protezione wireless in Criteri di gruppo di criteri di reti Wireless, sono presenti più combinazioni da selezionare. Tuttavia, solo il WPA2\-Enterprise, WPA\-Enterprise e aperta con 802.1 X standard di autenticazione sono supportati per 802.1 X autenticato le distribuzioni wireless.

>[!NOTE]
>Durante la configurazione dei criteri di rete Wireless, è necessario selezionare **WPA2\-Enterprise**, **WPA\-Enterprise**, o **aperta con 802.1 X** per ottenere l'accesso alle impostazioni EAP sono obbligatori per le distribuzioni wireless con autenticazione 802.1 X.  

#### <a name="wireless-authentication"></a>Autenticazione wireless
In questa guida si consiglia che l'utilizzo di ai seguenti standard di autenticazione wireless per 802.1 X autenticato distribuzioni wireless.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)** WPA è uno standard provvisorio sviluppato da Wi-Fi Alliance per rispettare il protocollo di sicurezza wireless 802.11. Il protocollo WPA è stato sviluppato in risposta a un numero di problemi gravi che sono stati individuati nel precedente Wired Equivalent Privacy \(WEP\) protocollo.

WPA\-Enterprise offre una maggiore sicurezza tramite WEP da:  

1. Richiedere l'autenticazione che usa il Framework 802.1 X EAP come parte dell'infrastruttura che garantisce l'autenticazione reciproca centralizzata e la gestione dinamica delle chiavi  

2. Migliorare il valore di controllo di integrità \(ICV\) con Message Integrity Check \(MIC\), per proteggere l'intestazione e il payload  

3. Implementazione di un contatore frame per scoraggiare gli attacchi di riproduzione  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)** come il WPA\-standard Enterprise, WPA2\-Enterprise utilizza 802.1 X ed EAP framework. WPA2\-Enterprise fornisce maggiore protezione dei dati per gli utenti più grandi e di reti gestite. WPA2\-Enterprise è un protocollo affidabile che è progettato per impedire accessi non autorizzati alla rete verificando gli utenti di rete tramite un server di autenticazione.  

#### <a name="wireless-security-encryption"></a>Crittografia di sicurezza wireless
La crittografia di sicurezza wireless viene usata per proteggere le trasmissioni wireless inviate tra il client wireless e il punto di accesso wireless. La crittografia di sicurezza wireless viene utilizzata insieme al metodo di autenticazione della sicurezza di rete selezionato. Per impostazione predefinita, i computer che eseguono Windows 10, Windows 8.1 e Windows 8 supportano due standard di crittografia:

1. **Protocollo di integrità della chiave temporale** TKIP è un protocollo di crittografia obsoleto originariamente progettato per fornire una crittografia wireless più sicura rispetto a quella fornita dal protocollo WEP\) per la privacy \(equivalente collegato intrinsecamente debole.\) \( TKIP è stato progettato per IEEE 802.11 i gruppo e il Wi attività\-Fi Alliance per sostituire WEP senza richiedere la sostituzione di hardware legacy. TKIP è una suite di algoritmi che incapsula il payload WEP e consente agli utenti di apparecchiature Wi-Fi legacy di eseguire l'aggiornamento a TKIP senza sostituire l'hardware. Analogamente a WEP, TKIP usa l'algoritmo di crittografia del flusso RC4 come base. Il nuovo protocollo, tuttavia, crittografa ogni pacchetto di dati con una chiave di crittografia univoca e le chiavi sono molto più complesse rispetto a quelle di WEP. Anche se TKIP è utile per l'aggiornamento della protezione nei dispositivi meno recenti progettati per l'uso di solo WEP, non risolve tutti i problemi di sicurezza connessi alle LAN wireless e nella maggior parte dei casi non è sufficientemente affidabile per proteggere i dati sensibili o aziendali trasmissioni.  

2. **Advanced Encryption Standard** \(AES\) è il protocollo di crittografia preferito per la crittografia dei dati commerciali e governativi. AES offre un livello più elevato di sicurezza della trasmissione wireless rispetto a TKIP o WEP. A differenza di TKIP e WEP, AES richiede hardware wireless che supporta lo standard AES. AES è un\-crittografia della chiave standard che utilizza tre blocchi, AES\-128, AES\-192 e AES\-256.

In Windows Server 2016, AES seguente\-metodi di crittografia wireless base sono disponibili per la configurazione nelle proprietà del profilo wireless quando si seleziona un metodo di autenticazione di WPA2\-Enterprise, è consigliabile.

1. **AES\-CCMP**. Modalità di crittografia blocco concatenamento messaggio codice protocollo di autenticazione del contatore \(CCMP\) implementa standard 802.11 i è progettato per la crittografia di sicurezza superiore rispetto a quelle fornite da WEP e utilizza le chiavi di crittografia AES a 128 bit.
2. **AES\-GCMP**. Protocollo di modalità contatore Galois \(GCMP\) è supportata da 802.11 AC, è più efficiente AES\-CCMP e fornisce prestazioni migliori per i computer client wireless. GCMP utilizza chiavi di crittografia AES a 256 bit.

> [!IMPORTANT]
> Wired Equivalency Privacy \(WEP\) è lo standard di protezione wireless originale utilizzato per crittografare il traffico di rete. Non è necessario distribuire WEP sulla rete perché esistono ben\-causa di vulnerabilità note in questa forma di protezione non aggiornata.

### <a name="active-directorydoman-services-adds"></a>Active Directory domini servizi \(di dominio Active Directory\)
Active Directory fornisce un database distribuito che archivia e gestisce le informazioni sulle risorse di rete e applicazione\-dati specifici provenienti dalla directory\-applicazioni abilitate. Gli amministratori possono utilizzare Servizi di dominio Active Directory per organizzare gli elementi di una rete, ad esempio utenti, computer e altri dispositivi, in una struttura di contenimento gerarchica. La struttura di contenimento gerarchica include la foresta di Active Directory, domini nella foresta e le unità organizzative \(unità organizzative\) in ogni dominio. Un server che esegue Active Directory viene definito un *controller di dominio*.  

Servizi di dominio Active Directory contiene gli account utente, gli account computer e le proprietà dell'account necessari per IEEE 802.1\-x\-e PEAP MS CHAP v2 per autenticare le credenziali utente e per valutare l'autorizzazione per le connessioni wireless.

### <a name="active-directory-users-and-computers"></a>Utenti e computer di Active Directory
Active Directory utenti e computer è un componente di servizi di dominio Active Directory che contiene gli account che rappresentano entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Oggetto *gruppo di sicurezza* è una raccolta di account utente o computer che gli amministratori possono gestire come singola unità. Account utente e computer appartenenti a un determinato gruppo vengono dette *i membri del gruppo*.  

### <a name="group-policy-management"></a>Gestione Criteri di gruppo
Gestione criteri di gruppo consente di directory\-basato su gestione delle modifiche e configurazione delle impostazioni utente e computer, incluse le informazioni di sicurezza e utente. Usare Criteri di gruppo per definire le configurazioni per gruppi di utenti e computer. Con Criteri di gruppo è possibile specificare le impostazioni per le voci del registro di sistema, sicurezza, installazione software, script, Reindirizzamento cartelle, servizi di installazione remota e manutenzione di Internet Explorer. Le impostazioni di criteri di gruppo creati dall'utente sono contenute in un oggetto Criteri di gruppo \(oggetto Criteri di gruppo\). Associando un oggetto Criteri di gruppo di contenitori di sistema di Active Directory selezionati, siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo a utenti e computer presenti in tali contenitori Active Directory. Per gestire gli oggetti Criteri di gruppo in un'organizzazione, è possibile utilizzare Microsoft Management Console Editor Gestione criteri di gruppo \(MMC\).  

In questa guida vengono fornite istruzioni dettagliate su come specificare le impostazioni della rete senza fili \(IEEE 802.11\) estensione criteri di gestione criteri di gruppo. La rete senza fili \(IEEE 802.11\) criteri configurare dominio\-accesso wireless autenticato con computer client wireless membri con la necessaria connettività e le impostazioni wireless per 802.1 X.

### <a name="server-certificates"></a>Certificati del server
Questo scenario di distribuzione richiede certificati del server per ogni server dei criteri di server che esegue l'autenticazione 802.1 X.  

Un certificato server è un documento digitale comunemente usato per l'autenticazione e per proteggere le informazioni sulle reti aperte. Un certificato associa in modo sicuro una chiave pubblica all'entità che contiene la corrispondente chiave privata. I certificati sono firmati digitalmente dalla CA emittente e possono essere emessi per un utente, un computer o un servizio.  

Un'autorità di certificazione \(CA\) è un'entità responsabile per stabilire e garantire l'autenticità delle chiavi pubbliche appartenenti ai soggetti di \(in genere utenti o computer\) o altre autorità di certificazione. Le attività di un'autorità di certificazione possono includere l'associazione di chiavi pubbliche a nomi distinti tramite certificati firmati, la gestione dei numeri di serie dei certificati e la revoca dei certificati.  

Active Directory servizi \(certificati\) Active Directory è un ruolo del server che rilascia certificati come CA di rete. Un'infrastruttura di certificati ad CS, nota anche come *infrastruttura \(a chiave pubblica\)PKI*, fornisce servizi personalizzabili per l'emissione e la gestione dei certificati per l'azienda.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>PEAP, EAP e PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) estende punto\-a\-protocollo Point \(PPP\) consentendo altri metodi di autenticazione che utilizzano informazioni e credenziali scambi di lunghezza arbitraria. Con l'autenticazione EAP, sia il client di accesso alla rete sia \(l'autenticatore,\) ad esempio il server dei criteri di rete, devono supportare lo stesso tipo EAP affinché venga eseguita correttamente l'autenticazione. Windows Server 2016 include un'infrastruttura EAP, supporta due tipi EAP e la possibilità di passare messaggi EAP a NPSs. Utilizzando EAP, è possibile supportare schemi di autenticazione aggiuntivi, noti come *tipi EAP*. I tipi EAP sono supportati da Windows Server 2016 sono:  

- Transport Layer Security \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>I tipi EAP avanzati \(ad esempio quelle basate sui certificati\) offrono una migliore protezione contro bruta\-forzare informatico di password, attacchi con dizionario e attacchi\-basato su protocolli di autenticazione \(ad esempio il protocollo CHAP o MS\-CHAP versione 1\).

\(PEAPPEAP\) utilizza TLS per creare un canale crittografato tra un client PEAP di autenticazione, ad esempio un computer senza fili e un autenticatore PEAP, ad esempio un server dei criteri di rete o altri server RADIUS. PEAP non specifica un metodo di autenticazione, ma fornisce protezione aggiuntiva per altri protocolli di autenticazione EAP \(ad esempio EAP\-MS\-CHAP v2\) che possono funzionare attraverso il canale crittografato TLS fornito da PEAP. PEAP viene utilizzato come un metodo di autenticazione per i client di accesso che si connettono alla rete dell'organizzazione tramite i seguenti tipi di server di accesso alla rete \(NAS\):

- 802.1 X\-punti di accesso wireless in grado di supportare

- 802.1 X\-in grado di supportare commutatori di autenticazione

- Computer che eseguono Windows Server 2016 e il servizio di accesso remoto \(RAS\) che sono configurati come rete privata virtuale \(VPN\) server, i server DirectAccess o entrambi

- Computer che eseguono Servizi Desktop remoto e Windows Server 2016

PEAP\-MS\-CHAP v2 è più facile da distribuire rispetto a EAP\-TLS perché l'autenticazione utente viene eseguita tramite password\-credenziali basate su \(nome utente e password\), anziché certificati o smart card. Per avere un certificato è necessario solo un server dei criteri di server o altri server RADIUS. Il certificato NPS viene usato dal server dei criteri di server durante il processo di autenticazione per dimostrare la propria identità ai client PEAP.

Questa guida fornisce le istruzioni per configurare i client wireless e i\(server\) dei criteri di\-rete\-per l'uso di PEAP MS CHAP v2 per l'accesso autenticato con 802.1 x.

### <a name="network-policy-server"></a>Server dei criteri di rete
Server dei criteri di rete \(NPS\) consente di configurare e gestire i criteri di rete utilizzando Remote Authentication Dial in modo centralizzato\-In User Service \(RADIUS\) server e proxy RADIUS. Criteri di RETE è necessaria quando si distribuisce l'accesso wireless 802.1 X.

Quando si configurano i punti di accesso wireless 802.1 X come client RADIUS in NPS, NPS elabora le richieste di connessione inviate dagli APs. Durante l'elaborazione della richiesta di connessione, NPS esegue l'autenticazione e l'autorizzazione. L'autenticazione determina se il client ha presentato credenziali valide. Se il server dei criteri di ricerca autentica il client richiedente, NPS determina se il client è autorizzato a effettuare la connessione richiesta e consente o nega la connessione. Questa operazione viene illustrata più dettagliatamente come segue:

#### <a name="authentication"></a>Authentication

PEAP reciproco corretta\-MS\-autenticazione CHAP v2 costituita da due parti principali:

1.  Il client autentica il server dei criteri di server.  Durante questa fase di autenticazione reciproca, il server dei criteri di NPS invia il proprio certificato server al computer client, in modo che il client possa verificare l'identità del server dei criteri di server con il certificato. Per autenticare il server dei criteri di server, il computer client deve considerare attendibile la CA che ha emesso il certificato NPS. Il client considera attendibile questa CA quando il certificato della CA è presente nell'archivio certificati delle autorità di certificazione radice attendibili nel computer client.

    Se si distribuisce una CA privata, il certificato della CA viene installato automaticamente nell'archivio certificati delle autorità di certificazione radice attendibili per l'utente corrente e per il computer locale quando Criteri di gruppo viene aggiornato nel computer client membro di dominio. Se si decide di distribuire i certificati del server da una CA pubblica, verificare che il certificato della CA pubblico si trovi già nell'archivio certificati delle autorità di certificazione radice attendibili.  

2.  Il server dei criteri di server autentica l'utente. Dopo che il client ha autenticato il server dei criteri di servizio\- \(, il client invia le credenziali basate su password dell'utente al server dei criteri di dominio, che verifica le credenziali dell'utente nel database degli account utente in Active Directory dominio Active Directory Servizi\)di dominio Active Directory.

Se le credenziali sono valide e l'autenticazione ha esito positivo, il server dei criteri di server inizia la fase di autorizzazione dell'elaborazione della richiesta di connessione. Se le credenziali non sono valide e l'autenticazione ha esito negativo, NPS Invia un messaggio di rifiuto di accesso e la richiesta di connessione viene negata.  

#### <a name="authorization"></a>Authorization

Il server che esegue NPS esegue l'autorizzazione come indicato di seguito:  

1. NPS controlla la presenza di restrizioni nella connessione\-dell'account utente o computer in proprietà in servizi di dominio Active Directory. Per ogni account utente e computer in Active Directory Users and Computers include più proprietà, inclusi quelli nel **connessione\-in** scheda. In questa scheda in **dell'autorizzazione di accesso di rete**, se il valore è **consentire l'accesso**, l'utente o il computer è autorizzato a connettersi alla rete. Se il valore è **negare l'accesso**, l'utente o il computer non è autorizzato a connettersi alla rete. Se il valore è **Controlla accesso tramite criteri di rete NPS**, dei criteri di RETE valuta i criteri di rete configurate per determinare se l'utente o il computer è autorizzato a connettersi alla rete. 

2. NPS elabora quindi i criteri di rete per trovare i criteri corrispondenti alla richiesta di connessione. Se viene trovato un criterio di corrispondenza, il server dei criteri di accesso concede o nega la connessione in base alla configurazione di tale criterio.  

Se l'autenticazione e l'autorizzazione hanno esito positivo e se il criterio di rete corrispondente concede l'accesso, il server dei criteri di rete concede l'accesso alla rete e l'utente e il computer possono connettersi alle risorse di rete per le quali dispongono delle autorizzazioni.  

>[!NOTE]
>Per distribuire l'accesso wireless, è necessario configurare i criteri NPS. Questa guida vengono fornite istruzioni per l'uso di **Configurazione 802.1 X wizard** in Criteri di RETE per creare criteri di criteri di RETE per l'accesso wireless autenticato 802.1 X.  

### <a name="bootstrap-profiles"></a>Profili bootstrap
802.1 X\-autenticato reti wireless, i client wireless devono fornire credenziali di sicurezza vengono autenticate da un server RADIUS per potersi connettere alla rete. Per \[PEAP\] \[EAP Microsoft Challenge Handshake Authentication Protocol versione 2MS\-CHAPv2\], le credenziali di sicurezza sono un nome utente e una password.\- Per l'autenticazione EAP\-Transport Layer Security \[TLS\] o PEAP\-TLS, le credenziali di sicurezza sono certificati, ad esempio le smart card o certificati utente e computer client.

Quando ci si connette a una rete che è configurata per eseguire PEAP\-MS\-CHAP v2, PEAP\-TLS o EAP\-l'autenticazione TLS, per impostazione predefinita, Windows client senza fili inoltre necessario convalidare un certificato del computer che viene inviato dal server RADIUS. Il certificato del computer inviato dal server RADIUS per ogni sessione di autenticazione viene comunemente definito certificato server.

Come accennato in precedenza, è possibile eseguire i server RADIUS il proprio certificato server in uno dei due modi: da un'autorità di certificazione commerciale \(ad esempio VeriSign, Inc.,\), o da una CA privata distribuiti nella rete. Se il server RADIUS invia un certificato del computer che è stato rilasciato da un'autorità di certificazione commerciale che dispone già di un certificato installato nell'archivio certificati Autorità di certificazione radice attendibili del client, il client senza fili possa convalidare certificato del computer del server RADIUS, indipendentemente dal fatto che il client senza fili venga aggiunto al dominio di Active Directory. In questo caso il client senza fili può connettersi alla rete wireless e quindi è possibile aggiungere il computer al dominio.

>[!NOTE]
>Il comportamento che richiede al client di convalidare il certificato del server può essere disabilitato, ma non è consigliabile disabilitare la convalida del certificato server negli ambienti di produzione.

I profili di bootstrap wireless sono profili temporanei che sono configurati in modo da consentire agli utenti di client wireless per connettersi a 802.1 X\-autenticazione di rete wireless prima che il computer è unito al dominio, e\/o prima che l'utente è connesso al dominio utilizzando un determinato computer wireless per la prima volta.  Questa sezione vengono riepilogati quale problema si verifica quando si tenta di aggiungere un computer wireless al dominio o per un utente di utilizzare un dominio\-unite in join i computer wireless per la prima volta accedere al dominio.

Per le distribuzioni in cui l'utente o un amministratore IT non può connettersi fisicamente un computer alla rete Ethernet cablata per aggiungere il computer al dominio e il computer non è necessario il rilascio radice certificato installato nel relativo **autorità di certificazione radice attendibili** archivio certificati, è possibile configurare i client senza fili con un profilo di connessione wireless temporaneo, denominato un *bootstrap profilo*, per connettersi alla rete wireless.

Oggetto *bootstrap profilo* Elimina la necessità di convalidare il certificato di computer del server RADIUS. Questa configurazione temporanea consente all'utente wireless aggiungere il computer al dominio, a ogni rete Wireless \(IEEE 802.11\) i criteri vengono applicati e il certificato CA radice appropriato viene automaticamente installato nel computer.

Quando viene applicato criteri di gruppo, vengono applicati uno o più profili di connessione wireless che imporre il requisito per l'autenticazione reciproca del computer. il profilo bootstrap non è più necessario e viene rimosso. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può utilizzare una connessione wireless per accedere al dominio.

Per una panoramica del processo di distribuzione di accesso wireless tramite tali tecnologie, vedere [Cenni preliminari sulla distribuzione di accesso Wireless](b-wireless-access-deploy-overview.md).
