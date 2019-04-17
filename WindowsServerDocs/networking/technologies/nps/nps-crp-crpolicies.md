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
ms.openlocfilehash: 496ba87597b0be6cad1109269d2f72f52de017f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-policies"></a>Criteri di richiesta di connessione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come usare criteri di richiesta di connessione dei criteri di rete per configurare il server dei criteri di rete come un server RADIUS, proxy RADIUS o entrambi.

>[!NOTE]
>Oltre a questo argomento, è disponibile la seguente documentazione criteri richiesta di connessione.
> - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
> - [Configurare gruppi di Server RADIUS remoti](nps-crp-rrsg-configure.md)

Criteri di richiesta di connessione sono insiemi di condizioni e le impostazioni che consentono agli amministratori di rete specificare quali server RADIUS Remote Authentication Dial-In User Service () eseguire l'autenticazione e autorizzazione delle richieste di connessione che il server che esegue Server dei criteri di rete (NPS) riceve da client RADIUS. Criteri di richiesta di connessione possono essere configurati per designare i server RADIUS che vengono utilizzati per l'accounting RADIUS.

È possibile creare connessione criteri di richiesta in modo che alcuni messaggi di richiesta RADIUS inviate dai client RADIUS vengono elaborate in locale (criteri di rete viene utilizzato come server RADIUS) e altri tipi di messaggi inoltrati a un altro server RADIUS (criteri di rete viene usato come proxy RADIUS).

Con criteri di richiesta di connessione, è possibile utilizzare criteri di rete come server RADIUS o come proxy RADIUS, basati su fattori quali le operazioni seguenti: 

- L'ora del giorno o giorno della settimana
- Il nome dell'area di autenticazione nella richiesta di connessione
- Il tipo di connessione a richiesta
- L'indirizzo IP del client RADIUS

Messaggi di richiesta di accesso RADIUS vengono elaborati o inoltrati da criteri di rete solo se le impostazioni del messaggio in arrivo corrispondono ad almeno uno dei criteri di richiesta di connessione configurati nel server dei criteri di rete. 

Se le impostazioni dei criteri corrispondano e i criteri richiedono che il server dei criteri di rete elaborare il messaggio, dei criteri di rete funge da un server RADIUS, l'autenticazione e autorizzazione la richiesta di connessione. Se le impostazioni dei criteri corrispondano e i criteri richiedono che il server dei criteri di rete inoltra il messaggio, dei criteri di rete funziona come proxy RADIUS e inoltra la richiesta di connessione a un server RADIUS remoto per l'elaborazione.

Se le impostazioni di un messaggio di richiesta di accesso RADIUS in ingresso non corrispondono ad almeno uno dei criteri di richiesta di connessione, viene inviato un messaggio di rifiuto di accesso al client RADIUS e l'utente o il computer tenta di connettersi alla rete è negato l'accesso.

## <a name="configuration-examples"></a>Esempi di configurazione

Gli esempi di configurazione seguenti illustrano come è possibile utilizzare criteri di richiesta di connessione.

### <a name="nps-as-a-radius-server"></a>Criteri di rete come server RADIUS

Il criterio di richiesta di connessione predefinito è l'unico criterio configurato. In questo esempio, dei criteri di rete è configurato come server RADIUS e tutte le richieste di connessione vengono elaborate dal server dei criteri di rete locale. Il server dei criteri di rete può autenticare e autorizzare gli utenti con account nel dominio del dominio del server dei criteri di rete e in domini trusted.


### <a name="nps-as-a-radius-proxy"></a>Criteri di rete come proxy RADIUS

Il criterio di richiesta di connessione predefinito viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per inoltrare le richieste a due domini diversi. In questo esempio, i criteri di rete è configurato come proxy RADIUS. Criteri di rete non elabora le richieste di connessione nel server locale. Al contrario, inoltra le richieste di connessione per i criteri di rete o da altri server RADIUS che sono configurati come membri di gruppi di server RADIUS remoti.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>Criteri di rete come proxy RADIUS e server RADIUS

Oltre il criterio di richiesta di connessione predefinito, viene creato un nuovo criterio di richiesta di connessione che inoltra le richieste di connessione a dei criteri di rete o un altro server RADIUS in un dominio non trusted. In questo esempio, il criterio proxy visualizzata per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione soddisfa i criteri di proxy, viene inoltrata la richiesta di connessione al server RADIUS nel gruppo di server RADIUS remoti. Se la richiesta di connessione non corrispondono al criterio di proxy ma soddisfa i criteri di richiesta di connessione predefinito, dei criteri di rete elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a uno di questi criteri, viene ignorata.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>Criteri di rete come server RADIUS con i server remoti di accounting

