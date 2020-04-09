---
title: Funzionalità avanzate di risoluzione dei problemi di Server Message Block (SMB)
description: Introduce i metodi avanzati per la risoluzione dei problemi di SMB (Server Message Block).
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 654cb1b0eea65457d521d201739721ed8c3c0203
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815194"
---
# <a name="advanced-troubleshooting-server-message-block-smb"></a>Funzionalità avanzate di risoluzione dei problemi di Server Message Block (SMB)

SMB (Server Message Block) è un protocollo di trasporto di rete per le operazioni di file System per consentire a un client di accedere alle risorse in un server. Lo scopo principale del protocollo SMB è quello di abilitare l'accesso file system remoto tra due sistemi su TCP/IP.

La risoluzione dei problemi SMB può essere estremamente complessa. In questo articolo non si tratta di una guida completa per la risoluzione dei problemi, ma si tratta di una breve introduzione per comprendere le nozioni di base su come risolvere efficacemente i problemi relativi a SMB.

## <a name="tools-and-data-collection"></a>Strumenti e raccolta dati

Un aspetto fondamentale della risoluzione dei problemi di qualità SMB è la comunicazione della terminologia corretta. Questo articolo introduce quindi la terminologia SMB di base per garantire l'accuratezza della raccolta e dell'analisi dei dati.

> [!Note]
> Il *server SMB (SRV)* si riferisce al sistema che ospita la file System, nota anche come file server. *Il client SMB (CLI)* si riferisce al sistema che tenta di accedere al file System, indipendentemente dalla versione o dall'edizione del sistema operativo.

Se ad esempio si usa Windows Server 2016 per raggiungere una condivisione SMB ospitata in Windows 10, Windows Server 2016 è il client SMB e il server SMB di Windows 10.

### <a name="collect-data"></a>Raccolta di dati

Prima di risolvere i problemi relativi a SMB, è consigliabile raccogliere prima una traccia di rete sul lato client e server. Si applicano le linee guida seguenti:

- Nei sistemi Windows, è possibile usare NetShell (netsh), Network Monitor, Message Analyst o Wireshark per raccogliere una traccia di rete.

- I dispositivi di terze parti hanno in genere uno strumento di acquisizione pacchetti, ad esempio tcpdump (Linux/FreeBSD/Unix) o pktt (NetApp). Se, ad esempio, il client SMB o il server SMB è un host UNIX, è possibile raccogliere dati eseguendo il comando seguente:
  
  ```cmd
  # tcpdump -s0 -n -i any -w /tmp/$(hostname)-smbtrace.pcap
  ```
  
  Interrompi la raccolta dei dati usando **CTRL + C** dalla tastiera.

Per individuare l'origine del problema, è possibile controllare le tracce bilaterali, ovvero l'interfaccia della riga di comando, SRV o un punto compreso tra.

#### <a name="using-netshell-to-collect-data"></a>Utilizzo di Netshell per la raccolta di dati

In questa sezione vengono illustrati i passaggi per l'utilizzo di Netshell per la raccolta della traccia di rete.

> [!NOTE]  
> Una traccia netsh crea un file ETL. I file ETL possono essere aperti solo in Message Analyzer (MA) e Network Monitor 3,4 (impostare il parser su Network Monitor parser \> Windows).

