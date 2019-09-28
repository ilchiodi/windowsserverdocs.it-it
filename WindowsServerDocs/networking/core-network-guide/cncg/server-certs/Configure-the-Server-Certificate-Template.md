---
title: Configurare il modello di certificato del server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35d5875c78dcd92f3b40b919568dabcf0d45d673
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356765"
---
# <a name="configure-the-server-certificate-template"></a>Configurare il modello di certificato del server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare il modello di certificato utilizzato da Active Directory Servizi certificati @ no__t-0 (AD CS) come base per i certificati server registrati nei server della rete.  
  
Durante la configurazione di questo modello, è possibile specificare i server per Active Directory gruppo che deve ricevere automaticamente un certificato server da AD CS.   
  
Nella procedura seguente sono incluse le istruzioni per la configurazione del modello per emettere certificati per tutti i tipi di server seguenti:  
  
- Server che eseguono il servizio di accesso remoto, inclusi i server gateway RAS, che sono membri del gruppo di **server RAS e IAS** .  
- I server che eseguono il servizio Strumentazione gestione Windows (NPS, Network Policy Server) che sono membri di **server RAS e IAS** gruppo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins** e al gruppo **Domain Admins** del dominio radice.  
  
### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato  
  
1.  In CA1, in Server Manager, fare clic su **strumenti**e quindi su **autorità di certificazione**. Si apre Microsoft Management Console (MMC) dell'autorità di certificazione.  
  
2.  In MMC fare doppio clic sul nome della CA, fare clic con il pulsante destro del mouse su **modelli di certificato**, quindi scegliere **Gestisci**.  
  
3.  Verrà visualizzata la console modelli di certificato. Tutti i modelli di certificato vengono visualizzati nel riquadro dei dettagli.  
  
4.  Nel riquadro dei dettagli fare clic sul modello **server RAS e IAS** .  
  
5.  Fare clic sul menu **azione** , quindi fare clic su **Duplica modello**. Verrà visualizzata la finestra di dialogo **Proprietà** modello.  
  
6.  Fare clic sulla scheda **Sicurezza**.   
  
7.  Nella scheda **sicurezza** , in **utenti e gruppi**, fare clic su **server RAS e IAS**.  
  
8.  In **autorizzazioni per server RAS e IAS**, in **Consenti**, assicurarsi che la casella di controllo **registra** sia selezionata, quindi selezionare la casella di controllo **registrazione automatica** . Fare clic su **OK**e chiudere lo MMC modelli di certificato.  
  
9.  In MMC Autorità di certificazione fare clic su **modelli di certificato**. Scegliere **nuovo**dal menu **azione** , quindi fare clic su **modello di certificato da emettere**. Verrà aperta la finestra di dialogo **Attivazione modelli di certificato**.  
  
10. In **Abilita modelli di certificato**, fare clic sul nome del modello di certificato appena configurato, quindi fare clic su **OK**. Se, ad esempio, non è stato modificato il nome del modello di certificato predefinito, fare clic su **copia del server RAS e IAS**, quindi fare clic su **OK**.  
  


