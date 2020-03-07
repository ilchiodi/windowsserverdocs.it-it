---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Creare un'attendibilità del provider di attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4539e8abd1af1eca7bacb51971e6d355bb0aab28
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371579"
---
# <a name="create-a-claims-provider-trust"></a>Creare un'attendibilità del provider di attestazioni

Per aggiungere una nuova attendibilità del provider di attestazioni utilizzando lo snap di gestione di ADFS\-manualmente e a configurare le impostazioni, eseguire la procedura seguente in un server federativo partner risorse nell'organizzazione partner risorse.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Per creare manualmente un'attendibilità del provider di attestazioni  
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**fare clic su **Aggiungi attendibilità provider di attestazioni**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Nella pagina iniziale fare clic **Avvia**. 
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Immetti dati sull'attendibilità del provider di attestazioni manualmente** e quindi scegliere **Avanti**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Nella pagina **Specifica nome visualizzato** digitare un **Nome visualizzato**, in **Note**, digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Nel **Configura URL** specificare il **URL protocollo passivo WS-Federation** se applicabile e fare clic su **Avanti**.
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Nella pagina **Configura identificatore** in **Identificatore dell'attendibilità del provider di attestazioni** digitare l'identificatore appropriato e quindi fare clic su **Avanti**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Nella pagina **Configura certificati** fare clic su **Aggiungi** per individuare un file di certificato e aggiungerlo all'elenco dei certificati, quindi fare clic su **Avanti**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Nella pagina **Aggiunta attendibilità** fare clic su **Avanti** per salvare le informazioni sull'attendibilità del provider di attestazioni.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Nella pagina **Fine** fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo **Modifica regole attestazione**. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere i seguenti riferimenti aggiuntivi.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Per creare un trust di provider di attestazioni tramite i metadati della federazione
Per aggiungere un nuovo provider di attestazioni trust, tramite lo snap-in di gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che ha pubblicato il partner a una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner risorse.

>[!NOTE]
>Sebbene sia stata molto pratica comune usare i certificati con nomi host non qualificati, ad esempio https:\//MyServer, questi certificati non hanno alcun valore di sicurezza e possono consentire a un utente malintenzionato di rappresentare una Servizio federativo che pubblica metadati federativi. Pertanto, quando si eseguono query sui metadati di federazione, è consigliabile utilizzare solo un nome di dominio completo, ad esempio `https://myserver.contoso.com`.

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**fare clic su **Aggiungi attendibilità provider di attestazioni**.  
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Nella pagina iniziale fare clic **Avvia**. 
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Importa dati sul provider di attestazioni pubblicati online o in una rete locale**. Federazione digitare l'indirizzo di metadati (nome host o URL), il **URL metadati federazione** o host, assegnare un nome per il partner e quindi fare clic su **Avanti**.
![attendibilità provider di attestazioni](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Tipo di pagina Specifica nome visualizzato un **nome visualizzato**, in note digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti**.

6.  Nella pagina Aggiunta attendibilità fare clic su **Avanti** per salvare le informazioni di attendibilità provider di attestazioni.

7.  Nell'ultima pagina, fare clic su **Chiudi**. Questo verrà visualizzato automaticamente nella finestra di dialogo Modifica regole attestazione. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere la sezione ulteriori riferimenti.



    
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione dell'organizzazione partner risorse](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Elenco di controllo: creazione di regole attestazione per un trust del provider di attestazioni](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Vedi anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
