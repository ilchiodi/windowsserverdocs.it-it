---
title: Introduzione alla raccolta eventi di configurazione e avvio
description: Impostazione di target e agenti di raccolta di Raccolta eventi di configurazione e avvio
ms.prod: windows-server
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: 1160a5dcf2f647a335a22eba54501589db6695ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852074"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Introduzione alla raccolta eventi di configurazione e avvio

>Si applica a: Windows Server

  
## <a name="overview"></a>Panoramica  
Il programma di installazione e la raccolta di eventi di avvio è una nuova funzionalità di Windows Server 2016 che consente di designare un computer dell'agente di raccolta in grado di raccogliere una serie di eventi importanti che si verificano in altri computer quando vengono avviati o passano attraverso il processo di installazione. Quindi è possibile analizzare gli eventi raccolti con i cmdlet di Windows PowerShell, Message Analyzer, Wevtutil o il Visualizzatore eventi in un secondo momento.  
  
In precedenza, questi eventi sono stati impossibili da monitorare, poiché l'infrastruttura necessaria per raccogliere tali informazioni non esiste fino a quando non è già stato configurato un computer. I tipi di eventi di avvio e l'installazione che è possibile monitorare includono:  
  
-   Caricamento dei moduli del kernel e driver  
  
-   Enumerazione dei dispositivi e inizializzazione dei relativi driver (inclusi i dispositivi come il tipo di CPU)  
  
-   Verifica e il montaggio del file System  
  
-   Avvio di file eseguibili  
  
-   Data di inizio e completamento degli aggiornamenti del sistema  
  
-   Quando il sistema diventa disponibile per l'accesso, i punti di connessione con un controller di dominio, il completamento dell'avvio del servizio e la disponibilità delle condivisioni di rete  
  
Il computer dell'agente di raccolta deve eseguire Windows Server 2016 (può essere in dei Server con la modalità di esperienza Desktop o Server Core). Il computer di destinazione deve essere in esecuzione Windows 10 o Windows Server 2016. È inoltre possibile eseguire questo servizio in una macchina virtuale che è ospitata in un computer che è **non** in esecuzione Windows Server 2016. Le seguenti combinazioni di agente di raccolta dati virtualizzati e computer di destinazione è attualmente possibile usare:  
  
|Host di virtualizzazione|Macchina virtuale dell'agente di raccolta|Macchina virtuale di destinazione|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|sì|sì|  
|Windows 10|sì|sì|  
|Windows Server 2016|sì|sì|  
|Windows Server 2012 R2|sì|no|  
  
## <a name="installing-the-collector-service"></a>L'installazione del servizio agente di raccolta dati  
A partire da Windows Server 2016, il servizio agente di raccolta di eventi è disponibile come funzionalità facoltativa. In questa versione, è possibile installarlo tramite DISM.exe con questo comando al prompt di Windows PowerShell con privilegi elevati:  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
Questo comando crea un servizio denominato BootEventCollector e viene avviata con un file di configurazione vuoto.  
  
Verificare che l'installazione eseguita controllando `get-service -displayname *boot*`. L'agente di raccolta di eventi di avvio deve essere in esecuzione. Viene eseguito con l'Account di servizio di rete e crea un file di configurazione vuoto (Active.xml) in **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.  
  
È inoltre possibile installare il servizio di installazione e la raccolta degli eventi di avvio con l'aggiunta guidata ruoli e funzionalità in Server Manager.  
  
## <a name="configuration"></a>Configurazione  
È necessario configurare due elementi per raccogliere gli eventi di avvio e l'installazione.  
  
-   Nei computer di destinazione che invia gli eventi (ovvero, i computer la cui proprietà di installazione e avvio si desidera monitorare), abilitare il trasporto di EVENTI/KDNET-NET e abilitare l'inoltro di eventi.  
  
-   Sul computer agente, specificare i computer per accettare gli eventi e la posizione in cui verranno salvati.  
  
> [!NOTE]  
> È possibile configurare un computer per inviare gli eventi di avvio o l'avvio a se stesso. Ma se si desidera monitorare due computer, è possibile configurare in modo da inviare gli eventi tra loro.  
  
### <a name="configuring-a-target-computer"></a>Configurazione di un computer di destinazione  
In ogni computer di destinazione, è necessario abilitare prima il trasporto di EVENTI/KDNET-NET, quindi consentire l'invio di eventi ETW tramite il trasporto e quindi riavviare il computer di destinazione. EVENTO-NET è un protocollo di trasporto nel kernel che è simile a KDNET (il protocollo del debugger del kernel). EVENTO NET solo trasmette gli eventi e non consentire l'accesso del debugger. Questi due protocolli si escludono a vicenda; è possibile abilitare solo uno di essi alla volta.  
  
