---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guida alla distribuzione di Windows Server 2012 AD ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guida alla distribuzione di Windows Server 2012 AD ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare Active Directory® Federation Services \(AD FS\) con il sistema operativo Windows Server® 2012 per creare una soluzione di gestione di identità federate che estende i limiti di piattaforma e organizzazione identificazione distribuita, l'autenticazione e autorizzazione servizi alle applicazioni basate sul Web. Tramite la distribuzione di ADFS, è possibile estendere funzionalità di gestione delle identità esistenti dell'organizzazione a Internet.  
  
È possibile distribuire ADFS per:  
  
-   Fornire ai dipendenti o ai clienti un'esperienza \(SSO\) basata sul Web, accesso in apartment quando hanno bisogno di accesso remoto alle ospitati internamente i siti Web o servizi.  
  
-   Fornire ai dipendenti o ai clienti un'esperienza SSO basata sul Web, quando accedono ai siti Web cross\ organizzazioni o servizi da entro il firewall della rete.  
  
-   Fornire ai dipendenti o ai clienti l'accesso trasparente alle risorse basata sul Web in qualsiasi organizzazione partner federativo su Internet senza chiedere ai dipendenti o ai clienti di accedere una sola volta.  
  
-   Mantenere il controllo completo delle identità di dipendenti o clienti senza usare altri provider di accesso in ingresso \ (Windows Live ID, Liberty Alliance e others\).  
  
## <a name="about-this-guide"></a>Informazioni sulla Guida  
Questa guida è destinata agli amministratori di sistema e sistemisti. Fornisce istruzioni dettagliate per la distribuzione di una progettazione di ADFS che è stata preselezionata dallo sviluppatore o un architetto di sistema o uno specialista dell'infrastruttura all'interno dell'organizzazione.  
  
Se non è ancora stata selezionata una progettazione, è consigliabile che prima di seguire le istruzioni riportate in questa guida fino a dopo aver esaminato le opzioni di progettazione nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) ed è stata selezionata la progettazione più adatta per l'organizzazione. Per ulteriori informazioni sull'utilizzo di questa Guida con una progettazione già selezionata, vedere [implementazione del piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Dopo aver selezionato la progettazione dalla Guida alla progettazione e raccogliere le informazioni necessarie su attestazioni, tipi di token, archivi di attributi e altri elementi, è possibile utilizzare questa guida per distribuire una progettazione di ADFS nell'ambiente di produzione. Questa guida fornisce passaggi per la distribuzione di uno dei seguenti progetti ADFS primari:  
  
-   Web SSO  
  
-   Web SSO Federativo  
  
Utilizzare gli elenchi di controllo in [implementazione del piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md) per determinare il modo migliore per utilizzare le istruzioni riportate in questa guida per distribuire una particolare progettazione. Per informazioni sui requisiti hardware e software per la distribuzione di ADFS, vedere il [appendice a: revisione AD requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx) nella Guida alla progettazione di ADFS.  
  
### <a name="what-this-guide-does-not-provide"></a>Cosa non fornisce questa Guida  
Questa Guida non contiene:  
  
-   Indicazioni su quando e dove posizionare i server federativi, proxy server federativi o server Web nell'infrastruttura di rete esistente. Per queste informazioni, vedere [pianificazione posizionamento del Server federativo](https://technet.microsoft.com/library/dd807069.aspx) e [pianificazione posizionamento del Proxy Server federativo](https://technet.microsoft.com/library/dd807130.aspx) nella Guida alla progettazione di ADFS.  
  
-   Indicazioni per l'uso di certificazione autorità \(CAs\) per configurare AD FS  
  
-   Indicazioni per installare o configurare specifiche applicazioni basata sul Web  
  
-   Istruzioni specifiche per la configurazione di un ambiente di testing di installazione.  
  
-   Informazioni su come personalizzare schermate di accesso federato, file Web. config o il database di configurazione.  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Pianificazione della distribuzione di ADFS](Planning-to-Deploy-AD-FS.md)  
  
-   [ADFS di implementazione del piano di progettazione](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Elenco di controllo: Implementazione di un progetto Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: Implementazione di un progetto Web SSO federativo](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configurazione di organizzazioni Partner](Configuring-Partner-Organizations.md)  
  
-   [Configurazione delle regole attestazioni](Configuring-Claim-Rules.md)  
  
-   [Distribuzione di server federativi](Deploying-Federation-Servers.md)  
  
-   [Distribuzione di proxy Server federativi](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperabilità con AD FS 1. x](Interoperating-with-AD-FS-1.x.md)  
