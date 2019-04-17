---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Distribuzione di proxy Server federativi
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy Server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per distribuire i proxy server federativi in Active Directory Federation Services \(AD FS\), completare tutte le operazioni in [elenco di controllo: impostazione di un Server Proxy federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando si usa questo elenco di controllo, è consigliabile prima leggere i riferimenti per il proxy server federativo nella Guida alla pianificazione di [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di iniziare le procedure per configurare i server. L'elenco di controllo seguente offre una migliore comprensione del processo di progettazione e distribuzione per la federazione proxy server federativi.  
  
## <a name="about-federation-server-proxies"></a>Sui server federativi  
I proxy server federativi sono computer che eseguono Windows Server® 2012 e ADFS software che sono state configurate manualmente per agire nel ruolo di proxy. È possibile utilizzare i proxy server federativi nell'organizzazione per fornire servizi intermediari tra un client Internet e un server federativo che si trova dietro un firewall nella rete aziendale.  
  
> [!NOTE]  
> Anche se il server federativo e i ruoli di proxy server federativo non possono essere installati nello stesso computer, un server federativo può eseguire funzioni di proxy server federativo. Per ulteriori informazioni, vedere [sulla creazione di un Server federativo](https://technet.microsoft.com/library/dd807101.aspx).  
  
L'atto di installare il software ADFS in un computer Windows Server® 2012 e configurarlo per servire nel ruolo proxy, tale computer risulta un proxy server federativo.  
  

