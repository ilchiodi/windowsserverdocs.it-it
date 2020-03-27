---
title: Pianificare Servizi dei criteri di rete come server RADIUS
description: In questo argomento vengono fornite informazioni sulla pianificazione della distribuzione del server RADIUS del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0e012746841bcf736b7698afb5d7c807194bbec0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315735"
---
# <a name="plan-nps-as-a-radius-server"></a>Pianificare Servizi dei criteri di rete come server RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando si distribuisce un server dei criteri di rete \(NPS\) come server Remote Authentication Dial-In User Service (RADIUS), NPS esegue l'autenticazione, l'autorizzazione e l'accounting per le richieste di connessione per il dominio locale e per i domini che considerano attendibile il dominio locale. È possibile usare queste linee guida di pianificazione per semplificare la distribuzione RADIUS.

Queste linee guida di pianificazione non includono le circostanze in cui si desidera distribuire NPS come proxy RADIUS. Quando si distribuisce NPS come proxy RADIUS, NPS Invia le richieste di connessione a un server che esegue NPS o altri server RADIUS in domini remoti, domini non trusted o entrambi. 

Prima di distribuire server dei criteri di rete come server RADIUS nella rete, usare le linee guida seguenti per pianificare la distribuzione.

- Pianificare la configurazione di NPS.

- Pianificare i client RADIUS.

- Pianificare l'utilizzo dei metodi di autenticazione.

- Pianificare i criteri di rete.

- Pianificare l'accounting NPS.

## <a name="plan-nps-configuration"></a>Pianificare la configurazione di NPS

È necessario decidere in quale dominio è membro il server dei criteri di dominio. Per gli ambienti con più domini, un server dei criteri di server può autenticare le credenziali per gli account utente nel dominio di cui è membro e per tutti i domini che considerano attendibile il dominio locale del server dei criteri di server. Per consentire al server dei criteri di gruppo di leggere le proprietà di connessione degli account utente durante il processo di autorizzazione, è necessario aggiungere l'account computer del server dei criteri di gruppo al gruppo RAS e NPSs per ogni dominio.

Dopo aver determinato l'appartenenza al dominio del server dei criteri di rete, è necessario configurare il server per la comunicazione con i client RADIUS, detti anche server di accesso alla rete, usando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi registrati da NPS nel registro eventi ed è possibile immettere una descrizione per il server.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione della configurazione di NPS, è possibile usare i passaggi seguenti.

- Determinare le porte RADIUS utilizzate dal server dei criteri di server per la ricezione di messaggi RADIUS dai client RADIUS. Le porte predefinite sono le porte UDP 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte 1813 e 1646 per i messaggi di accounting RADIUS.

- Se il server dei criteri di rete è configurato con più schede di rete, determinare le schede in cui si desidera consentire il traffico RADIUS.

- Determinare i tipi di eventi che si desidera registrare nel registro eventi del server dei criteri di server. È possibile registrare le richieste di autenticazione rifiutate, le richieste di autenticazione riuscite o entrambi i tipi di richieste.

- Determinare se si intende distribuire più server dei criteri di server. Per fornire la tolleranza di errore per l'autenticazione e l'accounting basati su RADIUS, usare almeno due NPSs. Un server dei criteri di database viene utilizzato come server RADIUS primario e l'altro viene utilizzato come backup. Ogni client RADIUS viene quindi configurato in entrambi i NPSs. Se il server dei criteri di database primario diventa non disponibile, i client RADIUS inviano messaggi di richiesta di accesso al server dei criteri di accesso alternativo.

