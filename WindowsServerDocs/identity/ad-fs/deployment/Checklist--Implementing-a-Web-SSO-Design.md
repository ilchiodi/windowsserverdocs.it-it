---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Elenco di controllo: implementazione di un progetto Web SSO'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864042"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo elenco di controllo padre include incrociato\-fanno riferimento i collegamenti a concetti importanti sul Web Single\-Sign\-sul \(SSO\) progettazione di Active Directory Federation Services \(AD FS \). Contiene inoltre collegamenti a elenchi di controllo subordinati che consentiranno di completare le attività necessarie per implementare il progetto.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Se un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare in questo argomento dopo aver consultato l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo da procedere con le attività rimanenti.  
  
![Web SSO federativo](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Implementazione di un progetto Web SSO**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![Web sso](media/icon_checkboxo.gif)|Esaminare i concetti importanti sul progetto di Web SSO e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione. **Nota:**|![Web SSO federativo](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione di Web SSO](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Web SSO federativo](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificazione degli obiettivi di distribuzione di AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|Esaminare i componenti hardware, software, certificati, Domain Name System \(DNS\), archivio di attributi e sui requisiti client per la distribuzione di ADFS nell'organizzazione.|![Web SSO federativo](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[appendice a: Requisiti per ADFS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi nella rete aziendale o nella rete perimetrale. **Nota:** La progettazione di Web SSO richiede solo un server federativo funzioni correttamente. Un server federativo singolo svolge il ruolo di provider di attestazioni sia relying party ruolo.|![Web SSO federativo](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Web sso](media/icon_checkboxo.gif)|\(Facoltativo\) determinare se l'organizzazione ha bisogno di un proxy server federativo nella rete perimetrale.|![Web SSO federativo](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Web sso](media/icon_checkboxo.gif)|In base al piano di progetto di Web SSO e a come si intende usarlo, aggiungere l'archivio di attributi, i trust della relying party, le attestazioni e le regole di attestazione appropriate al Servizio federativo.|![Web SSO federativo](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Web sso](media/icon_checkboxo.gif)|Se sei un amministratore nell'organizzazione partner risorse, le attestazioni\-abilitare un'applicazione Web browser, applicazione di servizio Web e applicazione di Microsoft® Office SharePoint® Server mediante WIF e WIF SDK. **Nota:**|![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
