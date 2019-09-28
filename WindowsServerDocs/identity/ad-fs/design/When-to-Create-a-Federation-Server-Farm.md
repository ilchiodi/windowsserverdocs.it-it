---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Quando creare una server farm federativa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358936"
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando creare una server farm federativa

Prendere in considerazione la creazione di una Federazione server farm in Active Directory Federation Services \(AD FS @ no__t-1 quando si dispone di una distribuzione di AD FS di dimensioni maggiori e si desidera fornire tolleranza di errore, caricare @ no__t-2balancing o la scalabilità alla Federazione dell'organizzazione Servizio. La creazione di due o più server federativi nella stessa rete, la configurazione di ognuno di essi per l'uso dello stesso Servizio federativo e l'aggiunta della chiave pubblica del token @ no__t-0signing dei certificati di ogni server allo snap-in di gestione AD FS @ no__t-1in crea un server farm della Federazione.  
  
È possibile creare una Federazione server farm o installare altri server federativi in una farm esistente usando la configurazione guidata server federativo di AD FS. Per altre informazioni, vedere [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si sceglie l'opzione per creare una **nuova federazione server farm** utilizzando la configurazione guidata server federativo ad FS, la procedura guidata tenterà di creare un oggetto contenitore \(per che condivide i certificati @ no__t-2 in Active Directory. È quindi importante accedere prima di tutto al computer dove si sta configurando il ruolo di server federativo con un account che abbia autorizzazioni sufficienti in Active Directory per creare questo oggetto contenitore.  
  
Prima che i server federativi possano essere raggruppati come farm, è necessario che siano prima di tutto in cluster, in modo che le richieste che arrivano a un singolo nome di dominio completo \(FQDN @ no__t-1 siano indirizzate ai vari server federativi del server farm. È possibile creare il cluster di server distribuendo bilanciamento carico di rete \(NLB @ no__t-1 all'interno della rete aziendale. In questa guida si presuppone che NLB sia stato configurato in modo appropriato per eseguire il clustering di ogni server federativo della farm.  
  
Per altre informazioni su come configurare un FQDN del cluster usando la tecnologia NLB Microsoft, vedere [specifica dei parametri del cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Procedure consigliate per la distribuzione di una server farm federativa  
Per la distribuzione di un server federativo in un ambiente di produzione sono consigliate le seguenti procedure consigliate:  
  
-   Se si intende distribuire più server federativi contemporaneamente o si prevede di aggiungere altri server alla farm nel corso del tempo, è consigliabile creare un'immagine server di un server federativo esistente nella farm e quindi eseguire l'installazione da tale immagine quando è necessario CR EA in modo rapido ulteriori server federativi.  
  
    > [!NOTE]  
    > Se si decide di utilizzare il metodo di immagine server per la distribuzione di server federativi aggiuntivi, non è necessario completare le attività in [Checklist: Configurazione di un server federativo @ no__t-0 ogni volta che si desidera aggiungere un nuovo server alla farm.  
  
-   Utilizzare NLB o un'altra forma di clustering per allocare un singolo indirizzo IP per molti computer server federativi.  
  
-   Riservare un indirizzo IP statico per ogni server federativo della farm e, a seconda della configurazione di Domain Name System \(DNS @ no__t-1, inserire un'esclusione per ogni indirizzo IP in Dynamic Host Configuration Protocol \(DHCP @ no__t-3. La tecnologia Bilanciamento carico di rete Microsoft richiede che a ogni server appartenente al cluster di bilanciamento carico di rete venga assegnato un indirizzo IP statico.  
  
-   Se il database di configurazione AD FS verrà archiviato in un database SQL, evitare di modificare il database SQL da più server federativi nello stesso momento.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurazione di server federativi per una farm  
Nella tabella seguente vengono descritte le attività che è necessario completare in modo che ogni server federativo possa partecipare a un ambiente farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Se si usa SQL Server per archiviare il database di configurazione di AD FS|Un server farm di federazione è costituito da due o più server federativi che condividono lo stesso database di configurazione AD FS e token @ no__t-0signing. Il database di configurazione può essere archiviato nel Database interno di Windows o in un database di SQL Server. Se si prevede di archiviare il database di configurazione in un database SQL, assicurarsi che il database di configurazione sia accessibile per poter accedere a tutti i nuovi server federativi che fanno parte della farm. **Nota:** Per gli scenari di farm, è importante che il database di configurazione si trovi in un computer che non fa parte anche di un server federativo nella farm. Bilanciamento carico di rete Microsoft non consente ai computer che fanno parte di una farm di comunicare tra loro. **Nota:** Verificare che l'identità del AD FS AppPool in Internet Information Services \(IIS @ no__t-1 @ no__t-2 in ogni server federativo che fa parte della farm abbia accesso in lettura al database di configurazione.|  
|Ottenere e condividere i certificati|È possibile ottenere un certificato di autenticazione server singolo da un'autorità di certificazione pubblica \(CA @ no__t-1, ad esempio VeriSign. È quindi possibile configurare il certificato in modo che tutti i server federativi condividano la stessa parte di chiave privata del certificato. Per ulteriori informazioni su come condividere lo stesso certificato, vedere [Checklist: Configurazione di un server federativo @ no__t-0. **Nota:** Lo snap-in di gestione AD FS @ no__t-0cm fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.<br /><br />Per altre informazioni, vedere [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Puntare alla stessa istanza di SQL Server|Se il database di configurazione AD FS verrà archiviato in un database SQL, il nuovo server federativo deve puntare alla stessa istanza di SQL Server utilizzata da altri server federativi della farm, in modo che il nuovo server possa partecipare alla farm.|  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