È possibile abilitare eventi di trasporto in modalità remota (con Windows PowerShell) o in locale.  
  
##### <a name="to-enable-event-transport-remotely"></a>Per abilitare il trasporto di eventi in modalità remota  
  
1.  Se Gestione remota di Windows PowerShell già stato configurato nel computer di destinazione, andare al passaggio 3. In caso contrario, quindi sul computer di destinazione, aprire un prompt dei comandi ed eseguire il comando seguente:  
  
    WinRM quickconfig  
  
2.  Rispondere alle richieste e quindi riavviare il computer di destinazione. Se il computer di destinazione non sono nello stesso dominio del computer dell'agente di raccolta, potrebbe essere necessario definirli come host attendibile. A tale scopo, effettuare l'operazione seguente:  
  
3.  Sul computer agente, eseguire questi comandi:  
  
    -   Nel prompt di Windows PowerShell: `Set-Item -Force WSMan:\localhost\Client\TrustedHosts <target1>,<target2>,...`, seguito da `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` in \<target1 > e così via sono i nomi o indirizzi IP dei computer di destinazione.  
  
    -   In alternativa, in un prompt dei comandi: **winrm set winrm/config/client @ {TrustedHosts =\<target1 >\<target2 >,...; AllowUnencrypted = true}**  
  
        > [!IMPORTANT]  
        > Consente di impostare la comunicazione non crittografata, pertanto non all'esterno di un ambiente lab.  
  
4.  Verificare la connessione remota accedendo al computer dell'agente di raccolta e in esecuzione uno di questi comandi di Windows PowerShell:  
  
    Se il computer di destinazione si trova nello stesso dominio del computer dell'agente di raccolta, eseguire `New-PSSession -Computer <target> | Remove-PSSession`  
  
    Se il computer di destinazione non è presente nello stesso dominio, eseguire `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`, che verranno richieste le credenziali.  
  
    Se il comando non restituisce alcun valore, .NET remoting è riuscita.  
  
5.  Nel computer di destinazione, aprire un prompt di Windows PowerShell con privilegi elevato ed eseguire questo comando:  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    Qui < target_name > è il nome del computer di destinazione, \<ip > è l'indirizzo IP del computer di raccolta. \<porta > è il numero di porta in cui verrà eseguito l'agente di raccolta. La chiave < a.b.c. d > è una chiave di crittografia richiesto per la comunicazione, che comprende quattro stringhe alfanumeriche separate da punti. La stessa chiave viene utilizzata sul computer agente. Se non si immette una chiave, il sistema genera una chiave casuale. sarà necessaria per il computer dell'agente di raccolta, quindi prendere nota di esso.  
  
6.  Se si dispone già di un computer agente configurare, aggiornare il file di configurazione sul computer agente con le informazioni per il nuovo computer di destinazione. Per informazioni dettagliate, vedere la sezione configurazione del computer dell'agente di raccolta.  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Per abilitare il trasporto di eventi in locale nel computer di destinazione  
  
1.  Avviare un prompt dei comandi con privilegi elevati e quindi eseguire questi comandi:  
  
    **bcdedit organizzazione bcdedit Sì**  
  
    **bcdedit/eventsettings NET HostIP: 1.2.3.4 porta: 50000 chiave: a. b. c. d**  
  
    Di seguito è riportato un esempio di 1.2.3.4. sostituire con l'indirizzo IP del computer dell'agente di raccolta. Sostituire anche 50000 con il numero di porta in cui viene eseguito l'agente di raccolta e a. b. c. d con la chiave di crittografia necessaria per la comunicazione. La stessa chiave viene utilizzata sul computer agente. Se non si immette una chiave, il sistema genera una chiave casuale. sarà necessaria per il computer dell'agente di raccolta, quindi prendere nota di esso.  
  
2.  Se si dispone già di un computer agente configurare, aggiornare il file di configurazione sul computer agente con le informazioni per il nuovo computer di destinazione. Per informazioni dettagliate, vedere la sezione configurazione del computer dell'agente di raccolta.  
  
