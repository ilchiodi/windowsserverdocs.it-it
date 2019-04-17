---
title: Configurare il modello di certificato Server
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>Configurare il modello di certificato Server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per configurare il modello di certificato da Active Directory&reg; utilizzato da Servizi certificati (AD CS) come base per i certificati server registrati nei server sulla rete.  
  
Durante la configurazione di questo modello, è possibile specificare i server dal gruppo di Active Directory che deve ricevere automaticamente un certificato server da Servizi certificati Active Directory.   
  
La procedura riportata di seguito vengono fornite istruzioni per la configurazione del modello per emettere certificati a tutti i tipi di server seguenti:  
  
- Server che eseguono il servizio di accesso remoto, inclusi i server Gateway RAS, che sono membri del **server RAS e IAS** gruppo.  
- Server che eseguono il servizio Server dei criteri di rete (NPS) che sono membri del **server RAS e IAS** gruppo.  
  
L'appartenenza a entrambi **Enterprise Admins** e del dominio radice **Domain Admins** gruppo è il requisito minimo necessario per completare questa procedura.  
  
### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato  
  
1.  Nel CA1, in Server Manager, fare clic su **strumenti**, quindi fare clic su **autorità di certificazione**. Verrà visualizzata la finestra di MMC Autorità di certificazione Microsoft Management Console ().  
  
2.  In MMC, fare doppio clic sul nome della CA, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestisci**.  
  
3.  Apre la console modelli di certificato. Nel riquadro dei dettagli verranno visualizzati tutti i modelli di certificato.  
  
4.  Nel riquadro dei dettagli, fare clic su di **Server RAS e IAS** modello.  
  
5.  Fare clic su di **azione** menu, quindi fare clic su **Duplica modello**. Il modello **proprietà** apre la finestra di dialogo.  
  
6.  Fare clic su di **sicurezza** scheda.   
  
7.  Nel **sicurezza** scheda **utenti e gruppi**, fare clic su **server RAS e IAS**.  
  
8.  In **le autorizzazioni per server RAS e IAS**, in **Consenti**, assicurarsi che **registrazione** sia selezionata e quindi seleziona il **registrazione automatica** casella di controllo. Fare clic su **OK**e chiudere MMC Modelli di certificato.  
  
9.  In MMC Autorità di certificazione, fare clic su **modelli di certificato**. Nel **azione** dal menu **New**, quindi fare clic su **modello di certificato da rilasciare**. Il **Attivazione modelli di certificato** apre la finestra di dialogo.  
  
10. In **Attivazione modelli di certificato**, fare clic sul nome del modello di certificato appena configurato e quindi fare clic su **OK**. Ad esempio, se non è stato modificato il nome del modello di certificato predefinito, fare clic su **copia di Server RAS e IAS**, quindi fare clic su **OK**.  
  


