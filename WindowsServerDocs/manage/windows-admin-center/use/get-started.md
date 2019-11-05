---
title: Introduzione all'interfaccia di amministrazione di Windows
description: Introduzione all'interfaccia di amministrazione di Windows
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: fac17cd5975eeb699f205888edbe3f1c30b43394
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73567144"
---
# <a name="get-started-with-windows-admin-center"></a>Introduzione all'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centro di amministrazione di Windows installato in Windows 10

> [!IMPORTANT]
> Per usare l'interfaccia di amministrazione di Windows in Windows 10, è necessario essere un membro del gruppo dell'amministratore locale

### <a name="selecting-a-client-certificate"></a>Selezione di un certificato client

La prima volta che si apre l'interfaccia di amministrazione di Windows in Windows 10, assicurarsi di selezionare il certificato *client di Windows Admin Center* . in caso contrario, verrà visualizzato un errore HTTP 403 che indica che "non è possibile accedere a questa pagina".

In Microsoft Edge, quando viene visualizzata la finestra di dialogo:
 
1. Fare clic su **altre opzioni**

    ![](../media/launch-cert-1.png)

2. Selezionare il certificato con etichetta **Windows Admin Center client** e fare clic su **OK** .

    ![](../media/launch-cert-2.png)

3. Assicurarsi che **l'opzione Consenti sempre l'accesso** sia selezionata e fare clic su **Consenti**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Connessione a nodi e cluster gestiti

Dopo aver completato l'installazione dell'interfaccia di amministrazione di Windows, è possibile aggiungere i server o i cluster da gestire dalla pagina Panoramica principale.

 **Aggiungere un singolo server o un cluster come nodo gestito**

1. Fare clic su **+ Aggiungi** in **tutte le connessioni**.

   ![](../media/launch/addserver0.png)

2. Scegliere di aggiungere un server, un cluster, un PC Windows o una macchina virtuale di Azure:
    
   ![](../media/launch/ChooseConnectionType.png)

3. Digitare il nome del server o del cluster da gestire e fare clic su **Invia**. Il server o il cluster verrà aggiunto all'elenco di connessioni nella pagina panoramica.

   ![](../media/launch/addserver2.png)

   **--O--**

**Importazione bulk di più server**

 1. Nella pagina **Aggiungi connessione al server** scegliere la scheda **Importa Server** .

    ![](../media/launch/import-servers.png)

 2. Fare clic su **Sfoglia** e selezionare un file di testo contenente una virgola o una nuova riga separata, un elenco di nomi di dominio completi per i server che si desidera aggiungere.

> [!Note]
> Il file con estensione CSV creato [esportando le connessioni con PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contiene informazioni aggiuntive oltre ai nomi dei server e non è compatibile con questo metodo di importazione.

  **--O--**

**Aggiungere server cercando Active Directory**

 1. Nella pagina **Aggiungi connessione al server** scegliere la scheda **Cerca Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Immettere i criteri di ricerca e fare clic su **Cerca**. Sono supportati i caratteri jolly (*).

 3. Al termine della ricerca, selezionare uno o più dei risultati, aggiungere facoltativamente i tag, quindi fare clic su **Aggiungi**.

## <a name="authenticate-with-the-managed-node"></a>Eseguire l'autenticazione con il nodo gestito ##

L'interfaccia di amministrazione di Windows supporta diversi meccanismi per l'autenticazione con un nodo gestito. Il valore predefinito è Single Sign-on.

**Single Sign-on**

È possibile usare le credenziali di Windows correnti per l'autenticazione con il nodo gestito. Si tratta dell'impostazione predefinita e l'interfaccia di amministrazione di Windows tenta di accedere quando si aggiunge un server. 

**Single Sign-on quando distribuito come servizio in Windows Server**

Se l'interfaccia di amministrazione di Windows è stata installata in Windows Server, è necessaria una configurazione aggiuntiva per Single Sign-On.  [Configurare l'ambiente per la delega](../configure/user-access-control.md)

**--O--**

**Usare *Gestisci come* per specificare le credenziali**

In **tutte le connessioni**selezionare un server dall'elenco e scegliere **Gestisci come** per specificare le credenziali che si utilizzeranno per l'autenticazione nel nodo gestito:

![](../media/launch-use-6.png)

Se l'interfaccia di amministrazione di Windows è in esecuzione in modalità servizio in Windows Server, ma non è configurata la delega Kerberos, è necessario immettere nuovamente le credenziali di Windows:

![](../media/launch-use-7.png)