**Ora che il trasporto eventi è abilitato, è necessario consentire al sistema di inviare effettivamente gli eventi ETW tramite il trasporto.**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Per consentire l'invio di eventi ETW tramite il trasporto in modalità remota  
  
1.  Sul computer agente, aprire un prompt di Windows PowerShell con privilegi elevato.  
  
2.  Eseguire `Enable-SbecAutologger -ComputerName <target_name>`, dove < target_name > è il nome del computer di destinazione.  
  
Se non è in grado di configurare la gestione remota di Windows PowerShell, è sempre possibile abilitare l'invio di eventi direttamente nel computer di destinazione.  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Per consentire l'invio di eventi ETW tramite il trasporto locale  
  
1.  Nel computer di destinazione, avviare Regedit.exe e trovare questa chiave del Registro di sistema:  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. Varie sessioni del log vengono elencate come sottochiavi di questa chiave. **Programma di installazione piattaforma**, **NT Kernel Logger**, e **Microsoft-Windows-Setup** sono possibili scelte per l'utilizzo con il programma di installazione e la raccolta degli eventi di avvio, ma l'opzione consigliata **registro eventi sistema**. Queste chiavi vengono descritti in dettaglio [configurazione e l'avvio di una sessione AutoLogger](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).  
  
2.  Nella chiave del registro eventi di sistema, modificare il valore di **LogFileMode** da **0x10000180** a **0x10080180**. Per ulteriori informazioni sui dettagli di queste impostazioni, vedere [costanti della modalità di registrazione](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).  
  
3.  Facoltativamente, è anche possibile attivare l'inoltro dei dati di controllo bug al computer dell'agente di raccolta. A tale scopo, individuare il Registro di sistema chiave HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager e creare la chiave **Debug stampa filtro** con un valore di **0x1**.  
  
4.  Riavviare il computer di destinazione.  
  
### <a name="choosing-the-network-adapter"></a>Scegliere la scheda di rete  
Se il computer di destinazione ha più di una scheda di rete, il driver KDNET sceglierà il primo supportati elencati. È possibile specificare una scheda di rete da utilizzare per l'inoltro di eventi di installazione eseguendo i passaggi seguenti:  
  
##### <a name="to-specify-a-network-adapter"></a>Per specificare una scheda di rete  
  
1.  Nel computer di destinazione, aprire Gestione dispositivi espandere **le schede di rete**, individuare la scheda di rete si desidera utilizzare e farvi clic.  
  
2.  Nel menu visualizzato, fai clic su **Proprietà**e quindi sulla scheda **Dettagli**. Espandi il menu nel campo **Proprietà** , scorri l'elenco per trovare **Informazioni sulla posizione** (l'elenco probabilmente non è in ordine alfabetico) e quindi fai clic su di esso. Il valore sarà rappresentato da una stringa nel formato **PCI bus X, device Y, function Z**. Prendi nota di X.Y.Z; si tratta dei parametri bus necessari per il comando seguente.  
  
