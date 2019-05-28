---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Creare una regola per inviare attestazioni mediante una regola personalizzata
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ade2a8304288d102608c81a0c29155478e5a4b7b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189429"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Creare una regola per inviare attestazioni mediante una regola personalizzata


Tramite il **inviare attestazioni mediante una regola personalizzata** modello in Active Directory Federation Services (ADFS), è possibile creare regole attestazioni personalizzate per la situazione in cui un modello di regola standard non soddisfa i requisiti del organizzazione. Regole attestazioni personalizzate vengono scritti nel linguaggio di regola attestazione e quindi deve essere copiate il **Custom rule** casella di testo prima di poter essere utilizzati in un set di regole. Per informazioni sulla creazione di sintassi per una regola avanzata, vedere [il ruolo del linguaggio di regola attestazione](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione utilizzando lo snap di gestione di ADFS\-in.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per pass-through o filtrare un'attestazione in ingresso in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **Send Claims Using a Custom Rule** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **regola personalizzata**, digitare o incollare la sintassi di linguaggio di regola attestazione da utilizzare per questa regola.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Scegliere **Fine**.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per pass-through o filtrare un'attestazione in ingresso in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **Send Claims Using a Custom Rule** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **regola personalizzata**, digitare o incollare la sintassi di linguaggio di regola attestazione da utilizzare per questa regola.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Scegliere **Fine**.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Per creare una regola per l'invio di attestazioni usando un'attestazione personalizzata in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS\\relazioni di Trust**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Nel **Modifica regole attestazione** la finestra di dialogo, selezionare una le schede seguenti, che dipende la relazione di trust che si sta modificando e nel quale set di regole si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **Send Claims Using a Custom Rule** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **regola personalizzata**, digitare o incollare la sintassi di linguaggio di regola attestazione da utilizzare per questa regola.  
![Crea regola](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Scegliere **Fine**.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Altri riferimenti 
[Configurare le regole delle attestazioni](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del componente](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del provider di attestazioni](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usare una regola di attestazione di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Ruolo delle regole delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
