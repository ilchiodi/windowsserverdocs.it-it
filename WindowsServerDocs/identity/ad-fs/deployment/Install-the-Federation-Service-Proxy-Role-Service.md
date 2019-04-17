---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Installare il servizio ruolo Proxy servizio federativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installare il servizio ruolo Proxy servizio federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver configurato un computer con le applicazioni prerequisito e i certificati, si è pronti per installare il servizio ruolo Proxy servizio federativo di Active Directory Federation Services \(AD FS\). È possibile utilizzare la procedura seguente per installare il servizio ruolo Proxy servizio federativo. Quando si installa il servizio ruolo Proxy servizio federativo in un computer, il computer diventa un proxy server federativo.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>Per installare il servizio ruolo Proxy servizio federativo  
  
1.  Nel **Start** digitare**Server Manager**, quindi premere INVIO.  
  
2.  Fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli e funzionalità.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su installazione**e fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione viene evidenziato e quindi fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **Active Directory Federation Services**e quindi fare clic su Avanti.  
  
    > [!NOTE]  
    > Se viene chiesto di installare funzionalità aggiuntive di .NET Framework o servizio Attivazione processo Windows, fare clic su **Aggiungi funzionalità** di installarli.  
  
7.  Nel **selezionare le funzionalità** verificare che le funzionalità siano impostate e quindi fare clic su **Avanti**.  
  
8.  Nel **Active Directory Federation Services \(AD FS\)** pagina, fare clic su **Avanti**.  
  
9. Nel **Selezione servizi ruolo** pagina, selezionare il **Proxy servizio federativo** casella di controllo, quindi fare clic su **Avanti**.  
  
10. Nel **ruolo Server Web \(IIS\)** pagina, fare clic su **Avanti**.  
  
11. Nel **Selezione servizi ruolo** pagina, fare clic su **Avanti**.  
  
12. Dopo aver verificato le informazioni sul **Conferma selezioni per l'installazione** pagina, selezionare il **riavvia automaticamente il server di destinazione se necessario** casella di controllo, quindi fare clic su **installare**.  
  
13. Nel **lo stato dell'installazione** pagina, verificare che tutto sia stato installato correttamente e quindi fare clic su **Chiudi**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

