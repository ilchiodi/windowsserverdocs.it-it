---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Informazioni sulla presenza di domini di errore
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 18b7a932cc8a22c356fde89baa316c0532ebc374
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475997"
---
# <a name="fault-domain-awareness"></a>Informazioni sulla presenza di domini di errore

> Si applica a: Windows Server 2019 e Windows Server 2016

Il clustering di failover consente a più server di interagire per garantire un'elevata disponibilità, o in altri termini, per offrire tolleranza di errore di nodo. Tuttavia, le aziende richiedono oggi sempre maggiore disponibilità dalla infrastruttura. Per ottenere tempi di attività di tipo cloud si deve disporre di una protezione anche in casi estremi, ad esempio problemi di chassis, interruzioni di rack o calamità naturali. Ecco perché il Clustering di Failover in Windows Server 2016 introdotto chassis, rack e sito nonché la tolleranza.

## <a name="fault-domain-awareness"></a>Informazioni sulla presenza di domini di errore

I concetti di domini di errore e tolleranza di errore sono strettamente correlati. Un dominio di errore è un set di componenti hardware che condividono un singolo punto di errore. Per un sistema a tolleranza di errore a un certo livello sono necessari più domini di errore per tale livello. Ad esempio, per una tolleranza di errore di rack, i server e i dati devono essere distribuiti in più rack.