- Pianificare lo script utilizzato per copiare una configurazione del server dei criteri di server in altri NPSs per risparmiare sul sovraccarico amministrativo e per impedire la cofigurazione errata di un server. NPS fornisce i comandi netsh che consentono di copiare interamente o in parte una configurazione NPS per l'importazione in un altro server dei criteri di accesso. È possibile eseguire manualmente i comandi al prompt di netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se si decide di modificare le configurazioni del server.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, server VPN (Virtual Private Network), commutatori compatibili con 802.1 X e server di connessione remota. I proxy RADIUS, che inoltrano i messaggi di richiesta di connessione ai server RADIUS, sono anche client RADIUS. NPS supporta tutti i server di accesso alla rete e i proxy RADIUS conformi al protocollo RADIUS come descritto in RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)" e RFC 2866, "RADIUS Accounting".

>[!IMPORTANT]
>I client di Access, ad esempio i computer client, non sono client RADIUS. Solo i server di accesso alla rete e i server proxy che supportano il protocollo RADIUS sono client RADIUS.

Inoltre, sia i punti di accesso wireless che i commutatori devono essere in grado di supportare l'autenticazione 802.1 X. Se si desidera distribuire Extensible Authentication Protocol \(EAP\) o Protected Extensible Authentication Protocol \(PEAP\), i punti di accesso e i commutatori devono supportare l'utilizzo di EAP.

Per testare l'interoperabilità di base per le connessioni PPP per i punti di accesso wireless, configurare il punto di accesso e il client di accesso per l'uso di Password Authentication Protocol (PAP). Usare protocolli di autenticazione basati su PPP aggiuntivi, ad esempio PEAP, fino a quando non sono stati testati quelli che si intende usare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione dei client RADIUS, è possibile usare i passaggi seguenti.

- Documentare gli attributi specifici del fornitore (VSA) che è necessario configurare in NPS. Se i server di accesso alla rete richiedono VSA, registrare le informazioni VSA per un uso successivo quando si configurano i criteri di rete nel server dei criteri di rete. 

- Documentare gli indirizzi IP dei client RADIUS e NPS per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurarli per l'utilizzo del protocollo RADIUS, con l'indirizzo IP del server dei criteri di server immesso come server di autenticazione. Quando si configura NPS per la comunicazione con i client RADIUS, è necessario immettere gli indirizzi IP del client RADIUS nello snap-in NPS.

- Creare segreti condivisi per la configurazione sui client RADIUS e nello snap-in NPS. È necessario configurare i client RADIUS con un segreto condiviso, o una password, che sarà anche possibile immettere nello snap-in NPS durante la configurazione dei client RADIUS in NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Pianificare l'uso dei metodi di autenticazione

NPS supporta sia i metodi di autenticazione basati su password che quelli basati sui certificati. Tuttavia, non tutti i server di accesso alla rete supportano gli stessi metodi di autenticazione. In alcuni casi, potrebbe essere necessario distribuire un metodo di autenticazione diverso in base al tipo di accesso alla rete.

Ad esempio, potrebbe essere necessario distribuire l'accesso wireless e VPN per l'organizzazione, ma usare un metodo di autenticazione diverso per ogni tipo di accesso: EAP-TLS per le connessioni VPN, a causa della sicurezza avanzata che EAP con Transport Layer Security (EAP-TLS) fornisce e PEAP-MS-CHAP v2 per le connessioni wireless 802.1 X.

Il protocollo PEAP con Microsoft Challenge Handshake Authentication Protocol versione 2 (PEAP-MS-CHAP v2) fornisce una funzionalità denominata riconnessione rapida progettata appositamente per l'uso con computer portatili e altri dispositivi wireless. La riconnessione rapida consente ai client wireless di spostarsi tra punti di accesso wireless nella stessa rete senza essere riautenticati ogni volta che si associano a un nuovo punto di accesso. Questo offre un'esperienza migliore per gli utenti wireless e consente loro di spostarsi tra i punti di accesso senza dover digitare nuovamente le proprie credenziali.
Grazie alla riconnessione rapida e alla sicurezza fornita da PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 è una scelta logica come metodo di autenticazione per le connessioni wireless.

