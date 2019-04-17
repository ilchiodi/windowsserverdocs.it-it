---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creare un Trust della Relying Party
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>Creare un Trust della Relying Party

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In questo documento vengono fornite informazioni sulla creazione di un trust della relying party manualmente e con i metadati federativi.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Per creare un attestazioni conoscenza Relying Party Trust manualmente 

Per aggiungere un nuovo trust della relying party usando snap-in di gestione di ADFS e configurare manualmente le impostazioni, eseguire la procedura seguente in un server federativo.  

Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale**, selezionare **grado di riconoscere attestazioni** e fare clic su **Start **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Nel **Seleziona origine dati** pagina, fare clic su **Immetti dati sul componente manualmente**, quindi fare clic su **Avanti **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Nel **Specifica nome visualizzato**, digitare un nome in **nome visualizzato**, in **note** digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Nel **Configura certificato** pagina, se si dispone di un certificato di crittografia token facoltativa, fare clic su **Sfoglia** consente di individuare un file di certificato e quindi fare clic su **Avanti **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Nel **Configura URL** pagina, effettuare una o entrambe le operazioni seguenti, fare clic su **Avanti**e quindi andare al passaggio 8:  
  
    -   Selezionare il **abilitare il supporto del protocollo passivo WS-Federation** casella di controllo. In **Relying party WS-Federation Passive protocol URL**, digitare l'URL per questo trust della relying party e quindi fare clic su **Avanti **.  
  
    -   Selezionare il **abilitare il supporto per il protocollo SAML 2.0 WebSSO** casella di controllo. In **URL del servizio SAML 2.0 SSO del componente**, digitare l'URL dell'endpoint servizio \(SAML\) Security Assertion Markup Language per questo trust della relying party e quindi fare clic su **Avanti **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Nel **Configura identificatori** pagina specificare uno o più identificatori per la relying party, fare clic su **Aggiungi** aggiungerli all'elenco, quindi fare clic su **Avanti**.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Nel **scegliere Criteri di controllo di accesso** selezionare un criterio e fare clic su **Avanti**.  Per ulteriori informazioni sui criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](Access-Control-Policies-in-AD-FS.md). 
![Relying party](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Nel **Aggiunta attendibilità** pagina, esaminare le impostazioni e quindi fare clic su **Avanti** per salvare la relying party trust informazioni.  
   ![Relying party](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Nel **fine** pagina, fare clic su **Chiudi**. Questa azione viene visualizzata automaticamente la **Modifica regole attestazione** la finestra di dialogo.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Per creare un attestazioni conoscenza Relying Party Trust usando metadati federativi

Per aggiungere un nuovo trust della relying party, utilizzando lo snap-in Gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che il partner pubblicato in una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner account.

>[!NOTE]
>Anche se è stato tempo pratica comune utilizzare certificati con nomi host non qualificato, ad esempio https://myserver, questi certificati non hanno alcun valore di sicurezza e possono consentire un attacco di rappresentare un servizio federativo che pubblica metadati federativi. Pertanto, quando una query di metadati di federazione, utilizzare solo un nome di dominio completo, ad esempio https://myserver.contoso.com.

Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).


1. In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale**, selezionare **grado di riconoscere attestazioni** e fare clic su **Start **.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Nel **Seleziona origine dati** pagina, fare clic su **Importa dati sul componente pubblicati online o in una rete locale*. In **indirizzo metadati federativi (nome host o URL)**, digitare il federazione metadati URL o il nome host per il partner e quindi fare clic su **Avanti**.  
![Relying party](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  Nella pagina Specifica nome visualizzato, digitare un nome nella **nome visualizzato**in note digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.

6.  Nella pagina Scegli regole di autorizzazione rilascio, selezionare **consentire tutti gli utenti di accedere a questo componente** o **negare tutti gli utenti l'accesso a questo componente**, quindi fare clic su **Avanti**.

7.  Nella pagina pronto per aggiunta attendibilità, rivedere le impostazioni, quindi fare clic su **Avanti** per salvare la relying party trust informazioni.

8.  Nella pagina fine, fare clic su **Chiudi**. Questa azione viene visualizzata automaticamente la finestra di dialogo Modifica regole attestazione. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazione per questo trust della relying party, vedere i riferimenti aggiuntivi.




## <a name="see-also"></a>Vedere anche  
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
