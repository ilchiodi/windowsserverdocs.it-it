---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Installare il servizio ruolo proxy del servizio federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed66800aa6bbfdf85816a992ee8eb39799efebb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865232"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installare il servizio ruolo proxy del servizio federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver configurato un computer con le applicazioni prerequisito e i certificati, si è pronti per installare il servizio ruolo Proxy servizio federativo di Active Directory Federation Services \(ADFS\). È possibile utilizzare la procedura seguente per installare il servizio ruolo Proxy servizio federativo. Quando si installa il servizio ruolo Proxy servizio federativo in un computer, il computer diventa un proxy server federativo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Per installare il servizio ruolo Proxy servizio federativo utilizzando Server Manager
  
1.  Nel **avviare** digitare**Server Manager**, quindi premere INVIO.  
  
2.  Fare clic su **Manage**, quindi fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli e funzionalità.  
  
3.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **ruolo\-o basata su funzionalità\-installazione basata su**, fare clic su **Avanti**.  
  
5.  Nel **Selezione server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione è evidenziato e quindi fare clic su **Next**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **accesso remoto**e quindi fare clic su Avanti.  
  
    > [!NOTE]  
    > Se viene chiesto di installare ulteriori funzionalità di .NET Framework o il servizio Attivazione processo Windows, fare clic su **Aggiungi funzionalità** installarli.  
  
7. Nel **Selezione servizi ruolo** pagina, selezionare la **Proxy servizio federativo** casella di controllo e quindi fare clic su **Next**.  

8. Dopo avere verificato le informazioni nella pagina **Conferma selezioni per l'installazione** , selezionare la casella di controllo **Riavvia automaticamente il server di destinazione se necessario** e quindi fare clic su **Installa**.  
  
13. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Per installare il servizio ruolo Proxy servizio federativo con PowerShell

1. Aprire Windows PowerShell (Esegui come amministratore)

2. Digitare il seguente comando e premere **invio**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

