---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creazione di un'attendibilità della relying party
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0d32edd7ebc23fa724439710c6511642d9c49a3
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371490"
---
# <a name="create-a-relying-party-trust"></a>Creazione di un'attendibilità della relying party


Il seguente documento vengono fornite informazioni sulla creazione manuale di un trust della relying party e l'utilizzo dei metadati di federazione.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Per creare un attestazioni compatibile con Relying Party Trust manualmente 

Per aggiungere un nuovo trust della relying party utilizzando lo snap di gestione di ADFS\-manualmente e a configurare le impostazioni, eseguire la procedura seguente in un server federativo.  

L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Nel **iniziale** scegliere pagina **grado di riconoscere attestazioni** e fare clic su **avviare**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Immetti dati sul componente manualmente**, quindi su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Nel **Specifica nome visualizzato** digitare un nome in **nome visualizzato**, in **note** digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Nel **Configura certificato** Se si dispone di un certificato di crittografia token facoltativa, fare clic su pagina **Sfoglia** per individuare un file di certificato e quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Nella pagina **Configura URL** eseguire una o entrambe le operazioni seguenti, fare clic su **Avanti**, quindi andare al passaggio 8:  
  
    -   Selezionare il **abilitare il supporto per WS\-Federation Passive protocol** casella di controllo. In **Relying party WS\-Federation Passive protocol URL**, digitare l'URL per il trust della relying party e quindi fare clic su **Avanti**.  
  
    -   Selezionare la casella di controllo **Abilita supporto del protocollo SAML 2.0 WebSSO**. In **URL del servizio SSO saml 2,0 di relying party**Digitare l'URL dell'endpoint del servizio Security Assertion Markup Language \(\) SAML per questo trust di relying party, quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Nella pagina **Configura identificatori** specificare uno o più identificatori per la relying party, fare clic su **Aggiungi** per aggiungerli all'elenco, quindi fare clic su **Avanti**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Nel **scegliere Criteri di controllo di accesso** Selezionare un criterio e fare clic su **Avanti**.  Per ulteriori informazioni sui criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](Access-Control-Policies-in-AD-FS.md). 
![relying party](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Nella pagina **Aggiunta attendibilità** esaminare le impostazioni e quindi fare clic su **Avanti** per salvare le informazioni sul trust della relying party.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Nella pagina **Fine** fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo **Modifica regole attestazione**.  
![relying party](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Per creare un attestazioni conoscenza Relying Party Trust utilizzando i metadati della federazione

Per aggiungere un nuovo trust della relying party, utilizzando lo snap-in di gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che il partner pubblicato in una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner account.

>[!NOTE]
>Sebbene sia stata molto pratica comune usare i certificati con nomi host non qualificati, ad esempio https://myserver, questi certificati non hanno alcun valore di sicurezza e possono consentire a un utente malintenzionato di rappresentare una Servizio federativo che pubblica metadati federativi. Pertanto, quando si eseguono query sui metadati di federazione, è consigliabile utilizzare solo un nome di dominio completo, ad esempio https://myserver.contoso.com.

L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).


1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2. In **azioni**, fare clic su **Aggiungi attendibilità**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Nel **iniziale** scegliere pagina **grado di riconoscere attestazioni** e fare clic su **avviare**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Nella pagina **Seleziona origine dati** fare clic su <strong>importa dati sul relying party pubblicato online o in una rete locale *. In * * indirizzo metadati federazione (nome host o URL)</strong>Digitare l'URL dei metadati della Federazione o il nome host per il partner e quindi fare clic su **Avanti**.  
   ![relying party](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Nella pagina Specifica nome visualizzato digitare un nome in **nome visualizzato**, in note digitare una descrizione per questo trust della relying party e quindi fare clic su **Avanti**.

6. Nella pagina Scegli regole di autorizzazione di rilascio, selezionare **consentire tutti gli utenti di accedere a questa relying party** o **negare tutti gli utenti l'accesso a questo componente**, quindi fare clic su **Avanti**.

7. Nella schermata Aggiunta attendibilità, rivedere le impostazioni e quindi fare clic su **Avanti** per salvare la relying party trust informazioni.

8. Nell'ultima pagina, fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo Modifica regole attestazione. Per altre informazioni su come procedere per l'aggiunta di regole attestazioni per il trust della relying party, vedere la sezione "Riferimenti aggiuntivi".




## <a name="see-also"></a>Vedi anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
