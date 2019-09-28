---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Risoluzione dei problemi di replica di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409065"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Risoluzione dei problemi di replica di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory problemi di replica possono avere origini diverse. Ad esempio, Domain Name System (DNS), problemi di rete o problemi di sicurezza possono causare la mancata riuscita della replica Active Directory. 

Nella parte restante di questo argomento vengono illustrati gli strumenti e una metodologia generale per correggere Active Directory errori di replica. Gli argomenti secondari seguenti riguardano i sintomi, le cause e la risoluzione di errori di replica specifici:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introduzione e risorse per la risoluzione dei problemi relativi alla replica di Active Directory

L'errore di replica in ingresso o in uscita causa l'incoerenza degli oggetti Active Directory che rappresentano la topologia di replica, la pianificazione della replica, i controller di dominio, gli utenti, i computer, le password, i gruppi di sicurezza, le appartenenze ai gruppi e i Criteri di gruppo tra controller di dominio. L'incoerenza della directory e l'errore di replica causano errori operativi o risultati incoerenti, a seconda del controller di dominio contattato per l'operazione e possono impedire l'applicazione di Criteri di gruppo e le autorizzazioni di controllo di accesso. Active Directory Domain Services (AD DS) dipende dalla connettività di rete, dalla risoluzione dei nomi, dall'autenticazione e dall'autorizzazione, dal database di directory, dalla topologia di replica e dal motore di replica. Quando la causa principale di un problema di replica non è immediatamente ovvia, per determinare la causa tra le diverse cause è necessario eliminare sistematicamente le possibili cause.