Per le connessioni VPN, EAP-TLS è un metodo di autenticazione basato su certificati che garantisce una sicurezza avanzata che protegge il traffico di rete, anche quando viene trasmesso attraverso Internet da computer domestici o portatili ai server VPN dell'organizzazione.

### <a name="certificate-based-authentication-methods"></a>Metodi di autenticazione basati su certificati

I metodi di autenticazione basati su certificati hanno il vantaggio di garantire una sicurezza avanzata; e presentano gli svantaggi di essere più difficili da distribuire rispetto ai metodi di autenticazione basati su password.

Sia PEAP-MS-CHAP v2 che EAP-TLS sono metodi di autenticazione basati sui certificati, ma vi sono molte differenze tra di essi e il modo in cui vengono distribuiti.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa i certificati per l'autenticazione client e server e richiede la distribuzione di un'infrastruttura a chiave pubblica (PKI) nell'organizzazione. La distribuzione di un'infrastruttura a chiave pubblica può essere complessa e richiede una fase di pianificazione indipendente dalla pianificazione dell'utilizzo di NPS come server RADIUS.

Con EAP-TLS, NPS registra un certificato server da un'autorità di certificazione \(CA\)e il certificato viene salvato nel computer locale nell'archivio certificati. Durante il processo di autenticazione, l'autenticazione server si verifica quando il server dei criteri di server invia il proprio certificato server al client di accesso per dimostrare la propria identità al client Access. Il client di accesso esamina varie proprietà del certificato per determinare se il certificato è valido ed è appropriato per l'utilizzo durante l'autenticazione del server. Se il certificato del server soddisfa i requisiti minimi del certificato del server e viene emesso da un'autorità di certificazione che il client di accesso considera attendibile, il server dei criteri di accesso viene autenticato dal client.

Analogamente, l'autenticazione client si verifica durante il processo di autenticazione quando il client invia il certificato client al server dei criteri di server per dimostrare la propria identità al server dei criteri di server. Il server dei criteri di dominio esamina il certificato e se il certificato client soddisfa i requisiti minimi del certificato client e viene emesso da una CA considerata attendibile dal server dei criteri di accesso, il client di accesso viene autenticato correttamente dal server dei criteri di dominio.

Sebbene sia necessario che il certificato del server sia archiviato nell'archivio certificati nel server dei criteri di server, il certificato client o utente può essere archiviato nell'archivio certificati del client o in una smart card.

Affinché il processo di autenticazione abbia esito positivo, è necessario che tutti i computer dispongano del certificato CA dell'organizzazione nell'archivio certificati delle autorità di certificazione radice attendibili per il computer locale e l'utente corrente.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 usa un certificato per l'autenticazione del server e le credenziali basate su password per l'autenticazione utente. Poiché i certificati vengono utilizzati solo per l'autenticazione server, non è necessario distribuire un'infrastruttura a chiave pubblica per utilizzare PEAP-MS-CHAP v2. Quando si distribuisce PEAP-MS-CHAP v2, è possibile ottenere un certificato server per NPS in uno dei due modi seguenti:

- È possibile installare Active Directory Servizi certificati (AD CS), quindi registrare automaticamente i certificati in NPSs. Se si usa questo metodo, è necessario registrare anche il certificato della CA nei computer client che si connettono alla rete in modo che considerino attendibile il certificato rilasciato al server dei criteri di rete.

- È possibile acquistare un certificato server da un'autorità di certificazione pubblica, ad esempio VeriSign. Se si usa questo metodo, assicurarsi di selezionare una CA già considerata attendibile dai computer client. Per determinare se i computer client considerano attendibile una CA, aprire lo snap-in certificati di Microsoft Management Console (MMC) in un computer client, quindi visualizzare l'archivio Autorità di certificazione radice attendibili per il computer locale e per l'utente corrente. Se è presente un certificato dell'autorità di certificazione in questi archivi certificati, il computer client considera attendibile l'autorità di certificazione e pertanto considera attendibile qualsiasi certificato emesso dalla CA.

