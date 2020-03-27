---
title: Ottimizzazione delle prestazioni delle schede di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
audience: Admin
ms.custom:
- CI ID 111485
- CSSTroubleshoot
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: dcscontentpm
ms.author: lizross
author: Teresa-Motiv
ms.date: 12/23/2019
ms.openlocfilehash: f802804d64b3047a2612b7f346de03aff61c30cd
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316548"
---
# <a name="performance-tuning-network-adapters"></a>Ottimizzazione delle prestazioni delle schede di rete

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

Utilizzare le informazioni in questo argomento per ottimizzare le schede di rete delle prestazioni per i computer che eseguono Windows Server 2016 e versioni successive. Se le schede di rete forniscono opzioni di ottimizzazione, è possibile usare queste opzioni per ottimizzare la velocità effettiva della rete e l'utilizzo delle risorse.

Le impostazioni di ottimizzazione corrette per le schede di rete dipendono dalle variabili seguenti:

- La scheda di rete e il relativo set di funzionalità  
- Tipo di carico di lavoro eseguito dal server  
- Le risorse hardware e software del server  
- Gli obiettivi in termini di prestazioni del server  

Nelle sezioni seguenti vengono illustrate alcune delle opzioni di ottimizzazione disponibili.  

##  <a name="enabling-offload-features"></a><a name="bkmk_offload"></a>Abilitazione delle funzionalità di offload

Di norma è utile attivare l'offload della scheda di rete. Tuttavia, la scheda di rete potrebbe non essere sufficientemente potente da gestire le funzionalità di offload con una velocità effettiva elevata.

> [!IMPORTANT]
> Non utilizzare le funzionalità di offload **offload attività IPSec** o **TCP Chimney Offload**. Queste tecnologie sono deprecate in Windows Server 2016 e potrebbero influire negativamente sulle prestazioni del server e della rete. Inoltre, queste tecnologie potrebbero non essere supportate da Microsoft in futuro.

Si consideri, ad esempio, una scheda di rete con risorse hardware limitate.
In tal caso, l'abilitazione delle funzionalità di offload della segmentazione potrebbe ridurre la velocità effettiva massima sostenibile dell'adapter. Tuttavia, se la velocità effettiva ridotta è accettabile, è consigliabile abilitare le funzionalità di offload di segmentazione.

> [!NOTE]  
> Per alcune schede di rete è necessario abilitare le funzionalità di offload indipendentemente per i percorsi di trasmissione e ricezione.

##  <a name="enabling-receive-side-scaling-rss-for-web-servers"></a><a name="bkmk_rss_web"></a>Abilitazione di Receive-Side Scaling (RSS) per i server Web

Con RSS è possibile migliorare scalabilità e prestazioni Web se il server dispone di un numero di schede di rete inferiore al numero di processori logici. Quando tutto il traffico Web passa attraverso le schede di rete che supportano RSS, il server è in grado di elaborare le richieste Web in ingresso da diverse connessioni simultaneamente tra diverse CPU.

> [!IMPORTANT]  
> Evitare di utilizzare schede di rete non RSS e schede di rete che supportano RSS nello stesso server. A causa della logica di distribuzione del carico in RSS e Hypertext Transfer Protocol (HTTP), le prestazioni potrebbero essere gravemente degradate se una scheda di rete che supporta non RSS accetta il traffico Web in un server che dispone di una o più schede di rete che supportano RSS. In questo caso, sarà necessario usare schede di rete che supportano RSS oppure disabilitare RSS nella scheda **Proprietà avanzate** delle proprietà della scheda di rete.
>  
> Per stabilire se una scheda di rete supporta RSS, è possibile visualizzare le informazioni su RSS nella scheda **Proprietà avanzate** delle proprietà della scheda di rete.

### <a name="rss-profiles-and-rss-queues"></a>Profili RSS e code RSS

Il profilo predefinito RSS predefinito è **NUMAStatic**, che è diverso da quello predefinito usato dalle versioni precedenti di Windows. Prima di iniziare a usare i profili RSS, esaminare i profili disponibili per capire quando sono utili e come si applicano all'ambiente di rete e all'hardware.

Ad esempio, se si apre Gestione attività ed esaminano i processori logici nel server e sembrano sottoutilizzati per il traffico di ricezione, è possibile provare ad aumentare il numero di code RSS dal valore predefinito di due al massimo supportato dalla scheda di rete. Nella scheda di rete, le opzioni per modificare il numero di code RSS potrebbero essere parte del driver.

