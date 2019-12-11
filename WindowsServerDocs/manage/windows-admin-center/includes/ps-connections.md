```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to a .csv file
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from a .csv file
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files, and remove any connections that are not explictly in the imported file using the -prune switch parameter 
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv" -prune
```
### <a name="csv-file-format-for-importing-connections"></a>Formato file CSV per l'importazione di connessioni

Il formato del file CSV inizia con le quattro intestazioni ```"name","type","tags","groupId"```, seguite da ciascuna connessione in una nuova riga.

**name** è il nome di dominio completo della connessione.

**type** è il tipo di connessione. Per le connessioni predefinite incluse in Windows Admin Center userai uno degli elementi seguenti:

| Tipo di connessione | Stringa di connessione |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| PC Windows 10 | msft.sme.connection-type.windows-client |
| Cluster di failover | msft.sme.connection-type.cluster |
| Cluster iperconvergente | msft.sme.connection-type.hyper-converged-cluster |

**tags** viene usato per elementi separati da pipe.

**groupId** viene usato per le connessioni condivise. Usa il valore ```global``` in questa colonna per rendere la connessione condivisa.

> [!NOTE]
> La modifica delle connessioni condivise spetta solo agli amministratori del gateway. Tutti gli utenti possono usare PowerShell per modificare l'elenco di connessioni personale.

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

## <a name="import-rdcman-connections"></a>Importazione di connessioni di RDCman

Usa lo script seguente per esportare le connessioni salvate in [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) in un file. Puoi quindi importare il file in Windows Admin Center, mantenendo la gerarchia di raggruppamento di RDCMan con l'uso di tag. Prova.

1. Copia e incolla il codice seguente nella sessione PowerShell:

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

2. Per creare un file CSV, esegui il comando seguente:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importa il file CSV risultante in Windows Admin Center e nell'elenco delle connessioni tutta la gerarchia di raggruppamento di RDCMan sarà rappresentata da tag. Per informazioni dettagliate, vedi [Usare PowerShell per importare o esportare le connessioni (con tag)](#use-powershell-to-import-or-export-your-connections-with-tags).