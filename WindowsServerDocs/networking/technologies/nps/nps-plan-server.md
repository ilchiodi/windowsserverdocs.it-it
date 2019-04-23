---
title: Pianificare Servizi dei criteri di rete come server RADIUS
description: In questo argomento vengono fornite informazioni sulla distribuzione di server RADIUS di Server dei criteri di rete relativa alla pianificazione in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5fd89ef8d95735b8cbe1334ba51ed0059595dbc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839172"
---
# <a name="plan-nps-as-a-radius-server"></a>Pianificare Servizi dei criteri di rete come server RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando si distribuisce il Server dei criteri di rete \(NPS\) come server RADIUS Remote Authentication Dial-In User Service (), dei criteri di rete esegue l'autenticazione, autorizzazione e accounting per le richieste di connessione per il dominio locale e per i domini che considera attendibile il dominio locale. È possibile usare queste linee guida sulla pianificazione per semplificare la distribuzione RADIUS.

Queste linee guida per la pianificazione non includono circostanze in cui si desidera distribuire dei criteri di rete come proxy RADIUS. Quando si distribuiscono criteri di rete come proxy RADIUS, dei criteri di rete inoltra le richieste di connessione a un server che eseguono criteri di rete o altri server RADIUS in domini remoti, domini non trusted o entrambi. 

Prima di distribuire criteri di rete come server RADIUS in rete, usare le linee guida seguenti per pianificare la distribuzione.

- Pianificare la configurazione dei criteri di rete.

- Pianificare i client RADIUS.

- Pianificare l'uso dei metodi di autenticazione.

- Pianificare i criteri di rete.

- Pianificare l'accounting NPS.

## <a name="plan-nps-configuration"></a>Pianificare la configurazione dei criteri di rete

È necessario decidere in quale dominio NPS è un membro. Per gli ambienti di più domini, un NPS è possibile autenticare le credenziali per gli account utente nel dominio di cui è membro e per tutti i domini trusting del dominio locale di NPS. Per consentire i criteri di rete leggere le proprietà di connessione remota degli account utente durante il processo di autorizzazione, è necessario aggiungere l'account computer dei criteri di rete al gruppo di server RAS e NPSs per ogni dominio.

Dopo aver determinato l'appartenenza al dominio di NPS, il server deve essere configurato per comunicare con i client RADIUS, chiamati anche il server di accesso di rete, utilizzando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi registrati da criteri di rete nel caso in cui log ed è possibile specificare una descrizione per il server.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per la configurazione dei criteri di rete, è possibile usare la procedura seguente.

- Determinare le porte RADIUS NPS utilizzata per ricevere i messaggi RADIUS dai client RADIUS. Le porte predefinite sono le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte 1813 e 1646 per i messaggi di accounting RADIUS.

- Se i criteri di rete è configurata con più schede di rete, determinare gli adapter su cui si desidera che il traffico RADIUS deve essere autorizzato.

- Determinare i tipi di eventi che si desidera registrare nel Log eventi dei criteri di rete. È possibile registrare le richieste di autenticazione rifiutati, le richieste di autenticazione riusciti o entrambi i tipi di richieste.

- Determinare se si distribuiscono più criteri di rete. Per fornire tolleranza di errore per l'autenticazione basata su RADIUS e accounting, usare almeno due NPSs. Uno dei criteri di rete viene utilizzato come server RADIUS primario e l'altro viene usato come backup. Quindi, ogni client RADIUS è configurato in entrambi NPSs. Se i criteri di rete primaria non è più disponibile, i client RADIUS, quindi inviano i messaggi di richiesta di accesso ai criteri di rete alternativo.

