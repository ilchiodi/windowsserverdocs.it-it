---
title: Criteri di richiesta di connessione
description: In questo argomento viene fornita una panoramica dei criteri di richiesta di connessione di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21330b3838064c7b31e78ef70bdc04f0db0f50db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872942"
---
# <a name="connection-request-policies"></a>Criteri di richiesta di connessione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come usare criteri di richiesta di connessione NPS per configurare i criteri di rete come un server RADIUS, proxy RADIUS o entrambi.

>[!NOTE]
>Oltre a questo argomento, è disponibile la documentazione di criteri richiesta di connessione seguente.
> - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
> - [Configurare gruppi di Server RADIUS remoti](nps-crp-rrsg-configure.md)

I criteri di richiesta di connessione sono set di condizioni e le impostazioni che consentono agli amministratori di rete definire quali server RADIUS Remote Authentication Dial-In User Service () eseguono l'autenticazione e autorizzazione di connessione richiede che la server che esegue Server dei criteri di rete (NPS) riceve dai client RADIUS. I criteri di richiesta di connessione possono essere configurati per designare i server RADIUS che sono utilizzati per l'accounting RADIUS.

È possibile creare connessione i criteri di richiesta in modo che alcuni messaggi di richiesta RADIUS inviate dai client RADIUS vengono elaborati in locale (criteri di rete viene usato come server RADIUS) e altri tipi di messaggi vengono inoltrati a un altro server RADIUS (criteri di rete viene usato come proxy RADIUS).

Con i criteri di richiesta di connessione, è possibile usare criteri di rete come server RADIUS o come proxy RADIUS, in base a fattori come il seguente: 

- L'ora del giorno o giorno della settimana
- Il nome dell'area di autenticazione nella richiesta di connessione
- Il tipo di connessione richiesta
- L'indirizzo IP del client RADIUS

I messaggi di richiesta di accesso RADIUS vengono elaborati o inoltrati da criteri di rete solo se le impostazioni del messaggio in arrivo corrispondono ad almeno uno dei criteri di richiesta di connessione configurati su NPS. 

Se le impostazioni dei criteri di corrispondenza e i criteri richiedono che i criteri di rete elabora il messaggio, NPS agisce come un server RADIUS, autenticare e autorizzare la richiesta di connessione. Se le impostazioni dei criteri di corrispondenza e i criteri richiedono che i criteri di rete inoltra il messaggio, NPS agisce come proxy RADIUS e inoltra la richiesta di connessione a un server RADIUS remoto per l'elaborazione.

Se le impostazioni di un messaggio di richiesta di accesso RADIUS in ingresso non corrispondono ad almeno uno dei criteri di richiesta di connessione, viene inviato un messaggio di rifiuto di accesso al client RADIUS e l'utente o il computer tenta di connettersi alla rete viene negato l'accesso.

## <a name="configuration-examples"></a>Esempi di configurazione

Gli esempi di configurazione seguenti illustrano come usare i criteri di richiesta di connessione.

### <a name="nps-as-a-radius-server"></a>NPS come server RADIUS

Il criterio di richiesta connessione predefinito è l'unico criterio configurato. In questo esempio, criteri di rete è configurato come server RADIUS e tutte le richieste di connessione vengono elaborate da criteri di rete locale. I criteri di rete possono autenticare e autorizzare gli utenti con account nel dominio del dominio dei criteri di rete e in domini trusted.


### <a name="nps-as-a-radius-proxy"></a>Server dei criteri di rete come proxy RADIUS

Il criterio di richiesta di connessione predefinita viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per inoltrare le richieste in due domini diversi. In questo esempio, i criteri di rete è configurato come proxy RADIUS. Criteri di rete non elabora le richieste di connessione nel server locale. Al contrario, inoltra le richieste di connessione per criteri di rete o da altri server RADIUS che sono configurati come membri di gruppi di server RADIUS remoti.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS come server RADIUS sia come proxy RADIUS

Oltre a criterio richiesta connessione predefinito, viene creato un nuovo criterio richiesta di connessione che inoltra le richieste di connessione a un criteri di rete o un altro server RADIUS in un dominio non trusted. In questo esempio, i criteri proxy compare per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione corrisponde al criterio del proxy, la richiesta di connessione viene inoltrata al server RADIUS nel gruppo di server RADIUS remoto. Se la richiesta di connessione non soddisfa il criterio di proxy, ma corrisponde al criterio di richiesta connessione predefinito, dei criteri di rete elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a dei due criteri, vengono eliminate.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS come server RADIUS con server remoti di accounting