##  <a name="increasing-network-adapter-resources"></a><a name="bkmk_resources"></a>Aumento delle risorse della scheda di rete

Per le schede di rete che consentono di configurare manualmente le risorse, ad esempio i buffer di ricezione e di invio, è necessario aumentare le risorse allocate.  

Alcune schede di rete impostano i buffer di ricezione su un valore basso per risparmiare memoria allocata dall'host. In presenza di un valore basso, alcuni pacchetti vengono scartati e le prestazioni calano. Di conseguenza, in caso di carichi di lavoro con attività di ricezione intensiva, è consigliabile portare al massimo il valore del buffer di ricezione.

> [!NOTE]  
> Se una scheda di rete non espone la configurazione manuale delle risorse, configura le risorse in modo dinamico oppure le risorse vengono impostate su un valore fisso che non può essere modificato.

### <a name="enabling-interrupt-moderation"></a>Abilitazione della moderazione interrupt

Per controllare la moderazione degli interrupt, alcune schede di rete espongono livelli di moderazione di interrupt diversi, parametri di Unione del buffer diversi (a volte separatamente per i buffer di invio e di ricezione) o entrambi.

È consigliabile prendere in considerazione la moderazione delle interruzioni per i carichi di lavoro associati alla CPU. Quando si usa la moderazione degli interrupt, considerare il compromesso tra il risparmio della CPU dell'host e la latenza rispetto all'aumento del risparmio della CPU dell'host, a causa di più interrupt e meno latenza. Se la scheda di rete non esegue la moderazione degli interrupt, ma espone l'Unione dei buffer, è possibile migliorare le prestazioni aumentando il numero di buffer Uniti per consentire più buffer per ogni trasmissione o ricezione.

##  <a name="performance-tuning-for-low-latency-packet-processing"></a><a name="bkmk_low"></a>Ottimizzazione delle prestazioni per l'elaborazione di pacchetti a bassa latenza

Molte schede di rete offrono opzioni di ottimizzazione della latenza indotta dal sistema operativo. La latenza è il tempo trascorso tra l'elaborazione di un pacchetto in arrivo da parte dell'unità di rete e la restituzione del pacchetto da parte dell'unità di rete. Questo intervallo di tempo è normalmente misurato in microsecondi. In confronto, il tempo di trasmissione per trasmissione di pacchetti a grande distanza viene di solito misurato in millisecondi (un ordine di grandezza maggiore). Questa ottimizzazione non accelererà il trasferimento dei pacchetti.

Seguono alcuni suggerimenti utili per l'ottimizzazione delle prestazioni di schede che rilevano i microsecondi.