- Pianificare lo script usato per copiare una configurazione di criteri di rete per altri NPSs per risparmiare sui costi generali amministrativi e per impedire il corretto della configurazione di un server. Criteri di rete offre i comandi Netsh per la copia tutta o parte di una configurazione di NPS per l'importazione in un'altra dei criteri di rete. È possibile eseguire manualmente i comandi al prompt dei comandi di Netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se si decide di modificare le configurazioni del server.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso di rete, ad esempio punti di accesso wireless, server di rete privata virtuale (VPN), 802.1 commutatori 802.1X e i server di accesso remoto. I proxy RADIUS, che inoltrano i messaggi di richiesta per i server RADIUS di connessione, sono anche i client RADIUS. NPS supporta tutti i server di accesso di rete e i proxy RADIUS che rispettano il raggio del protocollo come descritto nella RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)," e RFC 2866 "Accounting RADIUS".

>[!IMPORTANT]
>I client di accesso, ad esempio i computer client, non sono client RADIUS. Solo i server proxy che supportano il protocollo RADIUS e server di accesso alla rete sono client RADIUS.

Inoltre, entrambe opzioni e i punti di accesso wireless devono essere in grado di autenticazione 802.1x. Se si desidera distribuire Extensible Authentication Protocol \(EAP\) o Protected Extensible Authentication Protocol \(PEAP\), commutatori e i punti di accesso devono supportare l'uso di EAP.

Per testare l'interoperabilità di base per le connessioni PPP per punti di accesso wireless, configurare il punto di accesso e il client di accesso per usare l'autenticazione protocollo PAP (Password). Usare protocolli di autenticazione basate su PPP aggiuntivi, ad esempio PEAP, fino a quando il test di quelli che si intendono usare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per i client RADIUS, è possibile usare la procedura seguente.

- Documentare gli attributi specifici del fornitore (VSA), che è necessario configurare in Criteri di rete. Se i server di accesso di rete richiedono VSA, registrare le informazioni di VSA per un utilizzo successivo quando si configurano i criteri di rete in Criteri di rete. 

- Documentare gli indirizzi IP dei computer client RADIUS e i criteri di rete per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurare in modo che usino il protocollo RADIUS, con l'indirizzo IP di NPS immesso come server di autenticazione. E quando si configurano criteri di rete per comunicare con i client RADIUS, è necessario immettere gli indirizzi IP di client RADIUS nello snap-in dei criteri di rete.

- Creare i segreti condivisi per la configurazione nei client RADIUS e nello snap-in NPS. È necessario configurare i client RADIUS con un segreto condiviso, o una password, che immetterà anche nello snap-in Criteri di rete durante la configurazione del client RADIUS in Criteri di rete.

## <a name="plan-the-use-of-authentication-methods"></a>Pianificare l'uso dei metodi di autenticazione

NPS supporta entrambi i metodi di autenticazione basato su password e basata su certificati. Tuttavia, non tutti i server di accesso di rete supportano gli stessi metodi di autenticazione. In alcuni casi, si potrebbe essere necessario distribuire un metodo di autenticazione diverso in base al tipo di accesso alla rete.

Potrebbe ad esempio, si desidera distribuire sia wireless e VPN per l'organizzazione, ma usare un metodo di autenticazione diverso per ogni tipo di accesso: EAP-TLS per le connessioni VPN, a causa la sicurezza avanzata che fornisce EAP con Transport Layer Security (EAP-TLS) e PEAP-MS-CHAP v2 per connessioni wireless 802.1x.

Riconnessione PEAP con Microsoft Challenge Handshake Authentication Protocol versione 2 (PEAP-MS-CHAP v2) fornisce una funzionalità denominata fast progettato espressamente per l'uso con i computer portatili e altri dispositivi wireless. Riconnessione rapida consente ai client senza fili per spostarsi tra i punti di accesso wireless nella stessa rete senza essere riautenticati ogni volta che vengono associati a un nuovo punto di accesso. Ciò offre un'esperienza migliore per gli utenti wireless e consente di spostarsi tra i punti di accesso senza dover immettere nuovamente le proprie credenziali.
A causa di fast ristabilire la connessione e la sicurezza che fornisce il protocollo PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 è una scelta logica come metodo di autenticazione per le connessioni wireless.

