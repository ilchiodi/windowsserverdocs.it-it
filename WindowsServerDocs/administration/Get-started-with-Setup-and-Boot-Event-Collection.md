---
title: Informazioni di base su Raccolta eventi di configurazione e avvio
description: Impostazione di target e agenti di raccolta di Raccolta eventi di configurazione e avvio
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: high
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: c21c6f188a95caac79b461300bcc3d6e318f58d8
ms.sourcegitcommit: 8e2903c9b58646840eedd63b47a9bba6c6a06bf7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "1934412"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Informazioni di base su Raccolta eventi di configurazione e avvio

>Si applica a: Windows Server

  
## <a name="overview"></a>Panoramica  
Raccolta eventi di configurazione e avvio è una nuova funzionalità di Windows Server 2016 che consente di designare un computer "agente di raccolta" in grado di raccogliere una serie di eventi importanti che si verificano in altri computer quando vengono avviati o durante il processo di installazione. Puoi quindi analizzare gli eventi raccolti con i cmdlet Visualizzatore eventi, Message Analyzer o Windows PowerShell.  
  
In precedenza, questi eventi erano impossibili da monitorare poiché non esisteva l'infrastruttura necessaria per la loro raccolta fino a quando a meno che un computer non era già configurato. I tipi di eventi di avvio e installazione che puoi monitorare includono:  
  
-   Caricamento di driver e moduli kernel  
  
-   Enumerazione di dispositivi e inizializzazione dei loro driver (inclusi "dispositivi" quali il tipo di CPU)  
  
-   Verifica e installazione del file system  
  
-   Avvio di file eseguibili  
  
-   Avvio e completamento degli aggiornamenti del sistema  
  
-   I punti in cui il sistema diventa disponibile per l'accesso, stabilisce la connessione con un controller di dominio, il completamento del servizio viene avviato e le condivisioni di rete diventano disponibili  
  
Il computer dell'agente di raccolta deve eseguire Windows Server 2016 (può essere Server con la modalità di esperienza Desktop o Server Core). Il computer di destinazione deve eseguire Windows 10 o Windows Server 2016. È inoltre possibile eseguire questo servizio su una macchina virtuale ospitata in un computer che **non** esegue Windows Server 2016. Le seguenti combinazioni di computer di destinazione e di raccolta virtualizzati dovrebbero funzionare:  
  
|Host di virtualizzazione|Macchina virtuale agente di raccolta|Macchina virtuale di destinazione|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|sì|sì|  
|Windows10|sì|sì|  
|WindowsServer 2016|sì|sì|  
|Windows Server 2012 R2|sì|no|  
  
## <a name="installing-the-collector-service"></a>Installazione del servizio di raccolta  
A partire da Windows Server 2016, il servizio di raccolta eventi è disponibile come funzionalità facoltativa. In questa versione, puoi installarlo utilizzando DISM.exe con questo comando in un prompt di Windows PowerShell con privilegi elevati:  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
Questo comando consente di creare un servizio denominato BootEventCollector e inizia con un file di configurazione vuoto.  
  
Verifica che l'installazione sia completata controllando `get-service -displayname *boot*`. Agente di raccolta eventi di avvio dovrebbe essere in esecuzione. Viene eseguito con l'account del servizio di rete e crea un file di configurazione vuoto (Active.xml) in **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.  
  
Puoi installare il servizio Raccolta eventi di configurazione e avvio anche con la procedura guidata Aggiungi ruoli e funzionalità in Server Manager.  
  
## <a name="configuration"></a>Configurazione  
È necessario configurare due voci per raccogliere eventi di installazione e avvio.  
  
-   Nei computer di destinazione che invieranno gli eventi (vale a dire, i computer di cui vuoi monitorare installazione e avvio), abilita il trasporto KDNET/EVENT-NET e abilita l'inoltro di eventi.  
  
-   Sul computer di raccolta, specifica i computer da cui accettare eventi e la posizione in cui salvarli.  
  
> [!NOTE]  
> Non puoi configurare un computer per inviare gli eventi di avvio o a se stesso. Tuttavia, se vuoi monitorare due computer, puoi configurarli in modo da inviarsi gli eventi a vicenda.  
  