3.  Eseguire uno dei seguenti comandi:  
  
    Da un prompt di Windows PowerShell con privilegi elevati: `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    Da un prompt dei comandi con privilegi elevati: **/eventsettings bcdedit net hostip:aaa porta: 50000 chiave: bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>Convalidare la configurazione di computer di destinazione  
Per controllare le impostazioni nel computer di destinazione, aprire un prompt dei comandi con privilegi elevati ed eseguire **bcdedit /enum**. Al termine, quindi eseguire **bcdedit /eventsettings**. È possibile verificare i valori seguenti:  
  
-   Chiave  
  
-   DEBUGTYPE = NET  
  
-   Hostip = \<indirizzo IP dell'agente di raccolta >  
  
-   Porta = \<specificato per l'agente di raccolta da utilizzare numero porta >  
  
-   DHCP = Sì  
  
Controllare inoltre che sia stata abilitata **organizzazione di eventi / bcdedit**, poiché **/debug** e **organizzazione di eventi /** si escludono a vicenda. È possibile eseguire solo uno o l'altro. Analogamente, è possibile combinare /eventsettings con /debug o /dbgsettings con l'organizzazione di eventi /.  
  
Si noti inoltre che la raccolta degli eventi non funziona se è impostato su una porta seriale.  
  
### <a name="configuring-the-collector-computer"></a>Configurazione del computer dell'agente di raccolta  
Il servizio agente di raccolta riceve gli eventi e li salva nel file ETL. Questi file ETL possono essere letti da altri strumenti, ad esempio il Visualizzatore eventi, i cmdlet di Windows PowerShell, Wevtutil e Message Analyzer.  
  
Poiché il formato ETW non consente di specificare il nome di computer di destinazione, gli eventi per ogni computer di destinazione devono essere salvati in un file separato. Gli strumenti di visualizzazione potrebbero mostrare un nome di computer, ma sarà il nome del computer in cui viene eseguito lo strumento.  
  
Più precisamente, ogni computer di destinazione viene assegnato un anello di file ETL. Ogni nome di file include un indice da 000 a un valore massimo che si configura (fino a 999). Quando il file raggiunge la dimensione massima configurata, si passa gli eventi di scrittura al file successivo. Dopo il file più alto possibile passa indietro per file di indice 000. In questo modo, i file vengono automaticamente riciclati, limitare l'utilizzo di spazio su disco. È inoltre possibile impostare criteri di conservazione esterni aggiuntivi per limitare ulteriormente l'utilizzo del disco; ad esempio, è possibile eliminare i file più vecchi di un determinato numero di giorni.  
  
File ETL raccolti vengono in genere memorizzati nella directory **c:\ProgramData\Microsoft\BootEventCollector\Etl** (che potrebbero disporre di ulteriori sottodirectory). È possibile trovare il file di log più recente tramite l'ordinamento per l'ora dell'ultima modifica. È inoltre disponibile un log di stato (in genere in **c:\ProgramData\Microsoft\BootEventCollector\Logs**), che registra ogni volta che l'agente di raccolta attiva la scrittura di un nuovo file.  
  
È inoltre disponibile un log collector, che registra le informazioni sull'agente di raccolta stesso. È possibile mantenere questo file di registro in formato ETW (in cui gli eventi vengono segnalati al servizio Registro di Windows; l'impostazione predefinita) o in un file (in genere in **c:\ProgramData\Microsoft\BootEventCollector\Logs**). Utilizzo di un file può essere utile se si desidera abilitare la modalità dettagliata che generano una grande quantità di dati. È inoltre possibile impostare il log per scrivere un output standard eseguendo l'agente di raccolta dalla riga di comando.  
  
**Creazione del file di configurazione dell'agente di raccolta**  
  
Quando si Abilita il servizio, vengono creati tre file di configurazione XML e archiviati in **c:\ProgramData\Microsoft\BootEventCollector\Config**:  
  
-   **Active.XML** questo file contiene la configurazione attiva corrente del servizio agente di raccolta dati.  Subito dopo l'installazione, questo file è lo stesso contenuto di Empty.xml. Quando si imposta una nuova configurazione dell'agente di raccolta è salvare questo file.  
  
-   **Empty.XML** questo file contiene la configurazione minima necessaria gli elementi con valori predefiniti impostati. Non abilita qualsiasi raccolta. consente solo il servizio agente di raccolta dati in una modalità di inattività.  
  
-   **Example.XML** questo file fornisce esempi e le spiegazioni degli elementi di configurazione.  
  
**Scelta di un limite per le dimensioni del file**  
  
Una delle decisioni che è necessario apportare consiste nell'impostare un limite di dimensioni del file. Il limite delle dimensioni file migliore dipende dal volume previsto di eventi e spazio disponibile su disco. I file più piccoli sono più pratico dal punto di vista della pulizia dei dati precedenti. Tuttavia, ogni file comporta l'overhead di un'intestazione 64KB e la lettura di molti file per ottenere la cronologia combinata potrebbe non essere utile. Il limite di dimensioni minime assoluto del file è 256 KB. Un limite di dimensioni ragionevoli pratico file deve essere superiore a 1 MB e 10 MB è probabilmente un buon valore tipico. Un limite superiore potrebbe essere accettabile se si prevede che molti eventi.  
  
Esistono diversi aspetti da tenere in mente riguardo il file di configurazione:  
  
-   L'indirizzo di computer di destinazione. È possibile utilizzare l'indirizzo IPv4, un indirizzo MAC o un GUID SMBIOS. Tenere presente questi fattori quando si sceglie l'indirizzo da utilizzare:  
  
    -   L'indirizzo IPv4 funziona meglio con l'assegnazione degli indirizzi IP statico. Tuttavia, gli indirizzi IP anche statici devono essere disponibili tramite il protocollo DHCP.  
  
    -   Un indirizzo MAC o il GUID SMBIOS è utile quando sono noti in anticipo, ma gli indirizzi IP assegnati dinamicamente.  
  
    -   Gli indirizzi IPv6 non sono supportati dal protocollo di rete EVENTO.  
  
    -   È possibile specificare diversi modi per identificare il computer. Ad esempio, se l'hardware fisico sta per essere sostituito, è possibile immettere i nuovi indirizzi MAC sia la vecchia e verranno accettati.  
  
-   La chiave di crittografia utilizzata per la comunicazione con il computer dell'agente di raccolta  
  
-   Nome del computer di destinazione. È possibile utilizzare l'indirizzo IP, nome host o un altro nome come nome del computer.  
  
-   Il nome del file con estensione ETL da utilizzare e la configurazione delle dimensioni anello relativo  
  
##### <a name="to-create-the-configuration-file"></a>Per creare il file di configurazione  
  
1.  Aprire un prompt di Windows PowerShell con privilegi elevato e passare alla directory % SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.  
  
2.  Tipo `notepad .\newconfig.xml` e premere INVIO.  
  
3.  Nella finestra del blocco note, copiare questa configurazione di esempio:  
  
    ```  
    <collector configVersionMajor=1 statuslog=c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml>  
      <common>  
        <collectorport value=50000/>  
        <forwarder type=etl>  
          <set name=file value=c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl/>  
          <set name=size value=10mb/>  
          <set name=nfiles value=10/>  
          <set name=toxml value=none/>  
        </forwarder>  
        <target>  
          <ipv4 value=192.168.1.1/>  
          <key value=a.b.c.d/>  
          <computer value=computer1/>  
        </target>  
        <target>  
          <ipv4 value=192.168.1.2/>  
          <key value=d1.e2.f3.g4/>  
          <computer value=computer2/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > Il nodo radice è \<collector >. Gli attributi specificano la versione della sintassi del file di configurazione e il nome del file di log di stato.  
    >   
    > Il \<comuni > elemento raggruppa più destinazioni che specifica gli elementi di configurazione comuni, molto come un gruppo di utenti può essere utilizzato per specificare le autorizzazioni per gli utenti più comuni.  
    >   
    > Il \<collectorport > elemento definisce il numero di porta UDP in ascolto l'agente di raccolta dei dati in ingresso. Questa è la stessa porta, come specificato nel passaggio di configurazione di destinazione per Bcdedit. L'agente di raccolta supporta solo una porta e tutte le destinazioni devono connettersi alla stessa porta.  
    >   
    > Il \<server d'inoltro > elemento specifica come verranno inoltrati eventi ETW ricevuti dal computer di destinazione. Esiste un solo tipo di server d'inoltro, che li scrive i file ETL. I parametri specificano il modello di nome file, il limite delle dimensioni di ogni file dell'anello e la dimensione dell'anello per ogni computer. L'impostazione ToXml specifica che gli eventi ETW verranno scritti nel formato binario così come sono stati ricevuti, senza conversione in XML. Per informazioni su come decidere se conferire gli eventi a XML o meno, vedere la sezione conversione di eventi XML. Il modello di nome file contiene le sostituzioni: {computer} per {n. 3} e il nome del computer per l'indice del file nell'anello.  
    >   
    > In questo file di esempio, due computer di destinazione sono definiti con la \<destinazione > elemento. Ogni definizione specifica l'indirizzo IP con \<> IPv4, ma è anche possibile usare l'indirizzo MAC (ad esempio, `<mac value=11:22:33:44:55:66/>` o `<mac value=11-22-33-44-55-66/>`) o il GUID SMBIOS (ad esempio, `<guid value={269076F9-4B77-46E1-B03B-CA5003775B88}/>`) per identificare il computer di destinazione. Si noti inoltre la chiave di crittografia (lo stesso è stato specificato o generato con Bcdedit nel computer di destinazione) e il nome del computer.  
  
