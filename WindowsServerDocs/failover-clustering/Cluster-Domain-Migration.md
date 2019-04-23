---
title: Migrazione di Cluster di dominio in Windows Server 2016/2019 tra
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Questo articolo descrive lo spostamento di un cluster di Windows Server 2019 da un dominio a altro
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875942"
---
# <a name="failover-cluster-domain-migration"></a>Migrazione del dominio Cluster di failover

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento fornisce che una panoramica per lo spostamento di Windows Server failover cluster da un dominio a altro.

## <a name="why-migrate-between-domains"></a>Il motivo per cui eseguire la migrazione tra domini

Esistono diversi scenari in cui è necessaria la migrazione di un cluster da uno doamin a altro.

- CompanyA unisce con società CompanyB ed è necessario spostare tutti i cluster nel dominio CompanyA
- I cluster sono compilati nel data center principale e spediti a sedi remote
- Cluster è stato creato come un cluster del gruppo di lavoro e a questo punto deve far parte di un dominio
- Cluster è stato creato come un cluster di dominio e ora deve essere parte di un gruppo di lavoro
- Cluster è stato spostato in un'area della società a un altro ed è un sottodominio diverso

Microsoft non fornisce supporto per gli amministratori che tenta di spostare le risorse da un dominio a un altro se l'operazione dell'applicazione sottostante non è supportata. Ad esempio, Microsoft non fornisce supporto per gli amministratori che tenta di spostare un server Microsoft Exchange da un dominio a altro.

   > [!WARNING]
   > Si consiglia di eseguire un backup completo di tutti gli archivi condivisi del cluster prima di spostare il cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 e versioni precedenti

In Windows Server 2016 e versioni precedenti, il servizio Cluster non dispone della funzionalità di spostamento da un dominio a un altro.  Ciò è dovuto alla maggiore dipendenza da Active Directory Domain Services e i nomi virtuali creati.   

## <a name="options"></a>Opzioni

Per ottenere tale cambiamento, sono disponibili due opzioni.

La prima opzione prevede l'eliminazione definitiva del cluster e la ricompilazione del nuovo dominio.

