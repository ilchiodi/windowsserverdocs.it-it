---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Elenco di controllo: implementazione di un progetto SSO Web federativo'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869672"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo elenco di controllo padre include incrociato\-fanno riferimento i collegamenti a concetti importanti sul singolo Web federata\-Sign\-sul \(SSO\) progettazione di Active Directory Federation Services \(AD FS\). Contiene inoltre collegamenti a elenchi di controllo subordinati che consentiranno di completare le attività necessarie per implementare il progetto.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Se un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare in questo argomento dopo aver consultato l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo da procedere con le attività rimanenti.  
  
![web sso federato](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Implementazione di un progetto SSO Web federativo**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sul Web SSO federativo e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificazione degli obiettivi di distribuzione di AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Esaminare i componenti hardware, software, certificati, Domain Name System \(DNS\), archivio di attributi e sui requisiti client per la distribuzione di ADFS nell'organizzazione.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[appendice a: Requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sulle attestazioni, regole, archivi di attributi e il database di configurazione di ADFS di attestazione prima di distribuire ADFS in entrambe le organizzazioni partner.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi in ogni organizzazione partner. **Nota:** Per la progettazione di Web SSO federativo, è necessario almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|\(Facoltativo\) determinare se l'organizzazione ha bisogno di un proxy server federativo. Se il piano di progettazione chiama per un proxy, è possibile installare uno o più proxy server federativi in ogni organizzazione partner.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progetto, condividere i certificati e configurare i client e le relazioni di trust in entrambe le organizzazioni partner, in modo che possano comunicare attraverso un trust federativo.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione dell'organizzazione Partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|Se sei un amministratore nell'organizzazione partner risorse, le attestazioni\-abilitare un'applicazione Web browser, applicazione di servizio Web e applicazione di Microsoft® Office SharePoint® Server mediante WIF e WIF SDK.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