4.  Immettere i dettagli per ogni computer di destinazione come separato \<destinazione > elemento nel file di configurazione e quindi salvare Newconfig.xml e chiudere il blocco note.  
  
5.  Applicare la nuova configurazione con `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`. L'output deve restituire con il campo Success true. Se viene visualizzato un altro risultato, vedere la sezione Risoluzione dei problemi di questo argomento.  
  
È sempre possibile controllare la configurazione attiva corrente con `(Get-SbecActiveConfig).text`.  
  
È possibile eseguire un controllo di validità nel file di configurazione con `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result`.  
  
Se il comando di Windows PowerShell per applicare automaticamente una nuova configurazione aggiorna il servizio senza che sia necessario riavviarlo, è possibile sempre riavviare il servizio manualmente con uno di questi comandi:  
  
-   Con Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   In un prompt dei comandi comuni: **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>Configurazione di Nano Server come un computer di destinazione  
L'interfaccia minima offerto da Nano Server può talvolta rendere difficili da diagnosticare problemi di. È possibile configurare l'immagine di Nano Server per partecipare nel programma di installazione e la raccolta degli eventi di avvio automatico, l'invio di dati di diagnostica in un computer agente di raccolta senza ulteriori interventi da parte dell'utente. A tale scopo, effettuare le operazioni seguenti:  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Per configurare Nano Server come un computer di destinazione  
  
