---
title: Verificare la registrazione di Server di un certificato Server
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Verificare la registrazione di Server di un certificato Server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per verificare che il server di Server dei criteri di rete (NPS) hanno registrato un certificato del server dell'autorità di certificazione (CA).   
  
>[!NOTE]  
>Appartenenza al gruppo di **Domain Admins** gruppo è il requisito minimo necessario per completare queste procedure.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Verificare la registrazione di Server dei criteri di rete (NPS) di un certificato server  
  
Poiché dei criteri di rete viene utilizzato per autenticare e autorizzare le richieste di connessione di rete, è importante assicurarsi che il certificato del server che è stata immessa per server dei criteri di rete sia valido se utilizzata in Criteri di rete.  
  
Per verificare che un certificato server sia configurato correttamente e viene registrato il server dei criteri di rete, è necessario configurare un criterio di rete di test e consentire i criteri di rete verificare che Criteri di rete è possibile utilizzare il certificato per l'autenticazione.  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>Per verificare la registrazione di server dei criteri di rete di un certificato server  
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Verrà visualizzata la finestra di Network Policy Server Microsoft Management Console (MMC).  
  
2.  Fare doppio clic su **criteri**, fare doppio clic su **criteri di rete**e fare clic su **New**. Apre la procedura guidata nuovo criterio di rete.  
  
3.  In **specificare nome dei criteri di rete e tipo di connessione**, in **nome criterio**, tipo **testare i criteri**. Assicurarsi che **tipo di server di accesso di rete** ha il valore **Unspecified**, quindi fare clic su **Avanti**.  
  
4.  In **specificare condizioni**, fare clic su **Aggiungi**. In **selezionare condizione**, fare clic su **gruppi Windows**, quindi fare clic su **Aggiungi**.  
  
5.  In **gruppi**, fare clic su **Aggiungi gruppi**. In **Seleziona gruppo**, tipo **Domain Users**, quindi premere INVIO. Fare clic su **OK**, quindi fare clic su **Avanti**.  
  
6.  In **specificare l'autorizzazione di accesso**, assicurarsi che **accesso concesso** sia selezionata e quindi fare clic su **Avanti**.  
  
7.  In **configurare metodi di autenticazione**, fare clic su **Aggiungi**. In **aggiungere EAP**, fare clic su **Microsoft: PEAP (Protected EAP)**, quindi fare clic su **OK**. In **tipi EAP**selezionare **Microsoft: PEAP (Protected EAP)**, quindi fare clic su **modifica**. Il **Modifica proprietà PEAP** apre la finestra di dialogo.  
  
8.  Nel **Modifica proprietà PEAP** della finestra di dialogo **certificato emesso per**, dei criteri di rete Visualizza il nome del certificato server nel formato *ComputerName*. *Dominio*. Ad esempio, se il server dei criteri di rete è denominato NPS-01 e il dominio è example.com, dei criteri di rete viene visualizzato il certificato **NPS-01.example.com**. Inoltre, in **dell'autorità di certificazione**, viene visualizzato il nome dell'autorità di certificazione e in **data di scadenza**, verrà visualizzata la data di scadenza del certificato del server. Ciò dimostra che il server dei criteri di rete ha registrato un certificato server valido da utilizzare per dimostrare la propria identità ai computer client che sta tentando di accedere alla rete tramite i server di accesso di rete, ad esempio i server di rete privata virtuale (VPN), 802.1 X-punti di accesso wireless, server Gateway Desktop remoto e 802.1 commutatori Ethernet che supportano X.  
  
    > [!IMPORTANT]  
    > Se i criteri di rete non viene visualizzato un certificato server valido e fornisce il messaggio che tale certificato non è possibile trovare nel computer locale, esistono due possibili cause di questo problema. È possibile che Criteri di gruppo non ha aggiornato correttamente e il server dei criteri di rete non ha registrato un certificato dall'autorità di certificazione. In questo caso, riavviare il server dei criteri di rete. Al riavvio del computer, criteri di gruppo viene aggiornato ed è possibile eseguire questa procedura per verificare che il certificato del server è registrato. Se l'aggiornamento dei criteri di gruppo non risolve questo problema, il modello di certificato, la registrazione automatica del certificato o entrambi non sono configurate correttamente. Per risolvere questi problemi, eseguire tutti i passaggi per assicurarsi che le impostazioni fornite siano accurate avviare all'inizio di questa Guida.  
  
9. Dopo avere verificato la presenza di un certificato server valido, è possibile fare clic su **OK** e **Annulla** per uscire dalla procedura guidata nuovo criterio di rete.  
  
    > [!NOTE]  
    > Perché non si completa la procedura guidata, i criteri di rete di test non viene creato in Criteri di rete.  
  


