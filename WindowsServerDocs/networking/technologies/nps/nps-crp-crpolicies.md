---
title: Criteri di richiesta di connessione
description: Questo argomento fornisce una panoramica dei criteri di richiesta di connessione del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b373e496567f414657b380bad952baefcbe4b18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396363"
---
# <a name="connection-request-policies"></a>Criteri di richiesta di connessione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come utilizzare i criteri di richiesta di connessione NPS per configurare server dei criteri di accesso come server RADIUS, proxy RADIUS o entrambi.

>[!NOTE]
>Oltre a questo argomento, è disponibile la documentazione seguente relativa ai criteri di richiesta di connessione.
> - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
> - [Configurare gruppi di server RADIUS remoti](nps-crp-rrsg-configure.md)

I criteri di richiesta di connessione sono set di condizioni e impostazioni che consentono agli amministratori di rete di designare i server Remote Authentication Dial-In User Service (RADIUS) che eseguono l'autenticazione e l'autorizzazione delle richieste di connessione il server che esegue Server dei criteri di rete riceve dai client RADIUS. I criteri di richiesta di connessione possono essere configurati in modo da designare i server RADIUS usati per l'accounting RADIUS.

È possibile creare criteri di richiesta di connessione in modo che alcuni messaggi di richiesta RADIUS inviati dai client RADIUS vengano elaborati localmente (NPS viene usato come server RADIUS) e altri tipi di messaggi vengono inoltrati a un altro server RADIUS (NPS viene usato come proxy RADIUS).

Con i criteri di richiesta di connessione è possibile usare NPS come server RADIUS o come proxy RADIUS, in base a fattori come i seguenti: 

- Ora del giorno e giorno della settimana
- Nome dell'area di autenticazione nella richiesta di connessione
- Tipo di connessione richiesta
- Indirizzo IP del client RADIUS

Accesso RADIUS: i messaggi di richiesta vengono elaborati o inoltrati da NPS solo se le impostazioni del messaggio in arrivo corrispondono ad almeno uno dei criteri di richiesta di connessione configurati nel server dei criteri di accesso. 

Se le impostazioni dei criteri corrispondono e il criterio richiede che il server dei criteri di ricerca elabori il messaggio, NPS funge da server RADIUS, autenticando e autorizzando la richiesta di connessione. Se le impostazioni dei criteri corrispondono e il criterio richiede che NPS inoltri il messaggio, NPS funge da proxy RADIUS e Invia la richiesta di connessione a un server RADIUS remoto per l'elaborazione.

Se le impostazioni di un messaggio di richiesta di accesso RADIUS in ingresso non corrispondono ad almeno uno dei criteri di richiesta di connessione, viene inviato un messaggio di rifiuto di accesso al client RADIUS e l'utente o il computer che tenta di connettersi alla rete viene negato l'accesso.

## <a name="configuration-examples"></a>Esempi di configurazione

Gli esempi di configurazione seguenti illustrano come è possibile usare i criteri di richiesta di connessione.

### <a name="nps-as-a-radius-server"></a>NPS come server RADIUS

Il criterio di richiesta di connessione predefinito è l'unico criterio configurato. In questo esempio, NPS viene configurato come server RADIUS e tutte le richieste di connessione vengono elaborate dal server dei criteri di accesso locale. NPS è in grado di autenticare e autorizzare gli utenti i cui account si trovano nel dominio del dominio NPS e nei domini trusted.


### <a name="nps-as-a-radius-proxy"></a>Server dei criteri di rete come proxy RADIUS

Il criterio di richiesta di connessione predefinito viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per l'invio delle richieste a due domini diversi. In questo esempio, NPS viene configurato come proxy RADIUS. NPS non elabora le richieste di connessione nel server locale. Vengono invece inviate le richieste di connessione a server dei criteri di gruppo o altri server RADIUS configurati come membri di gruppi di server RADIUS remoti.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS come server RADIUS e proxy RADIUS