È possibile applicare le credenziali a tutte le connessioni, che verranno memorizzate nella cache per la sessione del browser specifica. Se si ricarica il browser, è necessario immettere nuovamente le credenziali **di gestione** .

**Soluzione password amministratore locale (giri)**

Se l'ambiente USA i [giri](https://technet.microsoft.com/mt227395.aspx)e l'interfaccia di amministrazione di Windows è installata nel PC Windows 10, è possibile usare le credenziali per l'autenticazione con il nodo gestito. **Se si usa questo scenario,** [fornire commenti e suggerimenti](https://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Uso dei tag per organizzare le connessioni

È possibile usare i tag per identificare e filtrare i server correlati nell'elenco di connessioni.  In questo modo è possibile visualizzare un subset di server nell'elenco connessione.  Questa operazione è particolarmente utile se si dispone di molte connessioni.

### <a name="edit-tags"></a>Modifica tag

* Selezionare un server o più server nell'elenco tutte le connessioni
* In **tutte le connessioni**fare clic su **Modifica tag**

![](../media/launch/tags-5.png)

Il riquadro **Modifica tag di connessione** consente di modificare, aggiungere o rimuovere tag dalle connessioni selezionate:

* Per aggiungere un nuovo tag alle connessioni selezionate, selezionare **Aggiungi tag** e immettere il nome del tag che si vuole usare.

* Per contrassegnare le connessioni selezionate con un nome di tag esistente, selezionare la casella accanto al nome del tag che si desidera applicare.

* Per rimuovere un tag da tutte le connessioni selezionate, deselezionare la casella accanto al tag che si vuole rimuovere.

* Se viene applicato un tag a un subset delle connessioni selezionate, la casella di controllo viene visualizzata in uno stato intermedio. È possibile fare clic sulla casella per verificarla e applicare il tag a tutte le connessioni selezionate, oppure fare di nuovo clic per deselezionarlo e rimuovere il tag da tutte le connessioni selezionate.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtra connessioni per tag

Una volta aggiunti i tag a una o più connessioni al server, è possibile visualizzare i tag nell'elenco di connessioni e filtrare l'elenco di connessioni in base ai tag.

* Per filtrare in base a un tag, selezionare l'icona del filtro accanto alla casella di ricerca.
![](../media/launch/tags-7.png)
* È possibile selezionare "or", "e" o "not" per modificare il comportamento del filtro dei tag selezionati.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usare PowerShell per importare o esportare le connessioni (con tag)

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

Il formato del file CSV inizia con le quattro intestazioni ```"name","type","tags","groupId"```, seguite da ogni connessione su una nuova riga.

**nome** è il nome di dominio completo (FQDN) della connessione

il **tipo è il tipo di** connessione. Per le connessioni predefinite incluse nell'interfaccia di amministrazione di Windows, si userà uno dei seguenti elementi:

| Tipo di connessione | Stringa di connessione |
|------|-------------------------------|
| Windows Server | MSFT. SME. Connection-Type. Server |
| PC Windows 10 | MSFT. SME. Connection-Type. Windows-client |
| Cluster di failover | MSFT. SME. Connection-Type. cluster |
| Cluster iperconvergente | MSFT. SME. Connection-Type. iperconvergente-cluster |

i **tag** sono delimitati da pipe.

**GroupID** viene usato per le connessioni condivise. Usare il valore ```global``` in questa colonna per creare una connessione condivisa.

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

## <a name="import-rdcman-connections"></a>Importa connessioni RDCman

Usare lo script seguente per esportare le connessioni salvate in [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) in un file. È quindi possibile importare il file nell'interfaccia di amministrazione di Windows, mantenendo la gerarchia di raggruppamento RDCMan usando i tag. Provalo!

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

2. Per creare un oggetto. File CSV, eseguire il comando seguente:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importare l'oggetto risultante. File CSV nell'interfaccia di amministrazione di Windows e tutte le gerarchie di raggruppamento di RDCMan verranno rappresentate dai tag nell'elenco di connessioni. Per informazioni dettagliate, vedere [usare PowerShell per importare o esportare le connessioni (con tag)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Visualizzare gli script di PowerShell usati nell'interfaccia di amministrazione di Windows

Una volta stabilita la connessione a un server, un cluster o un PC, è possibile esaminare gli script di PowerShell che consentono di potenziare le azioni dell'interfaccia utente disponibili nell'interfaccia di amministrazione di Windows. Dall'interno di uno strumento, fare clic sull'icona di PowerShell nella barra delle applicazioni superiore. Selezionare un comando di interesse nell'elenco a discesa per passare allo script di PowerShell corrispondente.

![](../media/launch/showscript.png)
