---
title: Migrazione di cluster tra domini in Windows Server 2016/2019
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Questo articolo descrive lo trasferimento di un cluster Windows Server 2019 da un dominio a un altro
ms.localizationpriority: medium
ms.openlocfilehash: 68f49795124dedf0655726853a4d865686f6d697
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361414"
---
# <a name="failover-cluster-domain-migration"></a>Migrazione del dominio del cluster di failover

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce una panoramica dello stato di trasferimento dei cluster di failover di Windows Server da un dominio a un altro.

## <a name="why-migrate-between-domains"></a>Vantaggi della migrazione tra domini

Esistono diversi scenari in cui è necessario eseguire la migrazione di un cluster da un doamin a un altro.

- CompanyA si unisce a CompanyB e deve spostare tutti i cluster nel dominio CompanyA
- I cluster sono compilati nel data center principale ed distribuiti in posizioni remote
- Il cluster è stato creato come cluster di gruppi di lavoro e ora deve essere parte di un dominio
- Il cluster è stato creato come cluster di dominio e ora deve essere parte di un gruppo di lavoro
- Il cluster è stato spostato in un'area della società in un altro e è un sottodominio diverso

Microsoft non fornisce supporto per gli amministratori che tentano di spostare le risorse da un dominio a un altro se l'operazione dell'applicazione sottostante non è supportata. Ad esempio, Microsoft non fornisce supporto per gli amministratori che tentano di spostare un server Microsoft Exchange da un dominio a un altro.

   > [!WARNING]
   > Prima di spostare il cluster, è consigliabile eseguire un backup completo di tutte le archiviazioni condivise nel cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 e versioni precedenti

In Windows Server 2016 e versioni precedenti, il Servizio cluster non ha avuto la possibilità di eseguire il passaggio da un dominio a un altro.  Ciò è dovuto alla maggiore dipendenza da Active Directory Domain Services e ai nomi virtuali creati.   

## <a name="options"></a>Opzioni

Per eseguire tale operazione, è possibile procedere in due modi.

La prima opzione consiste nell'eliminare definitivamente il cluster e ricompilarlo nel nuovo dominio.