In questo esempio, il server dei criteri di rete locale non è configurato per eseguire l'accounting e il criterio di richiesta di connessione predefinito viene modificato in modo che i messaggi di accounting RADIUS vengono inoltrati a dei criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoto. Anche se i messaggi di accounting vengono inoltrati, messaggi di autenticazione e autorizzazione, non vengono inoltrati e il server dei criteri di rete locale esegue queste funzioni per il dominio locale e tutti i domini trusted.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>Criteri di rete con server RADIUS remoto a Mapping utente Windows

In questo esempio, dei criteri di rete funge sia da server RADIUS che come proxy RADIUS per ogni richiesta di connessione per inoltrare le richieste di autenticazione a un server RADIUS remoto durante l'uso di un account utente locale di Windows per l'autorizzazione. Questa configurazione viene implementata tramite la configurazione RADIUS Remote all'attributo Mapping utente Windows come condizione per il criterio di richiesta di connessione. (Inoltre, un account utente deve essere creato in locale che ha lo stesso nome dell'account utente remoto rispetto al quale l'autenticazione viene eseguita dal server RADIUS remoti.)

## <a name="connection-request-policy-conditions"></a>Condizioni criteri richiesta di connessione

Condizioni criteri richiesta di connessione sono uno o più attributi RADIUS che vengono confrontati con gli attributi del messaggio di richiesta di accesso RADIUS in ingresso. Se sono presenti più condizioni, tutte le condizioni della connessione messaggio di richiesta e nella richiesta di connessione criteri devono corrispondere in modo che i criteri vengano imposte da criteri di rete.

Di seguito sono gli attributi di condizione disponibili che è possibile configurare in Criteri di richiesta di connessione.

### <a name="connection-properties-attribute-group"></a>Gruppo di attributi di proprietà di connessione

Il gruppo di attributi proprietà connessione contiene gli attributi seguenti.

- **Frame protocollo**. Usato per definire il tipo di frame per i pacchetti in ingresso. Esempi sono-to-Point Protocol (PPP), Serial SLIP Line Internet Protocol (), Frame Relay e x. 25.
- **Tipo di servizio**. Usato per definire il tipo di servizio richiesto. Esempi di frame (ad esempio, le connessioni PPP) e account di accesso (ad esempio, le connessioni Telnet). Per ulteriori informazioni sui tipi di servizio RADIUS, vedere RFC 2865 "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo di tunnel**. Usato per definire il tipo di tunnel che viene creato dal client richiedente. Tipi di tunnel includono Point to Point Tunneling Protocol (PPTP) e il Layer Two Tunneling Protocol (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Gruppo di attributi di giorno e restrizioni di tempo 

Il gruppo di attributi restrizioni data / ora contiene l'attributo di giorno e restrizioni di tempo. Con questo attributo, è possibile specificare il giorno della settimana e ora del giorno del tentativo di connessione. Il giorno e ora è relativo il giorno e l'ora del server dei criteri di rete.

### <a name="gateway-attribute-group"></a>Gruppo di attributi di gateway

Il gruppo di attributi Gateway contiene gli attributi seguenti.

- **ID stazione chiamata**. Usato per indicare il numero di telefono del server di accesso di rete. Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare i codici di area.
- **Identificatore NAS**. Usato per definire il nome del server di accesso di rete. Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare gli identificatori NAS.
- **Indirizzo IPv4 NAS**. Utilizzato per indicare il protocollo Internet versione 4 (IPv4) indirizzo del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare le reti IP.
- **Indirizzo IPv6 NAS**. Utilizzato per indicare il protocollo Internet versione 6 (IPv6) indirizzo del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare le reti IP.
- **Tipo di porta NAS**. Usato per definire il tipo di supporto utilizzato dal client di accesso. Alcuni esempi sono le linee telefoniche analogico (noto come async), servizi di rete ISDN (Integrated Digital), tunnel o reti private virtuali (VPN), IEEE 802.11 wireless, i commutatori Ethernet.

### <a name="machine-identity-attribute-group"></a>Gruppo di attributi di identità macchina

Il gruppo di attributi di identità macchina contiene l'attributo di identità macchina. Usando questo attributo, è possibile specificare il metodo con cui i client vengono identificati nei criteri.

