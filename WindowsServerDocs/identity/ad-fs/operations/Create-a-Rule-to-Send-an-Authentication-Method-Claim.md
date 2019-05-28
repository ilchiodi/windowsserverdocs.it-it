---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Creare una regola per l'invio di un'attestazione di metodo di autenticazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be3a16bac9c146637117aa7b9720cb4aa76177e2
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189395"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Creare una regola per l'invio di un'attestazione di metodo di autenticazione


È possibile utilizzare il **inviare l'appartenenza al gruppo come attestazioni** modello di regola o il **trasformare un'attestazione in ingresso** modello di regola per inviare un'attestazione di metodo di autenticazione. La relying party può usare un'attestazione di metodo di autenticazione per determinare il meccanismo di accesso che l'utente usa per autenticarsi e ottenere le attestazioni da Active Directory Federation Services \(ADFS\). È anche possibile usare la funzionalità di verifica del meccanismo di autenticazione di Active Directory Federation Services \(ADFS\) in Windows Server 2012 R2 come input per generare attestazioni metodo di autenticazione per le situazioni in cui la relying party desidera determinare il livello di accesso basato su accessi con smart card. Ad esempio, uno sviluppatore può assegnare diversi livelli di accesso agli utenti federati dell'applicazione relying party. I livelli di accesso sono basati su se gli utenti accedono con le credenziali nome utente e password, anziché le smart card.  
  
A seconda dei requisiti dell'organizzazione, utilizzare una delle seguenti procedure:  
  
-   Creare questa regola utilizzando il **Invia appartenenza ai gruppi come attestazioni** il modello di regola \- quando si desidera che il gruppo specificato in questo modello per determinare in definitiva il metodo di autenticazione richiesta emettere, è possibile utilizzare questo modello di regola.  
  
-   Creare questa regola utilizzando il **trasformare un'attestazione in ingresso** modello di regola \- quando si desidera modificare il metodo di autenticazione esistente in un nuovo metodo di autenticazione che funziona con un prodotto che non riconosce le attestazioni metodo di autenticazione ADFS standard, è possibile utilizzare questo modello di regola.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare tramite l'appartenenza al gruppo di invio come modello di regola attestazioni in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  Fare clic su **Sfoglia**, selezionare il gruppo i cui membri devono ricevere l'attestazione metodo di autenticazione e quindi fare clic su **OK**.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. In **valore attestazione in uscita**, digitare uno dei uniform resource identifier predefinito \(URI\) valori nella tabella seguente, a seconda del metodo di autenticazione preferito, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Livello di sicurezza del trasporto \(TLS\) l'autenticazione reciproca che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare tramite l'appartenenza al gruppo di invio come modello di regola attestazioni in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  Fare clic su **Sfoglia**, selezionare il gruppo i cui membri devono ricevere l'attestazione metodo di autenticazione e quindi fare clic su **OK**.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. In **valore attestazione in uscita**, digitare uno dei uniform resource identifier predefinito \(URI\) valori nella tabella seguente, a seconda del metodo di autenticazione preferito, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Livello di sicurezza del trasporto \(TLS\) l'autenticazione reciproca che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Per creare questa regola utilizzando la trasformazione un'attestazione in ingresso modello di regola in un Trust della Relying Party in Windows Server 2016 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **attendibilità**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica criteri di rilascio dell'attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Nel **Modifica criteri di rilascio dell'attestazione** nella finestra di dialogo **regole di trasformazione rilascio** fare clic su **Aggiungi regola** per avviare la creazione guidata regola. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare **metodo di autenticazione** nell'elenco.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. Selezionare **sostituire un valore di attestazione in ingresso con un diverso valore attestazione in uscita**, e quindi eseguire le operazioni seguenti:  
  
    1.  In **valore attestazione in ingresso**, digitare uno dei seguenti valori URI che sono in base al metodo di autenticazione effettiva URI utilizzato originariamente, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
    2.  In **valore attestazione in uscita**, digitare uno dei valori di URI predefinito nella tabella seguente, che dipende la scelta metodo di autenticazione preferito nuovo, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticazione reciproca TLS che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Oltre ai valori nella tabella, è possono utilizzare altri valori URI. I valori URI presenti ion nella tabella precedente rappresentano gli URI che accetta la relying party per impostazione predefinita.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Per creare questa regola utilizzando la trasformazione un'attestazione in ingresso modello di regola in un Trust di Provider di attestazioni in Windows Server 2016 
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS**, fare clic su **Provider di attestazioni**. 
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo **regole di trasformazione accettazione** fare clic su **Aggiungi regola** per avviare la creazione guidata regola.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare **metodo di autenticazione** nell'elenco.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. Selezionare **sostituire un valore di attestazione in ingresso con un diverso valore attestazione in uscita**, e quindi eseguire le operazioni seguenti:  
  
    1.  In **valore attestazione in ingresso**, digitare uno dei seguenti valori URI che sono in base al metodo di autenticazione effettiva URI utilizzato originariamente, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
    2.  In **valore attestazione in uscita**, digitare uno dei valori di URI predefinito nella tabella seguente, che dipende la scelta metodo di autenticazione preferito nuovo, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticazione reciproca TLS che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Creare la regola tramite l'appartenenza al gruppo di invio come modello di regola attestazioni in Windows Server 2012 R2  
  
