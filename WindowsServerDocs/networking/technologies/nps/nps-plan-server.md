---
title: Pianificare i criteri di rete come server RADIUS
description: Questo argomento fornisce informazioni sulla distribuzione di server RADIUS di Server dei criteri di rete di pianificazione in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9bb95428c17e5d7bde588693e84288ddfe64531
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-server"></a>Pianificare i criteri di rete come server RADIUS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Quando si distribuisce \(NPS\) Server dei criteri di rete come server RADIUS Remote Authentication Dial-In User Service (), dei criteri di rete esegue l'autenticazione, autorizzazione e accounting per le richieste di connessione per il dominio locale e per i domini trusting del dominio locale. È possibile utilizzare queste linee guida per la pianificazione per semplificare la distribuzione di RADIUS.

Queste linee guida per la pianificazione non includono le circostanze in cui si desidera distribuire i criteri di rete come proxy RADIUS. Quando si distribuiscono i criteri di rete come proxy RADIUS, dei criteri di rete inoltra le richieste di connessione a un server dei criteri di rete o da altri server RADIUS in domini remoti, domini non attendibili o entrambi. 

Prima di distribuire criteri di rete come server RADIUS in rete, utilizzare le seguenti linee guida per pianificare la distribuzione.

- Pianificare la configurazione del server dei criteri di rete.

- Pianificare i client RADIUS.

- Pianificare l'utilizzo di metodi di autenticazione.

- Pianificare i criteri di rete.

- Pianificare l'accounting NPS.

## <a name="plan-nps-server-configuration"></a>Pianificare la configurazione del server dei criteri di rete

È necessario decidere di dominio a cui il server dei criteri di rete è membro. Per gli ambienti più domini, un server dei criteri di rete può autenticare le credenziali per gli account utente nel dominio di cui è membro e per tutti i domini trusting del dominio locale del server dei criteri di rete. Per consentire al server dei criteri di rete leggere le proprietà di connessione remota degli account utente durante il processo di autorizzazione, è necessario aggiungere l'account computer del server dei criteri di rete al gruppo di server RAS e dei criteri di rete per ogni dominio.

Dopo aver determinato l'appartenenza al dominio del server dei criteri di rete, il server deve essere configurato per comunicare con i client RADIUS, chiamato anche il server di accesso di rete, utilizzando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi che registra dei criteri di rete nel caso in cui log ed è possibile specificare una descrizione per il server.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per la configurazione del server dei criteri di rete, è possibile utilizzare la procedura seguente.

- Determinare le porte RADIUS che il server dei criteri di rete utilizza per ricevere i messaggi RADIUS dai client RADIUS. Le porte predefinite sono le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e porte 1813 e 1646 per i messaggi di accounting RADIUS.

- Se il server dei criteri di rete è configurato con più schede di rete, determinare le schede attraverso la quale si desidera che il traffico RADIUS affinché sia consentita.

- Determinare i tipi di eventi che si desidera registrare nel registro eventi dei criteri di rete. È possibile registrare le richieste di autenticazione rifiutati, le richieste di autenticazione ha esito positivo o entrambi i tipi di richieste.

- Determinare se si distribuisce uno o più server dei criteri di rete. Per fornire la tolleranza di errore di accounting e autenticazione RADIUS, utilizzare almeno due server dei criteri di rete. Un server dei criteri di rete viene utilizzato come server RADIUS primario e l'altro è utilizzato come backup. Quindi, ogni client RADIUS è configurato su entrambi i server dei criteri di rete. Se il server dei criteri di rete primario non è più disponibile, i client RADIUS, quindi inviano messaggi di richiesta di accesso al server dei criteri di rete alternativo.