1.  Creare un'immagine Nano Server base. Vedere [Introduzione Nano Server](https://technet.microsoft.com/library/mt126167.aspx) per informazioni dettagliate.  
  
2.  Configurare un computer dell'agente di raccolta come nella sezione configurazione del computer dell'agente di raccolta di questo argomento.  
  
3.  Aggiungere le chiavi del Registro di sistema AutoLogger per consentire l'invio di messaggi diagnostico. A tale scopo, montare il disco rigido Virtuale Nano Server creata nel passaggio 1, caricare l'hive del Registro di sistema e quindi aggiungere alcune chiavi del Registro di sistema. In questo esempio, l'immagine di Nano Server è in C:\NanoServer; il percorso potrebbe essere diverso, quindi modificare di conseguenza i passaggi.  
  
    1.  Sul computer agente, copiare il **... \Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** cartella e incollarlo il **... \Windows\System32\WindowsPowerShell\v1.0\Modules** directory nel computer in uso per modificare il disco rigido Virtuale di Nano Server.  
  
    2.  Avviare una console di Windows PowerShell con autorizzazioni elevate ed eseguire `Import-Module BootEventCollector` .  
  
    3.  Aggiornare il Registro di sistema di Nano Server VHD per abilitare AutoLoggers. A tale scopo, eseguire `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`. Aggiunge un elenco di base dell'impostazione più comuni e gli eventi di avvio; è possibile ricercare altre [sessioni di traccia di eventi controllo](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).  
  
