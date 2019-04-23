---
title: Verificare la registrazione di server di un certificato del server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850902"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Verificare la registrazione di server di un certificato del server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per verificare che i server di Server dei criteri di rete (NPS) hanno registrato un certificato del server dell'autorità di certificazione (CA).   
  
>[!NOTE]  
>L'appartenenza di **Domain Admins** gruppo è il requisito minimo necessario per completare queste procedure.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Verificare la registrazione di Server dei criteri di rete (NPS) di un certificato server  
  
Poiché criteri di rete viene usato per autenticare e autorizzare le richieste di connessione di rete, è importante garantire che il certificato del server che è stata eseguita per NPSs è valido se utilizzata in Criteri di rete.  
  
Per verificare che un certificato server sia configurato correttamente e viene registrato per i criteri di rete, è necessario configurare un criterio di rete di test e consentire i criteri di rete verificare che NPS è possibile usare il certificato per l'autenticazione.  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Per verificare la registrazione dei criteri di rete di un certificato server  
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Verrà visualizzata la finestra di Network Policy Server Microsoft Management Console (MMC).  
  
2.  Fare doppio clic su **criteri**, fare doppio clic su **criteri di rete**, fare clic su **nuovo**. Verrà visualizzata la procedura guidata nuovo criterio di rete.  
  
3.  In **specificare nome dei criteri di rete e il tipo di connessione**, in **Nome criterio**, tipo **testare i criteri**. Assicurarsi che **tipo di server di accesso di rete** ha il valore **Unspecified**, quindi fare clic su **Avanti**.  
  
4.  In **specificare condizioni**, fare clic su **Aggiungi**. In **Selezionare condizione**, fare clic su **gruppi di Windows**, quindi fare clic su **Aggiungi**.  
  
5.  In **gruppi**, fare clic su **Aggiungi gruppi**. In **Seleziona gruppo**, tipo **gli utenti del dominio**, quindi premere INVIO. Fare clic su **OK** e quindi su **Avanti**.  
  
6.  In **specificare l'autorizzazione di accesso**, assicurarsi che **accesso concesso** sia selezionata e quindi fare clic su **Avanti**.  
  
7.  In **Configurare metodi di autenticazione**, fare clic su **Aggiungi**. Nelle **aggiungere EAP**, fare clic su **Microsoft: PEAP (Protected EAP)**, quindi fare clic su **OK**. Nelle **tipi di EAP**, selezionare **Microsoft: PEAP (Protected EAP)**, quindi fare clic su **modifica**. Il **Modifica proprietà PEAP** verrà visualizzata la finestra di dialogo.  
  
8.  Nel **Modifica proprietà PEAP** della finestra di dialogo **certificato emesso per**, dei criteri di RETE viene visualizzato il nome del certificato server nel formato *nomecomputer*.*Dominio*. Ad esempio, se i criteri di rete è denominato NPS-01 e il dominio è example.com, dei criteri di rete Visualizza certificato **NPS 01.example.com**. Inoltre, in **dell'autorità di certificazione**, viene visualizzato il nome dell'autorità di certificazione e in **Data di scadenza**, verrà visualizzata la data di scadenza del certificato del server. Ciò dimostra che i criteri di rete ha registrato un certificato server valido che può essere utilizzata per dimostrare la propria identità ai computer client che sta tentando di accedere alla rete tramite i server di accesso di rete, ad esempio i server di rete privata virtuale (VPN), 802.1x 802.1x punti di accesso wireless, server Gateway Desktop remoto e 802.1 commutatori Ethernet 802.1x.  
  
    > [!IMPORTANT]  
    > Se Criteri di RETE non viene visualizzato un certificato server valido e fornisce il messaggio che tale certificato non viene trovato nel computer locale, esistono due possibili cause di questo problema. È possibile che Criteri di gruppo non ha aggiornato correttamente e i criteri di rete non ha registrato un certificato dall'autorità di certificazione. In questo caso, riavviare i criteri di rete. Al riavvio del computer, criteri di gruppo viene aggiornato ed è possibile eseguire questa procedura per verificare che il certificato del server è registrato. Se l'aggiornamento di criteri di gruppo non risolve il problema, il modello di certificato, la registrazione automatica del certificato o entrambi non sono configurate correttamente. Per risolvere questi problemi, all'inizio di questa guida ed eseguire tutti i passaggi per assicurarsi che le impostazioni fornite siano accurate.  
  
9. Dopo avere verificato la presenza di un certificato server valido, è possibile fare clic su **OK** e **Annulla** per uscire dalla procedura guidata nuovo criterio di rete.  
  
    > [!NOTE]  
    > Poiché non si completa la procedura guidata, i criteri di rete di test non viene creato in Criteri di RETE.  
  


