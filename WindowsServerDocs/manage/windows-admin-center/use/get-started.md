---
title: Introduzione a Windows Admin Center
description: Introduzione a Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: bd35e439ee3c76af1306bbbd712d754dd79f555f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446096"
---
# <a name="get-started-with-windows-admin-center"></a>Introduzione a Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows Admin Center installato in Windows 10

> [!IMPORTANT]
> È necessario essere un membro del gruppo Administrators locale per l'uso di Windows Admin Center on Windows 10

### <a name="selecting-a-client-certificate"></a>Selezione di un certificato client

La prima volta che si apre Windows Admin Center in Windows 10, assicurarsi di selezionare il *Windows Admin Center Client* certificato (in caso contrario, si otterrà un errore HTTP 403 indicante che "non è possibile accedere a questa pagina").

In Microsoft Edge, quando viene richiesto con questa finestra di dialogo:
 
1. Fare clic su **altre opzioni**

    ![](../media/launch-cert-1.png)

2. Selezionare il certificato con l'etichetta **Client di Windows Admin Center** e fare clic su **OK**

    ![](../media/launch-cert-2.png)

3. Assicurarsi che **Always Allow Access** sia selezionata e fare clic su **Consenti**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>La connessione al cluster e nodi gestiti

Dopo aver completato l'installazione di Windows Admin Center, è possibile aggiungere server o cluster da gestire dalla pagina di panoramica principale.

 **Aggiungere un singolo server o un cluster come un nodo gestito**

1. Fare clic su **+ Aggiungi** sotto **tutte le connessioni**.

   ![](../media/launch/addserver0.png)

2. Scegliere di aggiungere una connessione Server, Cluster di Failover o Hyper-Converged Cluster:
    
   ![](../media/launch/addserver1.png)

3. Digitare il nome del server o del cluster per gestire e fare clic su **Submit**. Server o del cluster verrà aggiunto all'elenco di connessione nella pagina di panoramica.

   ![](../media/launch/addserver2.png)

   **-OPPURE-**

**Più server di importazione BULK**

 1. Nel **Aggiungi connessione al Server** pagina, scegliere il **importazione server** scheda.

    ![](../media/launch/import-servers.png)

 2. Fare clic su **esplorare** e selezionare un file di testo che contiene una virgola o nuova riga separati, elenco di nomi di dominio completi per i server da aggiungere.

    **-OPPURE-**

**Aggiungere i server eseguendo una ricerca in Active Directory**

 1. Nel **connessione del Server di aggiungere** pagina, scegliere il **ricerche in Active Directory** scheda.

    ![](../media/launch/search-ad.png)

 2. Immettere i criteri di ricerca e fare clic su **ricerca**. Sono supportati i caratteri jolly (*).

 3. Al termine dell'esecuzione della ricerca - selezionare uno o più dei risultati, se lo si desidera aggiungere i tag e fare clic su **Add**.

## <a name="authenticate-with-the-managed-node"></a>Eseguire l'autenticazione con il nodo gestito ##

Windows Admin Center supporta diversi meccanismi per l'autenticazione con un nodo gestito. Accesso Single sign-on è quello predefinito.

**Single Sign-on**

È possibile usare le credenziali Windows correnti per l'autenticazione con il nodo gestito. Questo è il valore predefinito e Windows Admin Center tenta l'accesso sign-on quando si aggiunge un server. 

**Accesso Single sign-on quando viene distribuita come servizio in Windows Server**