### <a name="radius-client-properties-attribute-group"></a>Gruppo di attributi di proprietà di Client RADIUS

Il gruppo di attributi di proprietà di Client RADIUS contiene gli attributi seguenti.

- **ID stazione chiamante**. Usato per indicare il numero di telefono usato dal chiamante (il client di accesso). Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare i codici di area.
- **Nome descrittivo del client**. Usato per definire il nome del computer client RADIUS che richiede l'autenticazione. Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare i nomi dei client.
- **Indirizzo IPv4 client**. Utilizzato per indicare l'indirizzo IPv4 del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare le reti IP.
- **Indirizzo IPv6 del client**. Utilizzato per indicare l'indirizzo IPv6 del server di accesso di rete (il client RADIUS). Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare le reti IP.
- **Fornitore client**. Utilizzato per indicare il fornitore del server di accesso di rete che richiede l'autenticazione. Un computer che esegue il servizio Routing e accesso remoto è il produttore NAS Microsoft. È possibile utilizzare questo attributo per configurare criteri distinti per produttori NAS diversi. Questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly.

### <a name="user-name-attribute-group"></a>Gruppo di attributi di nome utente

Il gruppo di attributi di nome utente contiene l'attributo di nome utente. Usando questo attributo, è possibile specificare il nome utente o una parte del nome utente, che deve corrispondere al nome utente fornito dal client di accesso nel messaggio di RADIUS. Questo attributo è una stringa di caratteri che in genere contiene un nome dell'area di autenticazione e un nome di account utente. È possibile utilizzare caratteri jolly per specificare i nomi utente.

## <a name="connection-request-policy-settings"></a>Impostazioni del criterio di richiesta di connessione

Impostazioni del criterio di richiesta di connessione sono un set di proprietà che vengono applicati a un messaggio di RADIUS in ingresso. Le impostazioni sono costituite dai seguenti gruppi di proprietà.

- Autenticazione
- Accounting
- Manipolazione degli attributi
- Richiesta di inoltro
- Avanzate

Le sezioni seguenti forniscono ulteriori dettagli su queste impostazioni.

### <a name="authentication"></a>Autenticazione

Con questa impostazione, è possibile eseguire l'override delle impostazioni di autenticazione che sono configurate in tutti i criteri di rete e i metodi di autenticazione e i tipi che sono necessarie per la connessione alla rete, è possibile designare.

>[!IMPORTANT]
>Se si configura un metodo di autenticazione in Criteri di richiesta di connessione sono meno sicuro rispetto al metodo di autenticazione che si configurano in Criteri di rete, il metodo di autenticazione più sicuro che configurare nei criteri di rete viene sostituito. Ad esempio, se si dispone di un criterio di rete che richiede l'utilizzo di Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol versione 2 \ (PEAP-MS-CHAP v2\), che è un metodo di autenticazione basata su password per connessioni wireless sicure e configuri anche un criterio di richiesta di connessione per consentire l'accesso autenticato, il risultato è che nessun client è necessario per eseguire l'autenticazione con PEAP-MS-CHAP v2. In questo esempio, tutti i client che si connettono alla rete dispongono di accesso non autenticato.

### <a name="accounting"></a>Accounting

Con questa impostazione, è possibile configurare criteri di richiesta di connessione per inoltrare le informazioni di accounting a dei criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoto in modo che esegua l'accounting il gruppo di server RADIUS remoto.

>[!NOTE]
>Se si dispone di più server RADIUS e vuoi che le informazioni di accounting per tutti i server archiviato in un database centrale di accounting RADIUS, è possibile utilizzare l'impostazione di accounting criteri di richiesta di connessione in un criterio in ogni server RADIUS per l'inoltro dei dati di accounting da tutti i server da uno dei criteri di rete o un altro server RADIUS che è designato come un server di accounting.

Impostazioni di accounting criteri richiesta di connessione sono indipendenti della configurazione di accounting del server dei criteri di rete locale. In altre parole, se si configura il server dei criteri di rete locale per registrare le informazioni di accounting RADIUS in un file locale o in un database Microsoft SQL Server, verranno eseguite in modo indipendentemente dal fatto che configurare un criterio di richiesta di connessione per l'inoltro di messaggi di accounting a un gruppo di server RADIUS remoti.

Se vuoi che le informazioni di accounting in modalità remota, ma non in locale, è necessario configurare il server dei criteri di rete locale per non eseguire l'accounting, durante la configurazione anche di accounting in un criterio di richiesta di connessione per l'inoltro dei dati di accounting a un gruppo di server RADIUS remoti.