4.  Aggiornare le impostazioni di avvio nell'immagine di Nano Server per abilitare il flag di eventi e impostare il computer agente per verificare gli eventi di diagnostica vengono inviati al server appropriato. Si noti l'indirizzo IPv4 del computer dell'agente di raccolta, la porta TCP e la chiave di crittografia configurato nel file Active.XML dell'agente di raccolta (descritto altrove in questo argomento). Usare questo comando in una console di Windows PowerShell con autorizzazioni elevate: `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Aggiornare il computer dell'agente di raccolta per ricevere l'evento inviato dal computer Nano server aggiungendo l'intervallo di indirizzi IPv4, l'indirizzo IPv4 specifico o l'indirizzo MAC del nano server al file Active. XML sul computer dell'agente di raccolta (vedere la sezione configurazione del computer dell'agente di raccolta di questo argomento).  
  
## <a name="starting-the-event-collector-service"></a>Avvio del servizio agente di raccolta eventi  
Una volta che viene salvato un file di configurazione valido sul computer agente e un computer di destinazione è configurato, non appena il computer di destinazione viene riavviato, viene stabilita la connessione all'agente di raccolta e verranno raccolti gli eventi.  
  
Il log per il servizio agente di raccolta dati stesso (che è diverso dal programma di installazione e avvio dati raccolti dal servizio) può trovarsi in Microsoft-Windows-BootEvent-agente di raccolta/Admin. Per un'interfaccia grafica per gli eventi, utilizzare il Visualizzatore eventi. Creare una nuova vista; Espandere **registri applicazioni e servizi**, quindi espandere **Microsoft** e quindi **Windows**. Trovare **Collector BootEvent**, espanderla e trovare **Admin**.  

-   Con Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   In un prompt dei comandi comuni: **wevtutil qe Microsoft-Windows-BootEvent-agente di raccolta/Admin**  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi  
  
### <a name="troubleshooting-installation-of-the-feature"></a>Risoluzione dei problemi di installazione della funzionalità  
  
||Errore|Descrizione dell'errore|Sintomo|Potenziale problema|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|L'opzione nome della funzionalità non è riconosciuto in questo contesto||-Questa situazione può verificarsi se il nome di funzionalità. Verificare di aver la versione corretta e riprovare.<br />-Verificare che questa funzionalità è disponibile nella versione del sistema operativo in uso. In Windows PowerShell eseguire **DISM/online/Get-features &#124; ? { $ _-match boot}** . Se non viene restituita alcuna corrispondenza, probabile che esegue una versione che non supporta questa funzionalità.|  
|Dism.exe|0x800f080c|Funzionalità \<name > è sconosciuto.||Come sopra|  
  
### <a name="troubleshooting-the-collector"></a>Risoluzione dei problemi l'agente di raccolta  
  
**Registrazione**  
L'agente di raccolta registra i propri eventi come provider ETW Microsoft-Windows-BootEvent-agente di raccolta. È il primo punto che è possibile cercare la risoluzione dei problemi con l'agente di raccolta. È possibile ottenerli nel Visualizzatore eventi in registri applicazioni e servizi > Microsoft > Windows > BootEvent Collector > amministratore oppure è possibile leggerli in una finestra di comando con uno di questi comandi:  
  
In un prompt dei comandi comuni: **wevtutil qe Microsoft-Windows-BootEvent-agente di raccolta/Admin**  
  
Nel prompt di Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (è possibile aggiungere `-Oldest` per restituire l'elenco in ordine cronologico con gli eventi meno recenti prima)  
  
È possibile modificare il livello di dettaglio nei log da errore, tramite avviso, informazioni (impostazione predefinita), verbose e debug. I livelli più dettagliati delle informazioni sono utili per diagnosticare i problemi con i computer di destinazione che non si connettono, ma possono generare una grande quantità di dati, quindi utilizzarli con cautela.  
  
Impostare il log minimo livello di \<collector > elemento del file di configurazione. Ad esempio: < Collector configVersionMajor = 1 minlog\=verbose >.  
  
Il livello di dettaglio registra un record per ogni pacchetto ricevuto viene elaborato. Il livello di debug aggiunge ulteriori dettagli di elaborazione e il dump di tutti ETW pacchetti ricevuti anche.  
  
A livello di debug, potrebbe essere utile scrivere il log in un file anziché tentare di visualizzarla nel sistema di registrazione normale. A tale scopo, aggiungere un ulteriore elemento nel \<collector > elemento del file di configurazione:  
  
< Collector configVersionMajor = 1 minlog = log di debug\=c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt >  
      
 **Un approccio consigliato per la risoluzione dei problemi dell'agente di raccolta:**  
   
1. In primo luogo, verificare che l'agente di raccolta ha ricevuto la connessione dalla destinazione (creerà il file solo quando la destinazione viene avviato l'invio dei messaggi) con   
   ```  
   Get-SbecForwarding  
   ```  
   Se viene restituito è disponibile una connessione di destinazione che il problema potrebbe essere nelle impostazioni di autologger. Se restituisce nothing, il problema è iniziare con la connessione KDNET. Per diagnosticare problemi di connessione KDNET, controllare la connessione da entrambe le estremità (vale a dire dall'agente di raccolta dati e dalla destinazione).  
  
2. Per vedere diagnostica estesa dall'agente di raccolta dati, aggiungere questa opzione per il \<collector > elemento del file di configurazione:  
   raccolta \<... minlog = verbose >  
   In questo modo i messaggi relativi a ogni pacchetto ricevuto.  
3. Controllare se i pacchetti vengono ricevuti affatto. Facoltativamente, è possibile scrivere il log in modalità dettagliata direttamente in un file anziché tramite ETW. A tale scopo, aggiungere quanto segue per il \<collector > elemento del file di configurazione:  
   raccolta \<... minlog = verbose log = c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt >  
      
4. Controllare i registri eventi per tutti i messaggi sui pacchetti ricevuti. Controllare se i pacchetti vengono ricevuti affatto. Se i pacchetti ricevuti ma non corretto, controllare i messaggi di evento per i dettagli.  
5. Dal lato di destinazione, KDNET scrive alcune informazioni diagnostiche nel Registro di sistema. Cerca in   
   **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** per i messaggi.  
   KdInitStatus (DWORD) = 0 in caso di riuscita e verrà visualizzata un codice di errore in caso di errore  
   KdInitErrorString = spiegazione dell'errore (contiene inoltre i messaggi informativi se nessun errore)  
  
6. Eseguire Ipconfig.exe nella destinazione e controllo per il nome del dispositivo che segnala. Se KDNET è stato caricato correttamente, il nome del dispositivo deve essere simile a kdnic anziché al nome della scheda del fornitore originale.  
7. Controlla se DHCP è configurato per la destinazione. KDNET è assolutamente necessario DHCP.  
8. Verificare che l'agente di raccolta nella stessa rete di destinazione. In caso contrario, controllare se il routing è configurato correttamente, in particolare il gateway predefinito impostazione per DHCP.  
  
  
**Stato connessione**  
  
È possibile controllare l'elenco corrente di connessioni stabilite e informazioni su dove i dati inoltrati con `Get-SbecForwarding`.  
  
È inoltre possibile ottenere la cronologia delle modifiche dello stato delle connessioni con `Get-SbecHistory`.  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>Risoluzione dei problemi di impostazione di una nuova configurazione  
Se è applicata la configurazione con il comando Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`, quindi la variabile `$result` conterrà informazioni sulla distribuzione. È possibile eseguire query di questa variabile per ottenere informazioni diverse uscirne:  
  