Durante il processo di autenticazione con PEAP-MS-CHAP v2, l'autenticazione server si verifica quando il server dei criteri di server invia il proprio certificato server al computer client. Il client di accesso esamina varie proprietà del certificato per determinare se il certificato è valido ed è appropriato per l'utilizzo durante l'autenticazione del server. Se il certificato del server soddisfa i requisiti minimi del certificato del server e viene emesso da un'autorità di certificazione che il client di accesso considera attendibile, il server dei criteri di accesso viene autenticato dal client.

L'autenticazione utente si verifica quando un utente tenta di connettersi alle credenziali basate su password dei tipi di rete e tenta di accedere. NPS riceve le credenziali ed esegue l'autenticazione e l'autorizzazione. Se l'utente viene autenticato e autorizzato correttamente e se il computer client ha eseguito correttamente l'autenticazione del server dei criteri di accesso, viene concessa la richiesta di connessione.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'utilizzo dei metodi di autenticazione, è possibile utilizzare i passaggi seguenti.

- Identificare i tipi di accesso alla rete che si intende offrire, ad esempio wireless, VPN, Commuter compatibile con 802.1 X e accesso remoto.

- Determinare il metodo o i metodi di autenticazione che si desidera utilizzare per ogni tipo di accesso. Si consiglia di utilizzare i metodi di autenticazione basati sui certificati che garantiscono una sicurezza avanzata; Tuttavia, potrebbe non essere pratico distribuire un'infrastruttura a chiave pubblica (PKI), in modo che altri metodi di autenticazione possano offrire un migliore equilibrio tra i dati necessari per la rete.

- Se si distribuisce EAP-TLS, pianificare la distribuzione dell'infrastruttura a chiave pubblica (PKI). Ciò include la pianificazione dei modelli di certificato che si intende usare per i certificati del server e i certificati del computer client. Include anche la determinazione della modalità di registrazione dei certificati per i computer membri del dominio e non appartenenti al dominio e di determinare se si desidera utilizzare smart card.

- Se si distribuisce PEAP-MS-CHAP v2, determinare se si desidera installare Servizi certificati Active Directory per emettere certificati server per la NPSs o se si desidera acquistare certificati server da una CA pubblica, ad esempio VeriSign.

### <a name="plan-network-policies"></a>Pianificare i criteri di rete

I criteri di rete vengono utilizzati da server dei criteri di rete per determinare se le richieste di connessione ricevute dai client RADIUS sono autorizzate NPS usa anche le proprietà di connessione remota dell'account utente per eseguire una determinazione dell'autorizzazione.

Poiché i criteri di rete vengono elaborati nell'ordine in cui sono visualizzati nello snap-in NPS, pianificare di inserire prima i criteri più restrittivi nell'elenco dei criteri. Per ogni richiesta di connessione, NPS tenta di trovare una corrispondenza tra le condizioni dei criteri e le proprietà della richiesta di connessione. NPS esamina ogni criterio di rete in ordine fino a quando non viene trovata una corrispondenza. Se non viene trovata alcuna corrispondenza, la richiesta di connessione viene rifiutata.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione dei criteri di rete, è possibile utilizzare i passaggi seguenti.

- Determinare l'ordine di elaborazione NPS preferito dei criteri di rete, dal più restrittivo al meno restrittivo.

- Determinare lo stato dei criteri. Il valore dello stato dei criteri può essere Enabled o Disabled. Se il criterio è abilitato, NPS valuta i criteri durante l'esecuzione dell'autorizzazione. Se il criterio non è abilitato, non viene valutato.

