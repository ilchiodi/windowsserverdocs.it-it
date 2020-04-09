---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7d6be5b9194060bb16673e01e0fee8b36b081769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816814"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso 


In Windows Server 2016, è possibile usare un **criterio di controllo di accesso** per creare una regola che consentirà o negherà gli utenti in base a un'attestazione in ingresso.  In Windows Server 2012 R2, usando il modello di regola **consentire o negare agli utenti in base a un'attestazione in ingresso** in Active Directory Federation Services \(ad FS\), è possibile creare una regola di autorizzazione che consentirà o negherà l'accesso dell'utente al relying party in base al tipo e al valore di un'attestazione in ingresso. 

Ad esempio, è possibile utilizzare per creare una regola che consente solo agli utenti che dispongono di un gruppo di un'attestazione con un valore del gruppo Domain Admins per accedere alla relying party. Se si desidera consentire tutti gli utenti di accedere alla relying party, usare il **consentire Everyone** criteri di controllo di accesso o **consentire tutti gli utenti** il modello di regola a seconda della versione di Windows Server. Agli utenti che hanno ricevuto l'autorizzazione di accesso alla relying parti da parte del Servizio federativo potrebbe comunque essere negato l'accesso dalla stessa relying party.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con lo snap di gestione di ADFS\-in.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Per creare una regola per consentire agli utenti in base a un'attestazione in ingresso in Windows Server 2016
 
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **criteri di controllo di accesso**. 
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Pulsante destro del mouse e selezionare **aggiungere criteri di controllo di accesso**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Nella casella Nome immettere un nome per il criterio, una descrizione e fare clic su **Aggiungi**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. Nel **Editor delle regole**, sotto gli utenti, inserire un segno di spunta **con le attestazioni specifiche nella richiesta** e fare clic sul sottolineato **specifico** nella parte inferiore.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. Nel **Selezionare attestazioni** schermata, fare clic sul **attestazioni** pulsante di opzione, selezionare il **tipo di attestazione**, il **operatore**, e **il valore dell'attestazione** quindi fare clic su **Ok**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Nel **Editor delle regole** fare clic su **Ok**.  Nel **aggiungere criteri di controllo di accesso** schermata, fare clic su **Ok**.

8. Nel **di gestione di ADFS** albero della console in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Pulsante destro del mouse il **Trust della Relying Party** che si desidera consentire l'accesso a e selezionare **Modifica criteri di controllo di accesso**.  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Nei criteri di controllo di accesso, selezionare i criteri e quindi fare clic su **Applica** e **Ok**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Per creare una regola per negare agli utenti in base a un'attestazione in ingresso in Windows Server 2016
 
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **criteri di controllo di accesso**. 
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Pulsante destro del mouse e selezionare **aggiungere criteri di controllo di accesso**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Nella casella Nome immettere un nome per il criterio, una descrizione e fare clic su **Aggiungi**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. Nel **Editor delle regole**, assicurarsi che tutti gli utenti sia selezionato e in **tranne** inserire un segno di spunta **con le attestazioni specifiche nella richiesta** e fare clic sul sottolineato **specifico** nella parte inferiore.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. Nel **Selezionare attestazioni** schermata, fare clic sul **attestazioni** pulsante di opzione, selezionare il **tipo di attestazione**, il **operatore**, e **il valore dell'attestazione** quindi fare clic su **Ok**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Nel **Editor delle regole** fare clic su **Ok**.  Nel **aggiungere criteri di controllo di accesso** schermata, fare clic su **Ok**.

8. Nel **di gestione di ADFS** albero della console in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Pulsante destro del mouse il **Trust della Relying Party** che si desidera consentire l'accesso a e selezionare **Modifica criteri di controllo di accesso**.  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Nei criteri di controllo di accesso, selezionare i criteri e quindi fare clic su **Applica** e **Ok**.
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Per creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso in Windows Server 2012 R2
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.    
  
2.  Nell'albero della console, in **ADFS\\relazioni di Trust\\attendibilità**, fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  Nel **Modifica regole attestazione** nella finestra di dialogo fare clic sul **regole di autorizzazione rilascio** scheda o **le regole di autorizzazione di delega** scheda \(in base al tipo di regola di autorizzazione necessari\), e quindi fare clic su **Aggiungi regola** per avviare il **Aggiunta guidata regole attestazione di autorizzazione**.  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **consentire o negare agli utenti in base a un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, **tipo di attestazione in ingresso** Selezionare un tipo di attestazione nell'elenco, in **valore attestazione in ingresso** digitare un valore o fare clic su Sfoglia \(se è disponibile\) e selezionare un valore e quindi selezionare una delle opzioni seguenti, a seconda delle esigenze dell'organizzazione:  
  
    -   **Consenti l'accesso agli utenti con questa attestazione in ingresso**  
  
    -   **Nega l'accesso agli utenti con questa attestazione in ingresso**  
![Crea regola](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Fare clic su **Fine**.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Altri riferimenti 
[Configurare le regole delle attestazioni](Configure-Claim-Rules.md)  
 
[Elenco di controllo: creazione di regole attestazione per un trust della relying party](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usare una regola attestazioni di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Ruolo delle regole delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
