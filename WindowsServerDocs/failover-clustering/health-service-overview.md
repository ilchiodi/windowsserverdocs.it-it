---
title: "Servizio di integrità in Windows Server 2016"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Servizio di integrità in Windows Server 2016
> Si applica a Windows Server 2016

Il servizio integrità è una nuova funzionalità di Windows Server 2016 che consente di migliorare il monitoraggio e l'esperienza per i cluster che eseguono spazi di archiviazione diretta.

## <a name="prerequisites"></a>Prerequisiti  

Il servizio integrità è abilitato per impostazione predefinita con spazi di archiviazione diretta. Non è necessaria alcuna azione aggiuntiva per configurarlo o avviarlo. Per ulteriori informazioni su spazi di archiviazione diretta, vedere [spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Report

Vedere [servizio integrità invia report](health-service-reports.md).

## <a name="faults"></a>Errori

Vedere [errori servizio integrità](health-service-faults.md).

## <a name="actions"></a>Azioni

Vedere [azioni servizio integrità](health-service-actions.md).

## <a name="automation"></a>Automazione  

Questa sezione descrive i flussi di lavoro che vengono automatizzati da servizio integrità nel ciclo di vita del disco.  

### <a name="disk-lifecycle"></a>Ciclo di vita del disco   

Il servizio integrità consente di automatizzare la maggior parte delle fasi del ciclo di vita del disco fisico. Si supponga che lo stato della distribuzione iniziale è in stato perfetto, ovvero che, tutti i dischi fisici funzionino correttamente.  

#### <a name="retirement"></a>Ritiro  

I dischi fisici vengono disconnessi automaticamente quando viene generato un errore corrispondente e non può più essere utilizzate. Esistono diversi casi:  

-   Errore dei supporti: il disco fisico è definitivamente non è riuscito o interrotto e deve essere sostituito.  

-   Perdita di comunicazione: il disco fisico ha perso la connettività per oltre 15 minuti consecutivi.  

-   Mancata risposta: il disco fisico è stata rilevata una latenza di oltre 5,0 secondi tre o più volte entro un'ora.  

>[!NOTE]
> Se si perde la connettività di diversi dischi fisici in una sola volta, o per un intero nodo o alloggiamento di archiviazione, il servizio integrità *non* disconnette tali dischi poiché è improbabile che sia il problema principale.  

Se il disco disconnesso fungeva da cache per molti altri dischi fisici, questi verranno automaticamente riassegnati a un altro disco cache se è disponibile. È richiesta alcuna azione utente speciale.  

#### <a name="restoring-resiliency"></a>Ripristino della resilienza  

Una volta che un disco fisico è stato disattivato, il servizio integrità inizia immediatamente a copiarne i dati nei dischi fisici rimanenti, per ripristinare la resilienza completa. Una volta completata questa operazione, i dati sono totalmente protetti e a tolleranza d'errore.  

>[!NOTE]
> Il ripristino immediato richiede una capacità sufficiente disponibile tra i dischi fisici rimanenti.  

#### <a name="blinking-the-indicator-light"></a>Lampeggiamento della spia  

Se possibile, il servizio integrità inizierà a far lampeggiare la luce della spia del disco fisico disconnesso o dello slot corrispondente. Continuerà per un tempo indefinito, finché non viene sostituito il disco disconnesso.  

>[!NOTE]
> In alcuni casi, il disco potrebbe non essere riuscita in modo da preclude persino il lampeggiamento chiaro il funzionamento, ad esempio, un'interruzione dell'alimentazione totale.  

#### <a name="physical-replacement"></a>Sostituzione disco fisico  

È necessario sostituire il disco fisico disconnesso quando possibile. In genere costituito un caldo - ad esempio Spegnere l'enclosure di archiviazione o del nodo non è obbligatorio. L'errore per la posizione utile e parte informazioni, vedere.  

#### <a name="verification"></a>Verifica

Quando viene inserito il disco sostitutivo, verrà verificato contro il documento dei componenti supportati (vedere la sezione successiva).

#### <a name="pooling"></a>Il pool  

Se è consentito, il disco sostitutivo verrà sostituito automaticamente nel pool del suo predecessore, immettere l'uso. A questo punto, il sistema è tornato allo stato iniziale di integrità perfetta e quindi l'errore scompare.  

## <a name="supported-components-document"></a>Documento componenti supportati  

Il servizio integrità offre un meccanismo di imposizione per limitare i componenti usati da spazi di archiviazione diretta a quelli inclusi in un documento di componenti supportati fornito dall'amministratore o fornitore della soluzione. Questo consente di evitare che insorgano usi hardware non supportato dallo sviluppatore o da altri utenti, che consentano di conformità contratto garanzia o supporto. Questa funzionalità è attualmente limitata ai dispositivi disco fisico, tra cui unità SSD, HDD, e unità NVMe. Limitare il documento di componenti è supportato nel modello, produttore (facoltativo) e del firmware versione (facoltativa).

### <a name="usage"></a>Utilizzo  

Il documento dei componenti supportati Usa una sintassi XML. Ti consigliamo di usare un editor di testo, ad esempio Visual Studio Code (disponibile gratuitamente [qui](http://code.visualstudio.com/)) o blocco note, per creare un documento XML che è possibile salvare e riutilizzare.

#### <a name="sections"></a>Sezioni

Il documento contiene due sezioni indipendenti: **dischi** e **Cache**.

Se il **dischi** sezione viene fornita, sono consentite solo le unità elencate aggiunto ai pool. Qualsiasi unità non in elenco non possono aggiungersi ai pool, che quindi in modo efficace il loro uso nell'ambiente di produzione. Se in questa sezione viene lasciata vuota, qualsiasi unità potrà essere aggiunto ai pool.

Se il **Cache** sezione viene fornita, verranno utilizzate per la memorizzazione nella cache solo le unità elencate. Se in questa sezione viene lasciata vuota, spazi di archiviazione diretta tenterà di indovinare in base a tipo di supporto e tipo di bus. Ad esempio, se la distribuzione Usa unità SSD (SSD) e unità disco rigido (HDD), il primo viene automaticamente scelto per la memorizzazione nella cache; Tuttavia, se la distribuzione Usa all-flash, si potrebbe essere necessario specificare i dispositivi di resistenza superiore che si desidera utilizzare per la memorizzazione nella cache qui.

>[!IMPORTANT]
> Il documento dei componenti supportati non vale in modo retroattivo per le unità già in pool e in uso.  

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
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

Per visualizzare un elenco più unità, aggiungere semplicemente altri ** &lt;disco&gt; ** tag all'interno di una delle due sezioni.

Per inserire il codice XML durante la distribuzione di spazi di archiviazione diretta, utilizzare il **- XML** flag:

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

Per impostare o modificare il documento di componenti supportati quando spazi di archiviazione diretta è stato distribuito (ad esempio, una volta che è già in esecuzione il servizio integrità), utilizzare il cmdlet PowerShell seguente:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>Il modello, produttore e le proprietà di versione del firmware devono corrispondere esattamente ai valori che si ottengano usando il **Get-PhysicalDisk** cmdlet. Questo può essere diverso dal tuo previsione "buon senso", a seconda di implementazione del proprio fornitore. Ad esempio, il produttore potrebbe essere "CONTOSO LTD" invece di "Contoso", oppure può essere vuoto mentre il modello è "Contoso-XZY9000".  

È possibile verificare tramite il cmdlet PowerShell seguente:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Impostazioni

Vedere [le impostazioni del servizio integrità](health-service-settings.md).

## <a name="see-also"></a>Vedere anche

- [Servizio integrità invia report](health-service-reports.md)
- [Errori del servizio integrità](health-service-faults.md)
- [Azioni del servizio integrità](health-service-actions.md)
- [Impostazioni del servizio integrità](health-service-settings.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
