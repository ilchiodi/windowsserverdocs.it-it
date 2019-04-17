---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Creare un Trust di Provider di attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>Creare un Trust di Provider di attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per aggiungere una nuova attendibilità del provider di attestazioni tramite snap-in di gestione di ADFS e configurare manualmente le impostazioni, eseguire la procedura seguente in un server federativo partner risorse nell'organizzazione partner risorse.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Per creare un trust di provider di attestazioni manualmente  
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità Provider di attestazioni **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Nel **iniziale** pagina, fare clic su **Start **. 
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Nel **Seleziona origine dati** pagina, fare clic su **invio dati sull'attendibilità del provider di attestazioni manualmente**, quindi fare clic su **Avanti **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Nel **Specifica nome visualizzato**, digitare un **nome visualizzato**, in **note**, digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Nel **Configura URL** specificare il **URL protocollo passivo WS-Federation** se applicabile e fare clic su **Avanti **.
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Nel **Configura identificatore** nella pagina **identificatore trust del provider di attestazioni**, digitare l'identificatore appropriato e quindi fare clic su **Avanti **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Nel **Configura certificati** pagina, fare clic su **Aggiungi** consente di individuare un file di certificato e aggiungerlo all'elenco dei certificati e quindi fare clic su **Avanti **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Nel **Aggiunta attendibilità** pagina, fare clic su **Avanti** per salvare le attestazioni informazioni sull'attendibilità del provider.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Nel **fine** pagina, fare clic su **Chiudi**. Questa azione viene visualizzata automaticamente la **Modifica regole attestazione** la finestra di dialogo. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere i seguenti riferimenti aggiuntivi.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Per creare un trust di provider di attestazioni con i metadati federativi
Per aggiungere un nuovo provider di attestazioni trust, tramite lo snap-in Gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che ha pubblicato il partner a una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner risorse.

>[!NOTE]
>Anche se è stato tempo pratica comune utilizzare certificati con nomi host non qualificato, ad esempio https://myserver, questi certificati non hanno alcun valore di sicurezza e possono consentire un attacco di rappresentare un servizio federativo che pubblica metadati federativi. Pertanto, quando una query di metadati di federazione, utilizzare solo un nome di dominio completo, ad esempio https://myserver.contoso.com.

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In **azioni**, fare clic su **Aggiungi attendibilità **.  
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Nel **iniziale** pagina, fare clic su **Start **. 
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Nel **Seleziona origine dati** pagina, fare clic su **Importa dati sul provider di attestazioni pubblicati online o in una rete locale **. Federazione indirizzo dei metadati (nome host o URL), digitare il **URL dei metadati federativi** host assegna un nome per il partner e quindi fare clic su o **Avanti **.
![Attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Tipo di pagina Specifica nome visualizzato un **nome visualizzato**, in note digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti **.

6.  Nella pagina pronto per aggiunta attendibilità, fare clic su **Avanti** per salvare le attestazioni informazioni sull'attendibilità del provider.

7.  Nella pagina fine, fare clic su **Chiudi**. Verrà visualizzata automaticamente la finestra di dialogo Modifica regole attestazione. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere la sezione ulteriori riferimenti.



    
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione dell'organizzazione Partner risorse](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Elenco di controllo: Creazione di regole attestazione per un Provider di attestazioni attendibile](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Vedere anche  
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
  
