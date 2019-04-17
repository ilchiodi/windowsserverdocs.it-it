---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Informazioni sulla presenza dominio di errore
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Informazioni sulla presenza di dominio di errore in Windows Server 2016

> Si applica a: Windows Server 2016

Clustering di failover consente a più server a lavorare insieme per fornire un'elevata disponibilità, o in altri termini, per fornire tolleranza di errore di nodo. Ma aziende richiedono sempre maggiore disponibilità dalla infrastruttura. Per ottenere tempi di attività di tipo cloud, anche in casi estremi, ad esempio problemi di chassis, interruzioni di rack o calamità naturali devono essere protetta dagli. Ecco perché il Clustering di Failover in Windows Server 2016 introduce anche chassis, rack e tolleranza di errore del sito.

Domini di errore e tolleranza di errore sono strettamente correlati concetti. Un dominio di errore è un set di componenti hardware che condividono un singolo punto di errore. Per uno specifico livello di tolleranza di errore, è necessario più domini di errore a tale livello. Ad esempio, per rack a tolleranza di errore, i server e i dati devono essere distribuiti in più rack.

Questo breve video presenta una panoramica di domini di errore in Windows Server 2016:  
[![Fare clic su questa immagine per guardare una panoramica di domini di errore in Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Vantaggi
- **Spazi di archiviazione, inclusi spazi di archiviazione diretta, Usa i domini di errore per massimizzare la sicurezza dei dati.**  
    La resilienza in spazi di archiviazione è concettualmente simile a RAID distribuiti, definita dal software. Più copie di tutti i dati vengono mantenute sincronizzate, e in caso di errore hardware e una copia viene persa, altre copie vengono usate per ripristinare la resilienza. Per ottenere la migliore resilienza possibile, devono essere conservate in domini di errore.

- **Il [servizio integrità](health-service-overview.md) Usa per offrire avvisi più utili i domini di errore.**  
    Ogni dominio di errore può essere associato a metadati del percorso che verranno inclusi automaticamente in tutti gli avvisi successivi. Questi descrittori possono facilitare le operazioni o personale addetto alla manutenzione e ridurre gli errori eliminando l'ambiguità dell'hardware.  

- **Clustering di estensione Usa domini di errore per l'affinità di archiviazione.** Il clustering di estensione consente ai server lontani di essere aggiunti a un cluster comune. Per ottenere prestazioni ottimali, le applicazioni o macchine virtuali deve essere eseguite su server vicini a quelli che offrono relativo spazio di archiviazione. Informazioni sulla presenza dominio di errore consente questa affinità di archiviazione.   

## <a name="levels-of-fault-domains"></a>Livelli di domini di errore  
Sono disponibili quattro livelli canonici di domini di errore - sito, rack, chassis e nodo. I nodi vengono individuati automaticamente. ogni livello aggiuntivo è facoltativo. Ad esempio, se la distribuzione non Usa server blade, il livello di chassis non è importante per te.  

![Diagramma dei diversi livelli di domini di errore](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Utilizzo  
È possibile utilizzare PowerShell o markup XML per specificare i domini di errore. Entrambi gli approcci sono equivalenti e offrono funzionalità complete.

>[!IMPORTANT]
> Specificare i domini di errore prima di abilitare spazi di archiviazione diretta, se possibile. In questo modo la configurazione automatica preparare il pool, livelli e le impostazioni quali resilienza e numero di colonne, per la tolleranza di errore chassis o rack. Dopo avere creati il pool e i volumi, i dati non si spostano retroattivamente in risposta alle modifiche alla topologia di dominio di errore. Per spostare i nodi tra chassis o rack dopo l'abilitazione di spazi di archiviazione diretta, è necessario innanzitutto rimuovere il nodo e relative unità dal pool usando `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definizione di domini di errore con PowerShell
Windows Server 2016 introduce i cmdlet seguenti per lavorare con i domini di errore:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Questo breve video illustra l'uso di questi cmdlet.
[![Fare clic su questa immagine per guardare un breve video sull'utilizzo dei cmdlet di dominio di errore del Cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Utilizzare `Get-ClusterFaultDomain`per visualizzare la topologia di dominio di errore corrente. Questo comando Elenca tutti i nodi del cluster, oltre a qualsiasi chassis, rack o siti creati. È possibile filtrare l'utilizzo di parametri come **-tipo** o **-nome**, ma non sono necessarie.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Utilizzare `New-ClusterFaultDomain`per creare nuovi chassis, rack o siti. Il `-Type`e `-Name`parametri sono obbligatori. I valori possibili per `-Type`sono `Chassis`, `Rack`, e `Site`. Il `-Name`può essere qualsiasi stringa. (Per `Node`domini di errore di tipo, il nome devono essere il nome effettivo del nodo impostato automaticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server non può e non consente di verificare che eventuali domini di errore creati corrispondano a domini nel mondo reale, fisico. (Ciò potrebbe sembrare ovvio, ma è importante comprendere). Se, nel mondo fisico, i nodi sono tutti in un singolo rack, creazione di due `-Type Rack`domini di errore nel software non offre automaticamente la tolleranza di errore rack. Si è responsabili per garantire che la topologia creata usando questi cmdlet corrisponda alla disposizione effettiva dell'hardware.

Utilizzare `Set-ClusterFaultDomain`per spostare un dominio di errore in un altro. I termini "padre" e "figlio" vengono comunemente utilizzati per descrivere la relazione di nidificazione. Il `-Name`e `-Parent`parametri sono obbligatori. In `-Name`, specificare il nome di dominio di errore da Sposta; in `-Parent`, specificare il nome dell'oggetto di destinazione. Per spostare contemporaneamente più domini di errore, elencare i relativi nomi.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Quando si spostano i domini di errore, i relativi elementi figlio vengono anche spostati. Nell'esempio precedente, se Rack A è l'elemento padre di server01.contoso.com, quest'ultimo non separatamente deve essere spostato nel sito Shanghai: è già presente grazie all'elemento padre, proprio come in tutto il mondo fisico.

È possibile visualizzare le relazioni padre-figlio nell'output di `Get-ClusterFaultDomain`, nel `ParentName`e `ChildrenNames`colonne.

È inoltre possibile utilizzare `Set-ClusterFaultDomain`per modificare altre proprietà di domini di errore. Ad esempio, è possibile fornire facoltativo `-Location`o `-Description`i metadati per ogni dominio di errore. Se specificato, queste informazioni vengono incluse nell'hardware avvisi dal servizio integrità. È inoltre possibile rinominare i domini di errore tramite il `-NewName`parametro. Non rinominare `Node`digitare i domini di errore.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Utilizzare `Remove-ClusterFaultDomain`per rimuovere chassis, rack o siti creati. Il `-Name`parametro è obbligatorio. È Impossibile rimuovere un dominio di errore che include elementi figlio: prima di tutto, rimuovere gli elementi figlio oppure spostarli all'esterno tramite `Set-ClusterFaultDomain`. Per spostare un dominio di errore di fuori di tutti gli altri domini di errore, impostare il relativo `-Parent`su una stringa vuota (""). È possibile rimuovere `Node`digitare i domini di errore. Per rimuovere contemporaneamente più domini di errore, elencare i relativi nomi.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definizione di domini di errore con markup XML
Domini di errore possono essere specificati usando una sintassi XML. Ti consigliamo di usare un editor di testo, ad esempio Visual Studio Code (disponibile gratuitamente *[qui](https://code.visualstudio.com/)*) o blocco note, per creare un documento XML che è possibile salvare e riutilizzare.  

Questo breve video illustra l'uso di Markup XML per specificare i domini di errore.

[![CFare clic su questa immagine per guardare un breve video su come usare XML per specificare i domini di errore](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

In PowerShell, eseguire il cmdlet seguente:`Get-ClusterFaultDomainXML`. Restituisce l'errore dominio specifica corrente per il cluster, in formato XML. Questo riflette ogni individuati `<Node>`, con wrapping in apertura e chiusura `<Topology>`tag.  

Eseguire il comando seguente per salvare l'output in un file.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Aprire il file e aggiungere `<Site>`, `<Rack>`, e `<Chassis>`tag per specificare come questi nodi vengono distribuiti attraverso siti, rack e chassis. Ogni tag deve essere identificato da un univoco **nome **. Per i nodi, è necessario mantenere il nome del nodo come popolati per impostazione predefinita.  

> [!IMPORTANT]  
> Mentre tutti i tag aggiuntivi sono facoltativi, devono essere conformi al sito transitivo &gt;Rack &gt;Chassis &gt;gerarchia nodo e deve essere chiuso in modo corretto.  
Oltre ai nomi, a mano libera `Location="..."`e `Description="..."`descrittori possono essere aggiunti a qualsiasi tag.  

#### <a name="example-two-sites-one-rack-each"></a>Esempio: Due siti, con un rack ciascuno  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>Esempio: due chassis, server blade  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

Per impostare la nuova specifica di dominio di errore, salvare il codice XML ed eseguire il comando seguente in PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Questa guida illustra due esempi, ma il `<Site>`, `<Rack>`, `<Chassis>`, e `<Node>`tag possono essere combinati e associati in molti altri modi per riflettere la topologia fisica della distribuzione, tutto quello che potrebbe essere. Speriamo che questi esempi illustrano la flessibilità dei tag e il valore dei descrittori di percorso con testo libero per evitare ambiguità.  

### <a name="optional-location-and-description-metadata"></a>Facoltativo: Percorso e la descrizione dei metadati

È possibile fornire facoltativo **percorso** o **descrizione** i metadati per ogni dominio di errore. Se specificato, queste informazioni vengono incluse nell'hardware avvisi dal servizio integrità. Questo breve video illustra il vantaggio di aggiungere descrittori di questo tipo.

[![CFare clic su un breve video che illustra il vantaggio di aggiungere descrittori di percorso a domini di errore](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Vedere anche  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
