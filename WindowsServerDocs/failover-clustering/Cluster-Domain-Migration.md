---
title: Tra dominio migrazione di Cluster in Windows Server 2016/2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Questo articolo viene descritto lo spostamento di un cluster di Windows Server 2019 da un dominio a altro
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042035"
---
# Migrazione di dominio di Cluster di failover

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento offre che una panoramica per spostare il failover di Windows Server di cluster da un dominio a un altro.

## Il motivo per cui eseguire la migrazione tra domini

Esistono molti scenari in cui è necessaria la migrazione di un cluster da uno doamin a altro.

- CompanyA unisce con società CompanyB e deve spostare tutti i cluster nel dominio CompanyA
- Cluster sono creati in Data Center principale e inviati a posizioni remote
- Cluster è stata creata come cluster di gruppo di lavoro e ora deve essere parte di un dominio
- Cluster è stata creata come cluster di dominio e ora deve essere parte di un gruppo di lavoro
- Cluster di cui è stato spostato a un'area della società a un'altra ed è un sottodominio diverso

Microsoft non fornisce supporto per gli amministratori che tenta di spostare le risorse da un dominio a un altro, se l'operazione di applicazioni sottostante non è supportata. Ad esempio, Microsoft non fornisce supporto per gli amministratori che tenta di spostare un server Microsoft Exchange da un dominio a un altro.

   > [!WARNING]
   > Ti consigliamo di eseguire un backup completo di archiviazione condivisa tutti nel cluster prima di spostare il cluster.

## Windows Server 2016 e versioni precedenti

In Windows Server 2016 e versioni precedenti, il servizio Cluster non dispone della funzionalità di spostamento da un dominio a un altro.  Questa è la conseguenza la dipendenza maggiore in Active Directory Domain Services e i nomi virtuali creati.   

## Opzioni

Per poter eseguire un movimento di questo tipo, sono disponibili due opzioni.

La prima opzione prevede la distruzione del cluster e ricompilando nel nuovo dominio.

![Eliminazione e ricompilare](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Come mostra l'animazione, questa opzione è distruttiva con i passaggi da:

1. L'eliminazione del Cluster.
2. Modificare l'appartenenza al dominio dei nodi nel nuovo dominio.
3. Ricreare il Cluster come nuovo nel dominio aggiornato.  Si comporti dover ricreare tutte le risorse.

La seconda opzione è meno distruttiva, ma richiede hardware aggiuntivi come un nuovo cluster dovrà essere integrate nel nuovo dominio.  Una volta il cluster nel nuovo dominio, Esegui la procedura guidata migrazione di Cluster per eseguire la migrazione di risorse. Si noti che ciò non eseguire la migrazione di dati, dovrai usare un altro strumento per eseguire la migrazione di dati, ad esempio [Il servizio di migrazione di archiviazione](../storage/storage-migration-service/overview.md)(dopo aver aggiunto il supporto di cluster).

![Compilare ed eseguire la migrazione](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Come mostra l'animazione, questa opzione non distruttiva ma richiedono diverso hardware o un nodo dal cluster esistente che è stata rimossa.

1. Creare un nuovo clusterin il nuovo dominio mantenendo il cluster precedente disponibile.
2. Utilizza la [Procedura guidata di migrazione di Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) per eseguire la migrazione di tutte le risorse per il nuovo cluster. Promemoria, questo non vengono copiati dati, quindi dovranno essere eseguito separatamente.
3. Rimuovere le autorizzazioni o eliminare il cluster precedente.

In entrambe le opzioni, il nuovo cluster deve disporre di tutte [le applicazioni compatibile con cluster](https://technet.microsoft.com/aa369082(v=vs.90)) installate, tutto aggiornato, i driver e possibilmente test per garantire che tutte verrà eseguito correttamente.  Questo è un processo molto tempo se i dati devono inoltre essere spostato.

## Windows Server 2019

In Windows Server 2019, abbiamo introdotto funzionalità di migrazione di cluster tra dominio.  Ora, possono essere eseguiti facilmente gli scenari elencati in precedenza e la necessità di ricostruzione non è più necessario.  

Spostamento di un cluster da un dominio è un processo ogni. A tale scopo, esistono due nuovi cmdlet di PowerShell.

**New-ClusterNameAccount** -crea un Account del nome del Cluster in Active Directory **ClusterNameAccount Remove** -rimuove gli account del nome del Cluster da Active Directory

Il processo per eseguire questa operazione consiste nel modificare il cluster da un dominio di un gruppo di lavoro e tornare al nuovo dominio.  La necessità di eliminare un cluster, ricompila un cluster, installare le applicazioni e così via non è un requisito. Ad esempio, potrebbe apparire come segue:

![Eseguire la migrazione](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## La migrazione di un cluster in un nuovo dominio

Nei passaggi seguenti, un cluster viene spostato da al dominio Contoso.com dell'azienda per il nuovo dominio Fabrikam.com.  Il nome del cluster è *CLUSCLUS* e con un ruolo file server chiamato *FS-CLUSCLUS*.

1. Creare un account amministratore locale con lo stesso nome e la password in tutti i server del cluster.  Questo potrebbe essere necessario effettuare l'accesso mentre i server si stanno muovendo tra domini.
2. Accedi al primo server con un account utente o amministratore di dominio che dispone delle autorizzazioni di Active Directory per il Cluster nome oggetto (CNO), gli oggetti di Computer virtuale (VCO) ha accesso il Cluster e Apri PowerShell.
3. Verificare tutte le risorse di nome di rete di Cluster siano in un Offline stato ed eseguire il seguente comando.  Questo comando rimuoverà gli oggetti Active Directory che può avere il cluster.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Usa Active Directory utenti e computer per garantire il computer CNO e VCO gli oggetti associati a tutti i nomi di cluster sono stati rimossi.

   > [!NOTE]
   > È una buona idea arrestare il servizio Cluster su tutti i server del cluster e imposta il tipo di avvio su manuale in modo che il servizio Cluster non viene avviata quando i server sono riavvio durante la modifica di domini.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modificare l'appartenenza al dominio dei server a un gruppo di lavoro, riavviare il server, aggiungere i server al nuovo dominio e riavvia nuovamente.
6. Una volta che i server presenti nel nuovo dominio, accedere a un server con un account utente o amministratore di dominio che dispone delle autorizzazioni di Active Directory per creare oggetti, ha accesso al Cluster e aprire PowerShell. Avviare il servizio Cluster e impostarlo nuovamente su automatico.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Visualizzare il nome del Cluster e tutti gli altri cluster risorse nome di rete a uno stato Online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modificare il cluster per far parte del dominio di nuovo con gli oggetti associati active directory. A tale scopo, è il comando seguente e le risorse di nome di rete devono essere in uno stato online.  Cosa farà questo comando è ricreare oggetti il nome in Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Nota: Se non hai ad altri gruppi con nomi di rete (ad esempio, un Cluster Hyper-V con solo macchine virtuali), il parametro - UpgradeVCOs non è necessaria.

9. Usa Active Directory utenti e computer per verificare il nuovo dominio e assicurati che sono stati creati gli oggetti computer associato. Se hanno, inserire quindi le altre risorse online i gruppi.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## Problemi noti

Se si utilizza la nuova funzionalità di controllo USB, sarai in grado di aggiungere il cluster per il nuovo dominio.  Il concetto di base è che il tipo di controllo di condivisione file deve usare Kerberos per l'autenticazione.  Modificare il controllo su Nessuno prima di aggiungere il cluster al dominio.  Una volta completata, ricreare il controllo USB.  L'errore che vedrai è:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

