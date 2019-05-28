---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Creare una regola per inviare attributi LDAP come attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00ea4f9f868b9c82c2a0859be971db26394251a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189353"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Creare una regola per inviare attributi LDAP come attestazioni


Tramite l'invio di attributi LDAP come modello di regola attestazioni in Active Directory Federation Services \(ADFS\), è possibile creare una regola di selezione degli attributi da un Lightweight Directory Access Protocol \(LDAP\)archivio di attributi, ad esempio Active Directory, da inviare come attestazioni alla relying party. Ad esempio, è possibile usare questo modello di regola per creare un inviare attributi LDAP come attestazioni di regole che verranno estratti i valori di attributo per gli utenti autenticati dal **displayName** e **telephoneNumber** Active Directory di attributi e quindi inviare questi valori come due diversi attestazioni in uscita.  
  
È anche possibile usare questa regola per inviare tutte le appartenenze a gruppi dell'utente. Se si vuole inviare solo le singole appartenenze ai gruppi, usare il modello di regola Invio dell'appartenenza a un gruppo come attestazione. È possibile utilizzare la procedura seguente per creare una regola attestazione con lo snap di gestione di ADFS\-in.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Per creare una regola per inviare attributi LDAP come attestazioni per un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, selezionare il **archivio attributi**, quindi selezionare l'attributo LDAP ed eseguirne il mapping al tipo di attestazione in uscita. 
![Crea regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Per creare una regola per inviare attributi LDAP come attestazioni per un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, selezionare il **archivio attributi**, quindi selezionare l'attributo LDAP ed eseguirne il mapping al tipo di attestazione in uscita. 
![Crea regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Per creare una regola per inviare attributi LDAP come attestazioni per Windows Server 2012 R2  
  
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
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attributi LDAP come attestazioni** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Nel **configurare la regola** pagina **Nome regola attestazione** digitare il nome visualizzato per questa regola, in **archivio attributi** selezionare **Active Directory**, e in **tipi di attestazione in uscita attributi di Mapping di LDAP** Selezionare l'elemento **attributo LDAP** e corrispondente **il tipo di attestazione in uscita** tipi dall'elenco\-giù elenchi.  
  
    È necessario selezionare un nuovo attributo LDAP e una coppia di tipo di attestazione in uscita su una riga diversa per ogni attributo di Active Directory che si desidera rilasciare un'attestazione per come parte di questa regola.  
![creare una regola](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Fare clic sui **Fine** pulsante.  
  
8.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK** per salvare la regola.  

## <a name="additional-references"></a>Altri riferimenti 
[Configurare le regole delle attestazioni](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del componente](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del provider di attestazioni](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usare una regola di attestazione di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Ruolo delle regole delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