- Pianificare lo script utilizzato per copiare una configurazione del server dei criteri di rete su altri server dei criteri di rete per salvare in sovraccarico amministrativo e impedire il corretta configurazione di un server. Criteri di rete fornisce i comandi Netsh che consentono di copiare tutto o parte di una configurazione di server dei criteri di rete per l'importazione in un altro server dei criteri di rete. È possibile eseguire manualmente i comandi al prompt di Netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se decidi di modificare le configurazioni di server.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso di rete, ad esempio punti di accesso wireless, server di rete privata virtuale (VPN), 802.1 commutatori che supportano X e i server di accesso remoto. Proxy RADIUS, che inoltra connessione messaggi di richiesta di server RADIUS, sono anche i client RADIUS. Criteri di rete supporta tutti i server di accesso di rete e proxy RADIUS che rispettano il raggio di protocollo come descritto nella RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)," e RFC 2866 "RADIUS contabilità".

>[!IMPORTANT]
>I client di accesso, ad esempio i computer client, non sono client RADIUS. Solo i server proxy che supportano il protocollo RADIUS e server di accesso alla rete sono client RADIUS.

Inoltre, i punti di accesso wireless e commutatori devono essere in grado di autenticazione 802.1 X. Se si desidera distribuire Extensible Authentication Protocol \(EAP\) o \(PEAP\) Protected Extensible Authentication Protocol, punti di accesso e commutatori devono supportare l'utilizzo di EAP.

Per verificare l'interoperabilità di base per le connessioni PPP per i punti di accesso wireless, configurare il punto di accesso e il client di accesso per utilizzare l'autenticazione protocollo PAP (Password). Utilizzare i protocolli di autenticazione PPP aggiuntive, ad esempio PEAP, fino a quando non si sono verificate quelli che si desidera utilizzare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per i client RADIUS, è possibile utilizzare la procedura seguente.

- Documentare gli attributi specifici del fornitore (VSA), che è necessario configurare in Criteri di rete. Se i server di accesso di rete richiedono VSA, registrare le informazioni di VSA per un uso successivo, quando si configurano i criteri di rete in Criteri di rete. 

- Documentare gli indirizzi IP dei client RADIUS e il server dei criteri di rete per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurare in modo che utilizzino il protocollo RADIUS, con l'indirizzo IP del server dei criteri di rete utilizzato come server di autenticazione. E quando si configura criteri di rete per comunicare con i client RADIUS, è necessario immettere gli indirizzi IP di client RADIUS nello snap-in Criteri di rete.

- Creare i segreti condivisi per la configurazione nei client RADIUS e nello snap-in Criteri di rete. È necessario configurare client RADIUS con un segreto condiviso, o una password, che è inoltre necessario immettere nello snap-in Criteri di rete durante la configurazione di client RADIUS in Criteri di rete.

## <a name="plan-the-use-of-authentication-methods"></a>Pianificare l'utilizzo di metodi di autenticazione

Criteri di rete supporta entrambi i metodi di autenticazione basata su password e basata su certificati. Tuttavia, non tutti i server di accesso di rete supportano gli stessi metodi di autenticazione. In alcuni casi, si potrebbe essere necessario distribuire un metodo di autenticazione diversi in base al tipo di accesso alla rete.

Ad esempio, si desidera distribuire sia wireless e VPN per l'organizzazione, ma usare un metodo di autenticazione diversi per ogni tipo di accesso: EAP-TLS per le connessioni VPN, a causa di sicurezza avanzata che fornisce EAP con Transport Layer Security (EAP-TLS) e PEAP-MS-CHAP v2 per connessioni wireless 802.1 X.

Riconnessione PEAP con Microsoft Challenge Handshake Authentication Protocol versione 2 (PEAP-MS-CHAP v2) fornisce una funzionalità denominata veloce che è progettato specificamente per l'utilizzo con i computer portatili e altri dispositivi wireless. Riconnessione rapida consente ai client senza fili per spostarsi tra i punti di accesso wireless nella stessa rete senza essere nuovamente autenticati ogni volta che vengono associati a un nuovo punto di accesso. Questo offre una migliore esperienza agli utenti wireless e consente loro di spostarsi tra i punti di accesso senza dover digitare di nuovo le proprie credenziali.
A causa di fast riconnessione e la sicurezza che fornisce PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 è una scelta logica come metodo di autenticazione per le connessioni wireless.