Per le connessioni VPN, EAP-TLS è un metodo di autenticazione basata su certificati che offre una protezione avanzata che protegge il traffico di rete anche se quest'ultima viene trasmessa nella rete Internet dal computer di casa o per dispositivi mobili per i server VPN di organizzazione.

### <a name="certificate-based-authentication-methods"></a>Metodi di autenticazione basata su certificati

Metodi di autenticazione basata su certificati hanno il vantaggio di fornire protezione avanzata. e hanno lo svantaggio di essere più difficili da distribuire rispetto ai metodi di autenticazione basata su password.

PEAP-MS-CHAP v2 sia EAP-TLS sono metodi di autenticazione basata su certificati, ma esistono molte differenze tra loro e il modo in cui vengono distribuiti.

### <a name="eap-tls"></a>Protocollo EAP-TLS

EAP-TLS Usa i certificati per l'autenticazione client e server e richiede la distribuzione di un'infrastruttura a chiave pubblica (PKI) all'interno dell'organizzazione. Distribuire un'infrastruttura PKI può risultare complesso e richiede una fase di pianificazione che è indipendente dalla pianificazione per l'uso di criteri di rete come server RADIUS.

Con EAP-TLS, i criteri di rete viene registrato un certificato del server da un'autorità di certificazione \(CA\), e il certificato salvato nel computer locale nell'archivio certificati. Durante il processo di autenticazione, l'autenticazione server si verifica quando i criteri di rete invia il certificato del server al client di accesso per dimostrare la propria identità ai client di accesso. Il client di accesso vengono esaminate diverse proprietà del certificato per determinare se il certificato è valido e può essere usata durante l'autenticazione server. Se il certificato del server soddisfi i requisiti del certificato server minimo e viene emesso da un'autorità di certificazione considerata attendibile dal client l'accesso, i criteri di rete viene correttamente autenticato dal client.

Analogamente, l'autenticazione del client si verifica durante il processo di autenticazione quando il client invia il certificato client per i criteri di rete per dimostrare la propria identità ai criteri di rete. I criteri di rete esamina il certificato e se il certificato client soddisfa i requisiti del certificato client minima e viene emesso da un'autorità di certificazione che considera attendibile i criteri di rete, il client di accesso viene correttamente autenticato da NPS.

Anche se è necessario che il certificato del server viene archiviato nell'archivio certificati su NPS, il certificato client o utente può essere archiviato nell'archivio di certificati sul client o in una smart card.

Per questo processo di autenticazione abbia esito positivo, è necessario che tutti i computer hanno certificati di autorità di certificazione dell'organizzazione nell'archivio certificati Autorità di certificazione radice attendibili per il Computer locale e l'utente corrente.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 utilizza un certificato per l'autenticazione server e credenziali basate su password per l'autenticazione utente. Poiché i certificati vengono usati solo per l'autenticazione server, non si è necessaria per distribuire un'infrastruttura a chiave pubblica per poter utilizzare PEAP-MS-CHAP v2. Quando si distribuisce PEAP-MS-CHAP v2, è possibile ottenere un certificato del server per i criteri di rete in uno dei due modi seguenti:

- È possibile installare Servizi certificati Active Directory (AD CS) e quindi registrare automaticamente i certificati per NPSs. Se si usa questo metodo, è anche necessario registrare il certificato della CA per i computer client connessi alla rete in modo che ritengano attendibile il certificato emesso per i criteri di rete.

- È possibile acquistare un certificato server da una CA pubblica, ad esempio VeriSign. Se si usa questo metodo, assicurarsi di selezionare un'autorità di certificazione è già considerato attendibile dai computer client. Per determinare se i computer client considera attendibile un'autorità di certificazione, aprire lo snap-in certificati di Microsoft Management Console (MMC) in un computer client e quindi visualizzare l'archivio di autorità di certificazione radice attendibili per il Computer locale e per l'utente corrente. Se è presente un certificato dall'autorità di certificazione in questi archivi certificati, il computer client considera attendibile l'autorità di certificazione e pertanto considereranno attendibile qualsiasi certificato rilasciato dall'autorità di certificazione.