![Eliminare e ricompilare](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Come mostra l'animazione, questa opzione è distruttiva con i passaggi in corso:

1. Eliminare il Cluster.
2. Modificare l'appartenenza al dominio dei nodi nel nuovo dominio.
3. Ricreare il Cluster come nuovo nel dominio aggiornato.  Questo sarebbe necessario che sia necessario ricreare tutte le risorse.

La seconda opzione è meno distruttiva ma richiede hardware aggiuntivo perché sarebbe necessario un nuovo cluster da compilare nel nuovo dominio.  Una volta il cluster nel nuovo dominio, eseguire la migrazione guidata Cluster per eseguire la migrazione di risorse. Si noti che questo non eseguire la migrazione dei dati, è necessario usare un altro strumento per la migrazione dei dati, ad esempio [servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md)(dopo aver aggiunto il supporto di cluster).

![Compilare ed eseguire la migrazione](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Come mostra l'animazione, questa opzione non è distruttiva ma richiedono un hardware diverso o un nodo dal cluster esistente che è stato rimosso.

1. Creare un nuovo clusterin il nuovo dominio mantenendo al contempo il vecchio cluster disponibili.
2. Usare la [migrazione guidata Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) per eseguire la migrazione di tutte le risorse nel nuovo cluster. Promemoria, ciò non copia i dati, in modo che dovrà essere eseguito separatamente.
3. Rimuovere le autorizzazioni o Elimina il vecchio cluster.

In entrambe le opzioni, nel nuovo cluster dovrà avere tutti [compatibile con cluster applicazioni](https://technet.microsoft.com/aa369082(v=vs.90)) installato, i driver tutto aggiornato, e possibilmente test per assicurarsi che tutte verrà eseguito correttamente.  Si tratta di un processo molto tempo se i dati devono anche essere spostato.

## <a name="windows-server-2019"></a>Windows Server 2019

In Windows Server 2019, abbiamo introdotto funzionalità di migrazione tra cluster dominio.  A questo punto, possono essere eseguiti facilmente gli scenari elencati in precedenza e la necessità di ricompilazione non è più necessario.  

Lo spostamento di un cluster da un dominio è un processo molto semplice. A tale scopo, sono disponibili due nuovi cmdlet di PowerShell.

**Nuovo ClusterNameAccount** : crea un Account del nome Cluster in Active Directory **Remove-ClusterNameAccount** : rimuove l'account del nome Cluster da Active Directory

Il processo per eseguire questa operazione consiste nel modificare il cluster da un dominio a un gruppo di lavoro e tornare al nuovo dominio.  La necessità di eliminare un cluster, ricreare un cluster, installare le applicazioni e così via non è un requisito. Ad esempio, sarebbe simile al seguente:

![Eseguire la migrazione](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migrazione di un cluster a un nuovo dominio

In questa procedura, un cluster viene spostato dal dominio Contoso.com nel nuovo dominio Fabrikam.com.  È il nome del cluster *CLUSCLUS* e con un ruolo file server chiamato *FS CLUSCLUS*.

1. Creare un account amministratore locale con lo stesso nome e password in tutti i server del cluster.  Ciò potrebbe essere necessario effettuare l'accesso mentre il server sono lo spostamento tra domini.
2. Accedi al primo server con un account utente o un amministratore di dominio che dispone delle autorizzazioni di Active Directory per il Cluster nome oggetto (CNO), gli oggetti di Computer virtuali (VCO) ha accesso al Cluster e aprire PowerShell.
3. Assicurarsi che tutte le risorse nome rete del Cluster sono in una non in linea dello stato ed eseguire il comando seguente.  Questo comando rimuoverà gli oggetti di Active Directory che può avere il cluster.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Usare Active Directory Users and Computers per assicurarsi che il computer oggetto nome cluster e oggetto computer virtuale sono stati rimossi gli oggetti associati a tutti i nomi dei cluster.

   > [!NOTE]
   > È consigliabile arrestare il servizio Cluster in tutti i server del cluster e impostare il tipo di avvio su manuale in modo che il servizio Cluster non viene avviato quando si riavviano i server durante la modifica di domini.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modificare l'appartenenza al dominio dei server in un gruppo di lavoro, riavviare i server, aggiungere i server nel nuovo dominio e riavviare di nuovo.
6. Una volta che i server sono nel nuovo dominio, accedere a un server con un account utente o un amministratore di dominio che disponga delle autorizzazioni di Active Directory per creare oggetti, ha accesso al Cluster e aprire PowerShell. Avviare il servizio Cluster e reimpostarlo su automatico.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Portare il nome del Cluster e tutti gli altri cluster risorse nome di rete allo stato Online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modificare il cluster per far parte del nuovo dominio con gli oggetti di active directory associata. A tale scopo, il comando è di sotto e le risorse nome di rete devono essere in uno stato online.  Cosa farà questo comando è ricreare gli oggetti di nome in Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    NOTA: Se non hai eventuali gruppi aggiuntivi con nomi di rete (ad esempio un Cluster Hyper-V con solo le macchine virtuali), l'opzione di parametro - UpgradeVCOs non è necessaria.

9. Usare Active Directory Users and Computers per controllare il nuovo dominio e verificare che gli oggetti computer associati sono stati creati. Se si dispone, quindi connettere le risorse rimanenti nei gruppi online.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemi noti

Se si usa la nuova funzionalità di controllo del mirroring USB, non sarà possibile aggiungere il cluster nel nuovo dominio.  Il concetto di base è che il tipo di server di controllo di condivisione file deve utilizzare Kerberos per l'autenticazione.  Modificare il controllo in nessuno prima di aggiungere il cluster al dominio.  Al termine, ricreare il controllo del mirroring USB.  L'errore che verrà visualizzato è:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