1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS\\relazioni di Trust**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Nel **Modifica regole attestazione** nella finestra di dialogo selezionare una delle seguenti schede, in base ai trust che si sta modificando e set di regola che si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare l'appartenenza al gruppo come attestazione** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  Fare clic su **Sfoglia**, selezionare il gruppo i cui membri devono ricevere l'attestazione metodo di autenticazione e quindi fare clic su **OK**.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. In **valore attestazione in uscita**, digitare uno dei uniform resource identifier predefinito \(URI\) valori nella tabella seguente, a seconda del metodo di autenticazione preferito, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Livello di sicurezza del trasporto \(TLS\) l'autenticazione reciproca che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Oltre ai valori nella tabella, è possono utilizzare altri valori URI. I valori URI che vengono visualizzati nella tabella precedente rappresentano gli URI che accetta la relying party per impostazione predefinita.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Per creare questa regola utilizzando la trasformazione di un modello di regola attestazione in ingresso in Windows Server 2012 R2  
   
  
  
1.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione ADFS**.  
  
2.  Nell'albero della console, in **ADFS\\relazioni di Trust**, fare clic su **Provider di attestazioni** o **attendibilità**, quindi fare clic su una relazione di trust specifico nell'elenco in cui si desidera creare la regola.  
  
3.  Destra\-fare clic sul trust selezionato e quindi fare clic su **Modifica regole attestazione**.  
![Crea regola](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  Nel **Modifica regole attestazione** la finestra di dialogo, selezionare una le schede seguenti, che dipende la relazione di trust che si sta modificando e nel quale set di regole si desidera creare questa regola e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola associata a tale set di regole:  
  
    -   **Regole di trasformazione accettazione**  
  
    -   **Regole di trasformazione rilascio**  
  
    -   **Regole di autorizzazione rilascio**  
  
    -   **Regole di autorizzazione di delega**  
![Crea regola](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **trasformare un'attestazione in ingresso** dall'elenco, quindi fare clic su **Avanti**.  
![Crea regola](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  Nel **configurare la regola** digitare un nome di regola attestazione.  
  
7.  In **tipo di attestazione in ingresso**, selezionare **metodo di autenticazione** nell'elenco.  
  
8.  In **attestazione in uscita**, selezionare **metodo di autenticazione** nell'elenco.  
  
9. Selezionare **sostituire un valore di attestazione in ingresso con un diverso valore attestazione in uscita**, e quindi eseguire le operazioni seguenti:  
  
    1.  In **valore attestazione in ingresso**, digitare uno dei seguenti valori URI che sono in base al metodo di autenticazione effettiva URI utilizzato originariamente, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
    2.  In **valore attestazione in uscita**, digitare uno dei valori di URI predefinito nella tabella seguente, che dipende la scelta metodo di autenticazione preferito nuovo, fare clic su **Fine**, quindi fare clic su **OK** per salvare la regola.  
  
|Metodo di autenticazione effettivo|URI corrispondente|  
|--------------------------------|---------------------|  
|Mediante autenticazione con nome utente e password|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticazione di Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticazione reciproca TLS che utilizza certificati x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-basato su autenticazione che utilizzano TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![creare una regola](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Oltre ai valori nella tabella, è possono utilizzare altri valori URI. I valori URI presenti ion nella tabella precedente rappresentano gli URI che accetta la relying party per impostazione predefinita.  

## <a name="additional-references"></a>Altri riferimenti 
[Configurare le regole delle attestazioni](Configure-Claim-Rules.md)  
 
[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del componente](https://technet.microsoft.com/library/ee913578.aspx)  

[Elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del provider di attestazioni](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usare una regola di attestazione di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[Ruolo delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[Ruolo delle regole delle attestazioni](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