Durante il processo di autenticazione PEAP-MS-CHAP v2 l'autenticazione server si verifica quando i criteri di rete invia il certificato del server nel computer client. Il client di accesso vengono esaminate diverse proprietà del certificato per determinare se il certificato è valido e può essere usata durante l'autenticazione server. Se il certificato del server soddisfi i requisiti del certificato server minimo e viene emesso da un'autorità di certificazione considerata attendibile dal client l'accesso, i criteri di rete viene correttamente autenticato dal client.

L'autenticazione dell'utente si verifica quando un utente tenta di connettersi alle credenziali di rete tipi basato su password e tenta di accedere. Criteri di rete riceve le credenziali ed esegue l'autenticazione e autorizzazione. Se l'utente viene autenticato e autorizzato correttamente e se il computer client viene autenticato correttamente i criteri di rete, la richiesta di connessione è consentita.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'uso dei metodi di autenticazione, è possibile usare la procedura seguente.

- Identificare i tipi di accesso remoto e si prevede di offrire, ad esempio wireless, VPN, 802.1x switch 802.1X, l'accesso alla rete.

- Determinare il metodo di autenticazione o i metodi che si desidera utilizzare per ogni tipo di accesso. È consigliabile usare i metodi di autenticazione basata su certificati che offrono sicurezza avanzata; Tuttavia, potrebbe non essere utile distribuire un'infrastruttura a chiave pubblica, in modo che altri metodi di autenticazione potrebbero fornire un equilibrio migliore di ciò che occorre per la rete.

- Se si intende distribuire EAP-TLS, pianificare la distribuzione di infrastruttura a chiave pubblica. Ciò consente di pianificare i modelli di certificato che si intende usare per i certificati del server e i certificati del computer client. Include inoltre determinare la modalità di registrazione certificati al membro del dominio e i computer non appartenente al dominio e determinare se si desidera utilizzare smart card.

- Se si distribuisce PEAP-MS-CHAP v2, determinare se si desidera installare Servizi certificati Active Directory per rilasciare certificati server per il NPSs o se si desidera acquistare i certificati del server da una CA pubblica, ad esempio VeriSign.

### <a name="plan-network-policies"></a>Pianificare i criteri di rete

I criteri di rete vengono utilizzati da NPS per determinare se le richieste di connessione ricevute dai client RADIUS sono autorizzate. Criteri di rete Usa anche la proprietà di connessione remota dell'account utente per eseguire un'operazione di autorizzazione.

Poiché i criteri di rete vengono elaborati nell'ordine in cui vengono visualizzati nello snap-in NPS, prevede di inserire i criteri più restrittivi prima di tutto nell'elenco dei criteri. Per ogni richiesta di connessione, NPS tenta di ottenere le condizioni dei criteri con le proprietà di connessione richiesta. Criteri di rete viene esaminato ogni criterio di rete in ordine fino a quando non viene trovata una corrispondenza. Se non trova una corrispondenza, la richiesta di connessione viene rifiutata.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per i criteri di rete, è possibile usare la procedura seguente.

- Determinare l'ordine di elaborazione dei criteri di rete preferita di criteri di rete, dalla più restrittiva alla più restrittiva.

- Determinare lo stato dei criteri. Lo stato dei criteri può avere il valore di abilitato o disabilitato. Se è abilitato il criterio, NPS valuta i criteri durante l'esecuzione dell'autorizzazione. Se il criterio non è abilitato, non viene valutata.

