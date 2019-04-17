---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificare che un Proxy Server federativo sia operativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificare che un Proxy Server federativo sia operativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare la procedura seguente per verificare che il proxy server federativo possa comunicare con il servizio federativo in Active Directory Federation Services \(AD FS\). Eseguire questa procedura dopo aver eseguito la **configurazione guidata di AD FS Federation Server Proxy** per configurare il computer per eseguire il ruolo di proxy server federativo. Per ulteriori informazioni su come eseguire questa procedura guidata, vedere [configurare un Computer per il ruolo di Proxy Server federativo](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Il risultato di questo test è la generazione di un evento specifico nel Visualizzatore eventi sul computer proxy server federativo.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Per verificare che un proxy server federativo sia operativo  
  
1.  Accedere al proxy server federativo come amministratore.  
  
2.  Nel **Start** digitare**Visualizzatore eventi**, quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli, fare doppio clic **registri applicazioni e servizi**, fare doppio clic **AD FS Eventing**, quindi fare clic su **Admin**.  
  
4.  Nel **ID evento** colonna, cercare l'evento ID 198.  
  
    Se il proxy server federativo è configurato correttamente, verrà visualizzato un nuovo evento nel registro delle applicazioni del Visualizzatore eventi, con l'evento ID 198. Questo evento verifica che il servizio proxy server federativo è stato avviato e adesso è in linea.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