Oltre ai criteri di richiesta di connessione predefiniti, viene creato un nuovo criterio di richiesta di connessione che consente di inviare le richieste di connessione a un server dei criteri di dominio o un altro server RADIUS in un dominio non trusted. In questo esempio il criterio proxy viene visualizzato per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione corrisponde al criterio proxy, la richiesta di connessione viene trasmessa al server RADIUS nel gruppo di server RADIUS remoti. Se la richiesta di connessione non corrisponde ai criteri del proxy ma corrisponde ai criteri di richiesta di connessione predefiniti, NPS elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a uno dei due criteri, viene ignorata.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>Server dei criteri di server come server RADIUS con server di contabilità remoto

In questo esempio, il server dei criteri di gruppo locale non è configurato per eseguire l'accounting e i criteri di richiesta di connessione predefiniti vengono rivisti in modo che i messaggi di accounting RADIUS vengano inviati a un server dei criteri di gruppo o un altro server RADIUS in un gruppo di server RADIUS remoto Sebbene i messaggi di contabilità vengano inviati, i messaggi di autenticazione e autorizzazione non vengono inviati e il server dei criteri di dominio locale esegue queste funzioni per il dominio locale e per tutti i domini trusted.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>Server dei criteri di server con RADIUS remoto al mapping degli utenti di Windows

In questo esempio, NPS funge sia da server RADIUS sia come proxy RADIUS per ogni singola richiesta di connessione tramite l'inoltro della richiesta di autenticazione a un server RADIUS remoto utilizzando un account utente di Windows locale per l'autorizzazione. Questa configurazione viene implementata configurando il raggio remoto in attributo mapping utenti Windows come condizione dei criteri di richiesta di connessione. Inoltre, è necessario creare un account utente localmente con lo stesso nome dell'account utente remoto rispetto al quale viene eseguita l'autenticazione da parte del server RADIUS remoto.

## <a name="connection-request-policy-conditions"></a>Condizioni dei criteri di richiesta di connessione

Le condizioni dei criteri di richiesta di connessione sono costituite da uno o più attributi RADIUS confrontati con gli attributi del messaggio di richiesta di accesso RADIUS in ingresso. Se sono presenti più condizioni, è necessario che tutte le condizioni nel messaggio di richiesta di connessione e nei criteri di richiesta di connessione corrispondano affinché il criterio venga applicato da NPS.


Di seguito sono riportati gli attributi di condizione disponibili che è possibile configurare nei criteri di richiesta di connessione.

### <a name="connection-properties-attribute-group"></a>Gruppo di attributi Proprietà connessione

Il gruppo di attributi Proprietà connessione contiene gli attributi seguenti.

- **Protocollo con frame**. Utilizzato per definire il tipo di frame per i pacchetti in ingresso. Esempi sono Point-to-Point Protocol (PPP), Serial Line Internet Protocol (SLIP), frame relay e X. 25.
- **Tipo di servizio**. Utilizzato per designare il tipo di servizio richiesto. Gli esempi includono i frame, ad esempio le connessioni PPP, e l'account di accesso, ad esempio le connessioni Telnet. Per ulteriori informazioni sui tipi di servizio RADIUS, vedere RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo di tunnel**. Utilizzato per designare il tipo di tunnel creato dal client richiedente. I tipi di tunnel includono il protocollo PPTP (Point-to-Point Tunneling Protocol) e il protocollo L2TP (Layer Two Tunneling Protocol).

### <a name="day-and-time-restrictions-attribute-group"></a>Gruppo di attributi restrizioni per giorno e ora 

Il gruppo di attributi restrizioni relative a giorno e ora contiene l'attributo delle restrizioni relative al giorno e all'ora. Con questo attributo è possibile designare il giorno della settimana e l'ora del giorno del tentativo di connessione. Il giorno e l'ora sono relativi al giorno e all'ora del server dei criteri di server.

### <a name="gateway-attribute-group"></a>Gruppo di attributi del gateway

Il gruppo di attributi del gateway contiene gli attributi seguenti.