![Eliminazione e ricompilazione](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Come illustrato nell'animazione, questa opzione è distruttiva con i passaggi seguenti:

1. Eliminare definitivamente il cluster.
2. Modificare l'appartenenza al dominio dei nodi nel nuovo dominio.
3. Ricreare il cluster come nuovo nel dominio aggiornato.  Ciò comporterebbe la necessità di ricreare tutte le risorse.

La seconda opzione è meno distruttiva ma richiede hardware aggiuntivo perché sarebbe necessario compilare un nuovo cluster nel nuovo dominio.  Quando il cluster si trova nel nuovo dominio, eseguire la migrazione guidata cluster per eseguire la migrazione delle risorse. Si noti che questo non comporta la migrazione dei dati. è necessario usare un altro strumento per eseguire la migrazione dei dati, ad esempio il [servizio migrazione archiviazione](../storage/storage-migration-service/overview.md)(dopo l'aggiunta del supporto del cluster).

![Compilazione e migrazione](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Come illustrato nell'animazione, questa opzione non è distruttiva, ma richiede hardware diverso o un nodo del cluster esistente rispetto a quello che è stato rimosso.

1. Creare un nuovo cluster nel nuovo dominio senza che sia ancora disponibile il vecchio cluster.
2. Utilizzare la [migrazione guidata cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) per eseguire la migrazione di tutte le risorse al nuovo cluster. Il promemoria non copia i dati, pertanto è necessario eseguirli separatamente.
3. Rimuovere le autorizzazioni o eliminare il cluster precedente.

In entrambe le opzioni, è necessario che il nuovo cluster disponga di tutte le [applicazioni compatibili](https://technet.microsoft.com/aa369082(v=vs.90)) con i cluster installate, che i driver siano tutti aggiornati ed eventualmente test per assicurarsi che tutti vengano eseguiti correttamente.  Si tratta di un processo che richiede molto tempo se è necessario spostare anche i dati.

## <a name="windows-server-2019"></a>Windows Server 2019

In Windows Server 2019 sono state introdotte funzionalità di migrazione di domini tra cluster.  Ora, gli scenari elencati sopra possono essere facilmente eseguiti e la necessità di ricompilare non è più necessaria.  

Lo stato di avanzamento di un cluster da un dominio è un processo semplice. A tale scopo, sono disponibili due nuovi cmdlet di PowerShell.

**New-ClusterNameAccount** : crea un account nome cluster in Active Directory **Remove-ClusterNameAccount** : rimuove gli account del nome del cluster da Active Directory

Il processo a tale scopo consiste nel modificare il cluster da un dominio a un gruppo di lavoro e viceversa al nuovo dominio.  La necessità di eliminare un cluster, ricompilare un cluster, installare le applicazioni e così via non è un requisito. Ad esempio, sarà simile al seguente:

![Migrazione](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migrazione di un cluster in un nuovo dominio

Nei passaggi seguenti un cluster viene spostato dal dominio Contoso.com al nuovo dominio Fabrikam.com.  Il nome del cluster è *CLUSCLUS* e con un ruolo file server denominato *FS-CLUSCLUS*.

1. Creare un account amministratore locale con lo stesso nome e la stessa password in tutti i server del cluster.  Questa operazione può essere necessaria per accedere mentre i server si trovano tra domini.
2. Accedere al primo server con un account utente di dominio o amministratore che disponga di autorizzazioni Active Directory per l'oggetto nome cluster (oggetto nome cluster), oggetti computer virtuali (VCO), ha accesso al cluster e aprire PowerShell.
3. Verificare che tutte le risorse del nome di rete del cluster siano in uno stato non in linea ed eseguire il comando seguente.  Tramite questo comando vengono rimossi gli oggetti di Active Directory che il cluster può avere.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Utilizzare Active Directory utenti e computer per assicurarsi che gli oggetti computer oggetto nome cluster e VCO associati a tutti i nomi cluster siano stati rimossi.

   > [!NOTE]
   > È consigliabile arrestare il Servizio cluster in tutti i server del cluster e impostare il tipo di avvio del servizio su manuale, in modo che il Servizio cluster non venga avviato quando i server vengono riavviati durante la modifica dei domini.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modificare l'appartenenza al dominio dei server a un gruppo di lavoro, riavviare i server, aggiungere i server al nuovo dominio e riavviarlo.
6. Una volta che i server si trovano nel nuovo dominio, accedere a un server con un account utente di dominio o amministratore che dispone di autorizzazioni Active Directory per creare oggetti, ha accesso al cluster e aprire PowerShell. Avviare il servizio cluster e impostarlo di nuovo su Automatic.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Portare il nome del cluster e tutte le altre risorse del nome di rete del cluster in uno stato online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modificare il cluster in modo che faccia parte del nuovo dominio con gli oggetti Active Directory associati. A tale scopo, il comando è riportato di seguito e le risorse del nome di rete devono trovarsi in uno stato online.  L'operazione che verrà eseguita da questo comando consiste nel ricreare gli oggetti nome in Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    NOTA: Se non sono presenti gruppi aggiuntivi con nomi di rete, ad esempio un cluster Hyper-V con solo macchine virtuali, l'opzione di parametro-UpgradeVCOs non è necessaria.

9. Utilizzare Active Directory utenti e computer per verificare il nuovo dominio e verificare che gli oggetti computer associati siano stati creati. In caso affermativo, portare online le risorse rimanenti nei gruppi.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemi noti

Se si usa la nuova funzionalità USB Witness, non sarà possibile aggiungere il cluster al nuovo dominio.  Il motivo è che il tipo di controllo della condivisione file deve usare Kerberos per l'autenticazione.  Modificare il server di controllo del mirroring in None prima di aggiungere il cluster al dominio.  Al termine, ricreare il server di controllo del mirroring USB.  Verrà visualizzato l'errore seguente:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

