---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Creare una regola per trasformare un'attestazione in ingresso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 43768b282005ba77d22985aa9a0d563125a97289
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816534"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Creare una regola per trasformare un'attestazione in ingresso


Utilizzando il trasforma un modello di regola **attestazione in ingresso** in Active Directory Federation Services \(ad FS\), è possibile selezionare un'attestazione in ingresso, modificare il tipo di attestazione e modificare il relativo valore attestazione. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che invia l'attestazione del ruolo con lo stesso valore di attestazione di un'attestazione in ingresso. È anche possibile usare questa regola per inviare un'attestazione di gruppo con un valore di attestazione per gli acquirenti quando è presente un'attestazione di gruppo in ingresso con un valore di amministratori oppure è possibile inviare solo il nome dell'entità utente \(UPN\) le attestazioni che terminano con @fabrikam.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con lo snap di gestione di ADFS\-in.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per trasformare un'attestazione in ingresso in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un tipo di attestazione nell'elenco. In **attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Passa tutti i valori di attestazione**  
  
    -   **Sostituire un valore di attestazione in ingresso con un valore attestazione in uscita diverso**  
  
    -   **Sostituzione delle attestazioni dei suffissi di posta elettronica in ingresso e\-con un nuovo suffisso di posta elettronica\-**  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza ADFS\-rilasciate le attestazioni, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **attestazione in uscita**, selezionare l'URL di richiesta che si desidera e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per trasformare un'attestazione in ingresso in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un tipo di attestazione nell'elenco. In **attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Passa tutti i valori di attestazione**  
  
    -   **Sostituire un valore di attestazione in ingresso con un valore attestazione in uscita diverso**  
  
    -   **Sostituzione delle attestazioni dei suffissi di posta elettronica in ingresso e\-con un nuovo suffisso di posta elettronica\-**  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza ADFS\-rilasciate le attestazioni, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **attestazione in uscita**, selezionare l'URL di richiesta che si desidera e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Per creare una regola per trasformare un'attestazione in ingresso in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS\\relazioni di Trust**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Nel **Modifica regole attestazione** la finestra di dialogo, selezionare una le schede seguenti, che dipende la relazione di trust che si sta modificando e nel quale set di regole si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione della delega**  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  Nel **configurare la regola** nella pagina **Nome regola attestazione**, digitare il nome visualizzato per questa regola. In **tipo di attestazione in ingresso**, selezionare un tipo di attestazione nell'elenco. In **attestazione in uscita**, selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, che dipende dai requisiti dell'organizzazione:  
  
    -   **Passa tutti i valori di attestazione**  
  
    -   **Sostituire un valore di attestazione in ingresso con un valore attestazione in uscita diverso**  
  
    -   **Sostituzione delle attestazioni dei suffissi di posta elettronica in ingresso e\-con un nuovo suffisso di posta elettronica\-**  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Se si imposta lo scenario controllo dinamico degli accessi che utilizza ADFS\-rilasciate le attestazioni, creare innanzitutto una regola di trasformazione sull'attendibilità del provider di attestazioni in **tipo di attestazione in ingresso**, digitare il nome per l'attestazione in ingresso o, se è stata creata in precedenza una descrizione di attestazione, selezionarla dall'elenco. Secondo, in **attestazione in uscita**, selezionare l'URL di richiesta che si desidera e quindi creare una regola di trasformazione su trust della relying party per l'emissione di attestazione del dispositivo.  
>   
> Per ulteriori informazioni sugli scenari di controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/dynamic-access-control--scenario-overview.md) o [tramite AD DS attestazioni con AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7. Fare clic su **Fine**.  
  
8. Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Altri riferimenti 
[Configurare le regole delle attestazioni](Configure-Claim-Rules.md)  
 
[Elenco di controllo: creazione di regole attestazione per un trust della relying party](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: creazione di regole attestazione per un trust del provider di attestazioni](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usare una regola attestazioni di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Ruolo delle regole delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