### <a name="configuring-a-target-computer"></a>Configurazione di un computer di destinazione  
In ogni computer di destinazione, devi abilitare prima il trasporto KDNET/EVENT-NET, quindi abilitare l'invio di eventi ETW mediante il trasporto e quindi riavviare il computer di destinazione. EVENT-NET è un protocollo di trasporto in kernel simile a KDNET (il protocollo debugger del kernel). EVENT-NET trasmette solo eventi e non consentire l'accesso al debugger. Questi due protocolli si escludono a vicenda; è possibile abilitare solo uno di essi alla volta.  
  
È possibile abilitare il trasporto di eventi in remoto (con Windows PowerShell) oppure in locale.  
  
##### <a name="to-enable-event-transport-remotely"></a>Per abilitare il trasporto di eventi in remoto  
  
1.  Se ha già installato Windows PowerShell Remoting nel computer di destinazione, procedi al passaggio 3. In caso contrario, nel computer di destinazione, apri un prompt dei comandi ed esegui il comando seguente:  
  
    winrm quickconfig  
  
2.  Seguire le istruzioni visualizzate e quindi riavvia il computer di destinazione. Se i computer di destinazione non si trovano nello stesso dominio del computer di raccolta, potrebbe essere necessario definirli come host attendibili. A tale scopo, effettua le seguenti operazioni:  
  
3.  Nel computer di raccolta, esegui uno dei seguenti comandi:  
  
    -   In un prompt dei comandi di Windows PowerShell: `Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`, seguito da `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` dove \ < target1 > e così via sono i nomi o indirizzi IP del computer di destinazione.  
  
    -   O in un prompt dei comandi: **winrm set winrm/config/client @{TrustedHosts="\<target1>,\<target2>,...";AllowUnencrypted="true"**  
  
        > [!IMPORTANT]  
        > In questo modo si imposta la comunicazione non crittografata, pertanto non eseguire questa operazione all'esterno di un ambiente di laboratorio.  
  
4.  Verifica la connessione remota passando al computer di raccolta ed eseguendo uno di questi comandi di Windows PowerShell:  
  
    Se il computer di destinazione è nello stesso dominio del computer di raccolta, esegui `New-PSSession -Computer <target> | Remove-PSSession`  
  
    Se il computer di destinazione non è presente nello stesso dominio, esegui `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`, che richiederà le credenziali.  
  
    Se il comando non restituisce alcun valore, la procedura in remoto è stata eseguita correttamente.  
  
5.  Nel computer di destinazione, apri un prompt dei comandi di Windows PowerShell con privilegi elevati ed esegui questo comando:  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    Di seguito < nome_destinazione > è il nome del computer di destinazione \ < ip > è l'indirizzo IP del computer di raccolta. \ < porta > è il numero di porta su cui verrà eseguito l'agente di raccolta. La chiave < a.b.c. d > è una chiave di crittografia obbligatoria per la comunicazione, che comprende quattro stringhe di caratteri alfanumerici separate da punti. La stessa chiave viene utilizzata nel computer di raccolta. Se non si immette una chiave, il sistema genererà una chiave in modo casuale. Questa sarà necessaria per il computer di raccolta, pertanto prendine nota.  
  
6.  Se disponi già di un computer di raccolta configurato, aggiorna il file di configurazione nel computer di raccolta con le informazioni per il nuovo computer di destinazione. Vedi la sezione "Configurazione del computer di raccolta" per ulteriori informazioni.  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Per abilitare il trasporto eventi in locale sul computer di destinazione  
  
1.  Avvia un prompt dei comandi con privilegi elevati ed esegui questi comandi:  
  
    **bcdedit /event yes**  
  
    **bcdedit /eventsettings net hostip:1.2.3.4 port:50000 key:a.b.c.d**  
  
    Qui "1.2.3.4" è un esempio; sostituiscilo con l'indirizzo IP del computer di raccolta. Sostituisci anche "50000" con il numero di porta in cui verrà eseguito l'agente di raccolta e "a.b.c.d" con la chiave di crittografia obbligatoria per la comunicazione. La stessa chiave viene utilizzata nel computer di raccolta. Se non immetti una chiave, il sistema genera una chiave in modo casuale. Questa sarà necessaria per il computer di raccolta, pertanto prendine nota.  
  
2.  Se disponi già di un computer di raccolta configurato, aggiorna il file di configurazione nel computer di raccolta con le informazioni per il nuovo computer di destinazione. Vedi la sezione "Configurazione del computer di raccolta" per ulteriori informazioni.  
  
**Ora che trasporto eventi è abilitato, devi abilitare il sistema ad inviare effettivamente gli eventi ETW tramite tale trasporto.**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Per abilitare l'invio di eventi ETW tramite il trasporto in remoto  
  
1.  Nel computer di raccolta, apri un prompt dei comandi di Windows PowerShell con privilegi elevati.  
  
2.  Esegui `Enable-SbecAutologger -ComputerName <target_name>`, dove < target_name > è il nome del computer di destinazione.  
  
Se non sei è in grado di configurare Windows PowerShell Remoting, puoi sempre abilitare l'invio di eventi direttamente nel computer di destinazione.  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Per abilitare l'invio di eventi ETW mediante il trasporto locale  
  
1.  Nel computer di destinazione, avvia Regedit.exe e cerca questa chiave del Registro di sistema:  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. Diverse sessioni di registrazione sono elencate come sottochiavi di questa chiave. **Setup Platform**, **NT Kernel Logger**, e **Microsoft-Windows-Setup** sono possibili scelte da utilizzare per Raccolta eventi di configurazione e avvio, ma l'opzione consigliata è **EventLog-System**. Queste chiavi vengono descritte in dettaglio [Configurazione e avvio di una sessione AutoLogger](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).  
  
2.  Nella chiave del Registro di sistema EventLog-System, modifica il valore di **LogFileMode** da **0x10000180** a **0x10080180**. Per ulteriori informazioni sui dettagli di queste impostazioni, vedi [Costanti della modalità di registrazione](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).  
  
3.  Facoltativamente, puoi abilitare anche l'inoltro dei dati di controllo bug al computer di raccolta. A tale scopo, trova la chiave del Registro di sistema HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager e crea la chiave **Debug Print Filter** con un valore di **0x1**.  
  
4.  Riavvia il computer di destinazione.  
  
### <a name="choosing-the-network-adapter"></a>Scelta della scheda di rete  
Se il computer di destinazione dispone di più di una scheda di rete, il driver KDNET sceglierà la prima supportata nell'elenco. Puoi specificare una scheda di rete particolare da utilizzare per inoltrare gli eventi di installazione con la procedura seguente:  
  
##### <a name="to-specify-a-network-adapter"></a>Per specificare una scheda di rete  
  
1.  Nel computer di destinazione, apri Gestione dispositivi, espandi **Schede di rete**, trova la scheda di rete che vuoi utilizzare e fai clic su di essa.  
  
2.  Nel menu visualizzato, fai clic su **Proprietà**e quindi sulla scheda **Dettagli**. Espandi il menu nel campo **Proprietà** , scorri l'elenco per trovare **Informazioni sulla posizione** (l'elenco probabilmente non è in ordine alfabetico) e quindi fai clic su di esso. Il valore sarà rappresentato da una stringa nel formato **PCI bus X, device Y, function Z**. Prendi nota di X.Y.Z; si tratta dei parametri bus necessari per il comando seguente.  
  