Per le connessioni VPN, EAP-TLS è un metodo di autenticazione basata su certificati che fornisce sicurezza avanzata che consente di proteggere il traffico di rete anche durante la trasmissione attraverso la rete Internet dai computer di casa o per dispositivi mobili ai server VPN dell'organizzazione.

### <a name="certificate-based-authentication-methods"></a>Metodi di autenticazione basata su certificati

Metodi di autenticazione basata su certificato hanno il vantaggio di fornire protezione avanzata. e hanno lo svantaggio di essere più difficile da distribuire metodi di autenticazione basata su password.

Sia PEAP-MS-CHAP v2 ed EAP-TLS sono metodi di autenticazione basata su certificati, ma esistono molte differenze tra di esse e il modo in cui vengono distribuiti.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS utilizza certificati per l'autenticazione client e server e, è necessario distribuire un'infrastruttura a chiave pubblica (PKI) dell'organizzazione. Distribuzione di un'infrastruttura a chiave pubblica può essere complessa e richiede una fase di pianificazione che è indipendente della pianificazione per l'utilizzo di criteri di rete come server RADIUS.

Con EAP-TLS, il server dei criteri di rete registra un certificato server da un'autorità di certificazione \(CA\) e il certificato viene salvato nel computer locale nell'archivio certificati. Durante il processo di autenticazione, l'autenticazione server si verifica quando il server dei criteri di rete invia il certificato del server al client di accesso per dimostrare la propria identità ai client di accesso. Il client di accesso esamina varie proprietà del certificato per determinare se il certificato è valido e che è appropriato per l'utilizzo durante l'autenticazione server. Se il certificato del server soddisfi i requisiti del certificato server minimo ed emesso da un'autorità di certificazione che considera attendibile il client di accesso, il server dei criteri di rete viene autenticato correttamente dal client.

Analogamente, l'autenticazione client si verifica durante il processo di autenticazione quando il client invia il certificato del client al server dei criteri di rete per dimostrare la propria identità per il server dei criteri di rete. Il server dei criteri di rete esamina il certificato e se il certificato client soddisfi i requisiti del certificato client minimo ed emesso da un'autorità di certificazione che considera attendibile il server dei criteri di rete, il client di accesso viene autenticato correttamente dal server dei criteri di rete.

Anche se è necessario che il certificato server viene archiviato nell'archivio certificati nel server dei criteri di rete, il certificato client o un utente può essere archiviato in uno dei due nell'archivio certificati sul client o in una smart card.

Per questo processo di autenticazione riuscita, è necessario che tutti i computer siano certificato CA dell'organizzazione nell'archivio certificati Autorità di certificazione radice attendibili per il Computer locale e l'utente corrente.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 utilizza un certificato per l'autenticazione server e le credenziali basate su password per l'autenticazione utente. Poiché i certificati vengono utilizzati solo per l'autenticazione server, non sono necessari per distribuire un'infrastruttura a chiave pubblica per poter usare PEAP-MS-CHAP v2. Quando si distribuisce PEAP-MS-CHAP v2, è possibile ottenere un certificato server per il server dei criteri di rete in uno dei due modi seguenti:

- È possibile installare Servizi certificati Active Directory (AD CS) e quindi registrare automaticamente i certificati per server dei criteri di rete. Se si utilizza questo metodo, è inoltre necessario registrare il certificato della CA per i computer client connessi alla rete in modo che ritengano attendibile il certificato emesso per il server dei criteri di rete.

- È possibile acquistare un certificato server da una CA pubblica, ad esempio VeriSign. Se si utilizza questo metodo, assicurarsi di selezionare un'autorità di certificazione è già considerato attendibile dai computer client. Per determinare se i computer client attendibile un'autorità di certificazione, aprire lo snap-in certificati di Microsoft Management Console (MMC) in un computer client e visualizzare l'archivio di autorità di certificazione radice attendibili per il Computer locale e per l'utente corrente. Se è presente un certificato dall'autorità di certificazione in questi archivi certificati, il client considera attendibile l'autorità di certificazione e, di conseguenza qualsiasi certificato rilasciato dalla CA.

