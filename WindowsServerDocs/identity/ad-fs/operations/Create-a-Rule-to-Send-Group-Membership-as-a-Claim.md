---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d4fb03de9de2dcca36b18ce089db11ed599de820
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Utilizza l'appartenenza al gruppo di invio come un modello di regola attestazione in Active Directory Federation Services \(AD FS\), è possibile creare una regola che consente di selezionare un gruppo di sicurezza di Active Directory per inviare come attestazione. Verrà generata solo una singola attestazione da questa regola, in base al gruppo selezionato. Ad esempio, è possibile utilizzare questo modello di regola per creare una regola che invierà un'attestazione di gruppo con un valore di amministratore, se l'utente è membro del gruppo di sicurezza Domain Admins. Questa regola deve essere utilizzata solo per gli utenti nel dominio Active Directory locale.  
  
È possibile utilizzare la procedura seguente per creare una regola attestazione con la gestione di ADFS snap-in.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per inviare l'appartenenza al gruppo come attestazione in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, in **gruppo dell'utente** fare clic su **Sfoglia** e selezionare un gruppo, in **tipo di attestazione in uscita** selezionare il tipo di attestazione desiderato e quindi in **il tipo di attestazione in uscita** digitare un valore.
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
## <a name="to-create-a-rule-to-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per inviare l'appartenenza al gruppo come attestazione in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, in **gruppo dell'utente** fare clic su **Sfoglia** e selezionare un gruppo, in **tipo di attestazione in uscita** selezionare il tipo di attestazione desiderato e quindi in **il tipo di attestazione in uscita** digitare un valore. 
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Per creare una regola per inviare l'appartenenza al gruppo come attestazione in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **AD FS\\Trust relazioni**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo selezionare una delle seguenti schede, in base ai trust che si sta modificando e set di regola che si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti**.  
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, in **gruppo dell'utente** fare clic su **Sfoglia** e selezionare un gruppo, in **tipo di attestazione in uscita** selezionare il tipo di attestazione desiderato e quindi in **il tipo di attestazione in uscita** digitare un valore.  
![Creare una regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Fare clic su **fine **.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  



## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole attestazione per un Provider di attestazioni attendibile](https://technet.microsoft.com/library/ee913564.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