### <a name="attribute-manipulation"></a>Manipolazione degli attributi

È possibile configurare un set di regole di ricerca e sostituzione che gestiscono le stringhe di testo di uno degli attributi seguenti.

- Nome utente
- ID stazione chiamata
- ID stazione chiamante

Elaborazione delle regole di ricerca e sostituzione si verifica per uno degli attributi precedenti prima che il messaggio RADIUS è soggetto alle impostazioni di autenticazione e accounting. Attributo manipolazione regole si applicano solo a un singolo attributo. È possibile configurare le regole di manipolazione degli attributi per ogni attributo. Inoltre, l'elenco di attributi che è possibile modificare è un elenco statico. è possibile aggiungere all'elenco di attributi disponibili per la manipolazione.

>[!NOTE]
>Se si utilizza il protocollo di autenticazione MS-CHAP v2, è possibile modificare l'attributo di nome utente se il criterio di richiesta di connessione viene utilizzato per inoltrare il messaggio RADIUS. L'unica eccezione si verifica quando viene utilizzato un carattere barra rovesciata (\) e la manipolazione riguarda solo le informazioni a sinistra di esso. Il carattere barra rovesciata viene in genere utilizzato per indicare un nome di dominio (le informazioni a sinistra del carattere barra rovesciata) e un nome di account utente all'interno del dominio (le informazioni a destra del carattere barra rovesciata). In questo caso, sono consentite solo regole di manipolazione attributi che modificano o sostituiscono il nome di dominio.

Per esempi su come modificare il nome dell'area di autenticazione nell'attributo di nome utente, vedere la sezione "Esempi per la modifica del nome dell'area di autenticazione nell'attributo di nome utente" nell'argomento [utilizzare espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Richiesta di inoltro

È possibile impostare le seguenti opzioni di richiesta che vengono utilizzate per i messaggi di richiesta di accesso RADIUS di inoltro:

- **Autenticare le richieste su questo server**. Con questa impostazione, dei criteri di rete utilizza un dominio Windows NT 4.0, Active Directory o database degli account utente locale gli account di protezione (SAM) per autenticare la richiesta di connessione. Questa impostazione specifica inoltre che il criterio di rete corrispondente configurato in Criteri di rete, insieme di proprietà di connessione remota dell'account utente, vengono utilizzati da criteri di rete per autorizzare la richiesta di connessione. In questo caso, il server dei criteri di rete è configurato per fungere da server RADIUS.

- **Inoltrare le richieste al seguente gruppo di server RADIUS remoto**. Con questa impostazione, dei criteri di rete inoltra le richieste di connessione al gruppo di server RADIUS remoto specificato. Se il server dei criteri di rete riceve un messaggio di autorizzazione di accesso valido che corrisponde al messaggio di richiesta di accesso, il tentativo di connessione è considerato autenticato e autorizzato. In questo caso, il server dei criteri di rete funge da proxy RADIUS.

- **Per accettare gli utenti senza la convalida delle credenziali**. Con questa impostazione, dei criteri di rete non verifica l'identità dell'utente sta tentando di connettersi alla rete e dei criteri di rete non tenta di verificare che l'utente o il computer ha il diritto di connettersi alla rete. Quando Criteri di rete è configurato per consentire l'accesso autenticato e riceve una richiesta di connessione, invia immediatamente che un messaggio di autorizzazione di accesso al raggio client e l'utente o computer è concesso l'accesso alla rete. Questa impostazione viene utilizzata per alcuni tipi di tunneling obbligatorio in cui il client di accesso viene effettuato il tunneling prima dell'autenticazione delle credenziali utente.

>[!NOTE]
>Questa opzione di autenticazione non può essere utilizzata quando il protocollo di autenticazione del client di accesso è MS-CHAP v2 o Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), che fornisce l'autenticazione reciproca. L'autenticazione reciproca, il client di accesso dimostra che è un client di accesso valido per il server di autenticazione (il server dei criteri di rete) e il server di autenticazione non dimostri di un server di autenticazione valido per il client di accesso. Quando si utilizza questa opzione di autenticazione, viene restituito il messaggio di autorizzazione di accesso. Tuttavia, il server di autenticazione non fornisce la convalida per il client di accesso e l'autenticazione reciproca non riesce.