- **ID stazione chiamato**. Utilizzato per designare il numero di telefono del server di accesso alla rete. Questo attributo è una stringa di caratteri. Per specificare i codici di area, è possibile usare la sintassi di corrispondenza del criterio.
- **Identificatore NAS**. Utilizzato per designare il nome del server di accesso alla rete. Questo attributo è una stringa di caratteri. Per specificare gli identificatori NAS, è possibile usare la sintassi di criteri di ricerca.
- **Indirizzo IPv4 NAS**. Consente di specificare l'indirizzo IP versione 4 (IPv4) del server di accesso alla rete (client RADIUS). Questo attributo è una stringa di caratteri. Per specificare le reti IP, è possibile usare la sintassi di criteri di ricerca.
- **Indirizzo IPv6 NAS**. Utilizzato per definire l'indirizzo IPv6 (Internet Protocol Version 6) del server di accesso alla rete (client RADIUS). Questo attributo è una stringa di caratteri. Per specificare le reti IP, è possibile usare la sintassi di criteri di ricerca.
- **Tipo porta NAS**. Utilizzato per designare il tipo di supporto utilizzato dal client di accesso. Gli esempi sono linee telefoniche analogiche (note come Async), Integrated Services Digital Network (ISDN), tunnel o reti private virtuali (VPN), IEEE 802,11 wireless e commutatori Ethernet.

### <a name="machine-identity-attribute-group"></a>Gruppo di attributi di identità del computer

Il gruppo di attributi Identity del computer contiene l'attributo Identity del computer. Utilizzando questo attributo, è possibile specificare il metodo con cui i client vengono identificati nei criteri.

### <a name="radius-client-properties-attribute-group"></a>Gruppo di attributi delle proprietà del client RADIUS

Il gruppo di attributi delle proprietà del client RADIUS contiene gli attributi seguenti.

- **ID stazione chiamante**. Utilizzato per designare il numero di telefono utilizzato dal chiamante (il client di accesso). Questo attributo è una stringa di caratteri. Per specificare i codici di area, è possibile usare la sintassi di corrispondenza del criterio.  Nelle autenticazioni 802.1 x l'indirizzo MAC viene in genere popolato e può essere associato al client.  Questo campo viene in genere usato per gli scenari di bypass degli indirizzi Mac quando i criteri di richiesta di connessione sono configurati per "accetta utenti senza convalidare le credenziali".  
- **Nome descrittivo del client**. Utilizzato per designare il nome del computer client RADIUS che richiede l'autenticazione. Questo attributo è una stringa di caratteri. Per specificare i nomi dei client, è possibile usare la sintassi di corrispondenza dei modelli.
- **Indirizzo IPv4 del client**. Consente di specificare l'indirizzo IPv4 del server di accesso alla rete (client RADIUS). Questo attributo è una stringa di caratteri. Per specificare le reti IP, è possibile usare la sintassi di criteri di ricerca.
- **Indirizzo IPv6 client**. Consente di specificare l'indirizzo IPv6 del server di accesso alla rete (client RADIUS). Questo attributo è una stringa di caratteri. Per specificare le reti IP, è possibile usare la sintassi di criteri di ricerca.
- **Fornitore client**. Utilizzato per designare il fornitore del server di accesso alla rete che richiede l'autenticazione. Un computer che esegue il servizio Routing e accesso remoto è il produttore del NAS Microsoft. È possibile utilizzare questo attributo per configurare criteri distinti per i diversi produttori di NAS. Questo attributo è una stringa di caratteri. È possibile usare la sintassi di corrispondenza del criterio.

### <a name="user-name-attribute-group"></a>Gruppo di attributi nome utente

Il gruppo di attributi nome utente contiene l'attributo nome utente. Utilizzando questo attributo, è possibile designare il nome utente o una parte del nome utente che deve corrispondere al nome utente fornito dal client di accesso nel messaggio RADIUS. Questo attributo è una stringa di caratteri che in genere contiene un nome dell'area di autenticazione e un nome di account utente. È possibile utilizzare la sintassi dei criteri di ricerca per specificare i nomi utente.

