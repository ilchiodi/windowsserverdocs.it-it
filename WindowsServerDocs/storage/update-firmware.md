---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: Aggiornamento del firmware delle unità in Windows Server 2016
ms.prod: windows-server-threshold
ms.author: toklima
ms.manager: dmoss
ms.technology: storage-spaces
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 50291bd4da05d9c2736c84443b444b9a43f46344
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884782"
---
# <a name="updating-drive-firmware-in-windows-server-2016"></a>Aggiornamento del firmware delle unità in Windows Server 2016
>Si applica a: Windows 10, Windows Server (canale semestrale), Windows Server 2016

L'aggiornamento del firmware per le unità è sempre stata un'attività complessa che può causare un certo tempo di inattività. Per tale motivo si stanno apportando miglioramenti a Spazi di archiviazione, Windows Server, Windows 10, versione 1703 e successive. Se si dispone di unità che supportano il nuovo meccanismo di aggiornamento del firmware incluso in Windows, è possibile aggiornare il firmware delle unità nell'ambiente di produzione senza tempo di inattività. Tuttavia, prima di aggiornare il firmware di un'unità di produzione, assicurarsi di leggere i suggerimenti su come ridurre al minimo il rischio durante l'uso di questa nuova funzionalità avanzata.

  > [!Warning]
  > Gli aggiornamenti del firmware sono operazioni di manutenzione potenzialmente rischiose che devono essere eseguite solo dopo aver effettuato un test approfondito dell'immagine del nuovo firmware. L'installazione di nuovo firmware in hardware non supportato può compromettere l'affidabilità e la stabilità o anche causare la perdita di dati. Gli amministratori devono leggere le note sulla versione specifiche dell'aggiornamento per determinarne l'impatto e l'applicabilità.

## <a name="drive-compatibility"></a>Compatibilità con le unità

Per aggiornare il firmware delle unità con Windows Server, è necessario che le unità siano supportate. Per garantire il comportamento corretto di dispositivi comuni, prima di tutto sono stati definiti requisiti HLK (Hardware Lab Kit) nuovi e, per Windows Server 2016 e per Windows 10, requisiti facoltativi per i dispositivi SAS, SATA e NVMe. Questi requisiti definiscono quali comandi devono essere supportati dai dispositivi SATA, SAS o NVMe perché questi ultimi siano aggiornabili tramite questi nuovi cmdlet PowerShell nativi di Windows. Per supportare tali requisiti è disponibile un nuovo test HLK per verificare se i prodotti dei fornitori supportano i comandi giusti e possono implementarli nelle revisioni future. 

Per informazioni sulla compatibilità dell'hardware in uso con l'aggiornamento del firmware delle unità da parte di Windows, contattare il fornitore della soluzione.
Di seguito sono riportati i collegamenti relativi ai diversi requisiti:

-   SATA: [Device.Storage.Hd.Sata](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsata) - in di **[Se implementato\] Firmware Download & Activate** sezione
    
-   SAS: [Device.Storage.Hd.Sas](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsas) - in di **[Se implementato\] Firmware Download & Activate** sezione

-   NVMe: [Device.Storage.ControllerDrive.NVMe](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragecontrollerdrivenvme) : nelle sezioni **5.7** e **5.8**.

## <a name="powershell-cmdlets"></a>Cmdlet PowerShell

I due cmdlet aggiunti a Windows sono:

-   Get–StorageFirmwareInformation
-   Update–StorageFirmware

Il primo cmdlet offre informazioni dettagliate sulle funzionalità del dispositivo, le immagini del firmware e le revisioni. In questo caso il computer contiene solo un singolo SSD SATA con uno slot del firmware. Di seguito è riportato un esempio:

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

Si noti che per i dispositivi SAS "SupportsUpdate" è sempre "True", poiché non è possibile eseguire query al dispositivo in modo esplicito per il supporto di questi comandi.

Il secondo cmdlet, Update–StorageFirmware, consente agli amministratori di aggiornare il firmware dell'unità con un file di immagine, se l'unità supporta il nuovo meccanismo di aggiornamento del firmware. Il file di immagine deve essere richiesto direttamente all'OEM o al fornitore dell'unità.

  > [!Note]
  > Prima di aggiornare qualsiasi tipo di hardware di produzione, eseguire il test dell'immagine del firmware specifico con hardware identico in un ambiente lab.

Prima l'unità caricherà l'immagine del nuovo firmware in un'area di gestione temporanea interna. Mentre viene eseguita questa operazione, in genere le attività di I/O continuano. L'immagine si attiva dopo il download. Durante questo periodo l'unità non sarà in grado di rispondere ai comandi di I/O poiché viene eseguita una reimpostazione interna. Ciò significa che durante l'attivazione tale unità non rende disponibili i propri dati. Se un'applicazione deve accedere ai dati di questa unità, dovrà attendere fino al completamento dell'attivazione del firmware. Ecco un esempio del cmdlet in azione:

   ```powershell 
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

Le unità in genere non completano richieste di I/O durante l'attivazione dell'immagine di un nuovo firmware. La durata dell'attivazione dipende dalla progettazione dell'unità e dal tipo di firmware di cui si esegue l'aggiornamento. Si è notato che i tempi di aggiornamento variano da meno di 5 secondi a più di 30 secondi.

Questa unità ha eseguito l'aggiornamento del firmware in ~5.8, come illustrato di seguito:

```powershell 
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>Aggiornamento di unità nell'ambiente di produzione

