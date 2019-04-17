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
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296653"
---
# Per iniziare con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## Windows Admin Center installato in Windows 10

> [!IMPORTANT]
> È necessario essere un membro del gruppo degli amministratori locali per utilizzare Windows Admin Center in Windows 10

### Selezione di un certificato client

La prima volta che Apri Windows Admin Center in Windows 10, assicurati di selezionare il certificato *Client di Windows Admin Center* (in caso contrario, riceverai un errore HTTP 403 che indica "non è possibile accedere a questa pagina").

In Microsoft Edge, quando ti viene chiesto con questa finestra di dialogo:
 
1. Fai clic su **altre scelte**

    ![](../media/launch-cert-1.png)

2. Seleziona il certificato con etichettato **Client di Windows Admin Center** e fai clic su **OK**

    ![](../media/launch-cert-2.png)

3. Verifica che sia selezionato **Sempre consentire l'accesso** e fai clic su **Consenti**

    ![](../media/launch-cert-3.png)

## Connessione ai nodi gestiti e cluster

Dopo aver completato l'installazione di Windows Admin Center, puoi aggiungere server o cluster per gestire dalla pagina della panoramica principale.

 **Aggiungere un unico server o un cluster come nodo gestito**

 1. Fare clic su **+ Aggiungi** in **tutte le connessioni**.

    ![](../media/launch/addserver0.png)

 2. Scegliere di aggiungere una connessione Server, Cluster di Failover o Cluster iperconvergente:
    
    ![](../media/launch/addserver1.png)

 3. Digita il nome del server o cluster per gestire e fai clic su **Invia**. Il server o cluster verrà aggiunto all'elenco di connessione nella pagina della panoramica.

    ![](../media/launch/addserver2.png)

   **-OPPURE-**

**Più server di importazione in blocco**

 1. Nella pagina **Aggiungi connessione al Server** , scegliere la scheda **Server di importazione** .

    ![](../media/launch/import-servers.png)

 2. Fai clic su **Sfoglia** e selezionare un file di testo che contiene una virgola o nuova riga separati, elenco di nomi FQDN per il server da aggiungere.

    **-OPPURE-**

**Aggiungere i server eseguendo una ricerca di Active Directory**

 1. Nella pagina **Aggiungi connessione al Server** , scegliere la scheda di **Ricerca in Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Immetti i criteri di ricerca e fai clic su **ricerca**. Sono supportati i caratteri jolly (*).

 3. Al termine della ricerca - seleziona uno o più dei risultati, facoltativamente, aggiungendo tag e fai clic su **Aggiungi**.

## Eseguire l'autenticazione con il nodo gestito ##

Windows Admin Center supporta diversi meccanismi per l'autenticazione con un nodo gestito. Accesso Single sign-on è il valore predefinito.

**Single Sign-on**

È possibile utilizzare le credenziali di Windows corrente per l'autenticazione con il nodo gestito. Questo è il valore predefinito e tenta di Windows Admin Center sign-on quando si aggiunge un server. 

**Accesso Single sign-on quando distribuito come servizio in Windows Server**

