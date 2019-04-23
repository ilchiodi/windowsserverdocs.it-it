---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Creare una regola per Pass-Through o filtrare un'attestazione in ingresso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860272"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Creare una regola per Pass-Through o filtrare un'attestazione in ingresso

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Utilizzando il Pass-Through o filtro a un modello di regola attestazione in ingresso in Active Directory Federation Services \(ADFS\), è possibile passare attraverso tutte le attestazioni in ingresso con un tipo di attestazione selezionato. È inoltre possibile filtrare i valori delle attestazioni in ingresso con un tipo di attestazione selezionato. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che invierà tutte le attestazioni di gruppo in ingresso. È anche possibile usare questa regola per inviare solo i nomi dell'entità utente \(UPN\) attestazioni che terminano con @fabrikam.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con lo snap di gestione di ADFS\-in.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per pass-through o filtrare un'attestazione in ingresso in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **il modello di regola attestazione**, selezionare **Pass Through or Filter an Incoming Claim** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, **tipo di attestazione in ingresso** Selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori di attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifico**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per pass-through o filtrare un'attestazione in ingresso in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **il modello di regola attestazione**, selezionare **Pass Through or Filter an Incoming Claim** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, **tipo di attestazione in ingresso** Selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori di attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifico**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Per creare una regola per pass-through o filtrare un'attestazione in ingresso in Windows Server 2012 R2

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **FSAD AD FS\\relazioni di Trust**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo selezionare una delle seguenti schede, in base ai trust che si sta modificando e set di regola che si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **il modello di regola attestazione**, selezionare **Pass Through or Filter an Incoming Claim** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, **tipo di attestazione in ingresso** Selezionare un tipo di attestazione nell'elenco e quindi selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Pass-through di tutti i valori di attestazione**  
  
    -   **Pass-through solo un valore attestazione specifico**  
  
    -   **Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica specifico**  
  
    -   **Pass-through solo dei valori attestazione che iniziano con un valore specifico**  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  



  
## <a name="additional-references"></a>Altri riferimenti  
[Configurare regole attestazioni](Configure-Claim-Rules.md)  
  
[Quando utilizzare un Pass-Through or Filter Claim Rule](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo delle regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