- Impostare il BIOS del computer su **Prestazioni elevate**, con i C-state disattivati. Si noti tuttavia che questa operazione dipende dal sistema e dal BIOS e che alcuni sistemi forniranno prestazioni più elevate se il sistema operativo controlla il risparmio energia. È possibile controllare e modificare le impostazioni di risparmio energia dalle **Impostazioni** o usando il comando **powercfg** . Per altre informazioni, vedere [Opzioni della riga di comando di Powercfg](https://docs.microsoft.com/windows-hardware/design/device-experiences/powercfg-command-line-options).

- Impostare il profilo del risparmio energia del sistema operativo su **Sistema a prestazioni elevate**.  
   > [!NOTE]  
   > Questa impostazione non funziona correttamente se il BIOS di sistema è stato impostato per disabilitare il controllo del sistema operativo del risparmio energia.

- Abilita offload statici. Ad esempio, abilitare le impostazioni checksum UDP, checksum TCP e Send large Offload (LSO).

- Se il traffico è a più flussi, ad esempio quando si riceve un traffico multicast con volume elevato, abilitare RSS.

- Disabilitare l'impostazione **Regolazione di interrupt** per driver di schede di rete che richiedono la minor latenza possibile. Tenere presente che questa configurazione può utilizzare più tempo della CPU e rappresenta un compromesso.

- Gestire interrupt di schede di rete e chiamate di procedura differita su un processore core che condivide cache CPU con il core usato dal programma (thread utente) che gestisce il pacchetto. A tale scopo, è possibile usare l'ottimizzazione dell'affinità CPU per indirizzare un processo verso determinati processori logici insieme alla configurazione di RSS. Usando lo stesso core per l'interrupt, chiamata di procedura differita e thread modalità utente thread le prestazioni peggiorano con l'aumentare del carico, perché IRS, chiamata di procedura differita e thread competono per l'uso del core.

##  <a name="system-management-interrupts"></a><a name="bkmk_smi"></a>Interruzioni di gestione del sistema

Molti sistemi hardware utilizzano System Management interrupt (SMI) per un'ampia gamma di funzioni di manutenzione, ad esempio la segnalazione di errori di memoria del codice di correzione degli errori, la gestione della compatibilità USB legacy, il controllo della ventola e la gestione di energia elettrica controllata dal BIOS Impostazioni.

SMI è l'interrupt con priorità più elevata nel sistema e inserisce la CPU in modalità di gestione. Questa modalità ha la precedenza su tutte le altre attività, mentre SMI esegue una routine del servizio di interrupt, in genere contenuta nel BIOS.

Sfortunatamente, questo comportamento può causare picchi di latenza di 100 microsecondi o più.

Se è necessario ridurre al minimo la latenza, è opportuno richiedere al provider hardware una versione BIOS che riduca gli SMI il più possibile. Queste versioni BIOS sono spesso denominate "BIOS a bassa latenza" o "SMI Free BIOS". In alcuni casi, non è possibile per una piattaforma hardware eliminare del tutto l'attività SMI, che viene usata per controllare funzioni essenziali (come le ventole di raffreddamento).

> [!NOTE]  
> Il sistema operativo non è in grado di controllare SMIs perché il processore logico viene eseguito in una modalità di manutenzione speciale che impedisce l'intervento del sistema operativo.

##  <a name="performance-tuning-tcp"></a><a name="bkmk_tcp"></a>Ottimizzazione delle prestazioni TCP

 Per ottimizzare le prestazioni TCP, è possibile utilizzare gli elementi seguenti.

###  <a name="tcp-receive-window-autotuning"></a><a name="bkmk_tcp_params"></a>Ottimizzazione automatica della finestra di ricezione TCP

In Windows Vista, Windows Server 2008 e versioni successive di Windows, lo stack di rete Windows utilizza una funzionalità denominata livello di *ottimizzazione automatica della finestra di ricezione TCP* per negoziare le dimensioni della finestra di ricezione TCP. Questa funzionalità può negoziare una dimensione della finestra di ricezione definita per ogni comunicazione TCP durante l'handshake TCP.

Nelle versioni precedenti di Windows, lo stack di rete Windows usava una finestra di ricezione a dimensione fissa (65.535 byte) che limitava la velocità effettiva complessiva potenziale per le connessioni. La velocità effettiva totale ottenibile delle connessioni TCP potrebbe limitare gli scenari di utilizzo della rete. L'ottimizzazione automatica della finestra di ricezione TCP consente a questi scenari di usare completamente la rete.

Per una finestra di ricezione TCP con una dimensione specifica, è possibile usare l'equazione seguente per calcolare la velocità effettiva totale di una singola connessione.

> *Velocità effettiva totale ottenibile in byte* = *dimensioni della finestra di ricezione TCP in byte* \* (1/ *latenza di connessione in secondi*)

Per una connessione con una latenza di 10 ms, ad esempio, la velocità effettiva totale ottenibile è solo di 51 Mbps. Questo valore è ragionevole per un'infrastruttura di rete aziendale di grandi dimensioni. Tuttavia, utilizzando l'ottimizzazione automatica per modificare la finestra di ricezione, la connessione può ottenere la velocità di riga completa di una connessione da 1 Gbps.  

Alcune applicazioni definiscono le dimensioni della finestra di ricezione TCP. Se l'applicazione non definisce le dimensioni della finestra di ricezione, la velocità del collegamento determina le dimensioni come indicato di seguito:

- Inferiore a 1 megabit al secondo (Mbps): 8 kilobyte (KB)
- da 1 Mbps a 100 Mbps: 17 KB
- 100 Mbps a 10 Gigabit al secondo (Gbps): 64 KB
- 10 Gbps o superiore: 128 KB

Ad esempio, in un computer in cui è installata una scheda di rete da 1 Gbps, le dimensioni della finestra devono essere pari a 64 KB.

Questa funzionalità consente inoltre di utilizzare tutte le altre funzionalità per migliorare le prestazioni di rete. Queste funzionalità includono le altre opzioni TCP definite nella [specifica RFC 1323](https://tools.ietf.org/html/rfc1323). Utilizzando queste funzionalità, i computer basati su Windows possono negoziare le dimensioni della finestra di ricezione TCP più piccole, ma vengono ridimensionate in base a un valore definito, a seconda della configurazione. Questo comportamento semplifica la gestione delle dimensioni per i dispositivi di rete.

> [!NOTE]  
> Potrebbe verificarsi un problema per cui il dispositivo di rete non è conforme all' **opzione di scalabilità della finestra TCP**, come definito in [RFC 1323](https://tools.ietf.org/html/rfc1323) e, pertanto, non supporta il fattore di scala. In questi casi, fare riferimento a questo [KB 934430, la connettività di rete non riesce quando si tenta di utilizzare Windows Vista dietro un dispositivo firewall](https://support.microsoft.com/help/934430/network-connectivity-fails-when-you-try-to-use-windows-vista-behind-a) o contattare il team di supporto per il fornitore del dispositivo di rete.  

#### <a name="review-and-configure-tcp-receive-window-autotuning-level"></a>Esaminare e configurare il livello di ottimizzazione automatica della finestra di ricezione TCP

È possibile utilizzare i comandi netsh o i cmdlet di Windows PowerShell per rivedere o modificare il livello di ottimizzazione automatica della finestra di ricezione TCP.

> [!NOTE]  
> Diversamente dalle versioni di Windows precedenti a Windows 10 o Windows Server 2019, non è più possibile usare il registro di sistema per configurare le dimensioni della finestra di ricezione TCP. Per ulteriori informazioni sulle impostazioni deprecate, vedere [parametri TCP deprecati](#deprecated-tcp-parameters).

> [!NOTE]  
> Per informazioni dettagliate sui livelli di ottimizzazione automatica disponibili, vedere [livelli di ottimizzazione automatica](#autotuning-levels).

**Per utilizzare Netsh per rivedere o modificare il livello di ottimizzazione automatica**

Per rivedere le impostazioni correnti, aprire una finestra del prompt dei comandi ed eseguire il comando seguente:

```cmd
netsh interface tcp show global
```

L'output di questo comando dovrebbe essere simile al seguente:

```
Querying active state...

TCP Global Parameters  
-----
Receive-Side Scaling State : enabled
Chimney Offload State : disabled
Receive Window Auto-Tuning Level : normal
Add-On Congestion Control Provider : default
ECN Capability : disabled
RFC 1323 Timestamps : disabled
Initial RTO : 3000
Receive Segment Coalescing State : enabled
Non Sack Rtt Resiliency : disabled
Max SYN Retransmissions : 2
Fast Open : enabled
Fast Open Fallback : enabled
Pacing Profile : off
```

Per modificare l'impostazione, eseguire il comando seguente al prompt dei comandi:

```cmd
netsh interface tcp set global autotuninglevel=<Value>
```

> [!NOTE]  
> Nel comando precedente \<*valore*> rappresenta il nuovo valore per il livello di ottimizzazione automatica.

Per ulteriori informazioni su questo comando, vedere [comandi Netsh per l'interfaccia Transmission Control Protocol](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731258(v=ws.10)).

**Per usare PowerShell per verificare o modificare il livello di ottimizzazione automatica**

Per rivedere le impostazioni correnti, aprire una finestra di PowerShell ed eseguire il cmdlet seguente.

```PowerShell
Get-NetTCPSetting | Select SettingName,AutoTuningLevelLocal
```

L'output di questo cmdlet dovrebbe essere simile al seguente.

```
SettingName           AutoTuningLevelLocal
-----------          --------------------
Automatic
InternetCustom       Normal
DatacenterCustom     Normal
Compat               Normal
Datacenter           Normal
Internet             Normal
```

Per modificare l'impostazione, eseguire il cmdlet seguente al prompt dei comandi di PowerShell.

```PowerShell
Set-NetTCPSetting -AutoTuningLevelLocal <Value>
```

> [!NOTE]  
> Nel comando precedente \<*valore*> rappresenta il nuovo valore per il livello di ottimizzazione automatica.

Per ulteriori informazioni su questi cmdlet, vedere gli articoli seguenti:

- [Get-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/get-nettcpsetting?view=win10-ps)
- [Set-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

#### <a name="autotuning-levels"></a>Livelli di ottimizzazione automatica

È possibile impostare l'ottimizzazione automatica della finestra di ricezione su uno dei cinque livelli. Il livello predefinito è **Normal**. Nella tabella seguente vengono descritti i livelli.

|Level |Valore esadecimale |Comments |
| --- | --- | --- |
|Normale (predefinita) |0x8 (fattore di scala 8) |Impostare la finestra di ricezione TCP in modo che cresca per adattarsi a quasi tutti gli scenari. |
|Disabled |Nessun fattore di scala disponibile |Impostare la finestra di ricezione TCP sul valore predefinito. |
|Restricted (Restrizioni) |0x4 (fattore di scala 4) |Impostare la finestra di ricezione TCP in modo che cresca oltre il valore predefinito, ma limitare tale crescita in alcuni scenari. |
|Con restrizioni elevata |0x2 (fattore di scala 2) |Impostare la finestra di ricezione TCP in modo che cresca oltre il valore predefinito, ma eseguire questa operazione in modo molto prudente. |
|sperimentale |0xE (fattore di scala 14) |Impostare la finestra di ricezione TCP in modo che cresca per adattarsi a scenari estremi. |

Se si usa un'applicazione per acquisire i pacchetti di rete, l'applicazione deve segnalare i dati simili ai seguenti per le diverse impostazioni del livello di ottimizzazione automatica della finestra.

- Livello di ottimizzazione automatica: **normale (stato predefinito)**

   ```
   Frame: Number = 492, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2667, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60975, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=4075590425, Ack=0, Win=64240 ( Negotiating scale factor 0x8 ) = 64240
   SrcPort: 60975
   DstPort: Microsoft-DS(445)
   SequenceNumber: 4075590425 (0xF2EC9319)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x8 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x8 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 8 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Livello di ottimizzazione automatica: **disabilitato**

   ```
   Frame: Number = 353, Captured Frame Length = 62, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2576, Total IP Length = 48
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60956, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2315885330, Ack=0, Win=64240 ( ) = 64240
   SrcPort: 60956
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2315885330 (0x8A099B12)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 112 (0x70)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( ) = 64240 ----------------------------------------> TCP Receive Window set as 64K as per NIC Link bitrate. Note there is no Scale Factor defined. In this case, Scale factor is not being sent as a TCP Option, so it will not be used by Windows.
   Checksum: 0x817E, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Livello di ottimizzazione automatica: con **restrizioni**

   ```
   Frame: Number = 3, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2319, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60890, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1966088568, Ack=0, Win=64240 ( Negotiating scale factor 0x4 ) = 64240
   SrcPort: 60890
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1966088568 (0x75302178)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x4 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x4 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 4 -------------------------------> Scale factor, defined by AutoTuningLevel.
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Livello di ottimizzazione automatica: con **restrizioni elevata**

   ```
   Frame: Number = 115, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2388, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60903, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1463725706, Ack=0, Win=64240 ( Negotiating scale factor 0x2 ) = 64240
   SrcPort: 60903
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1463725706 (0x573EAE8A)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x2 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x2 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 2 ------------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Livello di ottimizzazione automatica: **sperimentale**

   ```
   Frame: Number = 238, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2490, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60933, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2095111365, Ack=0, Win=64240 ( Negotiating scale factor 0xe ) = 64240
   SrcPort: 60933
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2095111365 (0x7CE0DCC5)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0xe ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0xe Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 14 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

#### <a name="deprecated-tcp-parameters"></a>Parametri TCP deprecati

Le seguenti impostazioni del registro di sistema di Windows Server 2003 non sono più supportate e vengono ignorate nelle versioni successive.

- **TcpWindowSize**
- **NumTcbTablePartitions**  
- **MaxHashTableSize**  

Tutte queste impostazioni si trovano nella seguente sottochiave del registro di sistema:

> **HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services\Tcpip\Parameters**  

###  <a name="windows-filtering-platform"></a><a name="bkmk_wfp"></a>Piattaforma filtro Windows

Windows Vista e Windows Server 2008 hanno introdotto Windows Filtering Platform (PAM). PAM fornisce API a fornitori di software indipendenti (ISV) non Microsoft per creare filtri di elaborazione pacchetti. Ne sono esempi i software per firewall e antivirus.

> [!NOTE]  
> Un filtro WFP scritto male può ridurre significativamente le prestazioni di rete di un server. Per ulteriori informazioni, vedere la pagina relativa [al porting di driver e app per l'elaborazione di pacchetti in WFP](https://docs.microsoft.com/windows-hardware/drivers/network/porting-packet-processing-drivers-and-apps-to-wfp) in Windows Dev Center.

Per i collegamenti a tutti gli argomenti di questa guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).
