---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Quando creare una Server Farm federativa
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando creare una Server Farm federativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È consigliabile creare una server farm federativa in Active Directory Federation Services \(AD FS\) quando si dispone di una distribuzione AD FS più grande e si vuole fornire tolleranza di errore, Load bilanciamento del carico o scalabilità al servizio federativo dell'organizzazione. L'atto di creare due o più server federativi nella stessa rete, configurare ognuno di essi per utilizzare lo stesso servizio federativo e aggiungere la chiave pubblica di certificati di firma token\ ogni server di gestione di AD FS snap-in consente di creare una server farm federativa.  
  
È possibile creare una server farm federativa o installare altri server federativi a una farm esistente usando la configurazione guidata Server federativo AD FS. Per ulteriori informazioni, vedere [sulla creazione di un Server federativo](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si sceglie l'opzione per creare un **nuova server farm federativa** tramite configurazione guidata Server federativo AD FS, la procedura guidata tenterà di creare un oggetto contenitore \(for sharing certificates\) in Active Directory. Pertanto, è importante di prima accedere al computer, in cui si imposta il ruolo server federativo, con un account che disponga di autorizzazioni sufficienti in Active Directory per creare questo oggetto contenitore.  
  
Prima di possono raggruppare i server federativi una farm, è necessario prima di tutto cluster in modo che le richieste che arrivano a un singolo completamente completo del dominio vengono instradati \(FQDN\) per i vari server federativi della server farm. È possibile creare il cluster di server distribuendo \(NLB\) bilanciamento carico di rete all'interno della rete azienda. Questa guida si presuppone che Bilanciamento carico di rete è stato configurato in modo appropriato per ogni server federativo della farm del cluster.  
  
Per ulteriori informazioni su come configurare un nome di dominio completo del cluster mediante la tecnologia NLB di Microsoft, vedere [specifica dei parametri del Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Procedure consigliate per la distribuzione di una server farm federativa  
Si consiglia le seguenti procedure ottimali per la distribuzione di un server federativo in un ambiente di produzione:  
  
-   Se si distribuiscono più server federativi nello stesso momento o sai che saranno aggiunti altri server alla farm nel tempo, è consigliabile creare un'immagine server di un server federativo esistente nella farm e quindi l'installazione da quell'immagine quando è necessario creare rapidamente altri server federativi.  
  
    > [!NOTE]  
    > Se si decide di utilizzare il metodo di immagine server per la distribuzione di altri server federativi, non è necessario completare le attività in [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) ogni volta che si desidera aggiungere un nuovo server alla farm.  
  
-   Utilizzare Bilanciamento carico di rete o altre forme di clustering per allocare un singolo indirizzo IP per molti computer del server federativo.  
  
-   Riservare un indirizzo IP statico per ogni server federativo nella farm e, a seconda della configurazione di Domain Name System \(DNS\), inserire un'esclusione per ogni indirizzo IP in Dynamic Host Configuration Protocol \(DHCP\). La tecnologia NLB Microsoft richiede che ogni server che fa parte del cluster di bilanciamento carico di rete essere assegnato un indirizzo IP statico.  
  
-   Se il database di configurazione di AD FS verrà archiviato in un database SQL, evitare di modificare il database SQL da più server federativi nello stesso momento.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurazione di server federativi per una farm  
Nella tabella seguente vengono descritte le attività che devono essere completate in modo che ogni server federativo possa far parte di un ambiente di farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Se si utilizza SQL Server per archiviare il database di configurazione di ADFS|Una server farm federativa è costituito da due o più server federativi che condividono la stessa configurazione di ADFS database e i certificati di firma token\. Il database di configurazione può essere archiviato nel Database interno di Windows o in un database di SQL Server. Se si prevede di archiviare il database di configurazione in un database SQL, assicurarsi che il database di configurazione sia accessibile in modo che sia accessibile da tutti i nuovi server federativi che fanno parte della farm. **Nota:** per gli scenari di farm, è importante che il database di configurazione trovarsi in un computer che non fa parte anche come server federativo della farm. Microsoft NLB non consente a qualsiasi computer che fanno parte di una farm di comunicare tra loro. **Nota:** assicurarsi che l'identità dell'AppPool di ADFS di Active Directory in Internet Information Services \(IIS\)\) in ogni server federativo che fa parte della farm abbia accesso in lettura al database di configurazione.|  
|Ottenere e condividere i certificati|È possibile ottenere un singolo server il certificato di autenticazione da un'autorità di certificazione pubblica \ (CA\), ad esempio VeriSign. È quindi possibile configurare il certificato in modo che tutti i server federativi condividano la stessa parte di chiave privata del certificato. Per ulteriori informazioni su come condividere lo stesso certificato, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Nota:** snap-in di gestione di ADFS si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.<br /><br />Per ulteriori informazioni, vedere [requisiti dei certificati per i server federativi](Certificate-Requirements-for-Federation-Servers.md).|  
|Puntare alla stessa istanza di SQL Server|Se il database di configurazione di AD FS verrà archiviato in un database SQL, il nuovo server federativo deve puntare alla stessa istanza di SQL Server utilizzata da altri server federativi nella farm in modo che il nuovo server di far parte della farm.|  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
