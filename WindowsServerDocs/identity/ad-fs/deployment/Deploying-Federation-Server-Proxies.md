---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Distribuzione di proxy server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816262"
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per distribuire i proxy server federativo in Active Directory Federation Services \(ADFS\), completare le attività in [elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando si usa questo elenco di controllo, è consigliabile leggere innanzitutto i riferimenti al proxy server federativo nella Guida alla pianificazione di [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di iniziare le procedure per la configurazione dei server. Seguire l'elenco di controllo fornisce una migliore comprensione del processo di progettazione e distribuzione per la federazione i proxy server federativi.  
  
## <a name="about-federation-server-proxies"></a>Sui server federativi  
Server federativi sono computer che eseguono Windows Server® 2012 e ADFS software che sono state configurate manualmente per agire nel ruolo proxy. Puoi utilizzare i proxy server federativi nell'organizzazione per fornire servizi intermediari tra un client Internet e un server federativo protetto da un firewall nella rete aziendale.  
  
> [!NOTE]  
> Anche se il server federativo e il ruolo proxy server federativo non può essere installato nello stesso computer, un server federativo può eseguire funzioni di proxy server federativo. Per altre informazioni, vedere [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
L'atto di installare il software ADFS in un computer Windows Server® 2012 e configurarlo per servire nel ruolo proxy rende tale computer proxy server federativo.  
  

