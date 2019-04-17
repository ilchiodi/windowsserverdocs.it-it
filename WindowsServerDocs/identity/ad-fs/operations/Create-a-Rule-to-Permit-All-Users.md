---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Creare una regola per consentire tutti gli utenti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>Creare una regola per consentire tutti gli utenti

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In Windows Server 2016, è possibile utilizzare un **criteri di controllo di accesso** per creare una regola che consentirà tutti gli utenti di accedere a una relying party.  In Windows Server 2012 R2, utilizzando il **consentire tutti gli utenti** modello di regola in Active Directory Federation Services \(AD FS\), è possibile creare una regola di autorizzazione che consentirà tutti gli utenti di accedere alla relying party. 

È possibile utilizzare le regole di autorizzazione aggiuntive per limitare ulteriormente l'accesso. Gli utenti autorizzati ad accedere alla relying party dal servizio federativo potrebbero comunque essere negato l'accesso dalla relying party.  
  
È possibile utilizzare le procedure seguenti per creare una regola attestazione con la gestione di ADFS snap-in.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Per creare una regola per consentire tutti gli utenti in Windows Server 2016

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Fare doppio clic su di **Trust della Relying Party** che si desidera consentire l'accesso a e selezionare **Modifica criteri di controllo di accesso **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. Per l'accesso alla controllo criteri selezionare **consentire tutti gli utenti** e quindi fare clic su **applica** e **Ok **.
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Per creare una regola per consentire tutti gli utenti in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **AD FS\\Trust Relationships\\Relying attendibilità**, fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  

3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic sul **regole di autorizzazione rilascio** scheda o **regole di autorizzazione di delega** scheda \ (in base al tipo di regola di autorizzazione è require\) e quindi fare clic su **Aggiungi regola** per avviare il **Aggiunta guidata regole attestazione di autorizzazione **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **consentire tutti gli utenti** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  Nel **configurare la regola** pagina, fare clic su **fine **.  
  
7.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
