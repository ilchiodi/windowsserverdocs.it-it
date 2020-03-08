---
title: Servizio integrità in Windows Server
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 158681e2038e3d8015933771d06d3bfb24d31586
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78865400"
---
# <a name="health-service-in-windows-server"></a>Servizio integrità in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016

Il Servizio integrità è una nuova funzionalità di Windows Server 2016 che migliora l'esperienza di monitoraggio e operatività quotidiana per i cluster che eseguono Spazi di archiviazione diretta.

## <a name="prerequisites"></a>Prerequisiti  

Servizio integrità è abilitato per impostazione predefinita con Spazi di archiviazione diretta. Non è necessario alcuna azione aggiuntiva per configurarlo o avviarlo. Per ulteriori informazioni su Spazi di archiviazione diretta, vedere [spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Report

Vedere [servizio integrità report](health-service-reports.md).

## <a name="faults"></a>Errori

Vedere [servizio integrità errori](health-service-faults.md).

## <a name="actions"></a>Azioni

Vedere [servizio integrità actions](health-service-actions.md).

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

È necessario sostituire il disco fisico disconnesso quando possibile. La maggior parte delle volte è costituita da uno scambio a caldo, ovvero la disattivazione del nodo o dell'enclosure di archiviazione non è obbligatoria. Vedere i dettagli dell'errore per le informazioni sulla posizione e sulla parte.  

#### <a name="verification"></a>Verifica

Quando viene inserito, il disco sostitutivo verrà verificato rispetto al documento dei componenti supportati (vedere la sezione successiva).

#### <a name="pooling"></a>Pooling  

Se autorizzato, il disco sostitutivo verrà sostituito automaticamente nel pool del disco precedente per iniziare a essere usato. A questo punto, il sistema è tornato allo stato iniziale di integrità perfetta e quindi l'errore scompare.  

## <a name="supported-components-document"></a>Documento componenti supportati  

Il Servizio integrità fornisce un meccanismo di imposizione per limitare i componenti utilizzati da Spazi di archiviazione diretta a quelli presenti in un documento dei componenti supportati fornito dall'amministratore o dal fornitore della soluzione. In questo modo si evita che l’utente usi hardware non supportato e che insorgano problemi di garanzia o di conformità con il contratto di assistenza. Questa funzionalità è attualmente limitata ai dispositivi disco fisico, incluse le unità SSD, HDD e NVMe. Il documento dei componenti supportati può limitare il modello, il produttore (facoltativo) e la versione del firmware (facoltativo).

### <a name="usage"></a>Utilizzo  

Il documento componenti supportati utilizza una sintassi ispirata a XML. È consigliabile utilizzare l'editor di testo preferito, ad esempio la [Visual Studio Code](https://code.visualstudio.com/) gratuita o il blocco note, per creare un documento XML che è possibile salvare e riutilizzare.

#### <a name="sections"></a>Sezioni

Il documento include due sezioni indipendenti: `Disks` e `Cache`.

Se viene fornita la sezione `Disks`, solo le unità elencate (come `Disk`) possono unire i pool. Le unità non in elenco vengono impedite dall'Unione dei pool, che ne impedisce l'uso nell'ambiente di produzione. Se questa sezione viene lasciata vuota, a tutte le unità sarà consentito aggiungere pool.

Se viene fornita la sezione `Cache`, per la memorizzazione nella cache vengono utilizzate solo le unità elencate (`CacheDisk`). Se questa sezione viene lasciata vuota, Spazi di archiviazione diretta tenta di [indovinare in base al tipo di supporto e al tipo di bus](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Le unità elencate qui dovrebbero essere elencate anche in `Disks`.

>[!IMPORTANT]
> Il documento dei componenti supportati non viene applicato in modo retroattivo alle unità già in pool e in uso.  

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

Per elencare più unità, è sufficiente aggiungere `<Disk>` o tag `<CacheDisk>` aggiuntivi.

Per inserire il codice XML quando si distribuisce Spazi di archiviazione diretta, usare il parametro `-XML`:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Per impostare o modificare il documento dei componenti supportati dopo la distribuzione di Spazi di archiviazione diretta:

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

Vedere [servizio integrità impostazioni](health-service-settings.md).

## <a name="see-also"></a>Vedere anche

- [Report Servizio integrità](health-service-reports.md)
- [Errori Servizio integrità](health-service-faults.md)
- [Azioni Servizio integrità](health-service-actions.md)
- [Impostazioni Servizio integrità](health-service-settings.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