## <a name="connection-request-policy-settings"></a>Impostazioni dei criteri di richiesta di connessione

Le impostazioni dei criteri di richiesta di connessione sono un set di proprietà applicate a un messaggio RADIUS in ingresso. Le impostazioni sono costituite dai gruppi di proprietà seguenti.

- Authentication
- Contabilità
- Manipolazione degli attributi
- Richiesta di invio
- Avanzate

Le sezioni seguenti forniscono ulteriori dettagli su queste impostazioni.

### <a name="authentication"></a>Authentication

Utilizzando questa impostazione, è possibile ignorare le impostazioni di autenticazione configurate in tutti i criteri di rete ed è possibile designare i metodi e i tipi di autenticazione necessari per la connessione alla rete.

>[!IMPORTANT]
>Se si configura un metodo di autenticazione nei criteri di richiesta di connessione che è meno sicuro rispetto al metodo di autenticazione configurato nei criteri di rete, viene eseguito l'override del metodo di autenticazione più sicuro configurato nei criteri di rete. Ad esempio, se si dispone di un criterio di rete che richiede l'uso di Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 \(PEAP-MS-CHAP v2\), che è un metodo di autenticazione basato su password per Secure wireless e si configura anche un criterio di richiesta di connessione per consentire l'accesso non autenticato, il risultato è che nessun client deve eseguire l'autenticazione tramite PEAP-MS-CHAP v2 In questo esempio, a tutti i client che si connettono alla rete viene concesso l'accesso non autenticato.

### <a name="accounting"></a>Contabilità

Utilizzando questa impostazione, è possibile configurare i criteri di richiesta di connessione per l'invio di informazioni di contabilità a un server dei criteri di gruppo o un altro server RADIUS in un gruppo di server RADIUS remoti, in modo che il gruppo di server RADIUS remoto esegua

>[!NOTE]
>Se si dispone di più server RADIUS e si desiderano informazioni di contabilità per tutti i server archiviati in un database di contabilità RADIUS centrale, è possibile utilizzare l'impostazione di contabilità dei criteri di richiesta di connessione in un criterio in ogni server RADIUS per l'invio di dati di contabilità da tutti i server in un server dei criteri di server o in un altro server RADIUS designato come server di contabilità.

Le impostazioni di contabilità dei criteri di richiesta di connessione sono indipendenti dalla configurazione di contabilità del server dei criteri di accesso locale. In altre parole, se si configura il server dei criteri di accesso locale per registrare le informazioni di accounting RADIUS in un file locale o in un database di Microsoft SQL Server, questa operazione viene eseguita indipendentemente dal fatto che si configuri un criterio di richiesta di connessione per l'invio di messaggi di contabilità a un raggio remoto gruppo di server.

Se si desidera che le informazioni di contabilità siano registrate in modalità remota ma non localmente, è necessario configurare il server dei criteri di gruppo locale in modo da non eseguire l'accounting, configurando anche l'accounting in un criterio di richiesta di connessione per l'invio dei dati di contabilità a un gruppo

### <a name="attribute-manipulation"></a>Manipolazione degli attributi

È possibile configurare un set di regole di ricerca e sostituzione che consentono di modificare le stringhe di testo di uno degli attributi seguenti.

- Nome utente
- ID stazione chiamato
- ID stazione chiamante

L'elaborazione della regola trova e Sostituisci viene eseguita per uno degli attributi precedenti prima che il messaggio RADIUS sia soggetto a impostazioni di autenticazione e accounting. Le regole di manipolazione degli attributi si applicano solo a un singolo attributo. Non è possibile configurare le regole di manipolazione degli attributi per ogni attributo. Inoltre, l'elenco degli attributi che è possibile modificare è un elenco statico; non è possibile aggiungere all'elenco di attributi disponibili per la manipolazione.