3.  Esegui uno dei seguenti comandi:  
  
    Da un prompt di Windows PowerShell con privilegi elevati: `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    Da un prompt dei comandi con privilegi elevati: **bcdedit /eventsettings net hostip:aaa port:50000 key:bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>Convalidare la configurazione del computer di destinazione  
Per controllare le impostazioni nel computer di destinazione, apri un prompt dei comandi con privilegi elevati ed esegui **bcdedit /enum**. Al termine, esegui **bcdedit /eventsettings**. Puoi controllare i valori seguenti:  
  
-   Chiave  
  
-   Debugtype = NET  
  
-   Hostip = \ < indirizzo IP dell'agente di raccolta>  
  
-   Porta = \ < numero di porta specificato utilizzabile dall'agente di raccolta>  
  
-   DHCP = yes  
  
Controlla inoltre che sia stato abilitato **bcdedit /event**, poiché **/debug** e **/event** si escludono a vicenda. È possibile eseguire solo uno o l'altro. Analogamente, non puoi combinare /eventsettings con /debug o /dbgsettings con /event.  
  
Nota inoltre che la raccolta degli eventi non funziona se impostata su una porta seriale.  
  
### <a name="configuring-the-collector-computer"></a>Configurazione del computer di raccolta  
Il servizio di raccolta riceve gli eventi e li salva nei file ETL. Questi file ETL possono essere letti da altri strumenti, ad esempio i cmdlets Visualizzatore eventi, Message Analyzer, Wevtutil e Windows PowerShell.  
  
Poiché il formato ETW non consente di specificare il nome del computer di destinazione, gli eventi per ogni computer di destinazione devono essere salvati in un file separato. Gli strumenti di visualizzazione possono visualizzare il nome del computer, ma sarà il nome del computer in cui viene eseguito lo strumento.  
  
Più precisamente, a ciascun computer di destinazione viene assegnato un anello di file ETL. Ogni nome di file include un indice da 000 a un valore massimo che puoi configurare (fino a 999). Quando il file raggiunge la dimensione massima configurata, passa gli eventi di scrittura sul file successivo. Dopo il file più alto possibile torna di nuovo all'indice di file 000. In questo modo, i file vengono automaticamente riciclati, limitando l'utilizzo dello spazio su disco. Puoi inoltre possibile impostare criteri di conservazione esterna aggiuntivi per limitare ulteriormente l'utilizzo del disco; ad esempio, puoi eliminare il file più vecchi di un numero determinato di giorni.  
  
I file ETL raccolti vengono in genere memorizzati nella directory **c:\ProgramData\Microsoft\BootEventCollector\Etl** (che potrebbe includere ulteriori sottodirectory). Puoi trovare il file di registro più recente ordinando in base all'ora dell'ultima modifica. È inoltre disponibile un log di stato (in genere in **c:\ProgramData\Microsoft\BootEventCollector\Logs**), che registra ogni volta che l'agente di raccolta attiva la scrittura su un nuovo file.  
  
È inoltre disponibile un registro dell'agente di raccolta, che registra le informazioni sull'agente di raccolta stesso. Puoi conservare questo registro in formato ETW (in cui gli eventi vengono segnalati al servizio del registro di Windows; l'impostazione predefinita) o in un file (in genere in **c:\ProgramData\Microsoft\BootEventCollector\Logs**). L'utilizzo di un file potrebbe essere utile se desideri abilitare le modalità dettagliate per produrre una grande quantità di dati. Puoi anche impostare il registro per scrivere su un output standard eseguendo l'agente di raccolta dati dalla riga di comando.  
  
**Creazione del file di configurazione dell'agente di raccolta**  
  
Quando abiliti il servizio, tre file di configurazione XML vengono creati e archiviati in **c:\ProgramData\Microsoft\ BootEventCollector\Config**:  
  
-   **Active.xml** questo file contiene la configurazione attiva corrente del servizio di raccolta.  Dopo l'installazione, questo file ha lo stesso contenuto di Empty.xml. Quando imposti una nuova configurazione dell'agente di raccolta, lo salvi in questo file.  
  
-   **Empty.xml** questo file contiene gli elementi di configurazione minima necessari con i valori predefiniti impostati. Non abilitata alcuna raccolta, ma consente solo al servizio di raccolta di essere avviato in modalità inattiva.  
  
-   **Example.xml** questo file fornisce esempi e spiegazioni dei possibili elementi di configurazione.  
  
**Scelta di un limite di dimensione file**  
  
Una delle decisioni che devi prendere è l'impostazione di un limite di dimensione file. Il limite di dimensioni del file migliore dipende dal volume previsto di eventi e dallo spazio disponibile su disco. File di dimensioni inferiori sono più pratici dal punto di vista della pulizia dei dati precedenti. Tuttavia, ogni file include un sovraccarico di intestazione di 64 KB e la lettura molti file per recuperare la cronologia combinata potrebbe non essere conveniente. Il limite di dimensioni minimo assoluto del file è 256 KB. Un limite di dimensione file pratico e ragionevole deve essere superiore a 1 MB, e 10 MB probabilmente è un valore tipico valido. Un limite maggiore potrebbe essere ragionevole se si prevede un gran numero di eventi.  
  
Esistono alcune informazioni da tenere presenti in relazione al file di configurazione:  
  
-   L'indirizzo di computer di destinazione. Puoi utilizzare il suo indirizzo IPv4, un indirizzo MAC o un GUID SMBIOS. Quando scegli l'indirizzo da utilizzare, tieni presente questi fattori:  
  
    -   L'indirizzo IPv4 funziona meglio con l'assegnazione statica degli indirizzi IP. Tuttavia, anche gli indirizzi IP statici devono essere disponibili tramite DHCP.  
  
    -   Un indirizzo MAC o il GUID SMBIOS sono utili quando noti in precedenza, ma gli indirizzi IP vengono assegnati in modo dinamico.  
  
    -   Gli indirizzi IPv6 non sono supportati dal protocollo EVENT-NET.  
  
    -   È possibile specificare più modi per identificare il computer. Ad esempio, se hardware fisico sta per essere sostituito, puoi immettere entrambi gli indirizzi MAC nuovo e vecchio, e possono essere entrambi accettati.  
  
-   La chiave di crittografia utilizzata per le comunicazioni con il computer di raccolta  
  
-   Il nome del computer di destinazione. Puoi utilizzare l'indirizzo IP, il nome host o un altro nome come nome del computer.  
  
-   Il nome del file ETL da utilizzare e la configurazione delle relative dimensioni dell'anello  
  
##### <a name="to-create-the-configuration-file"></a>Per creare il file di configurazione  
  
1.  Apri un prompt dei comandi di Windows PowerShell con privilegi elevati e cambia le directory in %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.  
  
2.  Digita `notepad .\newconfig.xml` e premi INVIO.  
  
3.  Copia questa configurazione di esempio nella finestra del Blocco note:  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > Il nodo radice è \<agente di raccolta dati>. Gli attributi specificano la versione della sintassi del file di configurazione e il nome del file di log di stato.  
    >   
    > L'elemento \<common> raggruppa insieme più destinazioni specificando gli elementi di configurazione comuni, in modo molto simile a come un gruppo di utenti può essere utilizzato per specificare le autorizzazioni comuni per più utenti.  
    >   
    > L'elemento \<collectorport> consente di definire il numero di porta UDP dove l'agente di raccolta resterà in ascolto dei dati in ingresso. Questa è la stessa porta specificata nel passaggio di configurazione di destinazione per Bcdedit. L'agente di raccolta supporta solo una porta e tutte le destinazioni devono connettersi alla stessa porta.  
    >   
    > L'elemento \<forwarder> specifica la modalità di inoltro degli eventi ETW ricevuti dal computer di destinazione. Esiste un solo tipo di server di inoltro, che li scrive nei file ETL. I parametri specificano il motivo del nome file, il limite di dimensioni per ogni del'anello e le dimensioni dell'anello per ogni computer. L'impostazione "toxml" consente di specificare che gli eventi ETW verranno scritti in formato binario come vengono ricevuti, senza la conversione in formato XML. Vedi la sezione "Conversione degli eventi XML" per informazioni sulla scelta tra conferire gli eventi in formato XML o non. Il motivo del nome file contiene queste sostituzioni: {computer} per il nome del computer e {#3} per l'indice del file dell'anello.  
    >   
    > In questo file di esempio, vengono definiti due computer di destinazione con l'elemento \<target>. Ogni definizione consente di specificare l'indirizzo IP con \ < ipv4 >, ma è anche possibile utilizzare l'indirizzo MAC (ad esempio, <mac value="11:22:33:44:55:66"\/> o <mac value="11-22-33-44-55-66"\/>) o il GUID SMBIOS (ad esempio, <guid value="{269076F9-4B77-46E1-B03B-CA5003775B88}"\/>) per identificare il computer di destinazione. Nota inoltre la chiave di crittografia (esattamente come specificata o generata con Bcdedit nel computer di destinazione) e il nome del computer.  
  
4.  Immetti i dettagli per ogni computer di destinazione come elemento \< target> separato nel file di configurazione, quindi salva Newconfig.xml e chiudi Blocco note.  
  
5.  Applica la nuova configurazione con `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`. L'output dovrebbe essere restituito con il campo "true". Se viene visualizzato un altro risultato, vedi la sezione Risoluzione dei problemi di questo argomento.  
  
Puoi sempre controllare la configurazione attiva corrente con `(Get-SbecActiveConfig).text`.  
  
Puoi eseguire una convalida sul file di configurazione con `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result`.  
  
Sebbene il comando di Windows PowerShell per applicare una nuova configurazione consente di aggiornare automaticamente il servizio senza che sia necessario riavviarlo, puoi sempre riavviare il servizio da solo con uno dei seguenti comandi:  
  
-   Con Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   In un prompt dei comandi comune: **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>Configurazione di Nano Server come computer di destinazione  
L'interfaccia minima offerto da Nano Server può talvolta rendere difficile diagnosticare i problemi ad esso relativi. Puoi configurare l'immagine di Nano Server per partecipare automaticamente a Raccolta eventi di configurazione e avvio, inviando dati di diagnostica a un computer di raccolta senza ulteriore intervento da parte tua. A tale scopo, attieniti alla seguente procedura:  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Per configurare Nano Server come computer di destinazione  
  
1.  Crea l'immagine di base di Nano Server. Vedi [Informazioni di base su Nano Server](https://technet.microsoft.com/library/mt126167.aspx) per informazioni dettagliate.  
  
2.  Configura un computer di raccolta come descritto nella sezione "Configurazione del computer di raccolta" di questo argomento.  
  
3.  Aggiungi le chiavi del Registro di sistema AutoLogger per consentire l'invio di messaggi di diagnostica. A tale scopo, monta il disco rigido virtuale Nano Server creato nel passaggio 1, carica l'hive del Registro di sistema, quindi aggiungi alcune chiavi del Registro di sistema. In questo esempio, l'immagine Nano Server è in C:\NanoServer; il percorso può essere diverso, pertanto modifica la procedura di conseguenza.  
  
    1.  Sul computer di raccolta, copiare la cartella **... \Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** e incollala nella directory **... \Windows\System32\WindowsPowerShell\v1.0\Modules** del computer che stai utilizzando per modificare il disco rigido virtuale di Nano Server.  
  
    2.  Avvia una console di Windows PowerShell con autorizzazioni elevate ed esegui `Import-Module BootEventCollector` .  
  
    3.  Aggiorna il Registro di sistema del disco rigido virtuale di Nano Server per abilitare AutoLoggers. A tale scopo, esegui `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`. In questo modo si aggiunge un elenco di base degli eventi di installazione e avvio più comuni; puoi cercarne altri in [Controllo di sessioni di Event Tracing](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).  
  
4.  Aggiorna le impostazioni di dati configurazione di avvio nell'immagine Nano Server per abilitare il flag Eventi e imposta il computer di raccolta per verificare che gli eventi di diagnostica vengano inviati al server appropriato. Prendi nota di indirizzo IPv4, porta TCP e chiave di crittografia del computer di raccolta che hai configurato nel file Active.XML dell'agente di raccolta (descritto altrove in questo argomento). Utilizza questo comando in una console di Windows PowerShell con autorizzazioni elevate: `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Aggiorna il computer di raccolta per la ricezione di eventi inviati dal computer Nano Server aggiungendo l'intervallo di indirizzi IPv4, l'indirizzo IPv4 specifico o l'indirizzo MAC di Nano Server al file Active.XML nel computer di raccolta (vedi la sezione "Configurazione del computer di raccolta" di questo argomento).  
  