In questo breve video offre una panoramica di domini di errore in Windows Server 2016:  
[![Fare clic su questa immagine per guardare una panoramica dei domini di errore in Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>Consapevolezza di dominio di errore in Windows Server 2019

La consapevolezza di dominio di errore è disponibile in Windows Server 2019 ma è disabilitato per impostazione predefinita e deve essere abilitata tramite il Registro di sistema di Windows.

Per abilitare la consapevolezza di dominio di errore in Windows Server 2019, passare al Registro di sistema Windows e impostare (Get-Cluster). Chiave del Registro di sistema AutoAssignNodeSite su 1.

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

Per disabilitare il riconoscimento di dominio di errore in Windows 2019, passare al Registro di sistema Windows e impostare (Get-Cluster). Chiave del Registro di sistema AutoAssignNodeSite su 0.

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>Vantaggi
- **Spazi di archiviazione, inclusi spazi di archiviazione diretta Usa domini di errore per massimizzare la sicurezza dei dati.**  
    La resilienza in Spazi di archiviazione è concettualmente simile ai RAID distribuiti definiti da software. Più copie di tutti i dati vengono mantenute sincronizzate, e se si verifica un errore hardware e una copia viene persa, le altre copie vengono usate per ripristinare la resilienza. Per ottenere la migliore resilienza possibile, le copie devono essere conservate in domini di errore separati.

- **Il [servizio integrità](health-service-overview.md) Usa domini per offrire avvisi più utili di errore.**  
    Ogni dominio di errore può essere associato a metadati del percorso che verranno inclusi automaticamente in tutti gli avvisi successivi. Questi descrittori possono aiutare il personale addetto alle operazioni o alla manutenzione e ridurre gli errori eliminando l'ambiguità dell'hardware.  

- **Clustering di estensione Usa domini di errore per l'affinità di archiviazione.** Il clustering di estensione consente ai server lontani di essere aggiunti a un cluster comune. Per ottenere prestazioni ottimali, le applicazioni o le macchine virtuali devono essere eseguite su server vicini a quelli che offrono lo spazio di archiviazione. La consapevolezza di dominio di errore Abilita l'affinità di archiviazione.   

## <a name="levels-of-fault-domains"></a>Livelli di domini di errore  
Sono disponibili quattro livelli canonici di domini di errore: sito, rack, chassis e nodo. I nodi vengono individuati automaticamente; ogni livello aggiuntivo è facoltativo. Ad esempio, se la distribuzione non usa server blade, il livello di chassis non è importante.  

![Diagramma dei diversi livelli di domini di errore](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso  
È possibile usare PowerShell o markup XML per specificare i domini di errore. Entrambi gli approcci sono equivalenti e offrono funzionalità complete.

>[!IMPORTANT]
> Specificare i domini di errore prima di abilitare Spazi di archiviazione diretta, se possibile. Ciò consente la configurazione automatica per preparare il pool, i livelli e le impostazioni quali resilienza e numero di colonne, per la tolleranza di errore chassis o rack. Dopo avere creato il pool e i volumi, i dati non si spostano retroattivamente in risposta alle modifiche della topologia di dominio di errore. Per spostare i nodi tra chassis o rack dopo l'abilitazione di Spazi di archiviazione diretta, è necessario innanzitutto rimuovere il nodo con le relative unità dal pool usando `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>La definizione di domini di errore con PowerShell
Windows Server 2016 introduce i cmdlet seguenti per lavorare con domini di errore:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Questo breve video illustra l'uso di questi cmdlet.
[![Fare clic su questa immagine per un breve video sull'uso dei cmdlet di dominio di errore del Cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Usare `Get-ClusterFaultDomain` per visualizzare la topologia di dominio di errore corrente. Questo comando elenca tutti i nodi del cluster, oltre a qualsiasi chassis, rack o siti creati. È possibile applicare un filtro usando parametri come **-Type** o **-Name**, ma non è obbligatorio.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Usare `New-ClusterFaultDomain` per creare nuovi chassis, rack o siti. Il `-Type` e `-Name` parametri sono obbligatori. I valori possibili per `-Type` vengono `Chassis`, `Rack`, e `Site`. Il `-Name` può essere qualsiasi stringa. (Per `Node` domini di errore di tipo, il nome devono essere il nome effettivo del nodo, come set automaticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server non è possibile e non consente di verificare che eventuali domini di errore creati corrispondano a qualsiasi cosa nel mondo reale, fisico. (Ciò potrebbe sembrare ovvio, ma è importante comprendere). Se tutti i nodi sono fisicamente in un singolo rack, la creazione di due domini di errore `-Type Rack` nel software non offre automaticamente la tolleranza di errore dei rack. Gli amministratori sono responsabili per garantire che la topologia creata usando questi cmdlet corrisponda alla disposizione effettiva dell'hardware.

Usare `Set-ClusterFaultDomain` per spostare un dominio di errore in un altro. I termini "padre" e "figlio" vengono comunemente usati per descrivere la relazione di nidificazione. Il `-Name` e `-Parent` parametri sono obbligatori. Nelle `-Name`, specificare il nome di dominio di errore che indica lo spostamento; in `-Parent`, specificare il nome dell'oggetto di destinazione. Per spostare contemporaneamente più domini di errore, elencare i relativi nomi.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Quando si spostano i domini di errore, vengono anche spostati i relativi domini figlio. Nell'esempio precedente, se Rack A è l'elemento padre di server01.contoso.com, quest'ultimo non deve essere spostato separatamente nel sito Shanghai perché si trova già nel sito insieme all'elemento padre.

È possibile visualizzare le relazioni padre-figlio nell'output del `Get-ClusterFaultDomain`, nella `ParentName` e `ChildrenNames` colonne.

È anche possibile usare `Set-ClusterFaultDomain` per modificare altre proprietà dei domini di errore. Ad esempio, è possibile fornire facoltativo `-Location` o `-Description` i metadati per ogni dominio di errore. Se specificate, queste informazioni vengono incluse nell'invio di avvisi sull'hardware dal servizio di integrità. È inoltre possibile rinominare i domini di errore tramite il `-NewName` parametro. Non rinominare `Node` digitare domini di errore.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Usare `Remove-ClusterFaultDomain` per rimuovere chassis, rack o siti creati. Il parametro `-Name` è obbligatorio. È non è possibile rimuovere un dominio di errore che contiene gli elementi figlio: in primo luogo, rimuovere gli elementi figlio oppure spostarli all'esterno tramite `Set-ClusterFaultDomain`. Per spostare un dominio di errore all'esterno di tutti gli altri domini di errore, impostare il `-Parent` su una stringa vuota (""). Non è possibile rimuovere `Node` digitare domini di errore. Per spostare contemporaneamente più domini di errore, elencare i relativi nomi.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definizione di domini di errore con markup XML
I domini di errore possono essere specificati tramite una sintassi XML. Si consiglia di usare un editor di testo, ad esempio Visual Studio Code (disponibile gratuitamente *[qui](https://code.visualstudio.com/)* ) o Blocco note, per creare un documento XML da salvare e usare di nuovo.  

Questo breve video illustra l'uso di markup XML per specificare i domini di errore.

[![Fare clic su questa immagine per un breve video su come usare XML per specificare i domini di errore](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

In PowerShell eseguire il cmdlet seguente: `Get-ClusterFaultDomainXML`. Questo comando restituisce il dominio di errore corrente per il cluster, in formato XML. Questo riflette ogni individuati `<Node>`, inserito in apertura e chiusura `<Topology>` tag.  

Eseguire il comando seguente per salvare l'output in un file.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Aprire il file e aggiungere `<Site>`, `<Rack>`, e `<Chassis>` tag per specificare come questi nodi sono distribuiti tra siti, rack e chassis. Ogni tag deve essere identificato da un **nome** univoco. Per i nodi, è necessario mantenere il nome specificato automaticamente.  

> [!IMPORTANT]  
> Anche se tutti i tag aggiuntivi sono facoltativi, devono comunque essere conformi alla gerarchia transitiva Site &gt; Rack &gt; Chassis &gt; Node e chiusi in maniera corretta.  
Oltre al nome, a mano libera `Location="..."` e `Description="..."` descrittori possono essere aggiunti a qualsiasi tag.  

#### <a name="example-two-sites-one-rack-each"></a>Esempio: Due siti, un rack ciascuno  

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

Questa guida illustra due esempi, ma il `<Site>`, `<Rack>`, `<Chassis>`, e `<Node>` tag possono essere combinati e associati in molti altri modi per riflettere la topologia fisica della distribuzione, qualsiasi esso sia. Questi esempi illustrano la flessibilità dei tag e il valore dei descrittori di percorso con testo libero per evitare ambiguità.  

### <a name="optional-location-and-description-metadata"></a>Facoltativo: Percorso e una descrizione dei metadati

È possibile fornire facoltativo **ubicazione** oppure **descrizione** i metadati per ogni dominio di errore. Se specificate, queste informazioni vengono incluse nell'invio di avvisi sull'hardware dal servizio di integrità. Questo breve video illustra il valore di aggiunta di descrittori di questo tipo.

[![Fare clic per vedere un breve video che illustra il valore di aggiunta di descrittori di percorso a domini di errore](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Vedere anche  
- [Introduzione a Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)  
- [Introduzione a Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)  
-   [Spazi di archiviazione diretta Panoramica](../storage/storage-spaces/storage-spaces-direct-overview.md) 
