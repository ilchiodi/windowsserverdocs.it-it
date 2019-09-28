---
title: Eseguire l'aggiornamento, il backup e il ripristino dell'infrastruttura SDN
description: In questo argomento viene illustrato come aggiornare, eseguire il backup e il ripristino di un'infrastruttura SDN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7f385e094ca70027d1b036bf53af23c1fc4a1bd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406053"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Eseguire l'aggiornamento, il backup e il ripristino dell'infrastruttura SDN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrato come aggiornare, eseguire il backup e il ripristino di un'infrastruttura SDN. 

## <a name="upgrade-the-sdn-infrastructure"></a>Aggiornare l'infrastruttura SDN
L'infrastruttura SDN può essere aggiornata da Windows Server 2016 a Windows Server 2019. Per l'ordine di aggiornamento, seguire la stessa sequenza di passaggi come indicato nella sezione "aggiornare l'infrastruttura SDN". Prima dell'aggiornamento, è consigliabile eseguire un backup del database del controller di rete.

Per i computer del controller di rete, usare il cmdlet Get-NetworkControllerNode per controllare lo stato del nodo al termine dell'aggiornamento. Verificare che il nodo torni allo stato "attivo" prima di aggiornare gli altri nodi. Dopo aver aggiornato tutti i nodi del controller di rete, il controller di rete aggiorna i microservizi in esecuzione all'interno del cluster di controller di rete entro un'ora. È possibile attivare un aggiornamento immediato usando il cmdlet Update-networkcontroller. 

Installare gli stessi aggiornamenti di Windows in tutti i componenti del sistema operativo del sistema SDN (Software Defined Networking), che include:

- Host Hyper-V abilitati per SDN
- Macchine virtuali del controller di rete
- Software Load Balancer macchine virtuali Mux
- VM del gateway RAS 

>[!IMPORTANT]
>Se si usa System Center Virtual Manager, è necessario aggiornarlo con gli aggiornamenti cumulativi più recenti.

Quando si aggiorna ogni componente, è possibile utilizzare uno dei metodi standard per l'installazione degli aggiornamenti di Windows. Tuttavia, per garantire un tempo di inattività minimo per i carichi di lavoro e l'integrità del database del controller di rete, attenersi alla procedura seguente:

1. Aggiornare le console di gestione.<p>Installare gli aggiornamenti in ognuno dei computer in cui si usa il modulo PowerShell del controller di rete.  Anche in qualsiasi punto in cui sia installato il ruolo di strumenti di amministrazione remota del server (NetworkController). Esclusione delle VM del controller di rete. per aggiornarli, procedere nel passaggio successivo.

2. Nella prima VM del controller di rete, installare tutti gli aggiornamenti e riavviare.

3. Prima di procedere alla successiva macchina virtuale del controller di rete, `get-networkcontrollernode` usare il cmdlet per controllare lo stato del nodo che è stato aggiornato e riavviato.

4. Durante il ciclo di riavvio, attendere il completamento del nodo del controller di rete, quindi tornare indietro.<p>Dopo il riavvio della macchina virtuale, potrebbero essere necessari alcuni minuti prima di tornare allo stato **_attivo_** . Per un esempio dell'output, vedere 

5. Installare gli aggiornamenti in ogni VM SLB Mux uno alla volta per garantire la disponibilità continua dell'infrastruttura del servizio di bilanciamento del carico.

6. Aggiornare gli host Hyper-V e i gateway RAS, a partire dagli host che contengono i gateway RAS in modalità **standby** .<p>Non è possibile eseguire la migrazione delle VM del gateway RAS in tempo reale senza perdere le connessioni dei tenant. Durante il ciclo di aggiornamento, è necessario prestare attenzione a ridurre al minimo il numero di volte in cui le connessioni tenant si failover su un nuovo gateway RAS. Con il coordinamento dell'aggiornamento degli host e dei gateway RAS, al massimo viene eseguito il failover di ogni tenant.

    a. Evacuare l'host di macchine virtuali in grado di supportare la migrazione in tempo reale.<p>Le macchine virtuali del gateway RAS devono rimanere nell'host.

    b. Installare gli aggiornamenti in ogni macchina virtuale del gateway in questo host.

    c. Se l'aggiornamento richiede il riavvio della macchina virtuale del gateway, riavviare la macchina virtuale.  

    d. Installare gli aggiornamenti nell'host che contiene la macchina virtuale del gateway appena aggiornata.

    e. Riavviare l'host se richiesto dagli aggiornamenti.

    f. Ripetere per ogni host aggiuntivo contenente un gateway di standby.<p>Se non restano gateway di standby, seguire la stessa procedura per tutti gli host rimanenti.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Esempio: Usare il cmdlet Get-networkcontrollernode 