In questo esempio, i criteri di rete locale non è configurato per eseguire l'accounting e il criterio di richiesta connessione predefinito viene modificato in modo che i messaggi di accounting RADIUS vengono inoltrati da un criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoti. Anche se vengono inoltrati messaggi di accounting, autenticazione e autorizzazione dei messaggi non vengono inoltrati e i criteri di rete locale esegue queste funzioni per il dominio locale e tutti i domini trusted.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>Criteri di rete con server RADIUS remoto a Mapping degli utenti Windows

In questo esempio, criteri di rete funge sia da server RADIUS che come proxy RADIUS per ogni richiesta di connessione da inoltrare la richiesta di autenticazione a un server RADIUS remoto quando si usa un account utente locale di Windows per l'autorizzazione. Questa configurazione viene implementata mediante la configurazione di RADIUS remoto all'attributo di Mapping di utenti di Windows come condizione per il criterio di richiesta di connessione. (Inoltre, un account utente deve essere creato in locale con lo stesso nome dell'account utente remoto rispetto al quale l'autenticazione viene eseguita dal server RADIUS remoti.)

## <a name="connection-request-policy-conditions"></a>Condizioni dei criteri di richiesta di connessione

Condizioni dei criteri di richiesta di connessione sono uno o più attributi RADIUS che vengono confrontati con gli attributi del messaggio di richiesta di accesso RADIUS in ingresso. Se sono presenti più condizioni, quindi tutte le condizioni nella connessione messaggio di richiesta e nella richiesta di connessione dei criteri devono corrispondere affinché i criteri possano essere applicate da criteri di rete.


Di seguito sono gli attributi di condizione disponibili che è possibile configurare nei criteri di richiesta di connessione.

### <a name="connection-properties-attribute-group"></a>Gruppo di attributi delle proprietà di connessione

Il gruppo di attributi di proprietà di connessione contiene gli attributi seguenti.