- Determinare il tipo di criteri. È necessario determinare se il criterio è progettato per concedere l'accesso quando le condizioni dei criteri sono corrisposti tramite la richiesta di connessione o se il criterio è stato progettato per negare l'accesso quando le condizioni dei criteri sono corrisposti tramite la richiesta di connessione. Ad esempio, se si vuole negare l'accesso wireless in modo esplicito ai membri di un gruppo di Windows, è possibile creare un criterio di rete che specifica il gruppo e il metodo di connessione wireless che contiene un criterio di tipo di impostazione di negare l'accesso.

- Determinare se si desidera che i criteri di rete per ignorare le proprietà di connessione remota degli account utente che sono membri del gruppo in cui si basa il criterio. Quando questa impostazione non è abilitata, le proprietà di connessione remota degli account utente sostituiscono le impostazioni configurate nei criteri di rete. Ad esempio, se un criterio di rete è configurato che concede l'accesso a un utente, ma la proprietà di connessione remota dell'account utente per l'utente è impostata per negare l'accesso, l'utente viene negato l'accesso. Ma se si abilita le proprietà dell'impostazione dial-all'account utente di ignorare criteri tipo, lo stesso account utente viene concesso l'accesso alla rete.

- Determinare se il criterio Usa l'impostazione di origine dei criteri. Questa impostazione consente di specificare in modo semplice un'origine per tutte le richieste di accesso. Le possibili origini sono un Gateway di servizi Terminal (TS Gateway), un server di accesso remoto (VPN o remote), un server DHCP, un punto di accesso wireless e un server Autorità registrazione integrità. In alternativa, è possibile specificare un'origine specifiche del fornitore.

- Determinare le condizioni che devono essere corrisposte affinché i criteri di rete da applicare.

- Determinare le impostazioni che vengono applicate se vengono soddisfatte le condizioni dei criteri di rete per la richiesta di connessione.

- Determinare se si desidera usare, modificare o eliminare i criteri di rete predefinito.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

Criteri di rete offre la possibilità di registrare i dati di accounting RADIUS, ad esempio l'autenticazione dell'utente e richieste di accounting, nei tre formati: Formato di IAS, formato compatibile con il database e la registrazione di Microsoft SQL Server. 

Formato di IAS e compatibile con il database creare file di log in Criteri di rete locale nel formato di file di testo. 

La registrazione di SQL Server offre la possibilità di accedere a un database compatibile con SQL Server 2005 XML, estensione di accounting RADIUS per sfruttare i vantaggi della registrazione a un database relazionale o di SQL Server 2000.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS, è possibile usare la procedura seguente.

- Determinare se si desidera archiviare i dati di accounting di NPS nei file di log o in un database di SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Accounting di NPS utilizzando i file di log locali

La registrazione di autenticazione dell'utente e richieste nel file di log di accounting viene utilizzato principalmente per scopi di analisi e la fatturazione di connessione ed è inoltre utile come strumento di analisi di sicurezza, offrendo un metodo per tenere traccia dell'attività di un utente malintenzionato dopo un attacco.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting di NPS utilizzando i file di log locale, è possibile usare la procedura seguente.

- Determinare il formato di file di testo che si desidera utilizzare per i file di log criteri di rete.

- Scegliere il tipo di informazioni che si desidera accedere. È possibile registrare le richieste di accounting e le richieste di autenticazione periodico dello stato.

- Determinare il percorso del disco rigido in cui si desidera archiviare i file di log.

- Progettare la soluzione di backup di file di log. Il percorso del disco rigido in cui vengono archiviati i file di log deve essere un percorso che consente di eseguire facilmente il backup dei dati. Inoltre, il percorso del disco rigido deve essere protetti configurando l'elenco di controllo di accesso (ACL) per la cartella in cui sono archiviati i file di log.

- Determinare la frequenza con cui si desidera nuovi file di log da creare. Se si desidera che i file di log deve essere creato in base alle dimensioni del file, determinare le dimensioni massime del file prima che venga creato un nuovo file di log da criteri di rete.

- Determinare se si desidera che i criteri di rete per eliminare i file di log meno recenti, se il disco rigido viene esaurito tutto lo spazio di archiviazione.

