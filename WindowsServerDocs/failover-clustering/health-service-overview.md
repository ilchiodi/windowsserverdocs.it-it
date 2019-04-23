---
title: Integrità dei servizi in Windows Server
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 5afb64dcf0c59697ed55d7cf51ef1bc36e7e0e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863812"
---
# <a name="health-service-in-windows-server"></a>Integrità dei servizi in Windows Server

> Si applica a Windows Server 2016

Il servizio integrità è una nuova funzionalità di Windows Server 2016 che consente di migliorare il monitoraggio quotidiano e l'esperienza operativa per i cluster che esegue spazi di archiviazione diretta.

## <a name="prerequisites"></a>Prerequisiti  

Servizio integrità è abilitato per impostazione predefinita con Spazi di archiviazione diretta. Non è necessario alcuna azione aggiuntiva per configurarlo o avviarlo. Per altre informazioni su spazi di archiviazione diretta, vedere [spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Rapporti

Visualizzare [servizio integrità invia report](health-service-reports.md).

## <a name="faults"></a>Errori

Visualizzare [gli errori di integrità servizio](health-service-faults.md).

## <a name="actions"></a>Azioni

Visualizzare [azioni del servizio integrità](health-service-actions.md).

## <a name="automation"></a>Automazione  

La sezione seguente descrive i flussi di lavoro che vengono automatizzati da Servizio integrità nel ciclo di vita del disco.  

### <a name="disk-lifecycle"></a>Ciclo di vita del disco   

Servizio integrità consente di automatizzare la maggior parte delle fasi del ciclo di vita del disco fisico. Si supponga che lo stato iniziale della distribuzione sia in stato perfetto, ovvero che tutti i dischi fisici funzionino correttamente.  

#### <a name="retirement"></a>Disconnessione  

I dischi fisici vengono disconnessi automaticamente quando non possono più essere usati e viene generato un errore corrispondente. Esistono diversi casi:  

-   Errore dei supporti: il disco fisico è definitivamente guasto e non può più essere sostituito.  

-   Perdita di comunicazione: il disco fisico ha perso la connettività per oltre 15 minuti consecutivi.  

-   Mancata risposta: è stata rilevata una latenza per il disco fisico di oltre 5,0 secondi tre o più volte all'interno di un'ora.  

>[!NOTE]
> Se si perde la connettività di diversi dischi fisici in una sola volta, o di un intero nodo o alloggiamento di archiviazione, Servizio integrità *non* disconnette tali dischi poiché è improbabile che costituiscano la causa radice del problema.  

Se il disco disconnesso fungeva da cache per molti altri dischi fisici, questi verranno automaticamente riassegnati a un altro disco cache disponibile. Non è richiesta alcuna azione specifica da parte dell'utente.  

#### <a name="restoring-resiliency"></a>Ripristino della resilienza  

Una volta che un disco fisico è stato disattivato, Servizio integrità inizia immediatamente a copiarne i dati nei dischi fisici rimanenti, per ripristinare la resilienza completa. Una volta completata questa operazione, i dati sono totalmente protetti e a tolleranza d’errore.  

>[!NOTE]
> Il ripristino immediato richiede una capacità sufficiente disponibile tra i dischi fisici rimanenti.  

#### <a name="blinking-the-indicator-light"></a>Lampeggiamento della spia  

Se possibile, Servizio integrità inizierà a far lampeggiare la luce della spia del disco fisico disconnesso o dello slot corrispondente. La spia continuerà a lampeggiare per un tempo indefinito, finché il disco non verrà sostituito.  

>[!NOTE]
> In alcuni casi il guasto del disco potrebbe precludere persino il lampeggiamento della spia, ad esempio in caso di mancanza totale di alimentazione elettrica.  

#### <a name="physical-replacement"></a>Sostituzione disco fisico  

È necessario sostituire il disco fisico disconnesso quando possibile. In genere, si tratta di una hot swap - vale a dire lo spegnimento dell'enclosure di archiviazione o di nodo non è obbligatorio. Vedere i dettagli dell'errore per le informazioni sulla posizione e sulla parte.  

#### <a name="verification"></a>Verifica

Quando viene inserito il disco sostitutivo, verrà verificato per il documento dei componenti supportati (vedere la sezione successiva).

#### <a name="pooling"></a>Inserimento nel pool  

Se autorizzato, il disco sostitutivo verrà sostituito automaticamente nel pool del disco precedente per iniziare a essere usato. A questo punto, il sistema è tornato allo stato iniziale di integrità perfetta e quindi l'errore scompare.  

## <a name="supported-components-document"></a>Documento dei componenti supportati  

Il servizio di integrità fornisce un meccanismo di imposizione per limitare i componenti usati per spazi di archiviazione diretta per quelle in un documento dei componenti supportati fornito dall'amministratore o fornitore della soluzione. In questo modo si evita che l’utente usi hardware non supportato e che insorgano problemi di garanzia o di conformità con il contratto di assistenza. Questa funzionalità è attualmente limitata ai dispositivi disco fisico, tra cui unità HDD, SSD e unità NVMe. Il documento dei componenti supportati possono limitare in versione di modello, produttore (facoltativo) e del firmware (facoltativa).

### <a name="usage"></a>Uso  

Il documento dei componenti supportati Usa una sintassi XML. È consigliabile usare un editor di testo, ad esempio la versione gratuita [Visual Studio Code](http://code.visualstudio.com/) o blocco note, per creare un documento XML che è possibile salvare e riutilizzare.

#### <a name="sections"></a>Sezioni

Il documento contiene due sezioni indipendenti: `Disks` e `Cache`.

Se il `Disks` sezione viene fornito, solo le unità elencati (come `Disk`) sono autorizzati ad aggiungere i pool. Tutte le unità non in elenco vengono impedite di aggiungersi ai pool, precluso loro uso in produzione. Se in questa sezione viene lasciata vuota, qualsiasi unità potrà essere aggiunto ai pool.

Se il `Cache` sezione viene fornito, solo le unità elencati (come `CacheDisk`) vengono usati per la memorizzazione nella cache. Se in questa sezione viene lasciata vuota, spazi di archiviazione diretta tenta [ipotesi basati sul tipo di supporti e tipo di bus](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Le unità elencate di seguito devono essere elencate anche in `Disks`.

>[!IMPORTANT]
> Il documento dei componenti supportati non si applica retroattivamente alle unità già in pool e in uso.  

#### <a name="example"></a>Esempio

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

Per elencare più unità, è sufficiente aggiungere ulteriori `<Disk>` o `<CacheDisk>` tag.

Per inserire il codice XML durante la distribuzione di spazi di archiviazione diretta, usare il `-XML` parametro:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Per impostare o modificare il documento dei componenti supportati spazi di archiviazione diretta è già stato distribuito:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>Il modello, il produttore e le proprietà della versione del firmware devono corrispondere esattamente ai valori che si ottengano usando il cmdlet **Get-PhysicalDisk**. Il risultato può essere diverso da quanto ci si aspetterebbe ma dipende dall’implementazione del proprio fornitore. Ad esempio, il produttore potrebbe essere "CONTOSO LTD" invece di "Contoso", oppure il campo potrebbe essere vuoto mentre il modello è "Contoso-XZY9000".  

Per verificare, usare il cmdlet PowerShell seguente:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Impostazioni

Visualizzare [le impostazioni del servizio integrità](health-service-settings.md).

## <a name="see-also"></a>Vedere anche

- [Report sull'integrità del servizio](health-service-reports.md)
- [Errori del servizio integrità](health-service-faults.md)
- [Azioni del servizio integrità](health-service-actions.md)
- [Impostazioni del servizio integrità](health-service-settings.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
