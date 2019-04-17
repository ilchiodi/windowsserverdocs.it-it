---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Creare una regola per trasformare un'attestazione in ingresso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Creare una regola per inviare un'attestazione compatibile di AD FS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2


In situazioni in cui si utilizza Active Directory Federation Services \(AD FS\) per rilasciare attestazioni che verrà ricevuto dal server federativo ADFS 1.0 \ (Windows Server 2003 R2\) o AD FS 1.1 \ (Windows Server 2008 o Windows Server 2008 R2\), è necessario eseguire le operazioni seguenti:  
  
-   Creare una regola che invierà un tipo di attestazione ID nome con un formato di UPN, posta elettronica o nome comune.  
  
-   Tutte le altre controversie inviati devono avere uno dei seguenti tipi di attestazione:  
  
    -   AD FS 1. *x* indirizzo di posta elettronica  
  
    -   AD FS 1. *x* UPN  
  
    -   Nome comune  
  
    -   Gruppo  
  
    -   Qualsiasi altro tipo di attestazione che inizia con https://schemas.xmlsoap.org/claims/, ad esempio https://schemas.xmlsoap.org/claims/EmployeeID  
  
A seconda delle esigenze dell'organizzazione, utilizzare una delle procedure seguenti per creare un'istanza di ADFS 1. *x* attestazione NameID compatibile:  
  
-   Creare questa regola problema ADFS 1. x ID Nome attestazione utilizzando il **Pass-Through o filtro a un modello di regola attestazione in ingresso**  
  
-   Creare questa regola problema ADFS 1. x ID Nome attestazione utilizzando il **Trasforma un modello di regola attestazione in ingresso**. È possibile utilizzare questo modello di regola in situazioni in cui si desidera modificare il tipo di attestazione esistente in un nuovo tipo di attestazione che funzionerà con AD FS 1. *x* attestazioni.  
  
> [!NOTE]  
> Per questa regola a funzionare come previsto, assicurarsi che il trust della relying party o trust di provider di attestazioni in cui si sta creando questa regola è stato configurato per utilizzare il **profilo ADFS 1.0 e 1.1**. 




## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per il rilascio di un'istanza di ADFS 1. *x* ID Nome attestazione utilizzando il Pass-Through o filtro a un modello di regola attestazione in ingresso in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **Pass-Through o filtro di un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**selezionare **ID nome** nell'elenco.  
  
8.  In **formato ID nome in ingresso**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
9. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifici**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Creare una regola](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per il rilascio di un'istanza di ADFS 1. *x* ID Nome attestazione utilizzando il Pass-Through o filtro a un modello di regola attestazione in ingresso in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **Pass-Through o filtro di un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**selezionare **ID nome** nell'elenco.  
  
8.  In **formato ID nome in ingresso**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
9. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifici**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Creare una regola](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  

  

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

6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare il tipo di attestazione in ingresso che si desidera trasformare nell'elenco.  
  
8.  In **tipo di attestazione in uscita**selezionare **ID nome** nell'elenco.  
  
9. In **formato ID nome in uscita**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
10. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  

  


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

6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare il tipo di attestazione in ingresso che si desidera trasformare nell'elenco.  
  
8.  In **tipo di attestazione in uscita**selezionare **ID nome** nell'elenco.  
  
9. In **formato ID nome in uscita**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
10. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  













  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Per creare una regola per il rilascio di un'istanza di ADFS 1. *x* ID Nome attestazione utilizzando il Pass-Through o filtro a un modello di regola attestazione in ingresso in Windows Server 2012 R2
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS **.  
  
2.  Nell'albero della console, in **AD FS\\Trust relazioni**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo selezionare una delle seguenti schede, in base ai trust che si sta modificando e set di regola che si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **Pass-Through o filtro di un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**selezionare **ID nome** nell'elenco.  
  
8.  In **formato ID nome in ingresso**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
9. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifici**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Creare una regola](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Per creare una regola per il rilascio di un'istanza di ADFS 1. *x* attestazione ID nome utilizzando la trasformazione di un modello di regola attestazione in ingresso in Windows Server 2012 R2  
  
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
  
6.  Nel **configurare la regola**, digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare il tipo di attestazione in ingresso che si desidera trasformare nell'elenco.  
  
8.  In **tipo di attestazione in uscita**selezionare **ID nome** nell'elenco.  
  
9. In **formato ID nome in uscita**, selezionare una delle ADFS 1. *x*\-compatible formati dall'elenco di attestazioni:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comune**  
  
10. Selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori attestazione**  
  
    -   **Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita**  
  
    -   **Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\**  
![Creare una regola](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Fare clic su **fine**, quindi fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole attestazione per un Provider di attestazioni attendibile](https://technet.microsoft.com/library/ee913564.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 