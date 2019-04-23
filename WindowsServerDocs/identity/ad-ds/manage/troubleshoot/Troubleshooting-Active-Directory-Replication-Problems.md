---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Risoluzione dei problemi di replica di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869422"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Risoluzione dei problemi di replica di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Problemi di replica di Active Directory possono avere diverse origini. Ad esempio, problemi, problemi di rete o problemi di sicurezza del sistema DNS (Domain Name) possono tutti questi casi replica Active Directory avrà esito negativo. 

La parte restante di questo argomento illustra gli strumenti e la metodologia generale per correggere gli errori di replica di Active Directory. Gli argomenti correlati seguenti descrivono i sintomi, cause e come risolvere gli errori di replica specifico:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introduzione e risorse per la risoluzione dei problemi di replica di Active Directory

Connessioni in entrata o errore di replica in uscita, gli oggetti di Active Directory che rappresentano la topologia di replica, pianificazione della replica, i controller di dominio, utenti, computer, le password, i gruppi di sicurezza, appartenenze ai gruppi e criteri di gruppo come non coerente tra i controller di dominio. Errore di incoerenza e la replica di directory causare problemi operativi o risultati incoerenti, a seconda del controller di dominio che viene contattato per l'operazione, possono impedire l'applicazione dei criteri di gruppo e le autorizzazioni di controllo di accesso. Servizi di dominio Active Directory (AD DS) dipende dalla connettività di rete, la risoluzione dei nomi, l'autenticazione e autorizzazione, il database di directory, la topologia di replica e il motore di replica. Quando la causa radice di un problema di replica non è immediatamente ovvia, determinare la causa tra le molte cause possibili prevede l'eliminazione sistematico di cause probabili.