## <a name="starting-the-event-collector-service"></a>Avvio del servizio di raccolta eventi  
Dopo aver salvato un file di configurazione valido nel computer di raccolta e aver configurato un computer di destinazione, non appena viene riavviato il computer di destinazione, viene eseguita la connessione all'agente di raccolta e gli eventi verranno raccolti.  
  
Il registro per il servizio di raccolta stesso (diverso dai dati di installazione e avvio raccolti dal servizio), è disponibile in Microsoft-Windows-BootEvent-Collector/Admin. Per un'interfaccia grafica degli eventi, utilizza Visualizzatore eventi. Crear una nuova visualizzazione; espandi **Registri applicazioni e servizi**, quindi espandi **Microsoft** e quindi **Windows**. Trova **BootEvent-Collector**, espandilo e trova **Admin**.  

-   Con Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   In un prompt dei comandi comune: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi  
  
### <a name="troubleshooting-installation-of-the-feature"></a>Risoluzione dei problemi di installazione della funzionalità  
  
||Errore|Descrizione dell'errore|Sintomo|Problema potenziale|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|L'opzione caratteristica-nome non viene riconosciuta in questo contesto||-   Questa situazione può verificarsi se il nome della funzionalità viene scritto male. Verifica di aver utilizzato l'ortografia corretta e riprova.<br />-   Verificare che questa funzionalità sia disponibile nella versione del sistema operativo in uso. In Windows PowerShell, esegui **dism /online /get-features &#124; ?{$_ -match "boot"}**. Se non viene restituita alcuna corrispondenza, probabilmente stai eseguendo una versione che non supporta questa funzionalità.|  
|Dism.exe|0x800f080c|Il \<nome> della funzionalità è sconosciuto.||Come sopra|  
  