- Determinare il tipo di criteri. È necessario determinare se il criterio è stato progettato per concedere l'accesso quando le condizioni dei criteri vengono confrontate con la richiesta di connessione o se il criterio è stato progettato per negare l'accesso quando le condizioni dei criteri corrispondono alla richiesta di connessione. Se ad esempio si vuole negare in modo esplicito l'accesso wireless ai membri di un gruppo di Windows, è possibile creare un criterio di rete che specifichi il gruppo, il metodo di connessione wireless e con un'impostazione del tipo di Criteri nega accesso.

- Determinare se si desidera che il server dei criteri di gruppo ignori le proprietà di accesso remoto degli account utente che sono membri del gruppo su cui si basa il criterio. Se questa impostazione non è abilitata, le proprietà di accesso remoto degli account utente sostituiscono le impostazioni configurate nei criteri di rete. Se, ad esempio, è configurato un criterio di rete che concede l'accesso a un utente, ma le proprietà di connessione dell'account utente per tale utente sono impostate su Nega accesso, all'utente viene negato l'accesso. Tuttavia, se si Abilita l'impostazione tipo di criteri Ignora proprietà connessione account utente, allo stesso utente viene concesso l'accesso alla rete.

- Determinare se il criterio utilizza l'impostazione dell'origine dei criteri. Questa impostazione consente di specificare facilmente un'origine per tutte le richieste di accesso. Le origini possibili sono un gateway di Servizi terminal (Gateway Servizi terminal), un server di accesso remoto (VPN o connessione remota), un server DHCP, un punto di accesso wireless e un server Autorità registrazione integrità. In alternativa, è possibile specificare un'origine specifica del fornitore.

- Determinare le condizioni per le quali è necessario trovare una corrispondenza per applicare i criteri di rete.

- Determinare le impostazioni che vengono applicate se le condizioni dei criteri di rete corrispondono alla richiesta di connessione.

- Determinare se si desidera utilizzare, modificare o eliminare i criteri di rete predefiniti.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

NPS offre la possibilità di registrare i dati di accounting RADIUS, ad esempio le richieste di autenticazione e accounting degli utenti, in tre formati: formato IAS, formato compatibile con il database e registrazione Microsoft SQL Server. 

Formato IAS e formato compatibile con il database creare file di log nel server dei criteri di accesso locale in formato file di testo. 

SQL Server registrazione consente di accedere a un database SQL Server 2000 o SQL Server 2005 conforme a XML, estendendo l'accounting RADIUS per sfruttare i vantaggi della registrazione in un database relazionale.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS, è possibile utilizzare i passaggi seguenti.

- Determinare se si desidera archiviare i dati di contabilità NPS nei file di log o in un database SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilità NPS con i file di log locali

La registrazione delle richieste di autenticazione e accounting degli utenti nei file di log viene utilizzata principalmente per scopi di analisi della connessione e fatturazione ed è utile anche come strumento di analisi della sicurezza, fornendo un metodo per tenere traccia dell'attività di un utente malintenzionato dopo un attacco.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS utilizzando i file di log locali, è possibile utilizzare i passaggi seguenti.

- Determinare il formato del file di testo che si desidera utilizzare per i file di log di NPS.

- Scegliere il tipo di informazioni che si desidera registrare. È possibile registrare le richieste di contabilità, le richieste di autenticazione e lo stato periodico.

- Determinare il percorso del disco rigido in cui si desidera archiviare i file di log.

- Progettare la soluzione di backup del file di log. Il percorso del disco rigido in cui archiviare i file di log deve essere un percorso che consente di eseguire facilmente il backup dei dati. Inoltre, il percorso del disco rigido deve essere protetto tramite la configurazione dell'elenco di controllo di accesso (ACL) per la cartella in cui sono archiviati i file di log.

- Determinare la frequenza con cui si desidera creare nuovi file di log. Se si desidera che i file di log vengano creati in base alle dimensioni del file, determinare le dimensioni massime consentite prima che venga creato un nuovo file di log da NPS.

- Determinare se si desidera che NPS elimini i file di log meno recenti se il disco rigido esaurisce lo spazio di archiviazione.

