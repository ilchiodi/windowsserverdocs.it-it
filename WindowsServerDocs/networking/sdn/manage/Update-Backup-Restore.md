---
title: Aggiornamento, Backup e ripristino Software definito dell'infrastruttura di rete
description: In questo argomento fa parte di rete definita dal Software alla Guida di sull'infrastruttura di aggiornamento, Backup e ripristino SDN in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>Aggiornamento, Backup e ripristino Software definito dell'infrastruttura di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento contiene le sezioni seguenti.

- [Aggiornare l'infrastruttura SDN](#bkmk_Updating)
- [Eseguire il backup dell'infrastruttura SDN](#bkmk_backup)
- [Ripristinare l'infrastruttura SDN da un backup](#bkmk_restore)

## <a name="bkmk_Updating"></a>Aggiornare l'infrastruttura SDN

L'aggiornamento è il processo di installazione degli aggiornamenti di Windows in tutti i componenti del sistema operativo del sistema di rete SDN (Software Defined).  Ciò include la SDN host Hyper-V, macchine virtuali di Controller di rete, le macchine virtuali Mux servizio di bilanciamento del carico Software e le macchine virtuali Gateway RAS abilitato.  È fondamentale che tutti questi componenti hanno esattamente lo stesso gruppo di aggiornamenti installati.  Se si utilizza System Center Virtual Machine Manager è inoltre consigliabile che è anche aggiornarlo con le più recenti aggiornamenti cumulativi anche.

L'aggiornamento di ogni componente viene eseguita utilizzando uno dei metodi standard per l'installazione degli aggiornamenti di windows, tuttavia la procedura descritta di seguito deve essere seguita per assicurare tempi per i carichi di lavoro di inattività minimo e per garantire l'integrità del database del Controller di rete.

### <a name="step-1-update-the-management-consoles"></a>Passaggio 1: Aggiornare le console di gestione
Installare gli aggiornamenti necessari in ogni computer in cui usi il modulo Powershell di Controller di rete.  Sono inclusi in qualsiasi punto che si è installato il ruolo NetworkController amministrazione remota del server da solo.  Questa operazione non include macchine virtuali Controller di rete man mano che verranno aggiornate nel passaggio 2.

### <a name="step-2-update-the-network-controllers"></a>Passaggio 2: Aggiornare i controller di rete
Questo è il passaggio più importante nel ciclo di aggiornamento, poiché ogni macchina virtuale Controller di rete deve essere aggiornato ed essere online eseguire un backup completo del cluster di Controller di rete prima di procedere a quello successivo.

Iniziare con una macchina virtuale Controller di rete e installare tutti gli aggiornamenti necessari.  Se necessario, riavviare la macchina virtuale.

Prima di procedere al successivo macchina virtuale Controller di rete usare get-networkcontrollernode per controllare lo stato del nodo che è stato aggiornato e riavviato.  Attendere che il nodo di Controller di rete per interruzioni durante il ciclo di riavviare il computer e quindi ritornino operativi.  Dopo il riavvio della macchina virtuale, comunque può richiedere alcuni minuti per poter tornare allo stato di backup.

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>Esempio: Utilizzo di get-networkcontrollernode per controllare lo stato dei nodi di Controller di rete

Questo esempio mostra l'output dell'esecuzione di get-networkcontrollernode all'interno di una delle macchine virtuali Controller di rete.  Mostra che NCNode1.contoso.com è verso il basso mentre gli altri due nodi siano integri.  È necessario attendere per diversi minuti finché lo stato di tale nodo diventa backup prima di procedere con l'aggiornamento di eventuali altri nodi.

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

Solo dopo che tutti i nodi di Controller di rete sono in stato di backup è possibile ripetere questi passaggi per ogni nodo di Controller di rete aggiuntivo.  Continuare ad aggiornare ogni nodo uno alla volta.

Una volta vengono aggiornati tutti i nodi di Controller di rete, il Controller di rete verrà aggiornato il microservizi in esecuzione all'interno del cluster di Controller di rete all'interno di un'ora.  È possibile attivare un aggiornamento immediato tramite il cmdlet update networkcontroller.

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>Esempio: Utilizzo di aggiornamento networkcontroller per forzare il Controller di rete per l'aggiornamento

Questo comando Visualizza il risultato di networkcontroller update quando non sono disponibili aggiornamenti rimanenti per essere installato.

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>Passaggio 3: Aggiornamento SLB Muxes

Installare gli aggiornamenti in ogni macchina virtuale Mux SLB uno alla volta per garantire la disponibilità continua dell'infrastruttura del servizio di bilanciamento del carico.

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>Passaggio 4: Aggiornamento host di Hyper-V e gateway RAS

Perché non possono essere animate macchine virtuali Gateway RAS eseguita la migrazione senza perdere connessioni tenant, è necessario prestare attenzione per ridurre al minimo il numero di volte in cui tale tenant connessioni verranno eseguite il failover a un nuovo gateway RAS durante il ciclo di aggiornamento.  Per coordinare l'aggiornamento degli host e i gateway RAS ogni tenant verrà solo failover al massimo una volta.  

Per ogni host, a partire dagli host che contengono i gateway RAS in modalità Standby, segui questi passaggi:

1.  Spostare l'host di macchine virtuali che sono in grado di migrazione in tempo reale.  Macchine virtuali Gateway RAS deve rimanere nell'host.
2.  Installare gli aggiornamenti in ogni macchina virtuale Gateway in questo host.
3.  Se l'aggiornamento richiede il gateway VM per riavviare, quindi riavviare la macchina virtuale.  
4.  Installare gli aggiornamenti nell'host che contiene il gateway VM che è stato appena aggiornato.
5.  Riavviare l'host, se necessario, gli aggiornamenti.
6.  Ripetere per ogni host aggiuntivo che contiene un gateway standby.  Se nessun gateway standby rimane, attenersi alla seguente procedura stesso per tutti gli host rimanenti.

## <a name="bkmk_backup"></a>Eseguire il backup dell'infrastruttura SDN

Backup regolari del database del Controller di rete sono fondamentali per garantire la continuità aziendale in caso di un'emergenza o perdita di dati.  Eseguire il backup di macchine virtuali Controller di rete non è sufficiente perché non garantisce che quorum vengono mantenute tra più nodi di Controller di rete.
Requisiti:
 * Una condivisione SMB e le credenziali con autorizzazioni di lettura/scrittura per il sistema di condivisione e file.
 * Se il Controller di rete è stato installato utilizzando un anche gestito, è possibile utilizzare facoltativamente un gruppo Service Account gestito.

Segui questi passaggi per eseguire un backup:

1. Eseguire il backup di macchine virtuali Controller di rete utilizzando il metodo di backup di macchina virtuale di tua scelta, oppure utilizzare Hyper-V per esportare una copia di ogni macchina virtuale Controller di rete.  In questo modo se viene eseguita una ricompilazione completa, inclusi il ripristino dell'infrastruttura di macchine virtuali, sono presenti i certificati necessari per decrittografare il database.
2. Se si utilizza System Center Virtual Machine Manager (SCVMM), arrestare il servizio SCVMM e backup tramite SQL Server per assicurarsi che non vengono effettuati aggiornamenti a SCVMM durante questo periodo in cui è riuscito a creare un'incoerenza tra i backup di Controller di rete e SCVMM.  Non riavviare il servizio SCVMM fino a quando il Controller di rete di backup è completo.
3. Il backup del database del Controller di rete utilizzando nuovo networkcontrollerbackup.

 #### <a name="example-backing-up-the-network-controller-database"></a>Esempio: Backup del database di Controller di rete
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

4. Utilizzare get-networkcontrollerbackup per verificare la riuscita del backup e il completamento.

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Esempio: Verifica dello stato di un'operazione di backup del Controller di rete

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

5.  Se tramite SCVMM è ora possibile avviare servizio SCVMM.

## <a name="bkmk_restore"></a>Ripristinare l'infrastruttura SDN da un backup

Ripristino è il processo di ripristino di tutti i componenti necessari dal backup per restituire un ambiente SDN allo stato operativo.  I passaggi variano leggermente a seconda della quantità di componenti che si desidera ripristinare.

1. Se necessario, è possibile ridistribuire host Hyper-V e lo spazio di archiviazione necessario.

2. Se necessario, ripristinare le macchine virtuali Controller di rete, macchine virtuali Gateway RAS e le macchine virtuali Mux dal backup. 

3. Arrestare l'agente Host contesto dei nomi e l'agente Host SLB in tutti gli host Hyper-V

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Arrestare le macchine virtuali Gateway RAS

5. Arrestare le macchine virtuali Mux SLB

6. Ripristinare il Controller di rete utilizzando il cmdlet nuovo networkcontrollerrestore.

 #### <a name="example-restoring-a-network-controller-database"></a>Esempio: Ripristino di un database di Controller di rete
 
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

7. Controllare il ripristino ProvisioningState sapere quando il ripristino fosse completata correttamente.

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Esempio: Verifica dello stato di ripristino di un database di Controller di rete

    ```
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

8. Se si usa SCVMM, ripristinare il database SCVMM utilizzando il backup è stato creato nello stesso momento, come il backup di Controller di rete.

9. Se il carico di lavoro macchine virtuali viene ripristinato dal backup, è possibile farlo ora.

10. Usare il cmdlet debug-networkcontrollerconfigurationstate per verificare l'integrità del sistema.

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

Per informazioni sui messaggi di stato di configurazione che possono essere visualizzati, vedere [risoluzione dei problemi di Windows Server 2016 Software definito Stack di rete](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).