---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Installare il servizio ruolo del servizio federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 80a6cb2bc8e6f0fdb1a777a42f5d245f98ac3dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192095"
---
# <a name="install-the-federation-service-role-service"></a>Installare il servizio ruolo del servizio federativo

Ora che è configurato correttamente un computer con le applicazioni prerequisito e i certificati, si è pronti per installare il servizio ruolo servizio federativo di Active Directory Federation Services \(ADFS\). Quando si installa il servizio federativo in un computer, il computer diventa un server federativo.  
  
> [!NOTE]  
> Per il singolo Web federata\-Sign\-sul \(SSO\) progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse . Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
È possibile utilizzare la procedura seguente per installare il servizio ruolo servizio federativo di AD FS in un computer che diventerà il primo server federativo o in un computer che diventerà un server federativo per una server farm federativa esistente.  
  
## <a name="prerequisites"></a>Prerequisiti  
Verificare che un certificato SSL con la chiave privata è già stato installato o importato nell'archivio certificati locale \(archivio personale\) prima di iniziare questa procedura. Se si intende usare un token\-firma certificato rilasciato da un'autorità di certificazione \(autorità di certificazione\), verificare che un token di\-certificato di firma con la chiave privata è già stato installato o importato in archivio certificati locale \(archivio personale\) prima di iniziare questa procedura. In alternativa, è possibile creare un self\-effettuato l'accesso, token\-firma del certificato tramite l'aggiunta guidata ruoli, come descritto in questa procedura. Per altre informazioni sui token\-firma dei certificati, vedere [Certificate Requirements for Federation Servers](https://technet.microsoft.com/library/dd807040.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Per installare il servizio ruolo del servizio federativo  
  
1.  Nel **avviare** digitare**Server Manager**, quindi premere INVIO.  
  
2.  Fare clic su **Manage**, quindi fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli e funzionalità.  
  
3.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **ruolo\-o basata su funzionalità\-installazione basata su**, fare clic su **Avanti**.  
  
5.  Nel **Selezione server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione è evidenziato e quindi fare clic su **Next**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **Active Directory Federation Services**e quindi fare clic su Avanti.  
  
    > [!NOTE]  
    > Se viene chiesto di installare ulteriori funzionalità di .NET Framework o il servizio Attivazione processo Windows, fare clic su **Aggiungi funzionalità** installarli.  
  
7.  Nel **Selezione funzionalità** pagina, verificare che le funzionalità sono impostate e quindi fare clic su **successivo**.  
  
8.  Nel **Active Directory Federation Services \(ADFS\)**  fare clic su **Next**.  
  
9. Nel **Selezione servizi ruolo** pagina, selezionare la **servizio federativo** casella di controllo e quindi fare clic su **Next**.  
  
10. Nel **ruolo Server Web \(IIS\)**  fare clic su **Next**.  
  
11. Nella pagina **Selezione servizi ruolo** fare clic su **Avanti**.  
  
12. Dopo avere verificato le informazioni nella pagina **Conferma selezioni per l'installazione** , selezionare la casella di controllo **Riavvia automaticamente il server di destinazione se necessario** e quindi fare clic su **Installa**.  
  
13. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  
  