- **Il protocollo di frame**. Usato per designare il tipo di frame per i pacchetti in ingresso. Esempi sono protocollo PPP (Point), Internet protocollo SLIP (Serial Line), Frame Relay e X.25.
- **Tipo di servizio**. Usato per designare il tipo di servizio richiesto. Gli esempi includono il frame (ad esempio, le connessioni PPP) e account di accesso (ad esempio, le connessioni di Telnet). Per altre informazioni sui tipi di servizio RADIUS, vedere RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo di tunnel**. Usato per designare il tipo di tunnel che viene creato dal client richiedente. I tipi di tunnel includono Point to Point Tunneling Protocol (PPTP) e Layer Two Tunneling Protocol (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Restrizioni data / ora gruppo di attributi 

Il gruppo di attributi restrizioni data / ora contiene l'attributo giorno e le restrizioni di tempo. Con questo attributo, è possibile designare il giorno della settimana e l'ora del giorno del tentativo di connessione. Il giorno e ora è relativo alla data e ora di NPS.

### <a name="gateway-attribute-group"></a>Gruppo di attributi di gateway

Il gruppo di attributi Gateway contiene gli attributi seguenti.

- **ID stazione chiamata**. Usato per designare il numero di telefono del server di accesso di rete. Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare i codici di area.
- **Identificatore NAS**. Utilizzato per definire il nome del server di accesso di rete. Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare gli identificatori NAS.
- **Indirizzo IPv4 NAS**. Usato per designare il protocollo Internet versione 4 (IPv4) indirizzo del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare le reti IP.
- **Indirizzo IPv6 NAS**. Usato per designare del protocollo Internet versione 6 (IPv6) indirizzo del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare le reti IP.
- **Tipo di porta NAS**. Usato per designare il tipo di supporto usata dal client di accesso. Sono esempi le linee telefoniche analogico (noto come asincrono), ISDN Integrated Services Digital Network (), i tunnel o reti private virtuali (VPN), IEEE 802.11 wireless, i commutatori Ethernet.

### <a name="machine-identity-attribute-group"></a>Gruppo di attributi di identità del computer

Il gruppo di attributi di identità del computer contiene l'attributo di identità del computer. Usando questo attributo, è possibile specificare il metodo con cui i client vengono identificati nei criteri.

### <a name="radius-client-properties-attribute-group"></a>Gruppo di attributi di proprietà di Client RADIUS

Il gruppo di attributi di proprietà di Client RADIUS contiene gli attributi seguenti.

- **ID stazione chiamante**. Usato per designare il numero di telefono utilizzato dal chiamante (il client di accesso). Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare i codici di area.  Nell'802.1x autenticazione l'indirizzo MAC viene in genere popolato e possono essere individuato dal client.  Questo campo è in genere usato per gli scenari di Bypass di indirizzo Mac quando il criterio di richiesta di connessione è configurato per "Utenti Accept senza convalidare le credenziali".  
- **Nome descrittivo del client**. Utilizzato per definire il nome del computer client RADIUS che richiede l'autenticazione. Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare i nomi dei clienti.
- **Indirizzo IPv4 del client**. Utilizzato per indicare l'indirizzo IPv4 del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare le reti IP.
- **Indirizzo IPv6 di client**. Utilizzato per indicare l'indirizzo IPv6 del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare le reti IP.
- **Fornitore client**. Usato per designare il fornitore del server di accesso di rete che richiede l'autenticazione. Un computer che esegue il servizio Routing e accesso remoto è il produttore NAS Microsoft. È possibile usare questo attributo per configurare criteri separati per diversi produttori NAS. Questo attributo è una stringa di caratteri. È possibile usare caratteri jolly.

### <a name="user-name-attribute-group"></a>Gruppo di attributi di nome utente

Il gruppo di attributi di nome utente contiene l'attributo nome utente. Usando questo attributo, è possibile designare il nome utente o una parte del nome utente, che deve corrispondere al nome utente fornito dal client di accesso nel messaggio RADIUS. Questo attributo è una stringa di caratteri che contiene in genere un nome dell'area di autenticazione e nome dell'account utente. È possibile usare caratteri jolly per specificare i nomi utente.

## <a name="connection-request-policy-settings"></a>Impostazioni dei criteri di richiesta di connessione

Le impostazioni dei criteri di richiesta di connessione sono un set di proprietà che vengono applicati a un messaggio in ingresso RADIUS. Le impostazioni sono costituiti i gruppi di proprietà seguenti.

- Autenticazione
- Accounting
- Manipolazione degli attributi
- Richiesta di inoltro
- Avanzate

Le sezioni seguenti forniscono ulteriori dettagli su queste impostazioni.

### <a name="authentication"></a>Autenticazione

Con questa impostazione, è possibile sostituire le impostazioni di autenticazione configurate in tutti i criteri di rete ed è possibile designare i metodi di autenticazione e che sono necessarie per connettersi alla rete.

>[!IMPORTANT]
>Se si configura un metodo di autenticazione nei criteri di richiesta di connessione che sono meno sicuro rispetto al metodo di autenticazione configurate nei criteri di rete, viene sottoposto a override il metodo di autenticazione più sicuro che si configura in Criteri di rete. Ad esempio, se si usa un criterio di rete che richiede l'utilizzo di Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 \(PEAP-MS-CHAP v2\), che è basato su password metodo di autenticazione per il wireless protetta e anche configurare un criterio di richiesta di connessione per consentire l'accesso non autenticato, il risultato è che nessun client è necessario per eseguire l'autenticazione con PEAP-MS-CHAP v2. In questo esempio, vengono concessi tutti i client che si connettono alla rete di accesso non autenticato.

### <a name="accounting"></a>Accounting

Con questa impostazione, è possibile configurare criteri di richiesta di connessione per inoltrare le informazioni di accounting da un criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoti in modo che il gruppo di server RADIUS remoto esegue accounting.

>[!NOTE]
>Se si dispone di più server RADIUS e si vogliono ottenere informazioni di accounting per tutti i server archiviati in un database centrale di accounting RADIUS, è possibile usare l'impostazione del criterio richiesta connessione accounting in un criterio in ogni server RADIUS per inoltrare i dati di accounting da tutti i server a uno dei criteri di rete o un altro server RADIUS che è designato come un server di accounting.

Le impostazioni dell'accounting criteri funzionano indipendente della configurazione di accounting di NPS locale richiesta di connessione. In altre parole, se si configurano i criteri di rete locale per registrare le informazioni di accounting RADIUS in un file locale o in un database Microsoft SQL Server, verranno eseguite in modo indipendentemente dal fatto che si configura un criterio di richiesta di connessione per inoltrare i messaggi di accounting da un raggio remoto gruppo di server.

Se si desidera che le informazioni di accounting in modalità remota, ma non in locale, è necessario configurare i criteri di rete locale per non eseguire l'accounting, durante la configurazione anche accounting in un criterio di richiesta di connessione per inoltrare i dati di accounting a un gruppo di server RADIUS remoti.

### <a name="attribute-manipulation"></a>Manipolazione degli attributi

È possibile configurare un set di regole di ricerca e sostituzione che consentono di modificare le stringhe di testo di uno dei seguenti attributi.

- Nome utente
- ID stazione chiamata
- ID stazione chiamante

L'elaborazione delle regole di ricerca e sostituzione viene generato per uno degli attributi precedenti prima il messaggio RADIUS è soggetto alle impostazioni di autenticazione e accounting. Attributo manipolazione regole si applicano solo a un singolo attributo. Non è possibile configurare le regole di modifica di attributi per ogni attributo. Inoltre, l'elenco degli attributi che è possibile modificare è un elenco statico; è possibile aggiungere all'elenco di attributi disponibili per la manipolazione.

>[!NOTE]
>Se si usa il protocollo di autenticazione MS-CHAP v2, è possibile manipolare l'attributo nome utente se vengono usati i criteri di richiesta di connessione per inoltrare i messaggi RADIUS. L'unica eccezione si verifica quando una barra rovesciata (\) viene utilizzato il carattere e la modifica interessa solo le informazioni a sinistra di essa. Un carattere barra rovesciata viene in genere utilizzato per indicare un nome di dominio (le informazioni a sinistra del carattere barra rovesciata) e un nome di account utente all'interno del dominio (le informazioni a destra del carattere barra rovesciata). In questo caso, sono consentite solo le regole manipolazione di attributo che modificano o sostituiscono il nome di dominio.

Per esempi che illustrano come modificare il nome dell'area di autenticazione nell'attributo nome utente, vedere la sezione "Esempi per la modifica del nome dell'area di autenticazione nell'attributo nome utente" nell'argomento [utilizzo delle espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Richiesta di inoltro

È possibile impostare le seguenti opzioni di richiesta utilizzate per i messaggi di richiesta di accesso RADIUS di inoltro:

- **Autenticare le richieste su questo server**. Con questa impostazione dei criteri di rete Usa un dominio Windows NT 4.0, Active Directory o il database degli account utente degli account di protezione (SAM) locale per autenticare la richiesta di connessione. Questa impostazione specifica anche che i criteri di corrispondenza rete configurato in Criteri di rete, e le relative proprietà dial-dell'account utente, vengono usati da NPS per autorizzare la richiesta di connessione. In questo caso, i criteri di rete è configurato per fungere da server RADIUS.

- **Inoltrare le richieste al gruppo di server RADIUS remoto seguente**. Con questa impostazione dei criteri di rete inoltra le richieste di connessione per il gruppo di server RADIUS remoto specificato. Se i criteri di rete riceve un messaggio di autorizzazione di accesso valido che corrisponde al messaggio di richiesta di accesso, il tentativo di connessione viene considerato autenticato e autorizzato. In questo caso, NPS agisce come proxy RADIUS.

- **Accettare gli utenti senza la convalida delle credenziali**. Con questa impostazione dei criteri di rete non verifica l'identità dell'utente che tenta di connettersi alla rete e dei criteri di rete non tenta di verificare che l'utente o il computer disponga del diritto per la connessione alla rete. Quando Criteri di rete è configurato per consentire l'accesso non autenticato, riceve una richiesta di connessione NPS invia immediatamente che un messaggio di autorizzazione di accesso per il raggio di client e l'utente o il computer viene concesso l'accesso alla rete. Questa impostazione viene usata per alcuni tipi di tunneling obbligatoria in cui il client di accesso viene effettuato il tunneling prima vengono autenticate le credenziali dell'utente.

>[!NOTE]
>Questa opzione di autenticazione non può essere utilizzata quando il protocollo di autenticazione del client di accesso è MS-CHAP versione 2 o Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), che offrono entrambe l'autenticazione reciproca. L'autenticazione reciproca, il client di accesso dimostra che è un client di accesso valido nel server che esegue l'autenticazione (NPS) e il server di autenticazione dimostra che è un server di autenticazione valido per il client di accesso. Quando viene utilizzata questa opzione di autenticazione, viene restituito il messaggio di autorizzazione di accesso. Tuttavia, il server di autenticazione non fornisce la convalida al client di accesso e l'autenticazione reciproca non riesce.

