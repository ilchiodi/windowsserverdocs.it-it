---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Creare un attestazioni Non compatibile con Trust della Relying Party
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Creare un'attendibilità componente Non-Claims-Aware

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Nella gestione di ADFS snap-in, claims\-compatibile con trust della relying party sono oggetti che vengono creati per rappresentare la relazione di trust tra il servizio federativo e un'applicazione basata sul Web singola che non è compatibile con claims\ e che si accede tramite il Proxy dell'applicazione Web.  
  
Un trust della relying party compatibile claims\ con è un trust della relying party costituito da identificatori, nomi e regole per l'autenticazione e autorizzazione quando l'attendibilità avviene tramite il Proxy dell'applicazione Web. Queste applicazioni basata sul Web non basate su attestazioni, in altre parole, queste applicazioni basate su Authentication\ integrata di Windows, può avere regole di autorizzazione che consentono l'accesso che è basato sulle attestazioni per l'accesso è esterno alla rete aziendale tramite il Proxy dell'applicazione Web.  
  
Per aggiungere un nuovo claims\-compatibile con trust della relying party, utilizzando la gestione di ADFS snap-in, eseguire la procedura seguente.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Per creare manualmente un attestazioni non compatibile con Trust della Relying Party 
1. In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale**, selezionare **grado di riconoscere attestazioni Non** e fare clic su **Start**.  
![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Nel **Specifica nome visualizzato**, digitare un nome in **nome visualizzato**, in **note** digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti **.  
![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Nel **Configura identificatori** pagina specificare uno o più identificatori per la relying party, fare clic su **Aggiungi** aggiungerli all'elenco, quindi fare clic su **Avanti**.  
![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Nel **scegliere Criteri di controllo di accesso** selezionare un criterio e fare clic su **Avanti**.  Per ulteriori informazioni sui criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](Access-Control-Policies-in-AD-FS.md). 
![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Nel **Aggiunta attendibilità** pagina, esaminare le impostazioni e quindi fare clic su **Avanti** per salvare la relying party trust informazioni.  
   ![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Nel **fine** pagina, fare clic su **Chiudi**. Questa azione viene visualizzata automaticamente la **Modifica regole attestazione** la finestra di dialogo.  
![Relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Vedere anche  
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
