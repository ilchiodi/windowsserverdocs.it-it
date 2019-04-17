---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: Elenco di controllo - implementazione di una progettazione di Web SSO
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo elenco di controllo padre include i collegamenti a concetti importanti sul Web \(SSO\) apartment accesso nella progettazione per Active Directory Federation Services \(AD FS\) cross\ riferimento. Contiene anche collegamenti a elenchi di controllo che consentiranno di completare le attività necessarie per implementare questa progettazione subordinati.  
  
> [!NOTE]  
> Completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare a questo argomento dopo aver consultato l'argomento concettuale o completare le attività nell'elenco di controllo subordinato, in modo che è possibile procedere con le attività rimanenti nell'elenco di controllo.  
  
![Web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: implementazione di una progettazione di Web SSO**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![Web sso](media/icon_checkboxo.gif)|Rivedere i concetti importanti sul progetto di Web SSO e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione. **Nota:**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione di Web SSO](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificazione degli obiettivi di distribuzione di AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|Esaminare i requisiti hardware, software, certificato, \(DNS\) Domain Name System, archivio di attributi e client per la distribuzione di ADFS nell'organizzazione.|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[appendice a: revisione AD requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi nella rete aziendale o nella rete perimetrale. **Nota:** la progettazione di Web SSO richiede solo un server federativo funzioni correttamente. Un server federativo singolo svolge il ruolo di provider di attestazioni sia relying party ruolo.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Checklist: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Web sso](media/icon_checkboxo.gif)|\(Optional\) determinare se l'organizzazione ha bisogno di un proxy server federativo nella rete perimetrale.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: impostazione di un Server Proxy federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Web sso](media/icon_checkboxo.gif)|A seconda del piano di progettazione di Web SSO Federativo e come si intende usarlo, aggiungere l'archivio di attributi appropriati, trust della relying party, le attestazioni e le regole per il servizio federativo.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Web sso](media/icon_checkboxo.gif)|Se sei un amministratore nell'organizzazione partner risorse, claims\ abilitare l'applicazione Web browser, applicazione servizio Web o applicazione Microsoft® Office SharePoint® Server mediante WIF e WIF SDK. **Nota:**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