Per esempi su come usare le espressioni regolari per creare le regole di routing che inoltrano i messaggi RADIUS con il nome specificato dell'area di autenticazione a un gruppo di server RADIUS remoti, vedere la sezione "Esempio per l'inoltro dei messaggi RADIUS da un server proxy" nell'argomento [usare Le espressioni regolari in NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzate

È possibile impostare le proprietà avanzate per specificare la serie di attributi RADIUS che sono:

- Aggiunta al messaggio di risposta RADIUS quando NPS viene utilizzato come l'autenticazione RADIUS o accounting server. Quando sono presenti gli attributi specificati in un criterio di rete sia il criterio di richiesta di connessione, gli attributi che vengono inviati nel messaggio di risposta RADIUS sono la combinazione dei due set di attributi.
- Aggiungere al messaggio di RADIUS quando NPS viene usato come proxy contabile o l'autenticazione RADIUS. Se l'attributo esiste già nel messaggio viene inoltrato, questo viene sostituito con il valore dell'attributo specificato nei criteri di richiesta di connessione. 

Inoltre, alcuni attributi che sono disponibili per la configurazione della connessione richiedono dei criteri **le impostazioni** scheda le **avanzate** categoria offrono funzionalità specializzate. Ad esempio, è possibile configurare il **RADIUS remoto per il Mapping utente Windows** attributo quando si vuole suddividere l'autenticazione e autorizzazione di una richiesta di connessione tra due account di database.