Se è stato installato Windows Admin Center su Windows Server, è necessaria per il single sign-on configurazione aggiuntiva.  [Configurare l'ambiente per la delega](../configure/user-access-control.md)

**-OPPURE-**

**Uso *gestire come* per specificare le credenziali**

Sotto **tutte le connessioni**, selezionare un server dall'elenco e scegliere **gestire come** per specificare le credenziali che si utilizzerà per eseguire l'autenticazione al nodo gestito:

![](../media/launch-use-6.png)

Se Windows Admin Center è in esecuzione in modalità servizio in Windows Server, ma non è configurata la delega Kerberos, è necessario immettere nuovamente le credenziali di Windows:

![](../media/launch-use-7.png)

È possibile applicare le credenziali di tutte le connessioni, che verranno memorizzarli nella cache per la sessione del browser specifiche. Se lo si ricarica il browser, è necessario immettere nuovamente le **gestire come** credenziali.

**Soluzione di Password di amministratore locale (LAPS)**

Se l'ambiente Usa [LAPS](https://technet.microsoft.com/mt227395.aspx)e si dispone di Windows Admin Center installato nel PC Windows 10, è possibile usare credenziali LAPS per l'autenticazione con il nodo gestito. **Se si usa questo scenario, please** [inviare commenti e suggerimenti](http://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Uso dei tag per organizzare le connessioni

È possibile usare tag per identificare e filtrare i server correlati nell'elenco di connessione.  In questo modo è possibile visualizzare un subset dei server nell'elenco delle connessioni.  Ciò è particolarmente utile se si dispone di un numero di connessioni.

### <a name="edit-tags"></a>Modifica tag

* Selezionare una o più server nell'elenco tutte le connessioni
* Sotto **tutte le connessioni**, fare clic su **Modifica tag**

![](../media/launch/tags-5.png)

Il **Modifica tag connessione** riquadro consente di modificare, aggiungere o rimuovere i tag dalle connessioni selezionate:

* Per aggiungere un nuovo tag per le connessioni selezionate, selezionare **aggiungere tag** e immettere il nome del tag da usare.

* Per applicare un tag di connessioni selezionate con un nome di tag esistente, selezionare la casella accanto al nome del tag da applicare.

* Per rimuovere un tag da connessioni tutti selezionate, deselezionare la casella accanto al tag a cui che si vuole rimuovere.

* Se un tag viene applicato a un subset delle connessioni selezionate, la casella di controllo viene visualizzata in uno stato intermedio. È possibile selezionare la casella per archiviarlo e si applicano al tag a tutte le connessioni selezionate oppure fare clic su Nuovo per disattivare la funzionalità e rimuovere il tag da tutte le connessioni selezionate.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrare le connessioni in base al tag

Dopo aver aggiunto i tag per una o più connessioni server, è possibile visualizzare i tag nell'elenco di connessione e filtrare l'elenco di connessioni per tag.

* Per filtrare in base a un tag, selezionare l'icona del filtro accanto alla casella di ricerca.
![](../media/launch/tags-7.png)
* È possibile selezionare "or", "e" o "No" per modificare il comportamento del filtro dei tag selezionati.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usare PowerShell per importare o esportare le connessioni (con tag)

> Si applica a: Anteprima di Windows Admin Center

Anteprima di Windows Admin Center include un modulo di PowerShell per importare o esportare l'elenco delle connessioni.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>Formato di file CSV per l'importazione di connessioni

Il formato del file CSV viene avviata con le intestazioni di quattro ```"name","type","tags","groupId"```, seguito da ogni connessione in una nuova riga.

**nome** è il FQDN della connessione

**tipo** è il tipo di connessione. Per le connessioni predefinite incluse in Windows Admin Center, si userà una delle operazioni seguenti:

| Tipo di connessione | Stringa di connessione |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| Windows 10 PC | msft.sme.connection-type.windows-client |
| Cluster di failover | msft.sme.connection-type.cluster |
| Cluster Iperconvergente | msft.sme.connection-type.hyper-converged-cluster |

**i tag** sono separate da pipe.

**groupId** viene usato per le connessioni condivise. Usare il valore ```global``` in questa colonna per rendere questa una connessione condivisa.

### <a name="example-csv-file-for-importing-connections"></a>File CSV di esempio per l'importazione di connessioni

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importazione RDCman connessioni

Usare lo script seguente per esportare le connessioni salvate in [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) in un file. È quindi possibile importare il file in Windows Admin Center, mantenendo la gerarchia di raggruppamento RDCMan usando i tag. Provarlo

1. Copiare e incollare il codice seguente nella sessione di PowerShell:

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. Per creare una. File CSV, eseguire il comando seguente:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importare l'oggetto risultante. File CSV in Windows Admin Center e tutti la gerarchia di raggruppamento RDCMan verrà rappresentato dai tag nell'elenco delle connessioni. Per informazioni dettagliate, vedere [usare PowerShell per importare o esportare le connessioni (con tag)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Visualizzare gli script di PowerShell usati in Windows Admin Center

Dopo la connessione a un server, cluster o PC, è possibile esaminare gli script di PowerShell che power le azioni dell'interfaccia utente disponibili in Windows Admin Center. Dall'interno di uno strumento, fare clic sull'icona di PowerShell sulla barra delle applicazioni principali. Selezionare un comando di interesse dall'elenco a discesa per passare allo script di PowerShell corrispondenti.

![](../media/launch/showscript.png)