Durante il processo di autenticazione con PEAP-MS-CHAP v2, l'autenticazione server si verifica quando il server dei criteri di rete invia il certificato del server per il computer client. Il client di accesso esamina varie proprietà del certificato per determinare se il certificato è valido e che è appropriato per l'utilizzo durante l'autenticazione server. Se il certificato del server soddisfi i requisiti del certificato server minimo ed emesso da un'autorità di certificazione che considera attendibile il client di accesso, il server dei criteri di rete viene autenticato correttamente dal client.

L'autenticazione utente quando un utente tenta di connettersi le credenziali basate su password di rete tipi e tenta di accedere. Criteri di rete riceve le credenziali ed esegue l'autenticazione e autorizzazione. Se l'utente viene autenticato e autorizzato correttamente e se il computer client autenticato correttamente il server dei criteri di rete, viene concessa la richiesta di connessione.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per l'utilizzo di metodi di autenticazione, è possibile utilizzare la procedura seguente.

- Identificare i tipi di accesso alla rete che si intende offrire, ad esempio senza fili, VPN, 802.1 X che supportano commutatore e accesso remoto.

- Determinare il metodo di autenticazione o i metodi che si desidera utilizzare per ogni tipo di accesso. È consigliabile utilizzare i metodi di autenticazione basata su certificati che forniscono protezione avanzata. Tuttavia, potrebbe non essere utile per distribuire un'infrastruttura PKI, in modo che altri metodi di autenticazione potrebbero fornire il giusto equilibrio tra gli elementi necessari per la rete migliore.

- Se si distribuisce PEAP-TLS, pianificare la distribuzione di infrastruttura a chiave pubblica. Ciò include la pianificazione di modelli di certificato che si intende utilizzare per i certificati server e i certificati del computer client. Include inoltre di determinare la modalità di registrare i certificati nei controller di dominio e i computer non appartenenti al dominio e determinare se si desidera utilizzare smart card.

- Se si distribuisce PEAP-MS-CHAP v2, determinare se si desidera installare Servizi certificati Active Directory per rilasciare certificati server per i server dei criteri di rete o se si desidera acquistare i certificati del server da una CA pubblica, ad esempio VeriSign.

### <a name="plan-network-policies"></a>Pianificare i criteri di rete

Criteri di rete vengono utilizzati da criteri di rete per determinare se le richieste di connessione ricevute dai client RADIUS sono autorizzate. Criteri di rete Usa anche la proprietà di connessione remota dell'account utente per eseguire un'operazione di autorizzazione.

Perché i criteri di rete vengono elaborati nell'ordine in cui vengono visualizzati nello snap-in Criteri di rete, prevede di collocare i criteri più restrittivi prima nell'elenco dei criteri. Per ogni richiesta di connessione, dei criteri di rete tenta di individuare le condizioni dei criteri con le proprietà di richiesta di connessione. Criteri di rete esamina ogni criterio di rete in ordine finché non viene trovata una corrispondenza. Se non trova una corrispondenza, viene rifiutata la richiesta di connessione.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per i criteri di rete, è possibile utilizzare la procedura seguente.

- Determinare l'ordine di elaborazione dei criteri di rete preferita dei criteri di rete, dal più restrittivi per più restrittiva.

- Determinare lo stato dei criteri. Lo stato dei criteri può avere il valore di abilitato o disabilitato. Se il criterio è abilitato, dei criteri di rete valuta i criteri durante l'esecuzione di autorizzazione. Se il criterio non è abilitato, non viene valutata.