In questo esempio viene visualizzato l'output per il `get-networkcontrollernode` cmdlet eseguito dall'interno di una delle macchine virtuali del controller di rete.  

Lo stato dei nodi visualizzati nell'output di esempio è:

- NCNode1.contoso.com = inattivo
- NCNode2.contoso.com = up
- NCNode3.contoso.com = up

>[!IMPORTANT]
>È necessario attendere alcuni minuti fino a quando lo stato del nodo diventa _**attivo**_ prima di aggiornare eventuali nodi aggiuntivi, uno alla volta.

Dopo aver aggiornato tutti i nodi del controller di rete, il controller di rete aggiorna i microservizi in esecuzione all'interno del cluster di controller di rete entro un'ora. 

>[!TIP]
>È possibile attivare un aggiornamento immediato utilizzando il `update-networkcontroller` cmdlet.


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Esempio: Usare il cmdlet Update-networkcontroller
In questo esempio viene visualizzato l'output del cmdlet per `update-networkcontroller` forzare l'aggiornamento del controller di rete. 

>[!IMPORTANT]
>Eseguire questo cmdlet quando non sono disponibili altri aggiornamenti da installare.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Eseguire il backup dell'infrastruttura SDN

I backup regolari del database del controller di rete garantiscono la continuità aziendale in caso di emergenza o perdita di dati.  Il backup delle VM del controller di rete non è sufficiente perché non garantisce che la sessione continui tra più nodi del controller di rete.

**Requisiti**
* Condivisione e credenziali SMB con autorizzazioni di lettura/scrittura per la condivisione e la file system.
* Facoltativamente, è possibile usare un account del servizio gestito del gruppo (GMSA) se il controller di rete è stato installato anche con una GMSA.

**Procedura**

1. Usare il metodo di backup della macchina virtuale preferito oppure usare Hyper-V per esportare una copia di ogni macchina virtuale del controller di rete.<p>Il backup della macchina virtuale del controller di rete garantisce che siano presenti i certificati necessari per la decrittografia del database.  

2. Se si usa System Center Virtual Machine Manager (SCVMM), arrestare il servizio SCVMM ed eseguirne il backup tramite SQL Server.<p>L'obiettivo è quello di garantire che non venga eseguito alcun aggiornamento a SCVMM durante questo periodo di tempo, il che potrebbe creare un'incoerenza tra il backup del controller di rete e SCVMM.  

   >[!IMPORTANT]
   >Non riavviare il servizio SCVMM fino al completamento del backup del controller di rete.

3. Eseguire il backup del database del controller `new-networkcontrollerbackup` di rete con il cmdlet.

4. Verificare il completamento e l'esito positivo del backup `get-networkcontrollerbackup` con il cmdlet.

5. Se si usa SCVMM, avviare il servizio SCVMM.



### <a name="example-backing-up-the-network-controller-database"></a>Esempio: Backup del database del controller di rete

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Esempio: Verifica dello stato di un'operazione di backup del controller di rete

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Ripristinare l'infrastruttura SDN da un backup

Quando si ripristinano tutti i componenti necessari dal backup, l'ambiente SDN torna a uno stato operativo.  

>[!IMPORTANT]
>La procedura varia a seconda del numero di componenti ripristinati.


1. Se necessario, ridistribuire gli host Hyper-V e lo spazio di archiviazione necessario.

2. Se necessario, ripristinare le macchine virtuali del controller di rete, le VM del gateway RAS e le macchine virtuali Mux dal backup. 

3. Arrestare l'agente host NC e l'agente host SLB in tutti gli host Hyper-V:

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Arrestare le VM del gateway RAS.

5. Arrestare SLB Mux VM.

6. Ripristinare il controller di rete con `new-networkcontrollerrestore` il cmdlet.

7. Controllare il **ProvisioningState** di ripristino per capire quando il ripristino è stato completato correttamente.

8. Se si usa SCVMM, ripristinare il database SCVMM usando il backup creato allo stesso tempo del backup del controller di rete.

9. Se si desidera ripristinare le macchine virtuali del carico di lavoro dal backup, procedere ora.

10. Verificare l'integrità del sistema con il cmdlet debug-networkcontrollerconfigurationstate.

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>Esempio: Ripristino di un database del controller di rete
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Esempio: Verifica dello stato di un ripristino del database del controller di rete

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


Per informazioni sui messaggi di stato di configurazione che possono essere visualizzati, vedere [risolvere i problemi relativi allo stack di Software Defined Networking di Windows Server 2016](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).