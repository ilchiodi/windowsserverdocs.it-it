---
title: Configurare il modello di certificato del server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886122"
---
# <a name="configure-the-server-certificate-template"></a>Configurare il modello di certificato del server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare il modello di certificato da Active Directory&reg; Servizi certificati (AD CS) viene utilizzato come base per i certificati server registrati nei server sulla rete.  
  
Quando si configura questo modello, è possibile specificare i server dal gruppo di Active Directory che deve ricevere automaticamente un certificato server da Servizi certificati Active Directory.   
  
La procedura riportata di seguito include istruzioni per la configurazione del modello per emettere certificati per tutti i tipi di server seguenti:  
  
- I server che eseguono il servizio di accesso remoto, compresi i server Gateway RAS, che sono membri del **server RAS e IAS** gruppo.  
- I server che eseguono il servizio Strumentazione gestione Windows (NPS, Network Policy Server) che sono membri di **server RAS e IAS** gruppo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins** e al gruppo **Domain Admins** del dominio radice.  
  
### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato  
  
1.  Nel CA1, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **autorità di certificazione**. Verrà visualizzata la finestra di MMC Autorità di certificazione Microsoft Management Console ().  
  
2.  In MMC fare doppio clic sul nome della CA, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestisci**.  
  
3.  Apre la console modelli di certificato. Tutti i modelli di certificato vengono visualizzati nel riquadro dei dettagli.  
  
4.  Nel riquadro dei dettagli, fare clic sui **Server RAS e IAS** modello.  
  
5.  Scegliere il **azione** dal menu e quindi fare clic su **Duplica modello**. Il modello **proprietà** verrà visualizzata la finestra di dialogo.  
  
6.  Fare clic sulla scheda **Sicurezza**.   
  
7.  Nel **sicurezza** nella scheda **nomi utente o gruppo**, fare clic su **server RAS e IAS**.  
  
8.  In **le autorizzazioni per server RAS e IAS**, in **Consenti**, verificare che **registrazione** sia selezionata e quindi selezionare il **registrazione automatica** controllare casella. Fare clic su **OK**, chiudere MMC di modelli di certificato.  
  
9.  In MMC Autorità di certificazione, fare clic su **modelli di certificato**. Nel **azione** dal menu **New**, quindi fare clic su **modello di certificato da emettere**. Verrà aperta la finestra di dialogo **Attivazione modelli di certificato**.  
  
10. Nelle **Attivazione modelli di certificato**, fare clic sul nome del modello di certificato appena configurata e quindi fare clic su **OK**. Ad esempio, se non è stato modificato il nome di modello di certificato predefinito, fare clic su **copia di Server RAS e IAS**, quindi fare clic su **OK**.  
  