- Determinare il tipo di criteri. È necessario determinare se il criterio è progettato per l'accesso quando le condizioni dei criteri vengono confrontate la richiesta di connessione o se il criterio è progettato per negare l'accesso quando le condizioni dei criteri sono soddisfatte dalla richiesta di connessione. Ad esempio, se si desidera negare esplicitamente l'accesso wireless per i membri di un gruppo di Windows, è possibile creare un criterio di rete che specifica il gruppo, il metodo di connessione wireless e che dispone di un criterio digitare l'impostazione di Nega accesso.

- Determinare se si desidera che i criteri di rete per ignorare la proprietà di connessione remota degli account utente che sono membri del gruppo su cui si basa il criterio. Quando questa impostazione non è abilitata, la proprietà di connessione remota degli account utente sostituire le impostazioni che sono configurate in Criteri di rete. Ad esempio, se è configurato un criterio di rete che consente l'accesso a un utente, ma le chiamate in ingresso dell'account utente per tale utente sono impostate per negare l'accesso, l'utente viene negato l'accesso. Ma se si abilita le proprietà di impostazione dial-in account utente Ignora tipo di criteri, lo stesso utente viene concesso l'accesso alla rete.

- Determinare se il criterio utilizza l'impostazione di origine. Questa impostazione consente di specificare facilmente un'origine per tutte le richieste di accesso. Possibili origini sono un Gateway di servizi Terminal (Gateway Servizi terminal), un server di accesso remoto (VPN o remote), un server DHCP, un punto di accesso wireless e un server Autorità registrazione integrità. In alternativa, è possibile specificare un'origine specifici del fornitore.

- Determinare le condizioni che devono essere corrisposte nell'ordine dei criteri di rete verrà applicato.

- Determinare le impostazioni che vengono applicate se sono soddisfatte le condizioni dei criteri di rete per la richiesta di connessione.

- Determinare se si desidera utilizzare, modificare o eliminare i criteri di rete predefinito.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

Criteri di rete offre la possibilità di accedere a dati di accounting RADIUS, ad esempio l'autenticazione utente e richieste di accounting in tre formati: formato IAS, compatibile con database e la registrazione di Microsoft SQL Server. 

Compatibile con database e il formato IAS creare file di log nel server dei criteri di rete locale in formato testo. 

La registrazione di SQL Server offre la possibilità di accedere a un database SQL Server 2005 XML conforme, l'estensione di accounting RADIUS per sfruttare i vantaggi di registrazione a un database relazionale o un SQL Server 2000.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per l'accounting NPS, è possibile utilizzare la procedura seguente.

- Determinare se si desidera archiviare i dati di accounting NPS nei file di registro o in un database di SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Accounting NPS utilizzando i file di registro locale

Utente di autenticazione e accounting richieste di registrazione nel file di registro viene utilizzato principalmente per scopi di fatturazione e di analisi di connessione ed è anche utile come strumento di analisi di sicurezza, offrendo un metodo per la verifica dell'attività di un utente malintenzionato dopo un attacco.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per l'accounting NPS utilizzando i file di registro locale, è possibile utilizzare la procedura seguente.

- Determinare il formato di file di testo che si desidera utilizzare per i file di registro dei criteri di rete.

- Scegli il tipo di informazioni che si desidera accedere. È possibile registrare le richieste di accounting, le richieste di autenticazione e lo stato periodico.

- Determinare il percorso del disco rigido in cui si desidera archiviare i file di log.

- Progettare la soluzione di backup di file di registro. Il percorso del disco rigido in cui si archiviano i file di registro deve essere un percorso che consente di eseguire il backup dei dati. Inoltre, il percorso del disco rigido deve essere protetto configurando l'elenco di controllo di accesso (ACL) per la cartella in cui sono archiviati i file di registro.

- Determinare la frequenza con cui si desidera nuovi file di log da creare. Se si desidera che il file di registro deve essere creato in base alle dimensioni del file, determinare la dimensione massima dei file prima che venga creato un nuovo file di registro da criteri di rete.

- Determinare se si desidera che i criteri di rete per eliminare i file di registro precedenti se il disco rigido si esaurisce lo spazio di archiviazione.

