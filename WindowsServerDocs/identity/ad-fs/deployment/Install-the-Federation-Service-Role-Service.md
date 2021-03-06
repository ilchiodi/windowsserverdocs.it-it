---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Installare il servizio ruolo del servizio federativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4952fad89502f9d81faab86cfd2e0e056543defd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855374"
---
# <a name="install-the-federation-service-role-service"></a>Installare il servizio ruolo del servizio federativo

Ora che un computer è stato configurato correttamente con le applicazioni e i certificati prerequisiti, è possibile installare il servizio ruolo Servizio federativo di Active Directory Federation Services \(AD FS\). Quando si installa il Servizio federativo in un computer, il computer diventa un server federativo.  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
È possibile utilizzare la procedura seguente per installare il servizio ruolo Servizio federativo di AD FS in un computer che diventerà il primo server federativo o in un computer che diventerà un server federativo per una server farm federativa esistente.  
  
## <a name="prerequisites"></a>Prerequisiti  
Verificare che un certificato SSL con la chiave privata sia già stato installato o importato nell'archivio certificati locale \(archivio personale\) prima di avviare questa procedura. Se si utilizzerà un token\-certificato di firma emesso da un'autorità di certificazione \(CA\), verificare che un token\-certificato di firma con la chiave privata sia già stato installato o importato nell'archivio certificati locale \(archivio personale\) prima di avviare questa procedura. In alternativa, è possibile creare un certificato auto\-firmato, un token\-firma utilizzando l'aggiunta guidata ruoli, come descritto in questa procedura. Per ulteriori informazioni sui certificati per la firma di token\-, vedere [requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx).  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Per installare il servizio ruolo del servizio federativo  
  
1.  Nella schermata **Start** Digitare**Server Manager**e quindi premere INVIO.  
  
2.  Fare clic su **Gestisci**, quindi su **Aggiungi ruoli e funzionalità** per avviare l'Aggiunta guidata ruoli e funzionalità.  
  
3.  Nella pagina **Prima di iniziare** fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione tipo di installazione** fare clic su **\-basata su ruoli o su\-installazione basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia evidenziato, quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Selezione ruoli server** fare clic su **Active Directory Federation Services**, quindi su Avanti.  
  
    > [!NOTE]  
    > Se viene richiesto di installare altre funzionalità di .NET Framework o del servizio Attivazione processo Windows, fare clic su **Aggiungi funzionalità** per installarle.  
  
7.  Nella pagina **Selezione funzionalità** verificare che le funzionalità siano impostate, quindi fare clic su **Avanti**.  
  
8.  Nella pagina **Active Directory Servizio federativo \(AD FS\)** fare clic su **Avanti**.  
  
9. Nella pagina **Selezione servizi ruolo** selezionare la casella di controllo **Servizio federativo**, quindi fare clic su **Avanti**.  
  
10. Nella pagina **ruolo server Web \(IIS\)** fare clic su **Avanti**.  
  
11. Nella pagina **Selezione servizi ruolo** fare clic su **Avanti**.  
  
12. Dopo aver verificato le informazioni nella pagina **Conferma selezioni per l'installazione**, selezionare la casella di controllo **Riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **Installa**.  
  
13. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  
  

