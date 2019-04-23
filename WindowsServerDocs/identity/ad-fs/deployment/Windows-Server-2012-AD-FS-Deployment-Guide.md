---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guida alla distribuzione di AD FS in Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882442"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guida alla distribuzione di AD FS in Windows Server 2012

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare Active Directory® Federation Services \(ADFS\) con il sistema operativo Windows Server® 2012 per compilare una soluzione di gestione delle identità federate che estende l'identificazione distribuita, l'autenticazione, e servizi di autorizzazione Web\-applicazioni basate su confini dell'organizzazione e piattaforma. Con la distribuzione di ADFS, è possibile estendere a Internet le funzionalità di gestione delle identità esistenti dell'organizzazione.  
  
È possibile distribuire AD FS:  
  
-   Offrire ai dipendenti o i clienti con un sito Web\-base, l'accesso single\-sign\-sul \(SSO\) esperienza nei momenti desiderati accesso remoto a internamente ospitata in siti Web o servizi.  
  
-   Offrire ai dipendenti o i clienti con un sito Web\-base, esperienza SSO durante l'accesso cross-\-siti Web dell'organizzazione o servizi all'interno di firewall della rete.  
  
-   Offrire ai dipendenti o i clienti con accesso in modo trasparente a Web\-in base alle risorse in qualsiasi organizzazione partner federativo su Internet senza chiedere ai dipendenti o ai clienti di accedere più volte.  
  
-   Mantenere il controllo completo delle identità di dipendenti o clienti senza usare altri sign\-sui provider \(Windows Live ID, Liberty Alliance e altri\).  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida è destinata ad amministratori di sistema e sistemisti. Vengono fornite istruzioni dettagliate per la distribuzione di una progettazione di ADFS che è stata preselezionata dall'utente o da un'infrastruttura specialista architetto di sistema all'interno dell'organizzazione.  
  
Se non è ancora stata selezionata una progettazione, è consigliabile attendere per seguire le istruzioni in questa guida fino a dopo aver esaminato le opzioni di progettazione nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) ed è stata selezionata la maggior parte progettazione appropriata per l'organizzazione. Per altre informazioni sull'uso di questa Guida con una progettazione che è già stata selezionata, vedere [che implementa il piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Dopo aver selezionato la progettazione dalla Guida alla progettazione e raccogliere le informazioni necessarie su attestazioni, tipi di token, archivi di attributi e altri elementi, è possibile usare questa guida per distribuire una progettazione di ADFS nell'ambiente di produzione. Questa guida fornisce passaggi per la distribuzione delle progettazioni di AD FS primarie seguente:  
  
-   Web SSO  
  
-   Web SSO federativo  
  
Usare gli elenchi di controllo nella [che implementa il piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md) per determinare il modo migliore per usare le istruzioni in questa guida per distribuire una particolare progettazione. Per informazioni sui requisiti hardware e software per la distribuzione di AD FS, vedere il [appendice a: Requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx) la progettazione di AD FS Guida.  
  
### <a name="what-this-guide-does-not-provide"></a>Informazioni non contenute in questa guida  
Questa guida non contiene:  
  
-   Indicazioni su quando e dove posizionare i server federativi, proxy server federativi o server Web nell'infrastruttura di rete esistente. Queste informazioni, vedere [pianificazione posizionamento del Server federativo](https://technet.microsoft.com/library/dd807069.aspx) e [pianificazione posizionamento del Proxy Server federativo](https://technet.microsoft.com/library/dd807130.aspx) nella Guida alla progettazione di AD FS.  
  
-   Linee guida per l'uso di autorità di certificazione \(CAs\) configurare AD FS  
  
-   Linee guida per l'installazione o configurazione Web specifiche\-applicazioni basate su  
  
-   Istruzioni di configurazione specifiche per l'impostazione di un ambiente lab di test.  
  
-   Informazioni su come personalizzare schermate di accesso federato, file web.config o il database di configurazione.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Pianificazione della distribuzione di AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementazione di ADFS progettare piano](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Elenco di controllo: Implementazione di un progetto Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: Implementazione di un progetto SSO Web federativo](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configurazione di organizzazioni Partner](Configuring-Partner-Organizations.md)  
  
-   [Configurazione delle regole attestazione](Configuring-Claim-Rules.md)  
  
-   [Distribuzione di server federativi](Deploying-Federation-Servers.md)  
  
-   [Distribuzione di Server federativi](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperabilità con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