Per uno strumento basato su interfaccia utente che consente di monitorare la replica e diagnosticare gli errori, vedere [strumento stato replica di Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Per un documento completo in cui viene descritto come utilizzare lo strumento repadmin per la risoluzione dei problemi Active Directory la replica è disponibile; vedere [monitoraggio e risoluzione dei problemi relativi alla replica di Active Directory tramite repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Per informazioni sul funzionamento della replica Active Directory, vedere i seguenti riferimenti tecnici:

- [Riferimento tecnico per il modello di replica Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Riferimento tecnico per la topologia di replica di Active Director](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Raccomandazioni per la soluzione di eventi e strumenti

Idealmente, gli eventi rossi (errore) e giallo (avviso) nel registro eventi del servizio directory suggeriscono il vincolo specifico che causa un errore di replica sul controller di dominio di origine o di destinazione. Se il messaggio di evento suggerisce i passaggi per una soluzione, provare i passaggi descritti nell'evento. Lo strumento repadmin e altri strumenti di diagnostica forniscono anche informazioni che consentono di risolvere gli errori di replica. 

Per informazioni dettagliate sull'uso di Repadmin per la risoluzione dei problemi di replica, vedere [monitoraggio e risoluzione dei problemi di replica Active Directory tramite repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Escludere interruzioni intenzionali o errori hardware

A volte si verificano errori di replica a causa di interruzioni intenzionali. Ad esempio, quando si risolvono Active Directory problemi di replica, escludere prima le disconnessioni intenzionali e gli errori hardware o gli aggiornamenti.

### <a name="intentional-disconnections"></a>Disconnessioni intenzionali

Se gli errori di replica vengono segnalati da un controller di dominio che tenta di eseguire la replica con un controller di dominio compilato in un sito di gestione temporanea ed è attualmente offline in attesa della distribuzione nel sito di produzione finale (un sito remoto, ad esempio una succursale ), è possibile tenere conto di tali errori di replica. Per evitare la separazione di un controller di dominio dalla topologia di replica per periodi prolungati, causando errori continui fino alla riconnessione del controller di dominio, è consigliabile aggiungere tali computer inizialmente come server membri e utilizzare l'installazione dal supporto ( INSTALLAZIONE da supporto) per installare Active Directory Domain Services (AD DS). È possibile utilizzare lo strumento da riga di comando Ntdsutil per creare supporti di installazione che è possibile archiviare su supporti rimovibili (CD, DVD o altri supporti) e spedire al sito di destinazione. Quindi, è possibile usare il supporto di installazione di per installare servizi di dominio Active Directory nei controller di dominio del sito, senza usare la replica. 

### <a name="hardware-failures-or-upgradestitle"></a>Errori hardware o aggiornamenti @ no__t-0

Se si verificano problemi di replica in seguito a un errore hardware, ad esempio un errore di una scheda madre, un sottosistema del disco o un disco rigido, inviare una notifica al proprietario del server in modo che il problema hardware possa essere risolto.

Gli aggiornamenti periodici dell'hardware possono anche causare la disattivazione dei controller di dominio. Assicurarsi che i proprietari del server dispongano di un sistema efficace per comunicare tali interruzioni in anticipo.

### <a name="firewall-configuration"></a>Configurazione del firewall

Per impostazione predefinita, le chiamate RPC (Remote Procedure Call) di replica Active Directory vengono eseguite in modo dinamico su una porta disponibile tramite il mapper dell'endpoint RPC sulla porta 135. Assicurarsi che Windows Firewall con sicurezza avanzata e altri firewall siano configurati correttamente per consentire la replica. Per informazioni su come specificare la porta per Active Directory le impostazioni di replica e porta, vedere [l'articolo 224196 della Microsoft Knowledge base](https://go.microsoft.com/fwlink/?LinkId=22578). 

Per informazioni sulle porte utilizzate da Active Directory replica, vedere [Active Directory strumenti e impostazioni di replica](https://go.microsoft.com/fwlink/?LinkId=123774).

Per informazioni sulla gestione della replica Active Directory sui firewall, vedere [Active Directory la replica su Firewall](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Risposta a un errore di un server obsoleto che esegue Windows 2000 Server

Se un controller di dominio che esegue Windows 2000 Server ha avuto esito negativo per un periodo di tempo superiore al numero di giorni nella durata di rimozione definitiva, la soluzione è sempre la stessa: 

1. Spostare il server dalla rete aziendale a una rete privata.
2. Rimuovere in modo forzato Active Directory o reinstallare il sistema operativo.
3. Rimuovere i metadati del server da Active Directory in modo che l'oggetto server non possa essere ripristinato. 

È possibile utilizzare uno script per pulire i metadati del server nella maggior parte dei sistemi operativi Windows. Per informazioni sull'uso di questo script, vedere [rimuovere dominio di Active Directory metadati del controller](https://go.microsoft.com/fwlink/?LinkID=123599). 

Per impostazione predefinita, gli oggetti Impostazioni NTDS eliminati vengono ripristinati automaticamente per un periodo di 14 giorni. Pertanto, se non si rimuovono i metadati del server (usare Ntdsutil o lo script indicato in precedenza per eseguire la pulizia dei metadati), i metadati del server vengono ripristinati nella directory, che richiede l'esecuzione di tentativi di replica. In questo caso, gli errori verranno registrati in modo permanente in seguito all'impossibilità di eseguire la replica con il controller di dominio mancante.

## <a name="root-causes"></a>Cause principali

Se si escludono disconnessioni intenzionali, errori hardware e controller di dominio Windows 2000 obsoleti, il resto dei problemi di replica ha quasi sempre una delle cause principali seguenti:

- Connettività di rete: La connessione di rete potrebbe non essere disponibile oppure le impostazioni di rete non sono configurate correttamente.
- Risoluzione dei nomi: Gli errori di configurazione DNS sono una delle cause comuni degli errori di replica.
- Autenticazione e autorizzazione: Problemi di autenticazione e autorizzazione provocano errori di "accesso negato" quando un controller di dominio tenta di connettersi al partner di replica.
- Database di directory (Archivio): Il database di directory potrebbe non essere in grado di elaborare le transazioni in modo sufficientemente rapido per restare al passo con i timeout di replica.
- Motore di replica: Se le pianificazioni della replica tra siti sono troppo brevi, le code di replica potrebbero essere troppo grandi per essere elaborate nel tempo richiesto dalla pianificazione della replica in uscita. In questo caso, la replica di alcune modifiche può essere bloccata per un tempo indefinito, a sufficienza per superare la durata di rimozione definitiva.
- Topologia di replica: I controller di dominio devono disporre di collegamenti tra siti in servizi di dominio Active Directory che eseguono il mapping a connessioni con Wide Area Network reale (WAN) o VPN (Virtual Private Network). Se si creano oggetti in servizi di dominio Active Directory per la topologia di replica che non sono supportati dalla topologia del sito effettiva della rete, la replica che richiede la topologia non configurata correttamente ha esito negativo.

## <a name="general-approach-to-fixing-problems"></a>Approccio generale alla risoluzione dei problemi

Usare l'approccio generale seguente per correggere i problemi di replica: 

1. Monitorare lo stato di integrità della replica ogni giorno oppure utilizzare Repadmin. exe per recuperare ogni giorno lo stato della replica.
2. Tentare di risolvere in modo tempestivo eventuali errori segnalati utilizzando i metodi descritti in messaggi di evento e in questa guida. Se il problema potrebbe essere causato dal software, disinstallare il software prima di continuare con altre soluzioni.
3. Se il problema che causa l'errore della replica non può essere risolto da metodi noti, rimuovere servizi di dominio Active Directory dal server e reinstallare Servizi di dominio Active Directory. Per ulteriori informazioni sulla reinstallazione di servizi di dominio Active Directory, vedere la pagina relativa alla [rimozione delle autorizzazioni da un controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se i servizi di dominio Active Directory non possono essere rimossi normalmente mentre il server è connesso alla rete, usare uno dei metodi seguenti per risolvere il problema:

   - Forzare la rimozione di servizi di dominio Active Directory in modalità ripristino servizi directory, pulire i metadati del server e reinstallare Servizi di dominio Active Directory.
   - Reinstallare il sistema operativo e ricompilare il controller di dominio.

Per ulteriori informazioni su come forzare la rimozione di servizi di dominio Active Directory, vedere [forzare la rimozione di un controller di dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Utilizzo di Repadmin per recuperare lo stato della replica @ no__t-0

Lo stato della replica è un modo importante per valutare lo stato del servizio directory. Se la replica funziona senza errori, si conoscono i controller di dominio online. Si sa anche che i sistemi e i servizi seguenti sono in funzione:


- Infrastruttura DNS
- Protocollo di autenticazione Kerberos
- Servizio ora di Windows (W32Time)
- RPC (Remote Procedure Call)
- Connettività di rete

Usare Repadmin per monitorare lo stato della replica quotidianamente eseguendo un comando che valuta lo stato di replica di tutti i controller di dominio nella foresta. La procedura genera un file con estensione CSV che è possibile aprire in Microsoft Excel e filtrare per gli errori di replica.

È possibile utilizzare la procedura seguente per recuperare lo stato di replica di tutti i controller di dominio nella foresta. 

Requisiti

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins**, o equivalente. 

Strumenti:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Per generare un foglio di calcolo repadmin/showrepl per i controller di dominio

1. Aprire una finestra del prompt dei comandi come amministratore. Nel menu Start, fare clic con il pulsante destro del mouse su prompt dei comandi e quindi scegliere Esegui come amministratore. Se viene visualizzata la finestra di dialogo controllo account utente, specificare le credenziali Enterprise Admins, se necessario, e quindi fare clic su continua.
2. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `repadmin /showrepl * /csv > showrepl.csv`
3. Aprire Excel.
4. Fare clic sul pulsante Office, fare clic su Apri, passare a showrepl. csv e quindi fare clic su Apri.
5. Nascondere o eliminare la colonna A e la colonna tipo di trasporto, come indicato di seguito:
6. Selezionare una colonna che si desidera nascondere o eliminare.

   - Per nascondere la colonna, fare clic con il pulsante destro del mouse sulla colonna, quindi scegliere Nascondi.
   - Per eliminare la colonna, fare clic con il pulsante destro del mouse sulla colonna selezionata, quindi scegliere Elimina.

7. Selezionare la riga 1 sotto la riga dell'intestazione di colonna. Nella scheda Visualizza fare clic su Blocca riquadri, quindi fare clic su blocca riga superiore.
8. Selezionare l'intero foglio di calcolo. Nella scheda dati fare clic su filtro.
9. Nella colonna ultima operazione completata, fare clic sulla freccia verso il basso, quindi fare clic su ordinamento crescente.
10. Nella colonna controller di dominio di origine fare clic sulla freccia a discesa filtro, scegliere filtri per testo, quindi fare clic su filtro personalizzato.
11. Nella finestra di dialogo Personalizza filtro automatico, in Mostra righe in cui, fare clic su non contiene. Nella casella di testo adiacente digitare <userInput>Canc</userInput> per evitare di visualizzare i risultati per i controller di dominio eliminati.
12. Ripetere il passaggio 11 per l'ultima colonna ora di errore, ma usare il valore non è uguale a, quindi digitare il valore 0.
13. Risolvere gli errori di replica.

Per ogni controller di dominio nella foresta, nel foglio di calcolo vengono visualizzati il partner di replica di origine, l'ora in cui si è verificata l'ultima replica e l'ora in cui si è verificato l'ultimo errore di replica per ogni contesto di denominazione (partizione di directory). Utilizzando Filtro automatico in Excel, è possibile visualizzare l'integrità della replica solo per i controller di dominio in esecuzione, solo per i controller di dominio che hanno esito negativo o per i controller di dominio più o meno recenti ed è possibile visualizzare i partner di replica che eseguono la replica correttamente.

## <a name="replication-problems-and-resolutions"></a>Problemi di replica e soluzioni

I problemi di replica vengono segnalati nei messaggi di evento e nei vari messaggi di errore che si verificano quando un'applicazione o un servizio tenta un'operazione. Idealmente, questi messaggi vengono raccolti dall'applicazione di monitoraggio o quando si recupera lo stato della replica.

La maggior parte dei problemi di replica viene identificata nei messaggi di evento registrati nel registro eventi del servizio directory. I problemi di replica possono anche essere identificati sotto forma di messaggi di errore nell'output del comando <system>repadmin/showrepl</system> .

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>messaggi di errore di repadmin/showrepl che indicano problemi di replica

Per identificare Active Directory problemi di replica, usare il comando <system>repadmin/showrepl</system> , come descritto nella sezione precedente. Nella tabella seguente vengono illustrati i messaggi di errore generati da questo comando, insieme alle cause principali degli errori e ai collegamenti ad argomenti che forniscono soluzioni per gli errori.

|Errore repadmin|Causa principale|Soluzione|
| --- | --- | --- |
|Il tempo trascorso dall'ultima replica con il server ha superato la durata dell'oggetto contrassegnato per la rimozione definitiva.|Un controller di dominio non ha superato la replica in ingresso con il controller di dominio di origine denominato per un periodo di tempo sufficiente perché un'eliminazione sia stata rimossa definitivamente, replicata e sottoposta a Garbage Collection da Active Directory Domain Services.|ID evento 2042: È trascorso troppo tempo da quando questo computer ha effettuato la replica|
|Nessun Neighbor in ingresso.|Se non viene visualizzato alcun elemento nella sezione "elementi adiacenti in ingresso" dell'output generato da repadmin/showrepl, il controller di dominio non è stato in grado di stabilire collegamenti di replica con un altro controller di dominio.|Correzione dei problemi di connettività della replica (ID evento 1925)| 
|Accesso negato.|Esiste un collegamento di replica tra due controller di dominio, ma la replica non può essere eseguita correttamente a causa di un errore di autenticazione.|Correzione dei problemi di sicurezza della replica| 
|L'ultimo tentativo di < > di data e ora non è riuscito con "il nome dell'account di destinazione non è corretto".|Questo problema può essere correlato a problemi di connettività, DNS o autenticazione. Se si tratta di un errore DNS, il controller di dominio locale non è riuscito a risolvere il nome DNS basato sull'identificatore univoco globale (GUID) del partner di replica.|Correzione dei problemi di ricerca DNS di replica (ID evento 1925, 2087, 2088) correzione dei problemi di sicurezza della replica correzione dei problemi di connettività della replica (ID evento 1925)| 
|Errore LDAP 49.|L'account computer del controller di dominio potrebbe non essere sincronizzato con il Centro distribuzione chiavi (KDC).|Correzione dei problemi di sicurezza della replica| 
|Impossibile aprire la connessione LDAP all'host locale|Lo strumento di amministrazione non è riuscito a contattare servizi di dominio Active Directory.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)| 
|Active Directory replica è stata interrotta.|Lo stato di avanzamento della replica in ingresso è stato interrotto da una richiesta di replica con priorità più elevata, ad esempio una richiesta generata manualmente con il comando repadmin/sync.|Attendere il completamento della replica. Questo messaggio informativo indica un normale funzionamento.| 
|Replica inviata, in attesa.| Il controller di dominio ha inviato una richiesta di replica ed è in attesa di una risposta. La replica è in corso da questa origine.|Attendere il completamento della replica. Questo messaggio informativo indica un normale funzionamento.| 

Nella tabella seguente sono elencati gli eventi comuni che potrebbero indicare problemi con la replica Active Directory, oltre alle cause principali dei problemi e ai collegamenti ad argomenti che forniscono soluzioni per i problemi. 

|ID evento e origine|Causa radice|Soluzione|
| --- | --- | --- | 
|1311 NTDS KCC|Le informazioni di configurazione della replica in servizi di dominio Active Directory non riflettono accuratamente la topologia fisica della rete.|Correzione dei problemi di topologia di replica (ID evento 1311)| 
|1388 replica NTDS|Non è attiva la coerenza di replica rigida e un oggetto residuo è stato replicato nel controller di dominio.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)|
|1925 NTDS KCC|Il tentativo di stabilire un collegamento di replica per una partizione di directory scrivibile non è riuscito. Questo evento può avere cause diverse, a seconda dell'errore.| Correzione dei problemi di connettività della replica (ID evento 1925) correzione dei problemi di ricerca DNS della replica (ID evento 1925, 2087, 2088)| 
|1988 replica NTDS|Il controller di dominio locale ha tentato di replicare un oggetto da un controller di dominio di origine che non è presente sul controller di dominio locale perché potrebbe essere stato eliminato e già sottoposto a Garbage Collection. La replica non procede per questa partizione di directory con questo partner fino a quando non viene risolta la situazione.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)|
|2042 replica NTDS|La replica non è stata eseguita con questo partner per una durata di rimozione definitiva e la replica non può continuare.|Correzione dei problemi relativi agli oggetti residui di replica (ID eventi 1388, 1988, 2042)| 
|2087 replica NTDS|Servizi di dominio Active Directory non è riuscito a risolvere il nome host DNS del controller di dominio di origine in un indirizzo IP e la replica non è riuscita.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)| 
|2088 replica NTDS |Servizi di dominio Active Directory non è riuscito a risolvere il nome host DNS del controller di dominio di origine in un indirizzo IP, ma la replica è riuscita.|Correzione dei problemi relativi alla ricerca DNS della replica (ID evento 1925, 2087, 2088)|
|Accesso net 5805|Non è stato possibile autenticare un account del computer, che in genere è dovuto a più istanze dello stesso nome computer o al nome del computer che non esegue la replica a ogni controller di dominio.|Correzione dei problemi di sicurezza della replica| 

Per ulteriori informazioni sui concetti relativi alla replica, vedere [Active Directory tecnologie di replica](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni, inclusi gli articoli di supporto specifici per i codici di errore, vedere l'articolo del supporto tecnico: [Risoluzione degli errori comuni di replica di Active Directory](https://support.microsoft.com/help/3108513)