- Determinare l'applicazione o applicazioni che si desidera utilizzare per visualizzare i dati di accounting e la generazione di report.

### <a name="nps-sql-server-logging"></a>Registrazione dei criteri di rete SQL Server

La registrazione dei criteri di rete SQL Server viene utilizzata quando le informazioni sullo stato di sessione, è necessario per scopi di analisi di dati e la creazione di report e per centralizzare e semplificare la gestione dei dati di accounting.

Criteri di rete offre la possibilità di utilizzare SQL Server, registrazione per l'autenticazione utente record e accounting richieste ricevute da uno o più server di accesso di rete per un'origine dati in un computer che esegue il \(MSDE 2000\) motore Desktop di Microsoft SQL Server, o qualsiasi versione di SQL Server in un secondo momento a SQL Server 2000.

I dati di accounting sono passati da server dei criteri in formato XML a una stored procedure nel database, che supporta entrambi linguaggio structured query language \(SQL\) e \(SQLXML\) XML. La registrazione di autenticazione dell'utente e richieste in un database di SQL Server XML compatibile di accounting consente ai server dei criteri di rete più di un'origine dati.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per l'accounting NPS utilizzando la registrazione dei criteri di rete SQL Server, è possibile utilizzare la procedura seguente.

- Determinare se è o un altro membro dell'organizzazione dispone di SQL Server 2000 o SQL Server 2005 sviluppo database relazionale esperienza e comprendere come usare questi prodotti per creare, modificare, gestire e gestire i database di SQL Server.

- Determinare se è installato SQL Server nel server dei criteri di rete o in un computer remoto.

- Progettare la stored procedure che userai nel database di SQL Server per elaborare i file XML in ingresso che contengono i dati di accounting NPS.

- Progettare la struttura di replica di database SQL Server e il flusso.

- Determinare l'applicazione o applicazioni che si desidera utilizzare per visualizzare i dati di accounting e la generazione di report.

- Pianificare l'utilizzo di server di accesso di rete che invia l'attributo di classe in tutte le richieste di accounting. L'attributo di classe viene inviato al client RADIUS in un messaggio di autorizzazione di accesso ed è utile per correlare i messaggi di richiesta di Accounting alle sessioni di autenticazione. Se l'attributo di classe viene inviato dal server di accesso rete nei messaggi di richiesta di accounting, può essere utilizzato per trovare la corrispondenza i record di accounting e autenticazione. La combinazione degli attributi univoci-numero di serie, ora di riavvio del servizio e l'indirizzo del Server deve essere univoca per ogni autenticazione che accetta il server.

- Pianificare l'utilizzo di server di accesso di rete che supportano accounting interim.

- Pianificare l'utilizzo di server di accesso di rete che inviano messaggi di attivazione e disattivazione.

- Pianificare l'utilizzo di server di accesso di rete che supportano l'archiviazione e l'inoltro dei dati di accounting. Server di accesso di rete che supportano questa funzionalità è possibile archiviare i dati di accounting quando il server di accesso di rete non può comunicare con il server dei criteri di rete. Quando il server dei criteri di rete è disponibile, il server di accesso di rete inoltra i record archiviati al server dei criteri di rete, fornendo una maggiore affidabilità di accounting sul server di accesso di rete che non si specifica questa funzionalità.

- Se si intende configurare sempre l'attributo Acct-Interim-Interval in Criteri di rete. L'attributo Acct-Interim-Interval imposta l'intervallo (in secondi) tra ciascun aggiornamento interim inviate dal server di accesso di rete. In base alla specifica RFC 2869, il valore dell'attributo Acct-Interim-Interval non deve essere inferiore a 60 secondi, ovvero un minuto e non deve essere inferiore a 600 secondi o 10 minuti. Per ulteriori informazioni, vedere RFC 2869, "RADIUS estensioni".

- Verificare che nei server dei criteri di rete è abilitata la registrazione di stato periodici.

