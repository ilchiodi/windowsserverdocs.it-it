---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Impostare un certificato di comunicazioni di servizio
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>Impostare un certificato di comunicazioni di servizio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server federativo in Active Directory Federation Services \(AD FS\) utilizzare il certificato di comunicazione del servizio per proteggere il traffico dei servizi Web per la comunicazione Secure Sockets Layer \(SSL\) con i client del Web o proxy server federativi. Questo è lo stesso certificato che utilizza un server federativo come il certificato SSL in Internet Information Services \(IIS\).  
  
È possibile utilizzare la procedura seguente per modificare il certificato di comunicazioni di servizio con la gestione di ADFS snap-in.  
  
> [!NOTE]  
> Snap-in di gestione di ADFS fa riferimento ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Per impostare un certificato di comunicazioni di servizi  
  
1.  Nel **Start** digitare**gestione di ADFS**, quindi premere INVIO.  
  
2.  Nell'albero della console fare doppio clic **servizio**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic su di **certificato per le comunicazioni del servizio impostato** collegamento.  
  
4.  Nel **selezionare un certificato di comunicazioni di servizio** finestra di dialogo casella, passare al file del certificato che si desidera impostare come certificato di comunicazione del servizio, selezionare il file di certificato e quindi fare clic su **aprire**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

