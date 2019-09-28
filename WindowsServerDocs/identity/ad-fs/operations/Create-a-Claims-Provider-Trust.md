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
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407651"
---
# <a name="create-a-claims-provider-trust"></a>Creare un'attendibilità del provider di attestazioni

Per aggiungere una nuova attendibilità del provider di attestazioni utilizzando lo snap di gestione di ADFS\-manualmente e a configurare le impostazioni, eseguire la procedura seguente in un server federativo partner risorse nell'organizzazione partner risorse.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Per creare manualmente un'attendibilità del provider di attestazioni  
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**fare clic su **Aggiungi attendibilità provider di attestazioni**.  
![claims provider trust @ no__t-1   
  
3.  Nella pagina **iniziale** fare clic **Avvia**. 
![claims provider trust @ no__t-1    
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Immetti dati sull'attendibilità del provider di attestazioni manualmente** e quindi scegliere **Avanti**.  
![claims provider trust @ no__t-1     

5.  Nel **Specifica nome visualizzato** digitare un **nome visualizzato**, in **note**, digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti**.  
![claims provider trust @ no__t-1     

6.  Nel **Configura URL** specificare il **URL protocollo passivo WS-Federation** se applicabile e fare clic su **Avanti**.
![claims provider trust @ no__t-1     

8. Nella pagina **Configura identificatore** in **Identificatore dell'attendibilità del provider di attestazioni** digitare l'identificatore appropriato e quindi fare clic su **Avanti**.  
![claims provider trust @ no__t-1    

9. Nel **Configura certificati** pagina, fare clic su **Aggiungi** per individuare un file di certificato e aggiungerlo all'elenco dei certificati e quindi fare clic su **Avanti**.  
![claims provider trust @ no__t-1    

10. Nella pagina **Aggiunta attendibilità** fare clic su **Avanti** per salvare le informazioni sull'attendibilità del provider di attestazioni.  
![claims provider trust @ no__t-1    

11. Nella pagina **Fine** fare clic su **Chiudi**. Con questa azione viene visualizzata automaticamente la finestra di dialogo **Modifica regole attestazione**. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere i seguenti riferimenti aggiuntivi.  
![claims provider trust @ no__t-1

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Per creare un trust di provider di attestazioni tramite i metadati della federazione
Per aggiungere un nuovo provider di attestazioni trust, tramite lo snap-in di gestione di ADFS, tramite l'importazione automatica dei dati di configurazione sul partner dai metadati di federazione che ha pubblicato il partner a una rete locale o a Internet, eseguire la procedura seguente in un server federativo nell'organizzazione partner risorse.

>[!NOTE]
>Sebbene sia stata molto pratica comune usare i certificati con nomi host non qualificati, ad esempio https: \//MyServer, questi certificati non hanno alcun valore di sicurezza e possono consentire a un utente malintenzionato di rappresentare una Servizio federativo che pubblica la Federazione metadati. Pertanto, quando si eseguono query sui metadati di federazione, è consigliabile utilizzare solo un nome di dominio completo, ad esempio `https://myserver.contoso.com`.

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  In **azioni**fare clic su **Aggiungi attendibilità provider di attestazioni**.  
![claims provider trust @ no__t-1   
  
3.  Nella pagina **iniziale** fare clic **Avvia**. 
![claims provider trust @ no__t-1    
  
4.  Nella pagina **Seleziona origine dati** fare clic su **Importa dati sul provider di attestazioni pubblicati online o in una rete locale**. Federazione digitare l'indirizzo di metadati (nome host o URL), il **URL metadati federazione** o host, assegnare un nome per il partner e quindi fare clic su **Avanti**.
![claims provider trust @ no__t-1    

5.  Tipo di pagina Specifica nome visualizzato un **nome visualizzato**, in note digitare una descrizione per questa attendibilità del provider di attestazioni e quindi fare clic su **Avanti**.

6.  Nella pagina Aggiunta attendibilità fare clic su **Avanti** per salvare le informazioni di attendibilità provider di attestazioni.

7.  Nell'ultima pagina, fare clic su **Chiudi**. Questo verrà visualizzato automaticamente nella finestra di dialogo Modifica regole attestazione. Per ulteriori informazioni su come procedere con l'aggiunta di regole attestazioni per questa attendibilità del provider di attestazioni, vedere la sezione ulteriori riferimenti.



    
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione dell'organizzazione partner risorse @ no__t-0  
  
[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del provider di attestazioni](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
