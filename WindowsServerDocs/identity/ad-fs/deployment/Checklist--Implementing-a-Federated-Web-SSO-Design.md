---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Elenco di controllo: implementazione di un progetto Web SSO federato'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 165471fd06031a68343a54d019357afee782d082
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359954"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO federativo

Questo elenco di controllo padre include i collegamenti cross @ no__t-0reference a concetti importanti sul Web single federativo @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 design per Active Directory Federation Services \(AD FS @ no__t-6. Contiene inoltre collegamenti a elenchi di controllo subordinati che consentiranno di completare le attività necessarie per implementare il progetto.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Se un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare in questo argomento dopo aver consultato l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo da procedere con le attività rimanenti.  
  
![federated Web SSO @ no__t-1Checklist: Implementazione di un progetto di Web SSO federativo**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sul Web SSO federativo e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione Web SSO federativo](https://technet.microsoft.com/library/dd807050.aspx) di ![federated Web SSO<br /><br />@no__t 0federated Web SSO che](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica gli obiettivi di distribuzione ad FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Esaminare i componenti hardware, software, certificati, Domain Name System \(DNS\), archivio di attributi e sui requisiti client per la distribuzione di ADFS nell'organizzazione.|![federated Web SSO @ no__t-1Appendix A: Verifica dei requisiti di AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sulle attestazioni, regole, archivi di attributi e il database di configurazione di ADFS di attestazione prima di distribuire ADFS in entrambe le organizzazioni partner.|![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[informazioni sui concetti chiave ad FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi in ogni organizzazione partner. **Nota:** Per il progetto SSO Web federativo, è necessario almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse.|![federated Web SSO @ no__t-1Checklist: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|\(Optional @ no__t-1 determinare se l'organizzazione ha bisogno di un proxy server federativo. Se il piano di progettazione chiama per un proxy, è possibile installare uno o più proxy server federativi in ogni organizzazione partner.|![federated Web SSO @ no__t-1Checklist: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progetto, condividere i certificati e configurare i client e le relazioni di trust in entrambe le organizzazioni partner, in modo che possano comunicare attraverso un trust federativo.|![federated Web SSO @ no__t-1Checklist: Configurazione dell'organizzazione partner account @ no__t-0<br /><br />![federated Web SSO @ no__t-1Checklist: Configurazione dell'organizzazione partner risorse @ no__t-0|  
|![sso web federativo](media/icon_checkboxo.gif)|Se si è un amministratore dell'organizzazione partner risorse, Claims @ no__t-0enable l'applicazione Web browser, l'applicazione del servizio Web o l'applicazione Microsoft® Office SharePoint® Server usando WIF e WIF SDK.|![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
