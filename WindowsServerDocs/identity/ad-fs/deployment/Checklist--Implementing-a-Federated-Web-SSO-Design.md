---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Elenco di controllo: implementazione di un progetto Web SSO federato'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 356d51899e930b487e844d8bdff0f9fe19086d34
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855014"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO federativo

Questo elenco di controllo padre include collegamenti di riferimento incrociato\-ai concetti importanti relativi al single Web federativo\-Sign\-on \(SSO\) design for Active Directory Federation Services \(ad FS\). Contiene inoltre collegamenti a elenchi di controllo subordinati che consentiranno di completare le attività necessarie per implementare il progetto.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Se un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare in questo argomento dopo aver consultato l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo da procedere con le attività rimanenti.  
  
![Web SSO federato](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: implementazione di un progetto Web SSO federato**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sul Web SSO federativo e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione di SSO Web](https://technet.microsoft.com/library/dd807050.aspx) federativo federativo web SSO ![<p>![SSO Web federato che](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica gli obiettivi di distribuzione di ad FS](https://technet.microsoft.com/library/dd807053.aspx)<p>![SSO Web federativo](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Esaminare i componenti hardware, software, certificati, Domain Name System \(DNS\), archivio di attributi e sui requisiti client per la distribuzione di ADFS nell'organizzazione.|![Web SSO federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Appendice A: revisione dei requisiti di ad FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sulle attestazioni, regole, archivi di attributi e il database di configurazione di ADFS di attestazione prima di distribuire ADFS in entrambe le organizzazioni partner.|![Web SSO federativo](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[informazioni sui concetti chiave ad FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi in ogni organizzazione partner. **Nota:** per la progettazione di Web SSO federativo, è necessario almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse.|![Web SSO federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|\(facoltativo\) determinare se l'organizzazione necessita o meno di un proxy server federativo. Se il piano di progettazione chiama per un proxy, è possibile installare uno o più proxy server federativi in ogni organizzazione partner.|![Web SSO federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progetto, condividere i certificati e configurare i client e le relazioni di trust in entrambe le organizzazioni partner, in modo che possano comunicare attraverso un trust federativo.|![Web SSO federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione partner account](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![Web SSO federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|Se si è un amministratore dell'organizzazione partner risorse, le attestazioni\-abilitare l'applicazione Web browser, l'applicazione del servizio Web o l'applicazione Microsoft&reg; Office SharePoint&reg; server usando WIF e WIF SDK.|![Web SSO federato di](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![Web SSO federato di](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
