---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Creare una regola per inviare attributi LDAP come attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Creare una regola per inviare attributi LDAP come attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Utilizza inviare attributi LDAP come modello di regola attestazioni in Active Directory Federation Services \(AD FS\), è possibile creare una regola di selezione degli attributi da un archivio di attributi \(LDAP\) Lightweight Directory Access Protocol, ad esempio Active Directory, da inviare come attestazioni alla relying party. Ad esempio, è possibile utilizzare questo modello di regola per creare un inviare attributi LDAP come attestazioni di regole che verranno estratti i valori di attributo per gli utenti autenticati dal **displayName** e **telephoneNumber** attributi di Active Directory e quindi inviare questi valori come due diverse attestazioni in uscita.  
  
È inoltre possibile utilizzare questa regola per inviare l'appartenenza al gruppo dell'utente. Se si desidera inviare solo le singole appartenenze, utilizzare l'appartenenza al gruppo di invio come un modello di regola attestazione. È possibile utilizzare la procedura seguente per creare una regola attestazione con la gestione di ADFS snap-in.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per inviare attributi LDAP come attestazioni per un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, seleziona il **archivio di attributi**, quindi selezionare l'attributo LDAP ed eseguirne il mapping al tipo di attestazione in uscita. 
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per inviare attributi LDAP come attestazioni per un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni **. 
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, seleziona il **archivio di attributi**, quindi selezionare l'attributo LDAP ed eseguirne il mapping al tipo di attestazione in uscita. 
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Per creare una regola per inviare attributi LDAP come attestazioni per Windows Server 2012 R2  
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  Nell'albero della console, in **AD FSAD FS\\Trust relazioni**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare questa regola.  
  
3.  Con-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione **.
![Creare una regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo selezionare una delle seguenti schede, in base ai trust che si sta modificando e set di regola che si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Creare una regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti **.  
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Nel **configurare la regola** pagina **nome regola attestazione** digitare il nome visualizzato per questa regola, in **archivio di attributi** selezionare **Active Directory**e in **i tipi di attestazione di Mapping degli attributi LDAP in uscita** seleziona le opzioni desiderate **attributo LDAP** e corrispondente **il tipo di attestazione in uscita** tipi dagli elenchi di elenchi a discesa.  
  
    È necessario selezionare un nuovo attributo LDAP e una coppia di tipo di attestazione in uscita su una riga diversa per ogni attributo di Active Directory che si desidera rilasciare un'attestazione per come parte di questa regola.  
![Creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Fare clic su di **fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[Configurare le regole di attestazione](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole attestazione per un Provider di attestazioni attendibile](https://technet.microsoft.com/library/ee913564.aspx)  
  
[When to Use an Authorization Claim Rule](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Il ruolo di regole attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
