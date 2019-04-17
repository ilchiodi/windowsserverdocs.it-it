---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Risoluzione dei problemi di replica di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Risoluzione dei problemi di replica di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Problemi di replica di Active Directory possono avere diverse origini. Ad esempio, i problemi del sistema DNS (Domain Name), problemi di rete o problemi di sicurezza tutti potrebbe di replica di Active Directory. 

Il resto di questo argomento illustra gli strumenti e la metodologia generale per correggere gli errori di replica di Active Directory. Per un laboratorio in cui viene illustrato come risolvere i problemi di replica di Active Directory, vedere [laboratorio virtuale TechNet: risoluzione dei problemi di Active Directory replica](https://go.microsoft.com/?linkid=9844718)

I seguenti argomenti secondari coprono sintomi, cause e come risolvere gli errori di replica specifico:
   
[Correzione residui oggetto problemi di replica (ID eventi 1388, 1988, 2042)](https://technet.microsoft.com/library/cc949124.aspx)
  
[Risoluzione dei problemi di sicurezza della replica](https://technet.microsoft.com/library/cc949132.aspx)

[Problemi di ricerca di correzione della replica DNS (ID evento 1925, 2087, 2088)](https://technet.microsoft.com/library/cc949133.aspx)
  
[Correzione dei problemi di connettività di replica (ID evento 1925)](https://technet.microsoft.com/library/cc949131.aspx)
  
[Correzione dei problemi di topologia di replica (ID evento 1311)](https://technet.microsoft.com/library/cc949129.aspx)
  
[Verificare le funzionalità DNS per supportare la replica di Directory](https://technet.microsoft.com/library/dd728017.aspx)
  
[Errore di replica 8614 di Active Directory non può replicare con questo server poiché il tempo trascorso dall'ultima replica con il server ha superato la durata di rimozione definitiva](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[Replica errore 8524 l'operazione DSA è in grado di procedere a causa di un errore di ricerca DNS](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[Errore di replica 8456 o 8457 l'origine | server di destinazione rifiuta le richieste di replica](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[È stato negato l'accesso di replica 8453 errore di replica](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[Errore di replica 8452 contesto dei nomi è in corso di rimozione o non viene replicato dal server specificato](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[Errore di replica 5 accesso negato](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[Errore di replica -2146893022 nome principale di destinazione non è corretto](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[Non sono di errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[Replica errore 1722 server RPC non disponibile](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[Errore di replica 1396 errore di accesso il nome di account di destinazione non è corretto](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[Errore di replica 1256 il sistema remoto non è disponibile](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[Errore di replica 1127 durante l'accesso al disco rigido, un'operazione su disco non è riuscita anche dopo diversi tentativi](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[Errore di replica 8451 l'operazione di replica ha rilevato un errore del database](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[Replica errore 8606 attributi insufficienti assegnati per creare un oggetto](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introduzione e risorse per la risoluzione dei problemi di replica di Active Directory

Connessioni in entrata o errore di replica in uscita fa sì che gli oggetti di Active Directory che rappresentano topologia di replica, pianificazione di replica, i controller di dominio, gli utenti, computer, password, gruppi di sicurezza, appartenenza al gruppo e criteri di gruppo per risultare incoerenti tra i controller di dominio. Errore di replica e un'incoerenza directory causare errori operativi o risultati incoerenti, a seconda del controller di dominio contattato per l'operazione e può impedire l'applicazione di criteri di gruppo e le autorizzazioni di controllo di accesso. Servizi di dominio Active Directory (AD DS) dipende dalla connettività di rete, la risoluzione dei nomi, autenticazione e autorizzazione, il database di directory, la topologia di replica e il motore di replica. Quando la causa principale di un problema di replica non è immediatamente evidente, determinare la causa le cause possibili molti richiede sistematico eliminazione delle cause probabili. 

Per uno strumento basato su interfaccia utente per il monitoraggio della replica e diagnosticare gli errori, vedere [Active Directory replica lo stato strumento](https://www.microsoft.com/download/details.aspx?id=30005). È inoltre disponibile un [laboratorio](https://go.microsoft.com/?linkid=9844718) che illustra come usare lo stato della replica di Active Directory e altri strumenti per risolvere gli errori. 

Per un documento completo che descrive come è possibile utilizzare lo strumento Repadmin per risolvere i problemi di Active Directory la replica è disponibile. vedere [monitoraggio e risoluzione dei problemi di replica di Active Directory mediante Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Per informazioni sul funzionamento della replica di Active Directory, vedere i riferimenti tecnici seguenti:


- [Documentazione tecnica di Active Directory replica modello](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Directory replica topologia Technical Reference](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Suggerimenti sulle soluzioni di evento e lo strumento

In teoria, il rosso (errore) e giallo (avviso) gli eventi nel registro eventi del servizio Directory suggeriscono il vincolo specifico che ha causato l'errore di replica nel controller di dominio di origine o di destinazione. Se il messaggio di evento indica i passaggi per una soluzione, prova le operazioni descritte nell'evento. Lo strumento Repadmin e altri strumenti di diagnostica forniscono anche informazioni che consentono di risolvere gli errori di replica. 

Per informazioni dettagliate sull'utilizzo di Repadmin per la risoluzione dei problemi di replica, vedere [monitoraggio e risoluzione dei problemi di replica di Active Directory mediante Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Escludendo le interruzioni intenzionali o errori hardware

A volte si verificano errori di replica a causa di interruzioni intenzionali. Ad esempio, per la risoluzione dei problemi di replica di Active Directory, escludere disconnessioni intenzionale e gli errori hardware o gli aggiornamenti prima di tutto.

### <a name="intentional-disconnections"></a>Disconnessioni intenzionale

Se vengono segnalati errori di replica da un controller di dominio che sta eseguendo la replica con un controller di dominio che è stato generato in un sito di gestione temporaneo e non è in linea in attesa sulla relativa distribuzione nel sito di produzione finale (un sito remoto, ad esempio una succursale), è possibile tenere conto di tali errori di replica. Per evitare la separazione di un controller di dominio dalla topologia di replica per lunghi periodi, causando errori continui fino a quando il controller di dominio viene riconnessa, prendere in considerazione aggiungendo inizialmente tali computer come server membro e utilizzando l'installazione dal metodo di supporto per installare servizi di dominio Active Directory (AD DS). È possibile utilizzare lo strumento da riga di comando Ntdsutil per creare supporti di installazione che è possibile archiviare un supporto rimovibile (CD, DVD o altri supporti) e fornire al sito di destinazione. Quindi, è possibile utilizzare il supporto di installazione per installare Active Directory nei controller di dominio nel sito, senza l'uso della replica. 

### <a name="hardware-failures-or-upgradestitle"></a>Gli errori hardware o gli aggiornamenti</title>

Se si verificano problemi di replica in seguito a un errore hardware (ad esempio, errore di una scheda madre, sottosistema disco o l'unità disco rigido), informare il proprietario del server in modo che può essere risolto il problema hardware.

Aggiornamenti periodici dell'hardware possono causare anche controller di dominio per essere fuori servizio. Assicurarsi che i proprietari del server dispongano di un sistema buona comunicare tali interruzioni in anticipo.

### <a name="firewall-configuration"></a>Configurazione del firewall

Per impostazione predefinita, remote procedure call (RPC) di Active Directory replica si verificano in modo dinamico su una porta disponibile tramite RPC Endpoint Mapper (RPCSS) sulla porta 135. Assicurarsi che Windows Firewall con sicurezza avanzata e altri firewall siano configurati correttamente per consentire la replica di. Per informazioni su come specificare la porta per la replica di Active Directory e le impostazioni delle porte, vedere [articolo della Microsoft Knowledge Base 224196](https://go.microsoft.com/fwlink/?LinkId=22578). 

Per informazioni sulle porte che utilizza la replica di Active Directory, vedere [impostazioni e strumenti di replica di Active Directory](https://go.microsoft.com/fwlink/?LinkId=123774). 

Per informazioni sulla gestione della replica di Active Directory tramite i firewall, vedere [replica di Active Directory tramite i firewall](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Rispondere agli errori di un server non aggiornato che eseguono Windows 2000 Server

Se un controller di dominio che eseguono Windows 2000 Server non è riuscita per un tempo maggiore del numero di giorni nel corso della durata di rimozione definitiva, la soluzione è sempre lo stesso: 

1. Spostare il server dalla rete aziendale in una rete privata.
2. In modo forzato rimuovere Active Directory o reinstallare il sistema operativo.
3. Rimuovere i metadati del server da Active Directory in modo che l'oggetto server non può essere ripristinata. 

È possibile utilizzare uno script per la pulizia dei metadati del server nella maggior parte dei sistemi operativi Windows. Per informazioni sull'uso di questo script, vedere [rimuovere Active Directory i metadati](https://go.microsoft.com/fwlink/?LinkID=123599). 

Per impostazione predefinita, vanno ripristinati automaticamente gli oggetti Impostazioni NTDS che vengono eliminati per un periodo di 14 giorni. Pertanto, se non si rimuovono i metadati del server, utilizzare Ntdsutil o lo script indicato in precedenza per eseguire la pulizia dei metadati, i metadati del server viene integrato nuovamente nella directory, che richiede i tentativi di replica si verifica. In questo caso, vengono registrati in modo permanente in seguito l'impossibilità di eseguire la replica con il controller di dominio mancante.

## <a name="root-causes"></a>Le cause principali

Se si escludono disconnessioni intenzionale, gli errori hardware e non aggiornati i controller di dominio Windows 2000, la parte restante del problemi di replica è quasi sempre una delle seguenti cause: 


- Connettività di rete: la connessione di rete potrebbe non essere disponibile o le impostazioni di rete non sono configurate correttamente.
- Risoluzione dei nomi: errori di configurazione di DNS sono una causa comune di errori di replica.
- Autenticazione e autorizzazione: problemi di autenticazione e autorizzazione causano errori di "Accesso negato" quando un controller di dominio tenta di connettersi al relativo partner di replica.
- Database di directory (archivio): il database di directory potrebbe non essere in grado di elaborare transazioni abbastanza rapidamente a tenere il passo con valori di timeout di replica.
- Motore di replica: se le pianificazioni di replica tra siti sono troppo breve, le code di replica potrebbero essere troppo grande per elaborare il tempo necessario per la pianificazione della replica in uscita. In questo caso, la replica di alcune modifiche può essere bloccata indefinitelypotentially, sufficienti a superare la durata di rimozione.
- Topologia di replica: controller di dominio devono avere i collegamenti tra siti in Active Directory che eseguono il mapping di rete reale wide area network (WAN) o connessioni di rete privata virtuale (VPN). Se si creano oggetti in Active Directory per la topologia di replica che non sono supportati per la topologia del sito effettiva della rete, che richiede la topologia non configurato correttamente la replica ha esito negativo.

## <a name="general-approach-to-fixing-problems"></a>Approccio generale per la risoluzione dei problemi

Utilizzare la seguente procedura generale per la risoluzione dei problemi di replica: 


1. Monitoraggio stato della replica giornaliera oppure usare Repadmin.exe per recuperare lo stato della replica giornaliera.
2. Tentare di risolvere eventuali errori segnalati in modo tempestivo usando i metodi descritti in questa Guida e messaggi di evento. Se il problema potrebbe essere causato da software, disinstallare il software prima di continuare con altre soluzioni.
3. Se il problema che provoca l'esito negativo della replica non può essere risolto da eventuali metodi noti, rimuovere il server di dominio Active Directory e quindi reinstallare Servizi di dominio Active Directory. Per ulteriori informazioni sulla reinstallazione di dominio Active Directory, vedere [rimozione di un Controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se di dominio Active Directory non possono essere rimossi normalmente, mentre il server è connesso alla rete, utilizzare uno dei metodi seguenti per risolvere il problema:
 

    - Forzare la rimozione di dominio Active Directory nella Directory servizi modalità di ripristino, di pulizia dei metadati del server, e quindi reinstallare Servizi di dominio Active Directory.
    - Reinstallare il sistema operativo e ricreare il controller di dominio.

Per ulteriori informazioni su come forzare la rimozione di servizi di dominio Active Directory, vedere [forzare la rimozione di un Controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Utilizzare Repadmin per recuperare lo stato della replica</title>

Lo stato di replica è un aspetto importante per valutare lo stato del servizio directory. Se la replica funziona senza errori, si conosce il controller di dominio che sono in linea. Si è visto inoltre che utilizza i sistemi e i servizi seguenti:


- Infrastruttura DNS
- Protocollo di autenticazione Kerberos
- Servizio ora di Windows (W32time)
- Chiamata di procedura remota (RPC)
- Connettività di rete

Utilizzare Repadmin per monitorare lo stato della replica giornaliera eseguendo un comando che consente di valutare lo stato della replica di tutti i controller di dominio della foresta. La procedura genera un file CSV che è possibile aprire in Microsoft Excel e filtro per gli errori di replica.

È possibile utilizzare la procedura seguente per recuperare lo stato della replica di tutti i controller di dominio nella foresta. 
      
Requisiti
      
Appartenenza al gruppo **Enterprise Admins**, o equivalente è il requisito minimo necessario per completare questa procedura. 

Strumenti: 


- Repadmin.exe 
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Per generare un foglio di calcolo di repadmin /showrepl per i controller di dominio



1. Aprire un prompt dei comandi come amministratore: nel menu Start, fare doppio clic su prompt dei comandi, quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, se necessario, fornire le credenziali per Enterprise Admins e quindi fare clic su Continua. 
2. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:`repadmin /showrepl * /csv &gt;showrepl.csv`
3. Aprire Excel.
4. Fare clic sul pulsante di Office, fare clic su Apri, passare a showrepl. csv e quindi fare clic su Apri.
5. Nascondere o eliminare una colonna, nonché la colonna tipo di trasporto, come indicato di seguito:
6. Selezionare una colonna che si desidera nascondere o eliminare.
      

    - Per nascondere la colonna, fare clic sulla colonna e quindi fare clic su Nascondi.
    - Per eliminare la colonna, fare clic sulla colonna selezionata e quindi fare clic su Elimina.
7. Seleziona la riga 1 sotto la riga di intestazione di colonna. Nella scheda Visualizza, scegliere Blocca riquadri e quindi fare clic sulla riga superiore di bloccare.
8. Selezionare l'intero foglio di calcolo. Nella scheda dati, fare clic sul filtro.
9. Nella colonna ultima operazione riuscita, fare clic sulla freccia verso il basso e quindi fare clic su ordinamento crescente.
10. Nella colonna controller di dominio di origine, fare clic sul filtro freccia giù filtri di testo e quindi fare clic su filtro personalizzato.
11. Nel filtro automatico personalizzato della finestra di dialogo, Mostra le righe in cui, fare clic su non contiene. Nella casella di testo adiacente, digita <userInput>CANC</userInput> al fine di eliminare Visualizza i risultati per eliminare i controller di dominio.
12. Ripetere il passaggio 11 per la colonna ultimo tempo di errore, ma utilizzare il valore non uguale e quindi digitare il valore 0.
13. Risolvere gli errori di replica.

Per ogni controller di dominio nella foresta, il foglio di calcolo Mostra il partner di replica di origine, l'ora dell'ultima occorrenza che la replica e l'ora dell'ultima replica errore per ogni contesto dei nomi (partizione di directory). Utilizzando Filtro automatico in Excel, è possibile visualizzare lo stato di replica per l'utilizzo, solo i controller di dominio non riesce solo controller di dominio o controller di dominio che sono almeno o la maggior parte dei corrente ed è possibile visualizzare i partner di replica che replicano correttamente.

## <a name="replication-problems-and-resolutions"></a>Problemi di replica e le risoluzioni
    
Problemi di replica vengono segnalati in messaggi di evento e in messaggi di errore che si verificano quando un'applicazione o servizio tenta un'operazione. In teoria, questi messaggi vengono raccolti dall'applicazione di monitoraggio o quando si recupera lo stato della replica.

La maggior parte dei problemi di replica vengono identificati nei messaggi di evento che vengono registrati nel registro eventi del servizio Directory. Problemi di replica potrebbero essere identificati anche sotto forma di messaggi di errore nell'output del <system>repadmin /showrepl</system> comando. 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl messaggi che indicano problemi di replica

Per identificare i problemi di replica di Active Directory, utilizzare il <system>repadmin /showrepl</system> comando, come descritto nella sezione precedente. La tabella seguente Mostra messaggi di errore che genera questo comando, insieme a cause principali degli errori e i collegamenti agli argomenti che offrono soluzioni per gli errori.


|Errore di repadmin|Causa principale|Soluzione|
| --- | --- | --- |
|Il tempo trascorso dall'ultima replica con questo server ha superato la durata di rimozione definitiva.|Un controller di dominio non riuscita di replica in ingresso con il controller di dominio di origine denominato tempo sufficiente per l'eliminazione è stata rimossa definitivamente, replicato e garbage collection da AD DS.|ID evento 2042: È trascorso troppo tempo da quando questo computer replicato|
|Nessun router adiacente.|Se nessun elemento visualizzato nella sezione "Inbound Neighbors" dell'output che viene generato da repadmin /showrepl, il controller di dominio non è in grado di stabilire i collegamenti di replica con un altro controller di dominio.|Correzione dei problemi di connettività di replica (ID evento 1925)| 
|Accesso negato.|Esiste un collegamento di replica tra due controller di dominio, ma la replica non eseguita correttamente in seguito a un errore di autenticazione.|Risoluzione dei problemi di sicurezza della replica| 
|Ultimo tentativo di < data - ora > non riuscita con il "nome account di destinazione è errata."|Questo problema può essere correlato a connettività, DNS o problemi di autenticazione. Se si tratta di un errore DNS, controller di dominio locale Impossibile risolvere l'identificatore univoco globale (GUID), in base a nome DNS del partner di replica.|Correzione replica (ID evento 1925, 2087, 2088) problemi correzione replica sicurezza problemi di ricerca DNS correzione dei problemi di connettività di replica (ID evento 1925)| 
|Errore LDAP 49.|L'account computer controller di dominio potrebbero non essere sincronizzati con il centro distribuzione chiavi (KDC).|Risoluzione dei problemi di sicurezza della replica| 
|Impossibile aprire connessione LDAP a host locale|Lo strumento di amministrazione non è in grado di contattare Active Directory.|Problemi di ricerca di correzione della replica DNS (ID evento 1925, 2087, 2088)| 
|Replica di Active Directory è stata interrotta.|Lo stato di avanzamento della replica in ingresso è stata interrotta da una richiesta di replica di priorità più alta, ad esempio una richiesta che è stata creata manualmente con il comando di repadmin /sync.|Attendere il completamento della replica. Questo messaggio informativo indica il normale funzionamento.| 
|Registrazione di replica, in attesa.| Il controller di dominio registrato una richiesta di replica e attende una risposta. La replica è in corso dall'origine.|Attendere il completamento della replica. Questo messaggio informativo indica il normale funzionamento.| 

La tabella seguente elenca gli eventi che possono indicare problemi con la replica, insieme a radice cause principali dei problemi e i collegamenti agli argomenti che offrono soluzioni per i problemi di Active Directory. 

|ID evento e origine|Causa principale|Soluzione|
| --- | --- | --- | 
|1311 NTDS KCC|Le informazioni di configurazione di replica in Active Directory non riflettere accuratamente la topologia fisica della rete.|Correzione dei problemi di topologia di replica (ID evento 1311)| 
|Replica NTDS 1388|Coerenza di replica rigida non è in vigore, e un oggetto residuo è stato replicato al controller di dominio.|Correzione residui oggetto problemi di replica (ID eventi 1388, 1988, 2042)|
|1925 NTDS KCC|Non è riuscito il tentativo di stabilire un collegamento di replica per una partizione di directory scrivibile. Questo evento può avere diverse cause, a seconda dell'errore.| Correzione connettività problemi (ID evento 1925) correzione replica DNS ricerca problemi di replica (ID evento 1925, 2087, 2088)| 
|Replica NTDS 1988|Il controller di dominio locale ha tentato di replicare un oggetto da un controller di dominio di origine che non è presente nel controller di dominio locale poiché può sono state eliminata e già garbage collection. Replica non proseguirà per questa partizione di directory con il partner finché non viene risolto il problema.|Correzione residui oggetto problemi di replica (ID eventi 1388, 1988, 2042)|
|Replica NTDS 2042|Replica non si è verificato con il partner per una durata di rimozione definitiva, e non è possibile continuare la replica.|Correzione residui oggetto problemi di replica (ID eventi 1388, 1988, 2042)| 
|Replica NTDS 2087|Impossibile risolvere il nome host DNS del controller di dominio di origine a un indirizzo IP e la replica non riuscito di dominio Active Directory.|Problemi di ricerca di correzione della replica DNS (ID evento 1925, 2087, 2088)| 
|Replica NTDS 2088 |Impossibile risolvere il nome host DNS del controller di dominio di origine a un indirizzo IP, ma la replica ha avuto esito positivo di dominio Active Directory.|Problemi di ricerca di correzione della replica DNS (ID evento 1925, 2087, 2088)|
|Accesso rete 5805|Un account del computer non è riuscito a eseguire l'autenticazione, che in genere è causato da uno dei due più istanze dello stesso nome computer o il nome del computer non la replica per ogni controller di dominio.|Risoluzione dei problemi di sicurezza della replica| 

Per ulteriori informazioni sui concetti di replica, vedere [le tecnologie di replica di Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
