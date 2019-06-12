---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creazione di un'attendibilità della relying party
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444365"
---
# <a name="create-a-relying-party-trust"></a>Creazione di un'attendibilità della relying party


Il seguente documento vengono fornite informazioni sulla creazione manuale di un trust della relying party e l'utilizzo dei metadati di federazione.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Per creare un attestazioni compatibile con Relying Party Trust manualmente 

Per aggiungere un nuovo trust della relying party utilizzando lo snap di gestione di ADFS\-manualmente e a configurare le impostazioni, eseguire la procedura seguente in un server federativo.  

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale** scegliere pagina **grado di riconoscere attestazioni** e fare clic su **avviare**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Immetti dati sul componente manualmente**, quindi scegliere **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Nel **Specifica nome visualizzato** digitare un nome in **nome visualizzato**, in **note** digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Nel **Configura certificato** Se si dispone di un certificato di crittografia token facoltativa, fare clic su pagina **Sfoglia** per individuare un file di certificato e quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Nel **Configura URL** pagina, effettuare una o entrambe le operazioni seguenti, fare clic su **successivo**, quindi andare al passaggio 8:  
  
    -   Selezionare il **abilitare il supporto per WS\-Federation Passive protocol** casella di controllo. In **Relying party WS\-Federation Passive protocol URL**, digitare l'URL per il trust della relying party e quindi fare clic su **Avanti**.  
  
    -   Selezionare la casella di controllo **Abilita supporto del protocollo SAML 2.0 WebSSO**. Sotto **Relying party SAML 2.0 SSO service URL**, digitare Security Assertion Markup Language \(SAML\) service URL dell'endpoint per questo trust della relying party e quindi fare clic su **successivo**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Nella pagina **Configura identificatori** specificare uno o più identificatori per la relying party, fare clic su **Aggiungi** per aggiungerli all'elenco, quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Nel **scegliere Criteri di controllo di accesso** Selezionare un criterio e fare clic su **Avanti**.  Per ulteriori informazioni sui criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](Access-Control-Policies-in-AD-FS.md). 
![relying party](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Nel **Aggiunta attendibilità** pagina, controllare le impostazioni e quindi fare clic su **Avanti** per salvare la relying party trust informazioni.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Nella pagina **Fine** fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo **Modifica regole attestazione**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Per creare un attestazioni conoscenza Relying Party Trust utilizzando i metadati della federazione

Per aggiungere un nuovo trust della relying party, utilizzando lo snap-in di gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che il partner pubblicato in una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner account.

>[!NOTE]
>Anche se è stato tempo pratica comune utilizzare certificati con nomi host non qualificato, ad esempio https://myserver, questi certificati non hanno alcun valore di sicurezza e può consentire un attacco di rappresentare un servizio federativo che pubblica metadati federativi. Pertanto, quando si esegue una query dei metadati di federazione, è necessario solo utilizzare un nome di dominio completo, ad esempio https://myserver.contoso.com.

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).


1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2. In **azioni**, fare clic su **Aggiungi attendibilità**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Nel **iniziale** scegliere pagina **grado di riconoscere attestazioni** e fare clic su **avviare**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Nel **selezionare un'origine dati** pagina, fare clic su <strong>Importa dati sulla relying party pubblicati online o in una rete locale *. In * * indirizzo di metadati di federazione (nome host o URL)</strong>, digitare il federazione metadati URL o il nome host per il partner e quindi fare clic su **successivo**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Nella pagina Specifica nome visualizzato digitare un nome in **nome visualizzato**, in note digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.

6. Nella pagina Scegli regole di autorizzazione di rilascio, selezionare **consentire tutti gli utenti di accedere a questa relying party** o **negare tutti gli utenti l'accesso a questo componente**, quindi fare clic su **Avanti**.

7. Nella schermata Aggiunta attendibilità, rivedere le impostazioni e quindi fare clic su **Avanti** per salvare la relying party trust informazioni.

8. Nell'ultima pagina, fare clic su **Chiudi**. Questa azione consente di visualizzare automaticamente la finestra di dialogo Modifica regole attestazione. Per altre informazioni su come procedere per l'aggiunta di regole attestazioni per il trust della relying party, vedere la sezione "Riferimenti aggiuntivi".




## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