1. Nel server SMB e nel client SMB creare una cartella **Temp** nell'unità **C**. Eseguire quindi il comando seguente:

   ```cmd
   netsh trace start capture=yes report=yes scenario=NetConnection level=5 maxsize=1024 tracefile=c:\\Temp\\%computername%\_nettrace.etl**
   ```
   
   Se si usa PowerShell, eseguire i cmdlet seguenti:
   
   ```PowerShell
   New-NetEventSession -Name trace -LocalFilePath "C:\Temp\$env:computername`_netCap.etl" -MaxFileSize 1024

   Add-NetEventPacketCaptureProvider -SessionName trace -TruncationLength 1500

   Start-NetEventSession trace
   ```
   
2. Riprodurre il problema.

3. Arrestare la traccia eseguendo il comando seguente:

   ```cmd
   netsh trace stop
   ```
   
   Se si usa PowerShell, eseguire i cmdlet seguenti:

   ```PowerShell
   Stop-NetEventSession trace  
   Remove-NetEventSession trace
   ```

> [!NOTE] 
> È consigliabile tracciare solo una quantità minima di dati trasferiti. Per i problemi di prestazioni, è consigliabile eseguire sempre una traccia valida e non valida, se la situazione lo consente.

### <a name="analyze-the-traffic"></a>Analizzare il traffico

SMB è un protocollo a livello di applicazione che utilizza TCP/IP come protocollo di trasporto di rete. Un problema SMB può pertanto essere causato anche da problemi TCP/IP.

Controllare se TCP/IP riscontra uno di questi problemi:

1. L'handshake TCP a tre vie non viene completato. Ciò indica in genere che è presente un blocco firewall o che il servizio Server non è in esecuzione.

2. Sono in corso ritrasmissioni. Questi possono causare trasferimenti di file lenti a causa della limitazione della congestione del protocollo TCP composta.

3. Cinque ritrasmissioni seguite da un ripristino TCP potrebbero indicare che la connessione tra i sistemi è stata persa o che uno dei servizi SMB si è arrestato in modo anomalo o interrotto.

4. La finestra di ricezione TCP sta diminuendo. Questa situazione può essere causata da un archivio lento o da un altro problema che impedisce che i dati vengano recuperati dal buffer Winsock del driver della funzione ausiliario (AFD).

Se non è presente alcun problema TCP/IP riscontrabile, cercare errori SMB. A tale scopo, effettuare le operazioni seguenti:

1. Controllare sempre gli errori SMB rispetto alla specifica del protocollo MS-SMB2. Molti errori SMB sono benigni (non dannosi). Vedere le informazioni seguenti per determinare il motivo per cui SMB ha restituito l'errore prima di concludere che l'errore è correlato a uno dei problemi seguenti:

   - L'argomento della [sintassi del messaggio MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6eaf6e75-9c23-4eda-be99-c9223c60b181) illustra in dettaglio ogni comando SMB e le relative opzioni.
    
   - L'argomento relativo all' [elaborazione del client MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/df0625a5-6516-4fbe-bf97-01bef451cab2) illustra in dettaglio come il client SMB crea richieste e risponde ai messaggi del server.

   - L'argomento relativo all' [elaborazione del server MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e1d08834-42e0-41ca-a833-fc26f5132a6f) illustra in dettaglio il modo in cui il server SMB crea le richieste e risponde alle richieste dei client.

2. Controllare se un comando di reimpostazione TCP viene inviato immediatamente dopo un FSCTL\_VALIDATE\_NEGOTIATe\_INFO (Validate Negotiate) Command. In tal caso, fare riferimento alle informazioni seguenti:

   - La sessione SMB deve essere terminata (reimpostazione TCP) quando il processo Validate Negotiate non riesce sul client o sul server.

   - Questo processo potrebbe non riuscire perché un utilità di ottimizzazione WAN sta modificando il pacchetto SMB Negotiate.

   - Se la connessione è terminata in modo anomalo, identificare l'ultima comunicazione di Exchange tra il client e il server.

#### <a name="analyze-the-protocol"></a>Analizzare il protocollo

Esaminare i dettagli del protocollo SMB effettivi nella traccia di rete per comprendere i comandi e le opzioni esatti usati.

> [!NOTE]
> Solo l'analizzatore di messaggi può analizzare i comandi SMBv3 e versioni successive.

- Tenere presente che SMB esegue solo le operazioni da eseguire.

- Per informazioni sulle operazioni che l'applicazione sta tentando di eseguire, è possibile esaminare i comandi SMB.

Confrontare i comandi e le operazioni con la specifica del protocollo per assicurarsi che tutto funzioni correttamente. In caso contrario, raccogliere i dati più vicini o a un livello inferiore per cercare ulteriori informazioni sulla causa principale. A tale scopo, effettuare le operazioni seguenti:

1. Raccolta di un'acquisizione di pacchetti standard.

2. Eseguire il comando **netsh** per tracciare e raccogliere informazioni dettagliate sull'eventuale presenza di problemi nello stack di rete o sulle eliminazioni nelle applicazioni Windows Filtering Platform (WFP), ad esempio firewall o programma antivirus.

3. Se tutte le altre opzioni hanno esito negativo, raccogliere un t. cmd se si ritiene che il problema si verifichi all'interno di SMB, oppure se nessuno degli altri dati è sufficiente per identificare la causa principale.

Ad esempio,

- Si verificano trasferimenti di file lenti a un singolo file server.

- Le tracce bilaterali indicano che il SRV risponde lentamente a una richiesta di lettura.

- La rimozione di un programma antivirus risolve i trasferimenti di file lenti.

- Per risolvere il problema, contattare la manufactory del programma antivirus.

> [!NOTE]
> Facoltativamente, è **anche** possibile disinstallare temporaneamente il programma antivirus durante la risoluzione dei problemi.

#### <a name="event-logs"></a>Registri eventi

Il client SMB e il server SMB hanno una struttura dettagliata del registro eventi, come illustrato nello screenshot seguente. Raccogliere i registri eventi per individuare la causa radice del problema.

![Registri eventi](media/troubleshooting-smb-1.png)

## <a name="smb-related-system-files"></a>File di sistema correlati a SMB

Questa sezione elenca i file di sistema correlati a SMB. Per aggiornare i file di sistema, verificare che sia installato l' [aggiornamento cumulativo](https://support.microsoft.com/help/4498140/windows-10-update-history) più recente.

Binari client SMB elencati in **% windir%\\system32\\driver**:

- RDBSS. sys

- MRXSMB. sys

- MRXSMB10. sys

- MRXSMB20. sys

- MUP. sys

- SMBdirect. sys

Binari del server SMB elencati sotto **% windir%\\system32\\drivers**:

- SRVNET. sys

- SRV. sys

- SRV2. sys

- SMBdirect. sys

- In **% windir%\\system32**

- srvsvc. dll

![Componenti SMB](media/troubleshooting-smb-2.png)

### <a name="update-suggestions"></a>Suggerimenti per l'aggiornamento

Prima di risolvere i problemi relativi a SMB, è consigliabile aggiornare i componenti seguenti:

- Per un file server è necessaria l'archiviazione file. Se la risorsa di archiviazione dispone di un componente iSCSI, aggiornare i componenti.

- Aggiornare i componenti di rete.

- Per migliorare le prestazioni e la stabilità, aggiornare Windows Core.

## <a name="reference"></a>Riferimento

[Scenario di scambio di pacchetti del protocollo SMB Microsoft](https://docs.microsoft.com/windows/win32/fileio/microsoft-smb-protocol-packet-exchange-scenario)