### <a name="troubleshooting-the-collector"></a>Risoluzione dei problemi dell'agente di raccolta dati  
  
**Registrazione:**  
L'agente di raccolta registra gli eventi come Microsoft-Windows-BootEvent-Collector del provider ETW. È il primo posto in cui si dovrebbe cercare la soluzione dei problemi con l'agente di raccolta. Puoi trovarli in Visualizzatore eventi in Registri applicazioni e servizi > Microsoft > Windows > BootEvent Collector > Admin o puoi leggerli in una finestra di comando con uno dei seguenti comandi:  
  
In un prompt dei comandi comune: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
In un prompt di Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (puoi aggiungere `-Oldest` per restituire l'elenco in ordine cronologico con gli eventi meno recenti prima)  
  
Puoi modificare il livello di dettaglio nei registri da "error" a "warning", "info" (impostazione predefinita), "verbose" e "debug". Livelli più dettagliati rispetto a "info" sono utili per la diagnosi di problemi con i computer di destinazione che non si connette, ma potrebbero generare una grande quantità di dati, pertanto presta particolare attenzione nell'utilizzarli.  
  
Imposta il livello di registrazione minimo nell'elemento \<collector> del file di configurazione. Ad esempio: <collector configVersionMajor="1" minlog\="verbose">.  
  
Il livello dettagliato registra un record per ogni pacchetto ricevuto mentre viene elaborato. Il livello debug consente di aggiungere ulteriori dettagli di elaborazione ed elimina anche il contenuto di tutti i pacchetti ETW ricevuti.  
  
A livello di debug, potrebbe essere utile scrivere il registro in un file anziché tentare di visualizzarlo nel sistema di registrazione normale. A tale scopo, aggiungi un ulteriore elemento nell'elemento \<collector> del file di configurazione:  
  
<collector configVersionMajor="1" minlog="debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
 **Un approccio consigliato per la risoluzione dei problemi dell'agente di raccolta dati:**  
   
 1. In primo luogo, verifica che l'agente di raccolta abbia ricevuto la connessione dalla destinazione (il file verrà creato solo quando la destinazione inizia a inviare i messaggi)   
```  
Get-SbecForwarding  
```  
Se il valore restituito indica che non vi è una connessione da questa destinazione il problema potrebbe essere nelle impostazioni di autologger. Se non viene restituito alcun risultato, il problema riguarda la connessione KDNET con cui iniziare. Per diagnosticare i problemi di connessione KDNET, controlla la connessione su entrambe le estremità (vale a dire l'agente di raccolta e la destinazione).  
  
2. Per verificare la diagnostica estesa dall'agente di raccolta, aggiungi quanto segue all'elemento \<agente di raccolta> del file di configurazione:  
\<collector ... minlog="verbose">  
In questo modo vengono abilitati i messaggi su tutti i pacchetti ricevuti.  
3. Controlla se i pacchetti sono stati ricevuti. All'occorrenza è consigliabile scrivere il registro in modalità dettagliata direttamente in un file anziché mediante ETW. A tale scopo, aggiungi quanto segue all'elemento \<collector> del file di configurazione:  
\<collector ... minlog="verbose" log="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
4. Controlla i registri eventi per i messaggi sui pacchetti ricevuti. Controlla se i pacchetti sono stati ricevuti. Se i pacchetti sono stati ricevuti ma non sono corretti, controlla i messaggi di evento per i dettagli.  
5. Dal lato destinazione KDNET scrive alcune informazioni diagnostiche nel Registro di sistema. Cerca   
**HKLM\SYSTEM\CurrentControlSet\Services\kdnet** per i messaggi.  
  KdInitStatus (DWORD) sarà = 0 su esito positivo e visualizza un codice di errore in caso di errore  
  KdInitErrorString = spiegazione dell'errore (contiene anche messaggi informativi se non vi sono errori)  
  
6. Esegui Ipconfig.exe nella destinazione e controlla il nome del dispositivo che viene visualizzato. Se KDNET viene caricato correttamente, il nome del dispositivo deve essere simile a "kdnic" anziché il nome scheda del fornitore originale.  
7. Verifica se DHCP è configurato per la destinazione. KDNET richiede obbligatoriamente DHCP.  
8. Verifica che l'agente di raccolta sia nella stessa rete della destinazione. In caso contrario, controlla se il routing è configurato correttamente, in particolare l'impostazione del gateway predefinito per DHCP.  
  
  
**Stato della connessione**  
  
Puoi controllare l'elenco corrente di connessioni stabilite e le informazioni su dove i dati vengono inoltrati con `Get-SbecForwarding`.  
  
Puoi inoltre scaricare la cronologia delle modifiche dello stato delle connessioni con `Get-SbecHistory`.  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>Risoluzione dei problemi di impostazione di una nuova configurazione  
Se hai applicato la configurazione con il comando Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`, la variabile `$result` conterrà informazioni sulla distribuzione. Puoi inviare una query a questa variabile per ottenere diverse informazioni:  
  
Ottieni informazioni sugli errori con `$result.ErrorString`. Se vengono segnalati errori qui, significa che la nuova configurazione non è stata applicata e la configurazione precedente è invariata.  
  
Ottieni avvisi con `$result.WarningString`.  
  
Ottieni informazioni sui dettagli della configurazione con `$result.InfoString`.  
  
Puoi ottenere il risultato completo con `$result | fl *`.  
In alternativa, se non vuoi salvare il risultato in una variabile, puoi utilizzare `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`.  
  
### <a name="troubleshooting-target-computers"></a>Risoluzione dei problemi relativi ai computer di destinazione  
  
||Errore|Descrizione dell'errore|Sintomo|Problema potenziale|  
|-|---------|---------------------|-----------|---------------------|  
|Computer di destinazione||Il computer di destinazione non è connesso all'agente di raccolta||-   Il computer di destinazione non è stato riavviato dopo che è stato configurato. Riavvia il computer di destinazione.<br />-   Il computer di destinazione contiene impostazioni di dati configurazione di avvio non corrette. Controlla le impostazioni nella sezione "Convalidare le Impostazioni del computer di destinazione". Risolvi il problema e quindi riavvia il computer di destinazione.<br />-   Il driver KDNET/EVENT-NET non è stato in grado di connettersi a una scheda di rete o è stato connesso alla scheda di rete non corretta. In Windows PowerShell, esegui `gwmi Win32_NetworkAdapter` e controlla l'output per uno dei ServiceName **kdnic**. Se viene selezionata la scheda di rete non corretta, esegui nuovamente i passaggi descritti in "Per specificare una scheda di rete". Se non viene visualizzata alcuna scheda di rete, è possibile che il driver non supporti nessuna delle tue schede di rete.<br>**Vedi anche** "Un approccio consigliato per la soluzione dei problemi dell'agente di raccolta" sopra, in particolare passaggi da 5 a 8.|  
|Agente di raccolta||Non vedo più gli eventi dopo la migrazione della macchina virtuale su cui è ospitato il mio agente di raccolta.||Verifica che l'indirizzo IP del computer di raccolta non sia stato modificato. In questo caso, rivedi "Per consentire l'invio di eventi ETW tramite il trasporto in modalità remota".|  
|Agente di raccolta||I file ETL non vengono creati.|`Get-SbecForwarding` Mostra che il computer di destinazione è connesso, senza errori, ma non vengono creati i file ETL.|Il computer di destinazione probabilmente non ha inviato ancora dati; I file ETL vengono creati solo quando si ricevono dati.|  
|Agente di raccolta||Un evento non viene visualizzato nel file ETL.|Il computer di destinazione ha inviato un evento ma quando viene letto il file ETL con Visualizzatore eventi di Message Analyzer, l'evento non è presente.|-   L'evento può essere ancora nel buffer. Gli eventi non vengono scritti nel file ETL fino a quando non viene raccolto un buffer completo di 64 KB o si è verificato un timeout di circa 10-15 secondi con nessun nuovo evento. Attendi la scadenza del timeout o scarica il buffer con `Save-SbecInstance`.<br />-   Il manifesto dell'evento non è disponibile nel computer di raccolta o nel computer in cui viene eseguito Visualizzatore eventi o Message Analyzer.  In questo caso, l'agente di raccolta potrebbe non essere in grado di elaborare l'evento (controlla il registro dell'agente di raccolta) o il visualizzatore potrebbe non essere in grado di visualizzarlo.  È consigliabile avere tutti i manifesti installati nel computer di raccolta e installare gli aggiornamenti nel computer di raccolta prima di installarli nei computer di destinazione.|