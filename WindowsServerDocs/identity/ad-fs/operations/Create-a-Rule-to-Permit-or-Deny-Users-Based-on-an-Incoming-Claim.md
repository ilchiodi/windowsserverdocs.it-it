---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In Windows Server 2016, è possibile utilizzare un **criteri di controllo di accesso** per creare una regola che consente di negare agli utenti in base a un'attestazione in ingresso.  In Windows Server 2012 R2, utilizzando il **consentire o negare agli utenti in base a un'attestazione in ingresso** modello di regola in Active Directory Federation Services \(AD FS\), è possibile creare una regola di autorizzazione che concedere o negare l'accesso dell'utente alla relying party in base al tipo e il valore di un'attestazione in ingresso. 

Ad esempio, è possibile utilizzare per creare una regola che consenta solo gli utenti che dispongono di un gruppo di un'attestazione con un valore del gruppo Domain Admins per accedere alla relying party. Se si desidera consentire tutti gli utenti di accedere alla relying party, usare il **consentire Everyone** criteri di controllo di accesso o **consentire tutti gli utenti** modello di regola a seconda della versione di Windows Server. Gli utenti autorizzati ad accedere alla relying party dal servizio federativo potrebbero comunque essere negato l'accesso dalla relying party.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con la gestione di ADFS snap-in.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Per creare una regola per consentire agli utenti in base a un'attestazione in ingresso in Windows Server 2016
 
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **criteri di controllo di accesso **. 
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Pulsante destro del mouse e selezionare **aggiungere criteri di controllo di accesso **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Nella casella Nome immettere un nome per il criterio, una descrizione e fare clic su **Aggiungi **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. Nel **Editor delle regole**, sotto gli utenti, inserire un segno di spunta **con le attestazioni specifiche nella richiesta** e fare clic sul sottolineato **specifico** nella parte inferiore.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. Nel **selezionare attestazioni** schermata, fare clic il **attestazioni** pulsante di opzione, selezionare il **tipo di attestazione**, **operatore**e il **valore attestazione** fare clic su **Ok **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Nel **Editor delle regole** fare clic su **Ok **.  Nel **aggiungere criteri di controllo di accesso** schermata, fare clic su **Ok **.

8. Nel **gestione di ADFS** albero della console in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Fare doppio clic su di **Trust della Relying Party** che si desidera consentire l'accesso a e selezionare **Modifica criteri di controllo di accesso **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Nel criterio di controllo di accesso, selezionare i criteri e quindi fare clic su **applica** e **Ok **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Per creare una regola per negare agli utenti in base a un'attestazione in ingresso in Windows Server 2016
 
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **criteri di controllo di accesso **. 
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Pulsante destro del mouse e selezionare **aggiungere criteri di controllo di accesso **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Nella casella Nome immettere un nome per il criterio, una descrizione e fare clic su **Aggiungi **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. Nel **Editor delle regole**, assicurarsi che tutti gli utenti sia selezionato e in **tranne** inserire un segno di spunta **con le attestazioni specifiche nella richiesta** e fare clic sul sottolineato **specifico** nella parte inferiore.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. Nel **selezionare attestazioni** schermata, fare clic il **attestazioni** pulsante di opzione, selezionare il **tipo di attestazione**, **operatore**e il **valore attestazione** fare clic su **Ok **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Nel **Editor delle regole** fare clic su **Ok **.  Nel **aggiungere criteri di controllo di accesso** schermata, fare clic su **Ok **.

8. Nel **gestione di ADFS** albero della console in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Fare doppio clic su di **Trust della Relying Party** che si desidera consentire l'accesso a e selezionare **Modifica criteri di controllo di accesso **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Nel criterio di controllo di accesso, selezionare i criteri e quindi fare clic su **applica** e **Ok **.
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Per creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso in Windows Server 2012 R2
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.    
  
2.  Nell'albero della console, in **AD FS\\Trust Relationships\\Relying attendibilità**, fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.  
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic sul **regole di autorizzazione rilascio** scheda o **regole di autorizzazione di delega** scheda \ (in base al tipo di regola di autorizzazione è require\) e quindi fare clic su **Aggiungi regola** per avviare il **Aggiunta guidata regole attestazione di autorizzazione **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **consentire o negare agli utenti in base a un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, in **tipo di attestazione in ingresso** selezionare un tipo di attestazione nell'elenco in **valore attestazione in ingresso** digitare un valore o fare clic su Sfoglia \ (se available\) e selezionare un valore e quindi selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Consentire l'accesso agli utenti con questa attestazione in ingresso**  
  
    -   **Nega accesso agli utenti con questa attestazione in ingresso**  
![Creare una regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Fare clic su **fine **.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