Il **RADIUS remoto per il Mapping utente Windows** attributo specifica che l'autorizzazione di Windows si verifica per gli utenti vengono autenticati da un server RADIUS remoto. In altre parole, un server RADIUS remoto esegue l'autenticazione con un account utente in un database di account utente remoto, ma i criteri di rete locale autorizza la richiesta di connessione con un account utente in un database di account utente locale. Ciò è utile quando si desidera consentire l'accesso alla rete ai visitatori.

Ad esempio, i visitatori di organizzazioni partner possono essere autenticati da un proprio server RADIUS di organizzazione partner e quindi usano un account utente di Windows nell'organizzazione per accedere a una guest rete locale (LAN) nella rete.

Altri attributi che offrono funzionalità specializzate sono:

- **Timeout della sessione di quarantena MS e MS-quarantena-IPFilter**. Questi attributi vengono utilizzati quando si distribuiscono controllo quarantena accesso di rete (reti) con la distribuzione di Routing e accesso remoto VPN.
- **Passport-User-Mapping-UPN-Suffix**. Questo attributo consente di autenticare le richieste di connessione con le credenziali dell'account utente di Windows Live™ ID.
- **Tunnel-Tag**. Questo attributo definisce il numero di ID VLAN in cui la connessione deve essere assegnata dal server NAS durante la distribuzione di reti locali virtuali (VLAN).

## <a name="default-connection-request-policy"></a>Criterio richiesta connessione predefinito

Quando si installa NPS, viene creato un criterio richiesta connessione predefinito. Questo criterio non ha la configurazione seguente.

- Non è configurata l'autenticazione.
- Accounting non è configurato per le informazioni di accounting in avanti a un gruppo di server RADIUS remoti.
- Attributo non è configurato con regole di manipolazione di attributo che inoltra le richieste di connessione di gruppi di server RADIUS remoti.
- Richiesta di inoltro viene configurato in modo che le richieste di connessione vengono autenticate e autorizzate in Criteri di rete locale.
- Gli attributi avanzati non sono configurati.

Il criterio di richiesta connessione predefinito utilizza criteri di rete come server RADIUS. Per configurare un server dei criteri di rete per funzionare come proxy RADIUS, è necessario anche configurare un gruppo di server RADIUS remoti. È possibile creare un nuovo gruppo di server RADIUS remoto mentre si sta creando un nuovo criterio richiesta di connessione usando la creazione guidata nuovo criterio richiesta di connessione. È possibile eliminare il criterio di richiesta connessione predefinito o verificare che il criterio di richiesta connessione predefinito è l'ultimo criterio elaborato da NPS inserendolo ultimo nell'elenco ordinato dei criteri.

>[!NOTE]
>Se Criteri di rete e il servizio di accesso remoto vengono installati nel computer stesso e l'accesso remoto servizio è configurato per l'autenticazione di Windows e accounting, è possibile per l'autenticazione di accesso remoto e le richieste di accounting da inoltrare a un server RADIUS . Ciò può verificarsi quando le richieste di accounting e autenticazione di accesso remoto corrisponde a un criterio di richiesta di connessione che è configurato per l'inoltro a un gruppo di server RADIUS remoti.