Per esempi su come usare le espressioni regolari per creare regole di routing per l'inoltro di messaggi RADIUS con un nome dell'area di autenticazione specificato a un gruppo di server RADIUS remoto, vedere la sezione "Esempio per l'inoltro dei messaggi da un server proxy RADIUS" nell'argomento [utilizzare espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzate

È possibile impostare le proprietà avanzate per specificare la serie di attributi RADIUS:

- Aggiungere al messaggio di risposta RADIUS quando il server dei criteri di rete viene utilizzato come server di accounting o autenticazione RADIUS. Quando sono presenti attributi specificati in un criterio di rete e i criteri di richiesta di connessione, gli attributi che vengono inviati nel messaggio di risposta RADIUS sono la combinazione dei due set di attributi.
- Aggiunta al messaggio quando viene utilizzato il server dei criteri di rete come proxy accounting o autenticazione RADIUS. Se l'attributo è già presente nel messaggio che viene inoltrato, sostituita con il valore dell'attributo specificato nel criterio di richiesta di connessione. 

Inoltre, alcuni attributi che sono disponibili per la configurazione per la connessione richiedono criteri **impostazioni** nella scheda il **avanzate** categoria offrono funzionalità specifiche. Ad esempio, è possibile configurare il **RADIUS remoto a Windows Mapping utente** attributo quando si desidera dividere l'autenticazione e autorizzazione di una richiesta di connessione tra due utenti account database.

Il **RADIUS remoto a Windows Mapping utente** attributo specifica che l'autenticazione di Windows si verifica per gli utenti autenticati da un server RADIUS remoto. In altre parole, un server RADIUS remoto esegue l'autenticazione per un account utente in un database di account utente remoto, ma il server dei criteri di rete locale autorizza la richiesta di connessione per un account utente in un database di account utente locale. Ciò è utile quando si desidera consentire l'accesso alla rete visitatori.

Visitatori di organizzazioni partner, ad esempio, possono essere autenticati per il proprio server RADIUS organizzazione partner e quindi utilizzano un account utente di Windows nell'organizzazione per accedere a una guest rete locale (LAN) nella rete.

Altri attributi che offrono funzionalità specifiche sono:

- **MS-Quarantine-IPFilter e MS-Quarantine-Session-Timeout**. Questi attributi vengono utilizzati quando si distribuisce controllo quarantena accesso di rete (reti) con la distribuzione di Routing e accesso remoto VPN.
- **Passport-utente-Mapping--suffisso UPN**. Questo attributo consente di autenticare le richieste di connessione con le credenziali dell'account utente di Windows Live™ ID.
- **Tunnel Tag**. Questo attributo indica il numero di ID VLAN a cui la connessione deve essere assegnata dal server NAS quando si distribuiscono reti locali virtuali (VLAN).

## <a name="default-connection-request-policy"></a>Criterio di richiesta di connessione predefinito

Quando si installa NPS, viene creato un criterio di richiesta di connessione predefinito. Questo criterio non ha la configurazione seguente.

- Autenticazione non è configurata.
- Accounting non è configurato per le informazioni di accounting diretta a un gruppo di server RADIUS remoti.
- Attributo non è configurato con le regole di manipolazione degli attributi che inoltra le richieste di connessione a gruppi di server RADIUS remoti.
- Inoltrare richieste è configurato in modo che le richieste di connessione di autenticazione e autorizzazione nel server dei criteri di rete locale.
- Gli attributi avanzati non sono configurati.

Il criterio di richiesta di connessione predefinito utilizza criteri di rete come server RADIUS. Per configurare un server dei criteri di rete come proxy RADIUS, è necessario configurare anche un gruppo di server RADIUS remoto. Durante la creazione di un nuovo criterio di richiesta di connessione tramite la creazione guidata nuovo criterio di richiesta di connessione, è possibile creare un nuovo gruppo di server RADIUS remoto. È possibile eliminare il criterio di richiesta di connessione predefinito o verificare che il criterio di richiesta di connessione predefinito è l'ultimo criterio elaborato da criteri di rete, inserendo ultima nell'elenco ordinato dei criteri.

>[!NOTE]
>Se sono installati dei criteri di rete e il servizio Accesso remoto nel computer stesso e l'accesso remoto servizio è configurato per l'autenticazione Windows e contabilità, è possibile per l'autenticazione di accesso remoto e le richieste di accounting vengano inoltrate a un server RADIUS. Ciò può verificarsi quando l'autenticazione di accesso remoto e le richieste di accounting corrispondono a un criterio di richiesta di connessione che è configurato per l'inoltro a un gruppo di server RADIUS remoto.