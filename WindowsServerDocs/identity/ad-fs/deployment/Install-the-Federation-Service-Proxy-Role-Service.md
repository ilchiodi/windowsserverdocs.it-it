---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Installare il servizio ruolo proxy del servizio federativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8d2ff177c821baf31bb5453b7c50e3eadca2aab7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855384"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installare il servizio ruolo proxy del servizio federativo

Dopo aver configurato un computer con le applicazioni e i certificati prerequisiti, si è pronti per installare il servizio ruolo Proxy servizio federativo di Active Directory Federation Services \(AD FS\). È possibile utilizzare la procedura seguente per installare il servizio ruolo Proxy servizio federativo. Quando si installa il servizio ruolo Proxy servizio federativo in un computer, il computer diventa un proxy server federativo.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Per installare il servizio ruolo Proxy servizio federativo utilizzando il Server Manager
  
1.  Nella schermata **Start** Digitare**Server Manager**e quindi premere INVIO.  
  
2.  Fare clic su **Gestisci**, quindi su **Aggiungi ruoli e funzionalità** per avviare l'Aggiunta guidata ruoli e funzionalità.  
  
3.  Nella pagina **Prima di iniziare** fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione tipo di installazione** fare clic su **\-basata su ruoli o su\-installazione basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia evidenziato, quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Selezione ruoli server** fare clic su **accesso remoto**, quindi fare clic su Avanti.  
  
    > [!NOTE]  
    > Se viene richiesto di installare altre funzionalità di .NET Framework o del servizio Attivazione processo Windows, fare clic su **Aggiungi funzionalità** per installarle.  
  
7. Nella pagina **Selezione servizi ruolo** selezionare la casella di controllo **Proxy servizio federativo**, quindi fare clic su **Avanti**.  

8. Dopo aver verificato le informazioni nella pagina **Conferma selezioni per l'installazione**, selezionare la casella di controllo **Riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **Installa**.  
  
13. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Per installare il servizio ruolo Proxy servizio federativo tramite PowerShell

1. Aprire Windows PowerShell (Esegui come amministratore)

2. Digitare il comando seguente e premere **invio**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

