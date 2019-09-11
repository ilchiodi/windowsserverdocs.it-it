---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: Risoluzione dei problemi degli aggiornamenti del firmware delle unità
ms.prod: windows-server-threshold
ms.author: toklima
ms.manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: 8283b87e9505b1d3f47ddc823016fbcc7c0c29e6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867051"
---
# <a name="troubleshooting-drive-firmware-updates"></a>Risoluzione dei problemi degli aggiornamenti del firmware delle unità

>Si applica a Windows 10, Windows Server (canale semestrale),

Windows 10, versione 1703 e successive e Windows Server (Canale semestrale) includono la possibilità di aggiornare il firmware delle unità disco rigido e delle unità SSD che sono state certificate con Firmware Upgradeable AQ (Additional Qualifier) tramite PowerShell.

Puoi trovare altre informazioni su questa funzionalità qui:

- [Aggiornamento del firmware delle unità in Windows Server 2016](update-firmware.md)
- [Aggiornare il firmware dell'unità senza tempi di inattività in Spazi di archiviazione diretta](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

Gli aggiornamenti del firmware potrebbero non riuscire per vari motivi. Lo scopo di questo articolo è la risoluzione avanzata dei problemi.

  > [!Note]
  > Le informazioni contenute in questo articolo, a seconda del problema, potrebbero non essere sufficienti per risolvere completamente tutti i casi di errore possibili.

## <a name="common-issues"></a>Problemi comuni
Dal punto di vista dell'architettura, questa nuova funzionalità si basa sulle API implementate nello stack di archiviazione di Windows, che chiama PowerShell. Lo stack di archiviazione si basa su driver e hardware per implementare correttamente i comandi definiti dal settore. Pertanto, i punti in cui possono verificarsi degli errori sono diversi. I problemi osservati più comunemente sono:

1.  Una determinata unità non implementa correttamente i comandi standard del settore (non dispone dell'AQ)
2.  Le API necessarie per eseguire l'aggiornamento non sono implementate o sono difettose (se vengono utilizzati driver di terze parti)
3.  Le API funzionano, ma esiste un problema con il firmware stesso (immagine non valida o danneggiata, ...)

Le sezioni seguenti delineano le informazioni sulla risoluzione dei problemi, a seconda se vengono utilizzati driver Microsoft o di terze parti.

## <a name="identifying-inappropriate-hardware"></a>Identificazione dell'hardware non appropriato
Il modo più rapido per identificare se un dispositivo supporta il set di comandi corretto consiste semplicemente nell'avviare PowerShell e passare l'oggetto PhysicalDisk di rappresentazione di un disco nel cmdlet Get-StorageFirmwareInfo. Di seguito è fornito un esempio:

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
Ed ecco l'output dell'esempio:

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

Il campo SupportsUpdate, almeno per i dispositivi SATA e NVMe, indicherà se la funzionalità integrata di PowerShell può essere utilizzata per aggiornare il firmware.

Il campo SupportsUpdate segnalerà sempre "True" per i dispositivi collegati a SAS, in quanto con i comandi standard del settore non è possibile eseguire una query per il supporto del comando appropriato.

Per verificare se un dispositivo SAS supporta il set di comandi necessario esistono due opzioni:
1.  Provare tramite il cmdlet Update-StorageFirmware con un'immagine del firmware appropriato o
2.  Consultare il catalogo di Windows Server per identificare i dispositivi SAS che hanno ottenuto correttamente l'aggiornamento del dispositivo FW (https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>Opzioni di correzione
Se un determinato dispositivo sottoposto a test non supporta il set di comandi appropriato, esegui una query al fornitore per vedere se è disponibile un firmware aggiornato che fornisce il set di comandi necessario o consulta Windows Server Catalogue per identificare i dispositivi per l'approvvigionamento che implementano il set di comandi appropriato.

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>Risoluzione dei problemi con i driver di terze parti (SAS)
I componenti software che interagiscono in modo più ravvicinato con l'hardware sono i driver miniport nello stack di archiviazione di Windows. Per alcuni protocolli di archiviazione, ad esempio SATA e NVMe, Microsoft fornisce driver di Windows nativi. Questi driver consentono informazioni di debug aggiuntive. I fornitori di hardware e software di terze parti, tuttavia, sono liberi di scrivere driver miniport personalizzati per i propri dispositivi e il loro livello di supporto per le informazioni di debug può variare.

Per identificare cosa è accaduto al download del firmware e attivare le API inviate allo stack di archiviazione a prescindere dal driver miniport, consultare il seguente canale del registro eventi:

Visualizzatore eventi - Registri applicazioni e servizi - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-ClassPnP/Operational**

Questo canale registra le informazioni sulle API Windows inviate ai driver miniport con le relative risposte. Ad esempio, la condizione di errore visualizzata direttamente sotto viene visualizzata quando si tenta di scaricare un'immagine del firmware in un dispositivo SATA, che è connesso tramite un HBA SAS che non implementa correttamente la conversione necessaria da SAS a SATA:

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
Ecco un esempio di output:

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
PowerShell genererà un errore e ha ricevuto le informazioni che la funzione chiamata (vale a dire l'API kernel) non era corretta. Ciò potrebbe indicare che l'API non è stata implementata dal driver miniport SAS di terze parti (in questo caso è vero) o che l'API ha avuto esito negativo per un altro motivo, ad esempio un disallineamento dei segmenti di download.

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

L'evento ETW 507 dal canale indica che una richiesta SRB SCSI ha avuto esito negativo e fornisce le informazioni aggiuntive che SenseKey è' 5' (richiesta non valida) e che le informazioni di AdditionalSense sono '36 ' (campo non valido in CDB).

   > [!Note]
   > Queste informazioni vengono fornite direttamente dal miniport in questione e la loro accuratezza dipenderà dall'implementazione e della complessità del driver miniport.

È possibile che condizioni di errore diverse mostrino gli stessi codici di errore, se il driver miniport non evita l'ambiguità tra di essi. Ad esempio, il tentativo di scaricare un'immagine del firmware non valida tramite un HBA SAS in un dispositivo SATA (operazione che si prevede non riesca nel dispositivo) può essere tradotto negli stessi codici di errore.

Nei casi in cui i protocolli vengono mescolati e si verifica la conversione, vale a dire SATA dietro SAS, è consigliabile eseguire il test del dispositivo SATA direttamente connesso a un controller SATA per escludere questo come potenziale problema.

### <a name="remediation-options"></a>Opzioni di correzione
Se viene identificato che il driver di terze parti non implementa le API o le conversioni necessarie, è possibile passare alle alternative offerte da Microsoft per SATA (StorAHCI.sys) e NVMe (StorNVMe.sys) o contattare l'OEM o il fornitore OEM oppure HBA che ha fornito il driver SAS e chiedere se esiste una versione più recente con il supporto appropriato.

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Altri strumenti di risoluzione dei problemi con i driver Microsoft (SATA/NVMe)
Quando per i dispositivi di archiviazione vengono utilizzati i driver nativi di Windows, ad esempio StorAHCI.sys o StorNVMe.sys, è possibile ottenere ulteriori informazioni sui casi di errore possibili durante le operazioni di aggiornamento del firmware.

Oltre al canale operativo ClassPnP, StorAHCI e StorNVMe registreranno i codici restituiti specifici del protocollo del dispositivo nel canale ETW seguente:

Visualizzatore eventi - Registri applicazioni e servizi - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-StorPort/Diagnose**

I registri di diagnostica non vengono visualizzati per impostazione predefinita e possono essere attivati o visualizzati facendo clic su "Visualizza" in Visualizzatore eventi e selezionando "Mostra registri di analisi e di debug" dal menu a discesa.

Per raccogliere queste voci di log avanzate, abilitare il log, riprodurre l'errore di aggiornamento del firmware e salvare il log di diagnostica.

Di seguito è riportato un esempio di aggiornamento del firmware in un dispositivo SATA che non riesce perché l'immagine da scaricare non è valida (ID evento: 258):

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

L'evento precedente contiene informazioni dettagliate sul dispositivo nei valori di parametro da 2 a 6. Qui stiamo osservando diversi valori di registro ATA. La specifica di ACS ATA può essere utilizzata per decodificare i valori di seguito per l'errore di un comando download microcodice:
- Codice restituito: 0 (0000 0000) (N/d: non significativo perché non è stato trasferito alcun payload)
- Funzionalità: 15 (0000 1111) (bit 1 è impostato su "1" e indica "Abort")
- SectorCount: 0 (0000 0000) (N/D)
- DriveHead: 160 (1010 0000) (N/d: sono impostati solo bit obsoleti)
- Comando 146 (1001 0010) (bit 1 è impostato su "1" che indica la disponibilità dei dati di rilevamento)

Ciò indica che l'operazione di aggiornamento del firmware è stata interrotta dal dispositivo.

Un livello simile di informazioni di debug è disponibile in questo canale quando si utilizzano dispositivi NVMe con driver NVMe nativo di Windows (StorNVMe.sys).
