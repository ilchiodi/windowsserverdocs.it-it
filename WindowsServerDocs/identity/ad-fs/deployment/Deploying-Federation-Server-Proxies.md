---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Distribuzione di proxy server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c6af319283de72963691ae3e91c3db5992bdec72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408407"
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy server federativi

Per distribuire i proxy server federativi in Active Directory Federation Services \(AD FS\), completare tutte le attività in [elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando si utilizza questo elenco di controllo, prima di iniziare le procedure per la configurazione dei server, è consigliabile leggere prima di tutto i riferimenti alle linee guida per la pianificazione del proxy server federativo nella [Guida alla progettazione di ad FS in Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) . L'elenco di controllo seguente fornisce una migliore comprensione del processo di progettazione e distribuzione per i proxy server federativi.  
  
## <a name="about-federation-server-proxies"></a>Informazioni sui proxy server federativi  
I proxy server federativi sono computer che eseguono Windows Server® 2012 e AD FS software che sono stati configurati manualmente per agire nel ruolo Proxy. Puoi utilizzare i proxy server federativi nell'organizzazione per fornire servizi intermediari tra un client Internet e un server federativo protetto da un firewall nella rete aziendale.  
  
> [!NOTE]  
> Sebbene i ruoli server federativo e proxy server federativo non possano essere installati nello stesso computer, un server federativo può eseguire le funzioni proxy server federativo. Per altre informazioni, vedere [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
Il fatto di installare il software AD FS in un computer Windows Server® 2012 e configurarlo in modo che funzioni nel ruolo Proxy rende il computer un proxy server federativo.  
  