Ottenere informazioni sugli errori con `$result.ErrorString`. Se vengono rilevati errori in questo caso, sarà la nuova configurazione non sia stata applicata e la configurazione precedente sarà invariata.  
  
Ottenere avvisi con `$result.WarningString`.  
  
Ottenere informazioni sui dettagli di configurazione con `$result.InfoString`.  
  
È possibile ottenere il risultato completo con `$result | fl *`.  
In alternativa, se non si desidera salvare il risultato in una variabile, è possibile utilizzare `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`.  
  
### <a name="troubleshooting-target-computers"></a>Risoluzione dei problemi relativi a computer di destinazione  
  
||Errore|Descrizione dell'errore|Sintomo|Potenziale problema|  
|-|---------|---------------------|-----------|---------------------|  
|Computer di destinazione||Destinazione non è connesso all'agente di raccolta||-Il computer di destinazione non ottenere riavviato dopo essere stato configurato. Riavviare il computer di destinazione.<br />-Il computer di destinazione dispone di impostazioni errate di BCD. Controllare le impostazioni nella sezione Convalida impostazioni computer di destinazione. Risolvere il problema e quindi riavviare il computer di destinazione.<br />-Il driver KDNET/EVENTO-NET non è in grado di connettersi a una scheda di rete o connesso alla scheda di rete errato. In Windows PowerShell, eseguire `gwmi Win32_NetworkAdapter` e controllare l'output di uno con la proprietà ServiceName **kdnic**. Se è selezionata la scheda di rete errata, ripetere i passaggi in per specificare una scheda di rete. Se non vengono visualizzate la scheda di rete, è possibile che il driver non supporta le schede di rete.<br>**Vedere anche** Un approccio consigliato per la risoluzione dei problemi dell'agente di raccolta, in particolare i passaggi da 5 a 8.|  
|Agente di raccolta dati||Dopo la migrazione della macchina Virtuale l'agente di raccolta dati è ospitato in non vengono visualizzati gli eventi.||Verificare che l'indirizzo IP del computer di raccolta non è stato modificato. In caso contrario, esaminare per consentire l'invio di eventi ETW tramite il trasporto in modalità remota.|  
|Agente di raccolta dati||Non vengono creati i file ETL.|`Get-SbecForwarding` indica che la destinazione è connessa, senza errori, ma non vengono creati i file ETL.|Il computer di destinazione è probabilmente non tutti i dati ancora inviato; File ETL vengono creati solo quando vengono ricevuti i dati.|  
|Agente di raccolta dati||Un evento non viene visualizzato nel file con estensione ETL.|Il computer di destinazione ha inviato un evento, ma quando viene letto il file ETL con Visualizzatore eventi di Message Analyzer, l'evento non è presente.|-L'evento può essere ancora nel buffer. Gli eventi non vengono scritti nel file ETL fino a quando non viene raccolto un buffer pieno di 64 KB o si è verificato un timeout di circa 10-15 secondi con nessun nuovo evento. Attendere il timeout di scadenza o scaricare il buffer con `Save-SbecInstance`.<br />-Il manifesto di eventi non è disponibile nel computer di raccolta o nel computer in cui viene eseguito il Visualizzatore eventi o Message Analyzer.  In questo caso, l'agente di raccolta potrebbe non essere in grado di elaborare l'evento (controllare il log di agente di raccolta) o il visualizzatore potrebbe non essere in grado di visualizzare.  È buona norma disporre di tutti i manifesti installati sul computer agente e installare gli aggiornamenti sul computer agente prima di eseguirne l'installazione nei computer di destinazione.|