- Determinare l'applicazione o le applicazioni che si desidera utilizzare per visualizzare i dati di accounting e generare report.

### <a name="nps-sql-server-logging"></a>Registrazione dei criteri di rete SQL Server

Registrazione dei criteri di rete SQL Server viene utilizzata quando le informazioni sullo stato della sessione, è necessario per scopi di analisi di dati e la creazione di report e per centralizzare e semplificare la gestione dei tuoi dati contabili.

Criteri di rete offre la possibilità di usare SQL Server per l'autenticazione utente del record di registrazione e l'accounting richieste ricevute da uno o più server di accesso di rete a un'origine dati in un computer che esegue Microsoft SQL Server Desktop Engine \(MSDE 2000\), o qualsiasi versione di SQL Server in un secondo momento rispetto a SQL Server 2000.

I dati di accounting sono passati da NPS in formato XML a una stored procedure nel database, che supporta entrambi linguaggio di query strutturate \(SQL\) e XML \(SQLXML\). L'autenticazione dell'utente di registrazione e l'accounting le richieste in un database di SQL Server compatibile con XML consente a più NPSs avere una sola origine dati.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting di NPS utilizzando la registrazione dei criteri di rete SQL Server, è possibile usare la procedura seguente.

- Determina se l'utente o un altro membro dell'organizzazione ha lo sviluppo di database relazionale di SQL Server 2000 o SQL Server 2005 esperienza ed è comprendere come usare questi prodotti per creare, modificare, amministrare e gestire database SQL Server.

- Determinare se i criteri di rete o in un computer remoto è installato SQL Server.

- Progettare stored procedure che utilizzerà nel database di SQL Server per elaborare file XML in ingresso che contengono i dati di accounting di NPS.

- Progettare la struttura della replica di database SQL Server e il flusso.

- Determinare l'applicazione o le applicazioni che si desidera utilizzare per visualizzare i dati di accounting e generare report.

- Prevede di usare server di accesso di rete che inviano l'attributo di classe in tutte le richieste di accounting. L'attributo di classe viene inviato al client RADIUS in un messaggio di autorizzazione di accesso ed è utile per la correlazione dei messaggi di richiesta di accesso con le sessioni di autenticazione. Se l'attributo di classe viene inviato dal server di accesso rete nei messaggi di richiesta di contabilità, può essere utilizzato in modo che corrisponda il record di autenticazione e accounting. La combinazione degli attributi del numero di serie univoco, tempo di riavvio del servizio e dall'indirizzo del Server deve essere un identificatore univoco per ciascuna autenticazione che il server accetta.

- Pianifica di utilizzare i server di accesso di rete che supportano accounting provvisorio.

- Prevede di usare server di accesso di rete che inviano messaggi di attivazione e disattivazione Accounting.

- Pianifica di utilizzare i server di accesso di rete che supportano l'archiviazione e l'inoltro dei dati di accounting. Server di accesso di rete che supportano questa funzionalità può archiviare i dati di accounting quando il server di accesso di rete non può comunicare con i criteri di rete. Quando i criteri di rete è disponibile, il server di accesso di rete inoltra i record archiviati per i criteri di rete, fornendo una maggiore affidabilità accounting sul server di accesso di rete che non forniscono questa funzionalità.

- Pianificare la configurazione sempre l'attributo Interim-Acct-Interval nei criteri di rete. L'attributo Interim-Acct-Interval imposta l'intervallo (in secondi) tra ciascun aggiornamento interim che invia il server di accesso di rete. In base alla specifica RFC 2869, il valore dell'attributo Interim-Acct-Interval non deve essere minore di 60 secondi, ovvero un minuto e non deve essere inferiore a 600 secondi, o 10 minuti. Per altre informazioni, vedere RFC 2869, "Estensioni RADIUS".

- Verificare che la registrazione dello stato periodica è abilitata nel NPSs.

