---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Impostare un certificato per le comunicazioni di servizi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888472"
---
# <a name="set-a-service-communications-certificate"></a>Impostare un certificato per le comunicazioni di servizi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server federativo in Active Directory Federation Services \(ADFS\) usare il certificato di comunicazioni di servizi per proteggere il traffico dei servizi Web per Secure Sockets Layer \(SSL\) la comunicazione con Web i client o server federativi. Questo è lo stesso certificato che usa un server federativo come certificato SSL in Internet Information Services \(IIS\).  
  
È possibile usare la procedura seguente per modificare il certificato di comunicazioni di servizi con lo snap di gestione di AD FS\-in.  
  
> [!NOTE]  
> Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Per impostare un certificato di comunicazioni di servizi  
  
1.  Nel **avviare** digitare**gestione di AD FS**, e quindi premere INVIO.  
  
2.  Nell'albero della console, fare doppio\-fare clic su **Service**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic sui **certificato per le comunicazioni del servizio impostato** collegamento.  
  
4.  Nel **selezionare un certificato di comunicazioni di servizi** finestra di dialogo passare al file del certificato che si desidera impostare come certificato per le comunicazioni del servizio, selezionare il file del certificato e quindi fare clic su **aprire**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

