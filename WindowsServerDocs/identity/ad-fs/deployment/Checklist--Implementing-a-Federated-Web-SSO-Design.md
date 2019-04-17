---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: Elenco di controllo - implementazione di un progetto Web SSO federativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo elenco di controllo padre include i collegamenti a concetti importanti sul progetto di \(SSO\) Single\-Sign\-On Web federata per Active Directory Federation Services \(AD FS\) cross\ riferimento. Contiene anche collegamenti a elenchi di controllo che consentiranno di completare le attività necessarie per implementare questa progettazione subordinati.  
  
> [!NOTE]  
> Completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare a questo argomento dopo rivedere l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo che è possibile procedere con le attività rimanenti nell'elenco di controllo.  
  
![web sso federato](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**Checklist: Implementing a Federated Web SSO Design**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sul progetto di Web SSO federativo e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificazione degli obiettivi di distribuzione di AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Esaminare i requisiti hardware, software, certificato, \(DNS\) Domain Name System, archivio di attributi e client per la distribuzione di ADFS nell'organizzazione.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[appendice a: revisione AD requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso web federativo](media/icon_checkboxo.gif)|Rivedere i concetti importanti sulle attestazioni, regole, archivi di attributi e il database di configurazione di ADFS di attestazione prima di distribuire ADFS in entrambe le organizzazioni partner.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi in ogni organizzazione partner. **Nota:** per la progettazione di Web SSO federativo, è necessario almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Checklist: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|\(Optional\) determinare se l'organizzazione ha bisogno di un proxy server federativo. Se il piano di progettazione chiama per un proxy, è possibile installare uno o più proxy server federativi in ogni organizzazione partner.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: impostazione di un Server Proxy federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|In base al piano di progettazione, condividere i certificati, configurare i client e configurare le relazioni di trust in entrambe le organizzazioni partner, in modo che possano comunicare attraverso un trust federativo.|![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![web sso federato](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione Partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso web federativo](media/icon_checkboxo.gif)|Se sei un amministratore nell'organizzazione partner risorse, claims\ abilitare l'applicazione Web browser, applicazione servizio Web o applicazione Microsoft® Office SharePoint® Server mediante WIF e WIF SDK.|![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![web sso federato](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