>[!NOTE]
>Se si usa il protocollo di autenticazione MS-CHAP v2, non è possibile modificare l'attributo del nome utente se il criterio di richiesta di connessione viene usato per l'invio del messaggio RADIUS. L'unica eccezione si verifica quando viene utilizzata una barra rovesciata (\) carattere e la manipolazione influiscono solo sulle informazioni a sinistra. Viene in genere utilizzato un carattere barra rovesciata per indicare un nome di dominio (le informazioni a sinistra del carattere barra rovesciata) e un nome di account utente all'interno del dominio (le informazioni a destra del carattere barra rovesciata). In questo caso, sono consentite solo le regole di manipolazione degli attributi che modificano o sostituiscono il nome di dominio.

Per esempi di come modificare il nome dell'area di autenticazione nell'attributo nome utente, vedere la sezione "esempi per la manipolazione del nome dell'area di autenticazione nell'attributo nome utente" nell'argomento [usare le espressioni regolari in NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Richiesta di invio

È possibile impostare le opzioni di richiesta di invio seguenti che vengono usate per i messaggi di richiesta di accesso RADIUS:

- **Autentica le richieste in questo server**. Utilizzando questa impostazione, NPS utilizza un dominio di Windows NT 4,0, Active Directory o il database degli account utente di gestione degli account di protezione (SAM) locale per autenticare la richiesta di connessione. Questa impostazione specifica inoltre che i criteri di rete corrispondenti configurati in NPS, insieme alle proprietà di connessione dell'account utente, vengono utilizzati da server dei criteri di rete per autorizzare la richiesta di connessione. In questo caso, il server dei criteri di server è configurato per essere eseguito come server RADIUS.

- **Inviare le richieste al gruppo di server RADIUS remoto seguente**. Utilizzando questa impostazione, NPS Invia le richieste di connessione al gruppo di server RADIUS remoti specificato. Se il server dei criteri di accesso riceve un messaggio di accettazione accesso valido che corrisponde al messaggio di richiesta di accesso, il tentativo di connessione viene considerato autenticato e autorizzato. In questo caso, il server dei criteri di server funge da proxy RADIUS.

- **Accetta utenti senza convalidare le credenziali**. Utilizzando questa impostazione, NPS non verifica l'identità dell'utente che tenta di connettersi alla rete e il server dei criteri di rete non tenta di verificare che l'utente o il computer disponga del diritto di connettersi alla rete. Quando NPS è configurato per consentire l'accesso non autenticato e riceve una richiesta di connessione, il server dei criteri di rete invia immediatamente un messaggio di accettazione dell'accesso al client RADIUS e all'utente o al computer viene concesso l'accesso alla rete. Questa impostazione viene usata per alcuni tipi di tunneling obbligatorio in cui il client di Access viene sottoposto a tunneling prima dell'autenticazione delle credenziali utente.

>[!NOTE]
>Non è possibile usare questa opzione di autenticazione quando il protocollo di autenticazione del client di accesso è MS-CHAP v2 o Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), che forniscono entrambi l'autenticazione reciproca. Nell'autenticazione reciproca il client di accesso dimostra che si tratta di un client di accesso valido al server di autenticazione (NPS) e il server di autenticazione dimostra che si tratta di un server di autenticazione valido per il client di accesso. Quando si utilizza questa opzione di autenticazione, viene restituito il messaggio di accesso-accettazione. Tuttavia, il server di autenticazione non fornisce la convalida per il client di accesso e l'autenticazione reciproca non riesce.

Per esempi relativi all'uso di espressioni regolari per creare regole di routing che inoltrino messaggi RADIUS con un nome dell'area di autenticazione specificato a un gruppo di server RADIUS remoti, vedere la sezione "esempio per l'inoltro dei messaggi RADIUS da un server proxy" nell'argomento [usare le espressioni regolari in NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzate

È possibile impostare proprietà avanzate per specificare la serie di attributi RADIUS che sono:

- Aggiunta al messaggio di risposta RADIUS quando il server dei criteri di server viene utilizzato come server di autenticazione o di accounting RADIUS. Quando sono specificati attributi in entrambi i criteri di rete e i criteri di richiesta di connessione, gli attributi inviati nel messaggio di risposta RADIUS sono la combinazione dei due set di attributi.
- Aggiunta al messaggio RADIUS quando il server dei criteri di server viene usato come proxy di autenticazione o di accounting RADIUS. Se l'attributo esiste già nel messaggio che viene inviato, viene sostituito con il valore dell'attributo specificato nei criteri di richiesta di connessione. 

Inoltre, alcuni attributi disponibili per la configurazione nella scheda **Impostazioni** criterio richiesta di connessione nella categoria **Avanzate** forniscono funzionalità specializzate. Ad esempio, è possibile configurare il **raggio remoto in attributo mapping utenti di Windows** quando si desidera suddividere l'autenticazione e l'autorizzazione di una richiesta di connessione tra due database di account utente.

L'attributo **RADIUS remoto per il mapping utente di Windows** specifica che si verifica l'autorizzazione di Windows per gli utenti autenticati da un server RADIUS remoto. In altre parole, un server RADIUS remoto esegue l'autenticazione con un account utente in un database di account utente remoto, ma il server dei criteri di accesso locale autorizza la richiesta di connessione a un account utente in un database degli account utente locale. Questa operazione è utile quando si desidera consentire ai visitatori di accedere alla rete.

Ad esempio, i visitatori di organizzazioni partner possono essere autenticati dal proprio server RADIUS dell'organizzazione partner e quindi utilizzare un account utente di Windows nell'organizzazione per accedere a una rete locale (LAN) Guest nella rete.

Altri attributi che forniscono funzionalità specializzate sono:

- **MS-Quarantine-IPFilter e MS-Quarantine-Session-Timeout**. Questi attributi vengono usati quando si distribuisce il controllo quarantena accesso alla rete (NAQC) con la distribuzione VPN di routing e accesso remoto.
- **Passport-user-mapping-UPN-suffisso**. Questo attributo consente di autenticare le richieste di connessione con le credenziali dell'account utente di Windows Live™ ID.
- **Tunnel-Tag**. Questo attributo designa il numero di ID VLAN a cui la connessione deve essere assegnata dal NAS quando si distribuiscono le reti locali virtuali (VLAN).

## <a name="default-connection-request-policy"></a>Criterio di richiesta di connessione predefinito

Quando si installa NPS, viene creato un criterio di richiesta di connessione predefinito. Questo criterio ha la configurazione seguente.

- L'autenticazione non è configurata.
- L'accounting non è configurato per l'invio di informazioni di contabilità a un gruppo di server RADIUS remoti.
- L'attributo non è configurato con regole di manipolazione degli attributi che inviano richieste di connessione ai gruppi di server RADIUS remoti.
- La richiesta di invio è configurata in modo che le richieste di connessione vengano autenticate e autorizzate nel server dei criteri di accesso locale
- Gli attributi avanzati non sono configurati.

Il criterio di richiesta di connessione predefinito usa NPS come server RADIUS. Per configurare un server che esegue NPS per fungere da proxy RADIUS, è necessario configurare anche un gruppo di server RADIUS remoti. È possibile creare un nuovo gruppo di server RADIUS remoto durante la creazione di un nuovo criterio di richiesta di connessione utilizzando la creazione guidata nuovo criterio di richiesta di connessione. È possibile eliminare i criteri di richiesta di connessione predefiniti o verificare che il criterio di richiesta di connessione predefinito sia l'ultimo criterio elaborato da NPS inserendolo per ultimo nell'elenco ordinato di criteri.

>[!NOTE]
>Se NPS e il servizio di accesso remoto sono installati nello stesso computer e il servizio di accesso remoto è configurato per l'autenticazione e l'accounting di Windows, è possibile che le richieste di autenticazione e accounting di accesso remoto vengano inviate a un server RADIUS . Questo problema può verificarsi quando le richieste di autenticazione e accounting di accesso remoto corrispondono a un criterio di richiesta di connessione configurato per l'invio a un gruppo di server RADIUS remoti.