Se hai installato Windows Admin Center in Windows Server, è necessaria single sign-on di configurazione aggiuntive.  [Configurare l'ambiente per la delega](..\configure\user-access-control.md)

**-OPPURE-**

**Usa *l'Account di gestione* per specificare le credenziali**

In **Tutte le connessioni**, selezionare un server dall'elenco e scegli **Account di gestione** per specificare le credenziali che usi per l'autenticazione al nodo gestito:

![](../media/launch-use-6.png)

Se Windows Admin Center è in esecuzione in modalità di servizio in Windows Server, ma non hai configurata la delega Kerberos, è necessario immettere nuovamente le credenziali di Windows:

![](../media/launch-use-7.png)

È possibile applicare le credenziali di tutte le connessioni, che li memorizzerà per tale sessione browser specifico. Se si ricarica il browser, è necessario immettere nuovamente le credenziali **dell'Account di gestione** .

**Soluzione Password dell'amministratore locale (LAPS)**

Se l'ambiente utilizza [LAPS](https://technet.microsoft.com/mt227395.aspx), è possibile utilizzare le credenziali LAPS per l'autenticazione con il nodo gestito. **Se usi questo scenario, tieni** [fornire feedback](http://aka.ms/WACFeedback).

## Utilizzo di tag per organizzare le connessioni

È possibile utilizzare i tag per identificare e filtrare server correlati nell'elenco di connessione.  Ciò consente di vedere un sottoinsieme dei server nell'elenco di connessione.  Ciò è particolarmente utile se hai molti connessioni.

### Modificare i tag

* Seleziona una o più server nell'elenco tutte le connessioni
* In **Tutte le connessioni**, fai clic su **Modifica tag**

![](../media/launch/tags-5.png)

Il riquadro di **Modificare i tag di connessione** consente di modificare, aggiungere o rimuovere i tag dal tuo connessioni selezionate:

* Per aggiungere un nuovo tag per le connessioni selezionate, seleziona **Aggiungi tag** e immetti il nome del tag che vuoi usare.

* Per contrassegnare le connessioni selezionate con un nome di tag esistente, seleziona la casella accanto al nome di tag che si desidera applicare.

* Per rimuovere un tag da connessioni tutti selezionate, deseleziona la casella accanto al tag che si desidera rimuovere.

* Se un tag viene applicato a un sottoinsieme delle connessioni selezionate, la casella di controllo viene visualizzata in uno stato intermedio. Fare clic sulla casella per controllare e il tag si applicano a tutte le connessioni selezionate o fare nuovamente clic per deselezionarla e rimuovere il tag da tutte le connessioni selezionate.

![](../media/launch/tags-6.png)

### Filtrare le connessioni tag

Dopo aver aggiunto i tag per le connessioni server di uno o più, puoi visualizzare i tag presenti nell'elenco di connessione e filtrare l'elenco di connessione per i tag.

* Per filtrare in base a un tag, seleziona l'icona di filtro accanto alla casella di ricerca.
![](../media/launch/tags-7.png)
* È possibile selezionare "o", "e" o "non" per modificare il comportamento di filtro dei tag di selezionato.
![](../media/launch/tags-8.png)

## Utilizzare PowerShell per l'importazione o esportazione le connessioni (con i tag)

> Si applica a: Windows Admin Center Preview

Windows Admin Center Preview include un modulo di PowerShell per importare o esportare l'elenco di connessione.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### Formato del file CSV per l'importazione delle connessioni

Il formato del file CSV inizia con le intestazioni di quattro ```"name","type","tags","groupId"```, seguito da ogni connessione una nuova riga.

**nome** è il nome FQDN della connessione

**è il tipo di connessione.** Per le connessioni predefinito incluse con Windows Admin Center, userai uno dei seguenti:

| Tipo di connessione | Stringa di connessione |
|------|-------------------------------|
| Windows Server | msft.SME.Connection type.server |
| PC Windows 10 | msft.SME.Connection-type.windows-client |
| Cluster di failover | msft.SME.Connection type.cluster |
| Cluster iper-convergenti | msft.SME.Connection-type.hyper--cluster iperconvergenti |

**i tag** sono delimitato da barre verticali.

**groupId** viene usato per le connessioni condivise. Usa il valore ```global``` in questa colonna per rendere il una connessione condivisa.

### File CSV di esempio per l'importazione di connessioni

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## Importare le connessioni RDCman

Usa lo script seguente per esportare connessioni salvate in [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) in un file. Puoi quindi importare il file in Windows Admin Center, mantenendo la gerarchia di raggruppamento RDCMan tramite i tag. Provalo!

1. Copia e Incolla il codice riportato di seguito nella sessione di PowerShell:

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

2. Per creare un. File CSV, Esegui il comando seguente:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importa risultante. File CSV in Windows Admin Center e tutte le la gerarchia di raggruppamento RDCMan sarà rappresentato da tag nell'elenco di connessione. Per ulteriori informazioni, vedere [Utilizzare PowerShell per l'importazione o esportazione le connessioni (con i tag)](#use-powershell-to-import-or-export-your-connections-with-tags).

## Visualizzare gli script di PowerShell usati in Windows Admin Center

Dopo essersi connessi a un server, cluster o PC, esaminare gli script di PowerShell energia che le azioni dell'interfaccia utente disponibili in Windows Admin Center. All'interno di uno strumento, fai clic sull'icona PowerShell sulla barra delle applicazioni superiore. Selezionare un comando di interesse dall'elenco a discesa per passa allo script di PowerShell corrispondenti.

![](../media/launch/showscript.png)