Prima di inserire un server nell'ambiente di produzione, è consigliabile aggiornare il firmware delle unità con il firmware consigliato dal produttore dell'hardware o dall'OEM che ha venduto la soluzione (alloggiamenti di archiviazione, unità e server) e la supporta.

Dopo l'inserimento del server nell'ambiente di produzione, è consigliabile apportare a questo il minor numero di modifiche possibile. Tuttavia, in alcuni casi il fornitore della soluzione potrebbe informare della disponibilità di aggiornamenti di importanza critica per il firmware delle unità. In questo caso ecco alcune procedure consigliate da seguire prima di applicare qualsiasi aggiornamento al firmware delle unità:

1. Esaminare le note sulla versione del firmware e verificare che l'aggiornamento risolva problemi che potrebbero influire sull'ambiente in uso e che il firmware non contenga problemi noti che potrebbero influire negativamente sull'ambiente stesso.

2. Installare il firmware nell'ambiente lab in un server che dispone di unità identiche (comprese tutte le revisioni delle unità, se sono presenti più revisioni della stessa unità) e testare l'unità in condizioni di carico con il nuovo firmware. Per informazioni su come eseguire un test di carico sintetico, vedere [Testare le prestazioni di Spazi di archiviazione usando carichi di lavoro sintetici in Windows Server](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>Aggiornamenti del firmware automatizzati con Spazi di archiviazione diretta

Windows Server 2016 include un Servizio integrità per le distribuzioni di Spazi di archiviazione diretta, incluse le soluzioni Microsoft Azure Stack. Lo scopo principale del Servizio integrità è di semplificare il monitoraggio e gestione della distribuzione di hardware. Nell'ambito delle funzioni di gestione, dispone della funzionalità di implementazione del firmware delle unità di un intero cluster senza disconnettere alcun carico di lavoro e senza tempi di inattività. Questa funzionalità, basata sui criteri, è controllata dall'amministratore.

L'uso del Servizio integrità per l'implementazione di firmware in un cluster è molto semplice e prevede i passaggi seguenti:

-   Identificare le unità HDD ed SSD che si prevede faranno parte del cluster di Spazi di archiviazione diretta e verificare se le unità supportano le versioni di Windows che eseguono gli aggiornamenti del firmware
-   Elencare tali unità nel file XML dei componenti supportati
-   Identificare le versioni del firmware di cui si prevede che siano dotate le unità elencate nel file XML dei componenti supportati, inclusi i percorsi delle immagini del firmware
-   Caricare il file XML nel DB del cluster

A questo punto il Servizio integrità ispezionerà e analizzerà il file XML e identificherà le eventuali unità in cui non è stata distribuita la versione del firmware voluta. Si procederà quindi al reindirizzamento delle operazioni di I/O dalle unità interessate, nodo per nodo, e all'aggiornamento del firmware su tali unità. Un cluster di Spazi di archiviazione diretta raggiunge la resilienza diffondendo i dati tra più nodi server. È possibile per il Servizio integrità isolare l'equivalente di un intero nodo di unità per gli aggiornamenti. Dopo che un nodo è stato aggiornato, Spazi di archiviazione avvierà il ripristino, sincronizzando di nuovo tra loro tutte le copie dei dati nel cluster prima di passare al nodo successivo. È previsto e normale che Spazi di archiviazione passi a una modalità di funzionamento "ridotta" durante l'implementazione del firmware.

Per garantire un'implementazione stabile e una quantità di tempo sufficiente per la convalida di una nuova immagine del firmware, c'è un ritardo significativo tra l'aggiornamento di un server e quello di un altro. Per impostazione predefinita, il Servizio integrità attende sette giorni prima di aggiornare il 2<sup>o</sup> server. Eventuali server successivi (3<sup></sup>, 4<sup>o</sup> e così via) vengono aggiornati con un giorno di ritardo. Se un amministratore si rende conto che il firmware è instabile o inadatto per altri motivi, può interrompere l'implementazione tramite Servizio integrità in qualsiasi momento. Se il firmware è stato convalidato in precedenza e si vuole eseguire un'implementazione più veloce, questi valori predefiniti possono essere modificati da giorni a ore o minuti.

Ecco un esempio di file XML dei componenti supportati per un cluster di Spazi di archiviazione diretta generico:

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

Per avviare l'implementazione del nuovo firmware nel cluster di Spazi di archiviazione, è sufficiente caricare il file con estensione xml nel database del cluster:

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

Modificare il file nell'editor preferito, ad esempio Visual Studio Code o Blocco note e quindi salvarlo.

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

Se si desidera visualizzare il servizio integrità in azione e altre informazioni su un meccanismo di implementazione, da uno sguardo a questo video: https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>Domande frequenti

Vedere anche [Risoluzione dei problemi degli aggiornamenti del firmware delle unità](troubleshoot-firmware-update.md).

### <a name="will-this-work-on-any-storage-device"></a>Funziona in qualsiasi dispositivo di archiviazione?

Funziona nei dispositivi di archiviazione che implementano nel firmware i comandi corretti. Il cmdlet Get–StorageFirmwareInformation viene visualizzato se il firmware di un'unità supporta effettivamente i comandi corretti (per SATA/NVMe) e il test HLK consente ai fornitori e agli OEM di testare questo comportamento.

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>Dopo l'aggiornamento di un'unità SATA, viene segnalato che l'unità non supporta più il meccanismo di aggiornamento. L'unità è guasta?

No, l'unità funziona correttamente, a meno che il nuovo firmware non consenta più gli aggiornamenti. Si tratta di un problema noto, in base al quale la versione delle funzionalità dell'unità memorizzata nella cache non è corretta. L'esecuzione di "Update–StorageProviderCache –DiscoveryLevel Full" enumererà nuovamente le funzionalità dell'unità e aggiornerà la copia memorizzata nella cache. Come soluzione alternativa è consigliabile eseguire una volta il comando riportato sopra prima di avviare l'aggiornamento del firmware o di completare l'implementazione in un cluster Spazi di archiviazione diretta.

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>È possibile aggiornare il firmware nella mia SAN tramite questo meccanismo?
No. Le SAN in genere dispongono di utilità e interfacce personalizzate per tali operazioni di manutenzione. Questo nuovo meccanismo è destinato alle risorse di archiviazione collegate direttamente, ad esempio dispositivi NVMe, SAS o SATA.

### <a name="from-where-do-i-get-the-firmware-image"></a>Dove è possibile ottenere l'immagine del firmware?

Qualsiasi firmware deve sempre essere richiesto direttamente all'OEM, al fornitore della soluzione o al fornitore dell'unità e non ottenuto tramite download da altri. Windows fornisce il meccanismo per portare l'immagine nell'unità, ma non è in grado di verificarne l'integrità.

### <a name="will-this-work-on-clustered-drives"></a>Questa operazione funziona con unità cluster?

I cmdlet possono funzionare anche con unità cluster. È però necessario tenere presente che l'orchestrazione di Servizio integrità riduce l'impatto delle operazioni di I/O sui carichi di lavoro in esecuzione. Se i cmdlet vengono usati direttamente sulle unità cluster, è probabile che le operazioni di I/O si blocchino. In genere la procedura consigliata è di eseguire gli aggiornamenti del firmware delle unità quando queste non sono interessate da carichi di lavoro o sono interessate da carichi di lavoro molto ridotti.

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>Cosa accade quando si aggiorna il firmware in Spazi di archiviazione?

In Windows Server 2016 con Servizio integrità distribuito in Spazi di archiviazione diretta è possibile eseguire questa operazione senza portare offline i carichi di lavoro, presupponendo che le unità supportino l'aggiornamento del firmware di Windows Server.

### <a name="what-happens-if-the-update-fails"></a>Cosa accade se l'aggiornamento non riesce?

L'aggiornamento potrebbe non riuscire per vari motivi, alcuni di essi sono: 1) l'unità non supporta i comandi che consentono a Windows di aggiornare il firmware. In questo caso, la nuova immagine del firmware non viene mai attivata e l'unità continua a funzionare con l'immagine precedente. 2) Non è possibile scaricare l'immagine o applicarla all'unità (versione non corrispondente, immagine danneggiata e così via). In questo caso l'unità non riesce a eseguire il comando di attivazione. Anche in questo caso, continuerà a funzionare l'immagine del firmware precedente.

Se dopo l'aggiornamento del firmware l'unità non risponde più, la causa probabile è un bug del firmware stesso. Testare tutti gli aggiornamenti del firmware in un ambiente lab prima di applicarli nell'ambiente di produzione. L'unica correzione potrebbe essere di sostituire l'unità.

Per altre informazioni, vedere [Risoluzione dei problemi degli aggiornamenti del firmware delle unità](troubleshoot-firmware-update.md).

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>Come si interrompe un'implementazione del firmware in corso?

Disabilitare l'implementazione in PowerShell tramite:
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>Durante l'implementazione viene visualizzato l'errore Accesso negato o Percorso non trovato. Come si può risolvere il problema?

Assicurarsi che l'immagine del firmware che si vuole usare per l'aggiornamento sia accessibile per tutti i nodi del cluster. A tale scopo, il modo più semplice è posizionare l'immagine in un assicurarsi per garantire che questo è posizionarlo in un volume condiviso cluster.
