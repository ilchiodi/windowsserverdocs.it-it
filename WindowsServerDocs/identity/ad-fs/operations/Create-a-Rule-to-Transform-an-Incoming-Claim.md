---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Creare una regola per trasformare un'attestazione in ingresso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Creare una regola per trasformare un'attestazione in ingresso

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Utilizzando il **trasformare un'attestazione in ingresso** modello di regola in Active Directory Federation Services \(AD FS\), è possibile selezionare un'attestazione in ingresso, modificarne il tipo di attestazione e modificarne il valore di attestazione. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che invia l'attestazione del ruolo con lo stesso valore di attestazione di un'attestazione in ingresso. È inoltre possibile utilizzare questa regola per inviare un'attestazione basata su gruppo con un valore di attestazione dell'acquirente quando esiste un'attestazione in ingresso con un valore del gruppo amministratori, oppure è possibile inviare solo nome dell'entità utente \(UPN\) attestazioni che terminano con @fabrikam.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con la gestione di ADFS snap-in.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per trasformare un'attestazione in ingresso in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un'attestazione digitare l'elenco. In **tipo di attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza le attestazioni rilasciate FS\ Active Directory, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **tipo di attestazione in uscita**, seleziona l'URL di attestazione desiderato e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS ](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per trasformare un'attestazione in ingresso in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un'attestazione digitare l'elenco. In **tipo di attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza le attestazioni rilasciate FS\ Active Directory, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **tipo di attestazione in uscita**, seleziona l'URL di attestazione desiderato e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS ](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Per creare una regola per trasformare un'attestazione in ingresso in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS **.  
  
2.  Nell'albero della console, in **AD FS\\Trust relazioni**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Nel **Modifica regole attestazione** la finestra di dialogo, selezionare una le schede seguenti, che dipende la relazione di trust che si sta modificando e nel quale set di regole si desidera creare questa regola, quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un'attestazione digitare l'elenco. In **tipo di attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza le attestazioni rilasciate FS\ Active Directory, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **tipo di attestazione in uscita**, seleziona l'URL di attestazione desiderato e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS ](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Fare clic su **fine **.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole attestazione per un Provider di attestazioni attendibile](https://technet.microsoft.com/library/ee913564.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