Per uno strumento basato su interfaccia utente consentire la replica di monitorare e diagnosticare gli errori, vedere [stato strumento replica di Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Per una documentazione completa che descrive come è possibile usare lo strumento Repadmin per risolvere i problemi di Active Directory replica è disponibile. visualizzare [il monitoraggio e la risoluzione dei problemi di replica di Active Directory mediante Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Per informazioni sul funzionamento della replica di Active Directory, vedere i riferimenti tecnici seguenti:

- [Riferimento tecnico ad Active Directory replica modello](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Directory replica topologia Technical Reference](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Eventi e lo strumento sulle raccomandazioni della soluzione

In teoria, il colore rosso (errore) e giallo (avviso) gli eventi nel registro eventi del servizio Directory suggeriscono il vincolo specifico che ha causato l'errore di replica nel controller di dominio di origine o destinazione. Se il messaggio di evento suggerisce i passaggi per una soluzione, provare i passaggi descritti nell'evento. Lo strumento Repadmin e altri strumenti di diagnostica forniscono inoltre informazioni che consentono di risolvere gli errori di replica. 

Per informazioni dettagliate sull'utilizzo di Repadmin per la risoluzione dei problemi di replica, vedere [il monitoraggio e la risoluzione dei problemi di replica di Active Directory mediante Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Escludendo le interruzioni intenzionali o di errori hardware

A volte si verificano errori di replica a causa di interruzioni intenzionali. Ad esempio, la risoluzione dei problemi di replica di Active Directory, escludere disconnessioni intenzionale e aggiornamenti o errori hardware prima di tutto.

### <a name="intentional-disconnections"></a>Disconnessioni intenzionale

Se vengono rilevati errori di replica da un controller di dominio che è il tentativo di replica con un controller di dominio che è stata creata in un sito di staging e is currently offline è in attesa la sua distribuzione nel sito di produzione finale (un sito remoto, ad esempio una succursale ), è possibile tenere conto per quegli errori di replica. Per evitare la separazione di un controller di dominio dalla topologia di replica per lunghi periodi, generando errori continui fino alla riconnessione il controller di dominio, prendere in considerazione l'aggiunta di tali computer inizialmente come server membri e utilizzando l'installazione da supporto ( Metodo di installazione da supporto) per installare Active Directory Domain Services (AD DS). È possibile utilizzare lo strumento da riga di comando Ntdsutil per creare supporti di installazione che è possibile archiviare su supporti rimovibili (CD, DVD o altri supporti) e spedizione al sito di destinazione. Quindi, è possibile utilizzare i supporti di installazione per installare Active Directory nei controller di dominio nel sito, senza l'uso della replica. 

### <a name="hardware-failures-or-upgradestitle"></a>Aggiornamenti o errori hardware</title>

Se si verificano problemi di replica in seguito a errori hardware (ad esempio, esito negativo di una scheda madre, sottosistema del disco o disco rigido), informare il proprietario del server in modo che possa essere risolto il problema hardware.

Gli aggiornamenti periodici hardware possono provocare i controller di dominio rimanga fuori servizio. Verificare che il proprietario del server ha un buon sistema di comunicazione, quali interruzioni in anticipo.

### <a name="firewall-configuration"></a>Configurazione del firewall

Per impostazione predefinita, Active Directory replica remote procedure call (RPC) si verificano in modo dinamico su una porta disponibile tramite il RPC Endpoint Mapper RPCSS () sulla porta 135. Assicurarsi che Windows Firewall con sicurezza avanzata e altri firewall siano configurati correttamente per consentire la replica. Per informazioni su come specificare la porta per la replica di Active Directory e le impostazioni delle porte, vedere [articolo della Microsoft Knowledge Base 224196](https://go.microsoft.com/fwlink/?LinkId=22578). 

Per informazioni sulle porte che utilizza la replica di Active Directory, vedere [gli strumenti di replica di Active Directory e le impostazioni](https://go.microsoft.com/fwlink/?LinkId=123774).

Per informazioni sulla gestione della replica di Active Directory tramite i firewall, vedere [replica di Active Directory tramite i firewall](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Risposta all'errore di un server obsoleto che eseguono Windows 2000 Server

Se un controller di dominio che eseguono Windows 2000 Server non è riuscita per un tempo maggiore del numero di giorni nel corso della durata di rimozione definitiva, la soluzione è sempre lo stesso: 

1. Spostare il server dalla rete aziendale in una rete privata.
2. In modo forzato rimuovere Active Directory o reinstallare il sistema operativo.
3. Rimuovere i metadati del server da Active Directory in modo che l'oggetto server non può essere riattivata. 

È possibile utilizzare uno script per la pulizia dei metadati del server nella maggior parte dei sistemi operativi Windows. Per informazioni sull'uso di questo script, vedere [rimuovere Active Directory Domain Controller metadati](https://go.microsoft.com/fwlink/?LinkID=123599). 

Per impostazione predefinita, gli oggetti Impostazioni NTDS che vengono eliminati vengano riattivati automaticamente per un periodo di 14 giorni. Pertanto, se non si rimuove i metadati del server (utilizzare Ntdsutil o lo script indicato in precedenza per eseguire la pulizia dei metadati), verranno ripristinati nella directory, che richiede la replica tenta si verificano i metadati del server. In questo caso, vengono registrati in modo permanente come risultato l'impossibilità di eseguire la replica con il controller di dominio mancanti.

## <a name="root-causes"></a>Cause principali

Se vorranno escludere disconnessioni intenzionale, errori hardware e controller di dominio Windows 2000 non aggiornati, il resto dei problemi di replica è quasi sempre una delle cause radice seguenti:

- Connettività di rete: La connessione di rete potrebbe non essere disponibile, o le impostazioni di rete non sono configurate correttamente.
- Risoluzione dei nomi: Gli errori di configurazione di DNS sono una causa comune degli errori di replica.
- Autenticazione e autorizzazione: Problemi di autenticazione e autorizzazione causano errori di "Accesso negato" quando un controller di dominio prova a connettersi al relativo partner di replica.
- Database di directory (archivio): Il database di directory potrebbe non essere in grado di elaborare le transazioni abbastanza rapidamente per stare al passo con i timeout di replica.
- Motore di replica: Se le pianificazioni di replica tra siti sono troppo breve, le code di replica potrebbero essere troppo grande per l'elaborazione nel momento in cui è necessario per la pianificazione della replica in uscita. In questo caso, la replica di alcune delle modifiche può essere bloccata per un periodo illimitato potenzialmente, il tempo sufficiente per superare la durata di rimozione definitiva.
- Topologia di replica: I controller di dominio devono avere i collegamenti tra siti in Active Directory Domain Services che eseguono il mapping di rete reale WAN (WAN) o connessioni di rete privata virtuale (VPN). Se si creano oggetti in Active Directory Domain Services per la topologia di replica che non sono supportati per la topologia del sito effettiva della rete, che richiede la topologia non configurato correttamente la replica ha esito negativo.

## <a name="general-approach-to-fixing-problems"></a>Approccio generale per la risoluzione dei problemi

Utilizzare la seguente procedura generale per la risoluzione dei problemi di replica: 

1. Monitorare l'integrità di replica ogni giorno, o usare Repadmin.exe per recuperare lo stato della replica giornaliera.
2. Prova a risolvere qualsiasi errore segnalato in modo tempestivo utilizzando i metodi descritti in questa Guida e i messaggi di evento. Se il problema potrebbe essere causato da software, disinstallare il software prima di continuare con altre soluzioni.
3. Se il problema che causa replica avrà esito negativo non può essere risolto da eventuali metodi noti, rimuovere Active Directory Domain Services dal server, quindi reinstallare Active Directory Domain Services. Per altre informazioni sulla reinstallazione di Active Directory Domain Services, vedere [rimozione di un Controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se Active Directory Domain Services non può essere rimosso in genere mentre il server sia connesso alla rete, usare uno dei metodi seguenti per risolvere il problema:

   - Forzare la rimozione di Active Directory Domain Services in servizi di ripristino in modalità (Directory), pulizia dei metadati del server, e quindi si reinstalla Active Directory Domain Services.
   - Reinstallare il sistema operativo e ricompilare il controller di dominio.

Per altre informazioni su come forzare la rimozione di dominio Active Directory, vedere [rimozione forzata di un Controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Utilizzare Repadmin per recuperare lo stato della replica</title>

Lo stato della replica è un modo importante per valutare lo stato del servizio directory. Se la replica è senza errori, si saprà già i controller di dominio che sono online. Si consce anche che utilizza i sistemi e i servizi seguenti:


- Infrastruttura DNS
- Protocollo di autenticazione Kerberos
- Servizio ora di Windows (W32time)
- Chiamata di procedura remota (RPC)
- Connettività di rete

Utilizzare Repadmin per monitorare lo stato della replica giornaliera eseguendo un comando che consente di valutare lo stato di replica di tutti i controller di dominio nella foresta. La procedura genera un file con estensione csv che è possibile aprire in Microsoft Excel e di filtro per gli errori di replica.

È possibile utilizzare la procedura seguente per recuperare lo stato di replica di tutti i controller di dominio nella foresta. 

Requisiti

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins**, o equivalente. 

Strumenti:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Per generare un foglio di calcolo di repadmin /showrepl per controller di dominio

1. Aprire una finestra del prompt dei comandi come amministratore. Nel menu Start, fare doppio clic su prompt dei comandi e quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, se necessario, fornire le credenziali per Enterprise Admins e quindi fare clic su Continua.
2. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `repadmin /showrepl * /csv > showrepl.csv`
3. Aprire Excel.
4. Fare clic sul pulsante di Office, fare clic su Apri, passare a showrepl. csv e quindi fare clic su Apri.
5. Nascondere o eliminare una colonna, nonché la colonna del tipo di trasporto, come indicato di seguito:
6. Selezionare una colonna che si desidera nascondere o eliminare.

   - Per nascondere la colonna, fare doppio clic su della colonna e quindi fare clic su Nascondi.
   - Per eliminare la colonna, fare doppio clic su della colonna selezionata e quindi fare clic su Elimina.

7. Selezionare la riga 1 sotto la riga di intestazione di colonna. Nella scheda Visualizza, fare clic su Blocca riquadri e quindi fare clic sulla riga superiore bloccare.
8. Selezionare l'intero foglio di calcolo. Nella scheda dati, fare clic sul filtro.
9. Nella colonna ora dell'ultimo esito positivo, fare clic sulla freccia verso il basso e quindi fare clic su ordinamento crescente.
10. Nella colonna controller di dominio di origine, fare clic sul filtro freccia giù, scegliere filtri per testo e quindi fare clic su filtro personalizzato.
11. In Personalizza filtro automatico la finestra di dialogo, sotto Mostra righe in cui, fare clic su non contiene. Nella casella di testo adiacenti, digitare <userInput>CANC</userInput> eliminare dalla vista i risultati per i controller di dominio eliminato.
12. Ripetere il passaggio 11 per l'ora dell'ultimo errore colonna, ma usare il valore non è uguale e quindi digitare il valore 0.
13. Risolvere gli errori di replica.

Per ogni controller di dominio nella foresta, il foglio di calcolo Mostra il partner di replica di origine, l'ora dell'ultima generazione che la replica e l'ora che si è verificato l'ultimo errore di replica per ogni contesto dei nomi (partizione di directory). Tramite il filtro automatico in Excel, è possibile visualizzare lo stato della replica per l'utilizzo, solo i controller di dominio esegua il solo controller di dominio o controller di dominio che sono minimi o la maggior parte delle corrente ed è possibile visualizzare i partner di replica che eseguono la replica correttamente.

## <a name="replication-problems-and-resolutions"></a>Le risoluzioni e i problemi di replica

Segnalare problemi di replica in messaggi di eventi e nei vari messaggi di errore che si verificano quando un'applicazione o servizio tenti di eseguire un'operazione. In teoria, questi messaggi vengono raccolti dall'applicazione monitoraggio o quando si recupera lo stato della replica.

La maggior parte dei problemi di replica vengono identificati nei messaggi di eventi che vengono registrati nel registro eventi del servizio Directory. Problemi di replica potrebbero essere inoltre individuati sotto forma di messaggi di errore nell'output del <system>repadmin /showrepl</system> comando.

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl i messaggi di errore indicano problemi di replica

Per identificare problemi di replica di Active Directory, usare il <system>repadmin /showrepl</system> comando, come descritto nella sezione precedente. La tabella seguente mostra i messaggi di errore genera questo comando, insieme a cause radice degli errori e i collegamenti ad argomenti che offrono soluzioni per gli errori.

|Errore di repadmin|Causa principale|Soluzione|
| --- | --- | --- |
|Il tempo trascorso dall'ultima replica con questo server ha superato la durata di rimozione definitiva.|Un controller di dominio non riuscita per la replica in ingresso con il controller di dominio di origine denominata tempo sufficiente per l'eliminazione è stata rimossa definitivamente, replicato e sottoposto a garbage collection da Active Directory Domain Services.|ID evento 2042: È trascorso troppo tempo da quando questo computer ha effettuato la replica|
|Nessun router adiacente.|Se nessun elemento visualizzato nella sezione "Elementi adiacenti in ingresso" dell'output generato da repadmin /showrepl, il controller di dominio non è in grado di stabilire collegamenti di replica con un altro controller di dominio.|Correzione dei problemi di connettività della replica (ID evento 1925)| 
|Accesso negato.|Non esiste un collegamento di replica tra due controller di dominio, ma la replica non è possibile eseguire correttamente in seguito a un errore di autenticazione.|Correzione dei problemi di sicurezza della replica| 
|Ultimo tentativo di < data - ora > non è riuscita con il "nome dell'account di destinazione non è corretto."|Questo problema può essere correlato alla connettività, DNS o problemi di autenticazione. Se si tratta di un errore DNS, controller di dominio locale non è stato possibile risolvere l'identificatore univoco globale (GUID), in base al nome DNS del partner di replica.|Correzione della replica DNS ricerca problemi (ID evento 1925, 2087, 2088) correzione sicurezza problemi di replica correzione dei problemi di connettività di replica (ID evento 1925)| 
|Errore LDAP 49.|L'account computer controller di dominio potrebbero non essere sincronizzati con il centro distribuzione chiavi (KDC).|Correzione dei problemi di sicurezza della replica| 
|Impossibile aprire la connessione LDAP per l'host locale|Lo strumento di amministrazione non è in grado di contattare Active Directory Domain Services.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)| 
|Replica di Active Directory è stata superata.|Lo stato di avanzamento della replica in ingresso è stata interrotta da una richiesta di replica con priorità più alta, ad esempio una richiesta che è stata generata manualmente con il comando di repadmin /sync.|Attendere il completamento della replica. Questo messaggio informativo indica il normale funzionamento.| 
|Replica registrato, in attesa.| Il controller di dominio registrato di una richiesta di replica e attende una risposta. La replica è in corso di questa origine.|Attendere il completamento della replica. Questo messaggio informativo indica il normale funzionamento.| 

Nella tabella seguente elenca gli eventi più comuni che potrebbero indicare problemi con la replica, insieme a root cause dei problemi e collegamenti ad argomenti che offrono soluzioni per i problemi di Active Directory. 

|ID evento e origine|Causa radice|Soluzione|
| --- | --- | --- | 
|1311 NTDS KCC|Le informazioni di configurazione della replica di Active Directory Domain Services non riflettano esattamente la topologia della rete fisica.|Correzione dei problemi di topologia di replica (ID evento 1311)| 
|Replica NTDS 1388|Coerenza di replica rigida non è attiva e un oggetto residuo è stato replicato nel controller di dominio.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)|
|1925 NTDS KCC|Non è riuscito il tentativo di stabilire un collegamento di replica per una partizione di directory scrivibile. Questo evento può avere diverse cause, a seconda dell'errore.| Correzione connettività problemi (ID evento 1925) correzione della replica DNS ricerca problemi di replica (ID evento 1925, 2087, 2088)| 
|Replica NTDS 1988|Il controller di dominio locale ha tentato di replicare un oggetto da un controller di dominio di origine che non è presente nel controller di dominio locale poiché può sono stati eliminato e già sottoposto a garbage collection. Replica non procede per questa partizione di directory con questo partner fino a quando non viene risolto la situazione.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)|
|2042 NTDS replica|Replica non si è verificato con il partner per una durata di rimozione definitiva e non è possibile continuare la replica.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)| 
|Replica NTDS 2087|Active Directory Domain Services non è stato possibile risolvere il nome host DNS del controller di dominio di origine a un indirizzo IP e la replica non è riuscita.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)| 
|Replica NTDS 2088 |Active Directory Domain Services non è stato possibile risolvere il nome host DNS del controller di dominio di origine a un indirizzo IP, ma la replica ha avuto esito positivo.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)|
|Accesso rete 5805|Un account del computer non è stato possibile eseguire l'autenticazione, che è causato generalmente da più istanze dello stesso nome computer o il nome del computer non eseguendo la replica per ogni controller di dominio.|Correzione dei problemi di sicurezza della replica| 

Per altre informazioni sui concetti di replica, vedere [le tecnologie di replica di Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, incluso il supporto gli articoli specifici codici di errore vedere l'articolo del supporto tecnico: [Come risolvere i problemi più comuni errori di replica di Active Directory](https://support.microsoft.com/help/3108513)
