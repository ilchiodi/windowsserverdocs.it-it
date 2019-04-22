---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Quando creare una server farm federativa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816232"
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando creare una server farm federativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È consigliabile creare una server farm federativa in Active Directory Federation Services \(ADFS\) quando si dispone di una distribuzione più ampia di AD FS e si vuole fornire tolleranza di errore, il carico\-bilanciamento del carico o scalabilità per il Servizio federativo dell'organizzazione. L'atto di creazione di due o più server federativi nella stessa rete, configurare ognuno di essi di utilizzare lo stesso servizio federativo e l'aggiunta di token di chiave pubblica di ogni server della\-firma dei certificati per lo snap di gestione di AD FS\-in Crea una server farm federativa.  
  
È possibile creare una server farm federativa o installare altri server federativi in una farm esistente utilizzando Configurazione guidata Server federativo AD FS. Per altre informazioni, vedere [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando si sceglie l'opzione per creare un **nuova server farm federativa** utilizzando Configurazione guidata Server federativo AD FS, la procedura guidata proverà a creare un oggetto contenitore \(per la condivisione certificati\) in Active Directory. È quindi importante accedere prima di tutto al computer dove si sta configurando il ruolo di server federativo con un account che abbia autorizzazioni sufficienti in Active Directory per creare questo oggetto contenitore.  
  
Prima che i server federativi possono essere raggruppati in una farm, è necessario prima di tutto il clustering, in modo che le richieste che arrivano completamente in un singolo nome di dominio completo \(FQDN\) vengono indirizzate ai vari server federativo nella server farm. È possibile creare il cluster di server tramite la distribuzione di bilanciamento del carico di rete \(NLB\) all'interno della rete azienda. Questa guida si presuppone che Bilanciamento carico di rete sia stato configurato in modo appropriato per inserire in cluster ogni server federativo nella farm.  
  
Per altre informazioni su come configurare un nome FQDN del cluster con la tecnologia Microsoft NLB, vedere [specificando i parametri del Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Procedure consigliate per la distribuzione di una server farm federativa  
Si consiglia le seguenti procedure ottimali per la distribuzione di un server federativo in un ambiente di produzione:  
  
-   Se si stanno distribuendo più server federativi allo stesso tempo o si sa che si aggiungerà altri server alla farm nel corso del tempo, prendere in considerazione la creazione di un'immagine server di un server di federazione esistente nella farm e successiva installazione da quell'immagine quando è necessario cr EA altri server federativi rapidamente.  
  
    > [!NOTE]  
    > Se si decide di usare il metodo di immagine server per la distribuzione di altri server federativi, non è necessario completare le attività in [elenco di controllo: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) ogni volta che si desidera aggiungere un nuovo server alla farm.  
  
-   Usare bilanciamento carico di rete o un'altra forma di clustering per allocare un indirizzo IP per il numero di computer server federativo.  
  
-   Riservare un indirizzo IP statico per ogni server federativo nella farm e, in base al sistema di nome di dominio \(DNS\) configuration, inserimento di indirizzi un'esclusione per ogni IP in Dynamic Host Configuration Protocol \(DHCP\). La tecnologia Bilanciamento carico di rete Microsoft richiede che a ogni server appartenente al cluster di bilanciamento carico di rete venga assegnato un indirizzo IP statico.  
  
-   Se il database di configurazione di AD FS verrà archiviato in un database SQL, evitare di modificare il database SQL da più server federativi nello stesso momento.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurazione di server federativi per una farm  
Nella tabella seguente vengono descritte le attività che devono essere completate in modo che ogni server federativo possa partecipare a un ambiente di farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Se si usa SQL Server per archiviare il database di configurazione di AD FS|Una server farm federativa è costituito da due o più server federativi che condividono lo stesso database di configurazione di AD FS e token\-i certificati di firma. Il database di configurazione può essere archiviato nel Database interno di Windows o in un database di SQL Server. Se si prevede di archiviare il database di configurazione in un database SQL, assicurarsi che il database di configurazione sia accessibile in modo che sia accessibile da tutti i nuovi server federativi che fanno parte della farm. **Nota:** Per gli scenari di farm, è importante che il database di configurazione si trovi in un computer che non fanno parte anche come server federativo della farm. Bilanciamento carico di rete Microsoft non consente ai computer che fanno parte di una farm di comunicare tra loro. **Nota:** Assicurarsi che l'identità del pool di applicazioni di AD FS in Internet Information Services \(IIS\) \) on ogni federazione server che fa parte della farm abbia accesso in lettura al database di configurazione.|  
|Ottenere e condividere i certificati|È possibile ottenere un singolo server certificato di autenticazione da un'autorità di certificazione pubblica \(CA\), ad esempio VeriSign. È quindi possibile configurare il certificato in modo che tutti i server federativi condividano la stessa parte di chiave privata del certificato. Per altre informazioni su come condividere lo stesso certificato, vedere [elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Nota:** Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.<br /><br />Per altre informazioni, vedere [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Puntare alla stessa istanza di SQL Server|Se il database di configurazione di AD FS verrà archiviato in un database SQL, il nuovo server federativo deve puntare alla stessa istanza di SQL Server utilizzata da altri server federativi della farm in modo che il nuovo server può far parte della farm.|  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