- Determinare l'applicazione o le applicazioni che si desidera utilizzare per visualizzare i dati di contabilità e generare report.

### <a name="nps-sql-server-logging"></a>Registrazione SQL Server NPS

NPS SQL Server la registrazione viene utilizzata quando sono necessarie informazioni sullo stato della sessione, per la creazione di report e l'analisi dei dati, nonché per centralizzare e semplificare la gestione dei dati contabili.

Server dei criteri di rete consente di usare SQL Server registrazione per registrare le richieste di autenticazione e accounting degli utenti ricevute da uno o più server di accesso alla rete a un'origine dati in un computer che esegue Microsoft SQL Server Desktop Engine \(MSDE 2000\)o qualsiasi versione di SQL Server successiva a SQL Server 2000.

I dati di contabilità vengono passati da server dei criteri di database in formato XML a una stored procedure nel database, che supporta sia il linguaggio di query strutturato \(SQL\) che XML \(SQLXML\). La registrazione delle richieste di autenticazione e accounting degli utenti in un database di SQL Server conforme a XML consente a più NPSs di avere un'origine dati.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS utilizzando NPS SQL Server la registrazione, è possibile utilizzare i passaggi seguenti.

- Determinare se l'utente o un altro membro dell'organizzazione ha SQL Server esperienza di sviluppo di database relazionali 2000 o SQL Server 2005 e si è appreso come usare questi prodotti per creare, modificare, amministrare e gestire SQL Server database.

- Determinare se SQL Server è installato nel server dei criteri di server o in un computer remoto.

- Progettare la stored procedure che verrà utilizzata nel database di SQL Server per elaborare i file XML in arrivo che contengono i dati di contabilità NPS.

- Progettare la struttura e il flusso di replica del database di SQL Server.

- Determinare l'applicazione o le applicazioni che si desidera utilizzare per visualizzare i dati di contabilità e generare report.

- Pianificare l'uso di server di accesso alla rete che inviano l'attributo della classe in tutte le richieste di accounting. L'attributo Class viene inviato al client RADIUS in un messaggio di accettazione dell'accesso ed è utile per la correlazione dei messaggi di richiesta di contabilità con sessioni di autenticazione. Se l'attributo Class viene inviato dal server di accesso alla rete nei messaggi di richiesta di contabilità, può essere usato per trovare la corrispondenza con i record di accounting e di autenticazione. La combinazione degli attributi Unique-Serial-Number, Service-Reboot-Time e server-address deve essere un'identificazione univoca per ogni autenticazione accettata dal server.

- Pianificare l'utilizzo di server di accesso alla rete che supportano la contabilità temporanea.

- Pianificare l'utilizzo di server di accesso alla rete che inviano messaggi di contabilità e di accounting.

- Pianificare l'utilizzo di server di accesso alla rete che supportano l'archiviazione e l'invio di dati di contabilità. I server di accesso alla rete che supportano questa funzionalità possono archiviare i dati di contabilità quando il server di accesso alla rete non è in grado di comunicare con NPS. Quando il server dei criteri di rete è disponibile, il server di accesso alla rete trasmette i record archiviati al server dei criteri di rete, garantendo una maggiore affidabilità nella contabilità dei server di accesso alla rete che non forniscono questa funzionalità.

- Pianificare di configurare sempre l'attributo Acct-Interim-Interval nei criteri di rete. L'attributo Acct-Interim-Interval imposta l'intervallo (in secondi) tra ogni aggiornamento provvisorio inviato dal server di accesso alla rete. In base allo standard RFC 2869, il valore dell'attributo Acct-Interim-Interval non deve essere minore di 60 secondi, un minuto e non deve essere inferiore a 600 secondi oppure 10 minuti. Per ulteriori informazioni, vedere RFC 2869, "RADIUS Extensions".

- Assicurarsi che la registrazione dello stato periodico sia abilitata nella NPSs.

