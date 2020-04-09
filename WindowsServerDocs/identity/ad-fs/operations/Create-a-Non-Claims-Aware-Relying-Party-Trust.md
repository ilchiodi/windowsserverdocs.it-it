---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Creazione di un'attendibilità della relying party non in grado di riconoscere attestazioni
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c6886145e910b76edbe99549266d651cdd7c3edf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816924"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Creare un Trust Non attestazioni Relying Party


Nello snap di gestione di ADFS\-in, non\-attestazioni\-compatibile con attendibilità componente sono oggetti che vengono creati per rappresentare la relazione di trust tra il servizio federativo e una sola web\-basata su un'applicazione che non\-conoscenza e che si accede tramite il Proxy dell'applicazione Web.  
  
Non\-attestazioni\-trust della relying party è un trust della relying party costituito da identificatori, nomi e le regole per l'autenticazione e autorizzazione quando l'attendibilità avviene tramite il Proxy dell'applicazione Web. Queste web\-applicazioni non basate su attestazioni, in altre parole, queste autenticazione integrata di Windows basate su\-applicazioni basate su, è possibile definire regole di autorizzazione che consentono l'accesso che è basato sulle attestazioni per l'accesso è esterno alla rete aziendale tramite il Proxy dell'applicazione Web.  
  
Per aggiungere un nuovo non\-attestazioni\-compatibile con trust della relying party, utilizzando lo snap di gestione di ADFS\-eseguire la procedura seguente.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Per creare manualmente un attestazioni non compatibile con Trust della Relying Party 
1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale** scegliere pagina **grado di riconoscere attestazioni Non** e fare clic su **avviare**.  
![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Nel **Specifica nome visualizzato** digitare un nome in **nome visualizzato**, in **note** digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Nella pagina **Configura identificatori** specificare uno o più identificatori per la relying party, fare clic su **Aggiungi** per aggiungerli all'elenco, quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Nel **scegliere Criteri di controllo di accesso** Selezionare un criterio e fare clic su **Avanti**.  Per ulteriori informazioni sui criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](Access-Control-Policies-in-AD-FS.md). 
![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Nella pagina **Aggiunta attendibilità** esaminare le impostazioni e quindi fare clic su **Avanti** per salvare le informazioni sul trust della relying party.  
   ![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Nella pagina **Fine** fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo **Modifica regole attestazione**.  
![relying party](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Vedi anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
