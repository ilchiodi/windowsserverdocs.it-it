---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guida alla distribuzione di ADFS in Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aeb1d9042cea6be77ea15f6c753d720d7f814fe6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855824"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guida alla distribuzione di ADFS in Windows Server 2012


È possibile utilizzare Active Directory&reg; Federation Services \(AD FS\) con il sistema operativo Windows Server&reg; 2012 per creare una soluzione di gestione delle identità federate che estende i servizi di identificazione, autenticazione e autorizzazione distribuiti alle applicazioni basate su\-Web oltre i confini dell'organizzazione e della piattaforma. Distribuendo AD FS, è possibile estendere a Internet le funzionalità di gestione delle identità esistenti dell'organizzazione.  
  
È possibile distribuire AD FS in:  
  
-   Offrire ai dipendenti o ai clienti\-un\-Single\-Sign sull'esperienza \(SSO\) quando necessitano di accesso remoto a siti o servizi Web ospitati internamente.  
  
-   Offrire ai dipendenti o ai clienti un'esperienza SSO basata su\-Web quando accedono ai servizi o ai siti Web aziendali tra\-all'interno dei firewall della rete.  
  
-   Fornire ai dipendenti o ai clienti l'accesso trasparente alle risorse basate su\-Web in qualsiasi organizzazione partner federativo su Internet senza richiedere ai dipendenti o ai clienti di accedere più di una volta.  
  
-   Mantieni il controllo completo sulle identità di dipendenti o clienti senza usare altri\-di accesso per i provider \(Windows Live ID, Liberty Alliance e altri\).  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida è destinata ad amministratori di sistema e sistemisti. Fornisce indicazioni dettagliate per la distribuzione di un progetto di AD FS preselezionato dall'utente o da uno specialista dell'infrastruttura o un architetto di sistema all'interno dell'organizzazione.  
  
Se non è ancora stata selezionata una progettazione, è consigliabile attendere per seguire le istruzioni riportate in questa guida fino a quando non sono state esaminate le opzioni di progettazione nella [Guida alla progettazione ad FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) ed è stata selezionata la progettazione più appropriata per l'organizzazione. Per ulteriori informazioni sull'utilizzo di questa guida con una progettazione già selezionata, vedere [implementazione del piano di progettazione ad FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Dopo aver selezionato la progettazione dalla guida alla progettazione e avere raccolto le informazioni necessarie su attestazioni, tipi di token, archivi di attributi e altri elementi, è possibile usare questa guida per distribuire la progettazione AD FS nell'ambiente di produzione. Questa guida illustra i passaggi per la distribuzione di uno dei seguenti progetti di AD FS primari:  
  
-   Web SSO  
  
-   Web SSO federativo  
  
Utilizzare gli elenchi di controllo in [implementazione del piano di progettazione di ad FS](Implementing-Your-AD-FS-Design-Plan.md) per determinare il modo migliore per utilizzare le istruzioni in questa guida per distribuire la progettazione specifica. Per informazioni sui requisiti hardware e software per la distribuzione di AD FS, vedere l' [appendice a: revisione dei requisiti di ad FS](https://technet.microsoft.com/library/ff678034.aspx) nella Guida alla progettazione ad FS.  
  
### <a name="what-this-guide-does-not-provide"></a>Informazioni non contenute in questa guida  
Questa guida non contiene:  
  
-   Indicazioni su quando e dove collocare i server federativi, i proxy server federativi o i server Web nell'infrastruttura di rete esistente. Per informazioni, vedere [Planning Federation Server Placement](https://technet.microsoft.com/library/dd807069.aspx) and [Planning Federation Server Proxy Placement](https://technet.microsoft.com/library/dd807130.aspx) nella Guida alla progettazione ad FS.  
  
-   Linee guida per l'uso di autorità di certificazione \(CAs\) per configurare AD FS  
  
-   Linee guida per la configurazione o la configurazione di specifiche applicazioni basate su Web\-  
  
-   Istruzioni di configurazione specifiche per l'impostazione di un ambiente lab di test.  
  
-   Informazioni su come personalizzare schermate di accesso federato, file web.config o il database di configurazione.  
  
## <a name="in-this-guide"></a>In questa guida  
  
-   [Pianificazione della distribuzione di AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementazione del piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Elenco di controllo: implementazione di un progetto Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: implementazione di un progetto Web SSO federato](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configurazione di organizzazioni partner](Configuring-Partner-Organizations.md)  
  
-   [Configurazione delle regole delle attestazioni](Configuring-Claim-Rules.md)  
  
-   [Distribuzione di server federativi](Deploying-Federation-Servers.md)  
  
-   [Distribuzione di proxy server federativi](Deploying-Federation-Server-Proxies.md)  
  
-   [Interazione con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
