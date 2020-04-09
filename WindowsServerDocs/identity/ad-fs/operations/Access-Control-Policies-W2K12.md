---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Criteri di controllo degli accessi in AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7355ff9ed49a5e4ee8bca3a3d266a0ec1ecc0780
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814894"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Criteri di controllo degli accessi in Windows Server 2012 R2 e Windows Server 2012 AD FS


I criteri descritti in questo articolo fanno uso di due tipi di attestazioni  

1.  Attestazioni AD FS crea in base alle informazioni che i AD FS e il proxy dell'applicazione Web possono esaminare e verificare, ad esempio l'indirizzo IP del client che si connette direttamente al AD FS o al WAP.  

2.  Attestazioni AD FS crea in base alle informazioni trasmesse al AD FS dal client come intestazioni HTTP  

>**Importante**: i criteri come descritto di seguito bloccano l'aggiunta a un dominio di Windows 10 e gli scenari di accesso che richiedono l'accesso ai seguenti endpoint aggiuntivi

AD FS endpoint necessari per l'aggiunta a un dominio e l'accesso a Windows 10
- [nome servizio federativo]/ADFS/Services/Trust/2005/windowstransport
- [nome servizio federativo]/ADFS/Services/Trust/13/windowstransport
- [nome servizio federativo]/ADFS/Services/Trust/2005/usernamemixed
- [nome servizio federativo]/ADFS/Services/Trust/13/usernamemixed 
- [nome servizio federativo]/ADFS/Services/Trust/2005/certificatemixed
- [nome servizio federativo]/ADFS/Services/Trust/13/certificatemixed

>**Importante**: gli endpoint/ADFS/Services/Trust/2005/windowstransport e/ADFS/Services/Trust/13/windowstransport devono essere abilitati solo per l'accesso alla Intranet perché sono destinati ad endpoint con connessione Intranet che usano il binding WIA su HTTPS. L'esposizione a Extranet potrebbe consentire le richieste a questi endpoint di ignorare le protezioni di blocco. Questi endpoint devono essere disabilitati sul proxy (disabilitato dalla Extranet) per proteggere il blocco degli account di Active Directory. 

Per risolvere il problemi, aggiornare tutti i criteri che negano l'attestazione basata sull'endpoint per consentire l'eccezione per gli endpoint precedenti.

Ad esempio, la regola seguente:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

verrà aggiornato a:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Le attestazioni di questa categoria devono essere usate solo per implementare i criteri di business e non come criteri di sicurezza per proteggere l'accesso alla rete.  È possibile che i client non autorizzati invii intestazioni con informazioni false come modo per ottenere l'accesso.  

I criteri descritti in questo articolo devono essere sempre usati con un altro metodo di autenticazione, ad esempio nome utente e password o autenticazione a più fattori.  

## <a name="client-access-policies-scenarios"></a>Scenari di criteri di accesso client  

|**Scenario**|**Descrizione**| 
| --- | --- | 
|Scenario 1: bloccare tutto l'accesso esterno a Office 365|L'accesso a Office 365 è consentito da tutti i client della rete aziendale interna, ma le richieste provenienti da client esterni vengono negate in base all'indirizzo IP del client esterno.|  
|Scenario 2: bloccare l'accesso esterno a Office 365 eccetto Exchange ActiveSync|L'accesso a Office 365 è consentito da tutti i client della rete aziendale interna, nonché da qualsiasi dispositivo client esterno, ad esempio smartphone, che usano Exchange ActiveSync. Tutti gli altri client esterni, ad esempio quelli che usano Outlook, sono bloccati.|  
|Scenario 3: bloccare l'accesso esterno a Office 365 ad eccezione delle applicazioni basate su browser|Blocca l'accesso esterno a Office 365, ad eccezione delle applicazioni passive (basate su browser), ad esempio Outlook Accesso Web o SharePoint Online.|  
|Scenario 4: bloccare tutto l'accesso esterno a Office 365 ad eccezione dei gruppi di Active Directory designati|Questo scenario viene usato per il test e la convalida della distribuzione dei criteri di accesso client. Blocca l'accesso esterno a Office 365 solo per i membri di uno o più Active Directory gruppo. Può essere usato anche per fornire accesso esterno solo ai membri di un gruppo.|  

## <a name="enabling-client-access-policy"></a>Abilitazione dei criteri di accesso client  
 Per abilitare i criteri di accesso client in AD FS in Windows Server 2012 R2, è necessario aggiornare il relying party trust della piattaforma di identità Microsoft Office 365. Scegliere uno degli scenari di esempio seguenti per configurare le regole attestazioni nella **piattaforma di identità Microsoft Office 365** relying party attendibilità che soddisfi meglio le esigenze dell'organizzazione.  

###  <a name="scenario-1-block-all-external-access-to-office-365"></a><a name="scenario1"></a>Scenario 1: bloccare tutto l'accesso esterno a Office 365  
 Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e blocca tutti i client esterni in base all'indirizzo IP del client esterno. È possibile usare le procedure seguenti per aggiungere le regole di autorizzazione di rilascio corrette al trust di Office 365 relying party per lo scenario scelto.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Per creare regole per bloccare l'accesso esterno a Office 365  

1.  In **Server Manager**fare clic su **strumenti**, quindi su **gestione ad FS**.  

2.  Nell'albero della console, in **ad Adfs\relazioni relazioni**, fare clic su **attendibilità componente**, fare clic con il pulsante destro del mouse su **Microsoft Office 365 Identity Platform** Trust, quindi fare clic su **modifica regole attestazione**.  

3.  Nella finestra di dialogo **modifica regole attestazione** selezionare la scheda **regole di autorizzazione rilascio** e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola attestazione.  

4.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione**, selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se esiste un'attestazione IP al di fuori dell'intervallo desiderato, negare". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente (sostituire il valore precedente per "x-ms-inoltred-client-IP" con un'espressione IP valida):  
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco regole di autorizzazione rilascio prima della regola predefinita **Consenti accesso a tutti gli utenti** (la regola nega avrà la precedenza anche se viene visualizzata in precedenza nell'elenco).  Se non si ha la regola di accesso consentito predefinita, è possibile aggiungerne una alla fine dell'elenco usando il linguaggio delle regole attestazioni come indicato di seguito:  </br>

    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Per salvare le nuove regole, nella finestra di dialogo **modifica regole attestazione** fare clic su **OK**. L'elenco risultante avrà un aspetto simile al seguente.  

     ![Regole di autorizzazione di rilascio](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a><a name="scenario2"></a>Scenario 2: bloccare l'accesso esterno a Office 365 eccetto Exchange ActiveSync  
 Nell'esempio seguente viene consentito l'accesso a tutte le applicazioni Office 365, incluso Exchange Online, da client interni, incluso Outlook. Blocca l'accesso dai client che si trovano all'esterno della rete aziendale, come indicato dall'indirizzo IP del client, ad eccezione dei client di Exchange ActiveSync, ad esempio smartphone.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Per creare regole per bloccare l'accesso esterno a Office 365 eccetto Exchange ActiveSync  

1.  In **Server Manager**fare clic su **strumenti**, quindi su **gestione ad FS**.  

2.  Nell'albero della console, in **ad Adfs\relazioni relazioni**, fare clic su **attendibilità componente**, fare clic con il pulsante destro del mouse su **Microsoft Office 365 Identity Platform** Trust, quindi fare clic su **modifica regole attestazione**.  

3.  Nella finestra di dialogo **modifica regole attestazione** selezionare la scheda **regole di autorizzazione rilascio** e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola attestazione.  

4.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione**, selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se esiste un'attestazione IP al di fuori dell'intervallo desiderato, emettere ipoutsiderange claim". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente (sostituire il valore precedente per "x-ms-inoltred-client-IP" con un'espressione IP valida):  

    `c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

7.  Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

8.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione**, selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se c'è un indirizzo IP non compreso nell'intervallo desiderato ed è presente un'attestazione x-MS-client-applicazione non EAS, nega". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

11. Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

12. Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione,** Selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

13. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "controlla se l'attestazione dell'applicazione esiste". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente:  

   ```  
   NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

15. Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

16. Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione,** Selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

17. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "nega utenti con ipoutsiderange true e applicazione non riuscita". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente:  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola precedente e prima della regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se viene visualizzata in precedenza nell'elenco).  </br>Se non si ha la regola di accesso consentito predefinita, è possibile aggiungerne una alla fine dell'elenco usando il linguaggio delle regole attestazioni come indicato di seguito:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Per salvare le nuove regole, nella finestra di dialogo **modifica regole attestazione** fare clic su OK. L'elenco risultante avrà un aspetto simile al seguente.  

    ![Regole di autorizzazione rilascio](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a><a name="scenario3"></a>Scenario 3: bloccare l'accesso esterno a Office 365 ad eccezione delle applicazioni basate su browser  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare regole per bloccare l'accesso esterno a Office 365 ad eccezione delle applicazioni basate su browser  

1.  In **Server Manager**fare clic su **strumenti**, quindi su **gestione ad FS**.  

2.  Nell'albero della console, in **ad Adfs\relazioni relazioni**, fare clic su **attendibilità componente**, fare clic con il pulsante destro del mouse su **Microsoft Office 365 Identity Platform** Trust, quindi fare clic su **modifica regole attestazione**.  

3.  Nella finestra di dialogo **modifica regole attestazione** selezionare la scheda **regole di autorizzazione rilascio** e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola attestazione.  

4.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione**, selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se esiste un'attestazione IP al di fuori dell'intervallo desiderato, emettere ipoutsiderange claim". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente (sostituire il valore precedente per "x-ms-inoltred-client-IP" con un'espressione IP valida):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

7.  Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

8.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione,** Selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se è presente un indirizzo IP non compreso nell'intervallo desiderato e l'endpoint non è/ADFS/ls, nega". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco regole di autorizzazione rilascio prima della regola predefinita **Consenti accesso a tutti gli utenti** (la regola nega avrà la precedenza anche se viene visualizzata in precedenza nell'elenco).  </br></br> Se non si ha la regola di accesso consentito predefinita, è possibile aggiungerne una alla fine dell'elenco usando il linguaggio delle regole attestazioni come indicato di seguito:  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Per salvare le nuove regole, nella finestra di dialogo **modifica regole attestazione** fare clic su **OK**. L'elenco risultante avrà un aspetto simile al seguente.  

    ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario-4-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a><a name="scenario4"></a>Scenario 4: bloccare tutto l'accesso esterno a Office 365 ad eccezione dei gruppi di Active Directory designati  
 Nell'esempio seguente viene abilitato l'accesso da client interni in base all'indirizzo IP. Blocca l'accesso dai client che si trovano all'esterno della rete aziendale che dispongono di un indirizzo IP client esterno, ad eccezione di quelli inclusi in un gruppo di Active Directory specificato. attenersi alla procedura seguente per aggiungere le regole di autorizzazione di rilascio corrette al **Microsoft Office 365 Identity Platform** relying party trust mediante la creazione guidata regola attestazione:  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Per creare regole per bloccare l'accesso esterno a Office 365, ad eccezione dei gruppi di Active Directory designati  

1.  In **Server Manager**fare clic su **strumenti**, quindi su **gestione ad FS**.  

2.  Nell'albero della console, in **ad Adfs\relazioni relazioni**, fare clic su **attendibilità componente**, fare clic con il pulsante destro del mouse su **Microsoft Office 365 Identity Platform** Trust, quindi fare clic su **modifica regole attestazione**.  

3.  Nella finestra di dialogo **modifica regole attestazione** selezionare la scheda **regole di autorizzazione rilascio** e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola attestazione.  

4.  Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione**, selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se esiste un'attestazione IP al di fuori dell'intervallo desiderato, emettere l'attestazione ipoutsiderange". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente (sostituire il valore precedente per "x-ms-inoltred-client-IP" con un'espressione IP valida):  


~~~
`c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

7. Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

8. Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione,** Selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "check Group SID". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente (sostituire "groupsid" con il SID effettivo del gruppo Active Directory in uso):  

    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata nell'elenco **regole di autorizzazione rilascio** .  

11. Quindi, nella scheda **regole di autorizzazione rilascio** della finestra di dialogo **modifica regole attestazione** fare clic su **Aggiungi regola** per avviare di nuovo la creazione guidata regola attestazione.  

12. Nella pagina **Seleziona modello di regola** , in **modello di regola attestazione,** Selezionare **Invia attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

13. Nella pagina **Configura regola** , in **Nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "nega utenti con ipoutsiderange true e GroupSid fail". In **regola personalizzata**Digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente:  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Fare clic su **Fine**. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola precedente e prima della regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se viene visualizzata in precedenza nell'elenco).  </br></br>Se non si ha la regola di accesso consentito predefinita, è possibile aggiungerne una alla fine dell'elenco usando il linguaggio delle regole attestazioni come indicato di seguito:  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Per salvare le nuove regole, nella finestra di dialogo **modifica regole attestazione** fare clic su OK. L'elenco risultante avrà un aspetto simile al seguente.  

     ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="building-the-ip-address-range-expression"></a><a name="buildingip"></a>Compilazione dell'espressione intervallo di indirizzi IP  
 L'attestazione x-ms-inoltred-client-IP viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. Il valore dell'attestazione può essere uno dei seguenti:  

> [!NOTE]
>  Exchange Online supporta attualmente solo indirizzi IPV4 e non IPV6.  

-   Un singolo indirizzo IP: l'indirizzo IP del client connesso direttamente a Exchange Online  

> [!NOTE]
> - L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP dell'interfaccia esterna del proxy o del gateway dell'organizzazione in uscita.  
>   -   I client connessi alla rete aziendale da una VPN o da Microsoft DirectAccess (DA) possono essere visualizzati come client aziendali interni o come client esterni, a seconda della configurazione di VPN o DA.  

-   Uno o più indirizzi IP: quando Exchange Online non è in grado di determinare l'indirizzo IP del client che si connette, il valore verrà impostato in base al valore dell'intestazione x-inoltro-for, un'intestazione non standard che può essere inclusa nelle richieste basate su HTTP ed è supportata da molti client, bilanciamenti del carico e proxy sul mercato.  

> [!NOTE]
> 1. Più indirizzi IP, che indicano l'indirizzo IP del client e l'indirizzo di ogni proxy che ha superato la richiesta, saranno separati da una virgola.  
>    2. Gli indirizzi IP correlati all'infrastruttura di Exchange Online non vengono elencati.  

### <a name="regular-expressions"></a>Espressioni regolari  
 Quando è necessario trovare una corrispondenza per un intervallo di indirizzi IP, è necessario creare un'espressione regolare per eseguire il confronto. Nella serie successiva di passaggi, si forniranno esempi per la creazione di un'espressione di questo tipo in modo che corrispondano agli intervalli di indirizzi seguenti (si noti che sarà necessario modificare questi esempi in modo che corrispondano all'intervallo di indirizzi IP pubblici):  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1-10.0.0.14  

  In primo luogo, il modello di base che corrisponderà a un singolo indirizzo IP è il seguente: \b # # #\\. # # #\\. # # #\\. # # # \b  

  Estendendo questo, possiamo trovare la corrispondenza con due indirizzi IP diversi con un'espressione OR come segue: \b # # #\\. # # #\\. # # #\\.&#124;# # # \b \b # # #\\. # # #\\. # # #\\. # # # \b  

  Un esempio per trovare una corrispondenza con due soli indirizzi, ad esempio 192.168.1.1 o 10.0.0.1, è: \b192\\. 168\\1\\1 \ b&#124;\b10\\. 0\\. 0\\. 1 \ b  

  In questo modo si ottiene la tecnica con cui è possibile immettere un numero qualsiasi di indirizzi. Se è necessario consentire un intervallo di indirizzi, ad esempio 192.168.1.1-192.168.1.25, la corrispondenza deve essere eseguita carattere per carattere: \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b  

  Tenere presente quanto segue:  

- L'indirizzo IP viene considerato come stringa e non come numero.  

- La regola viene suddivisa nel modo seguente: \b192\\. 168\\. 1\\.  

- Corrisponde a qualsiasi valore che inizia con 192.168.1.  

- Il codice seguente corrisponde agli intervalli necessari per la parte dell'indirizzo dopo il separatore decimale finale:  

  -   ([1-9] corrisponde a indirizzi che terminano con 1-9  

  -   &#124;1 [0-9] corrisponde a indirizzi che terminano con 10-19  

  -   &#124;2 [0-5]) corrisponde a indirizzi che terminano con 20-25  

- Si noti che le parentesi devono essere posizionate correttamente, in modo che non venga avviata la corrispondenza di altre parti di indirizzi IP.  

- Con la corrispondenza del blocco 192, è possibile scrivere un'espressione simile per il blocco 10: \b10\\. 0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

- E inserendoli insieme, l'espressione seguente deve corrispondere a tutti gli indirizzi per "192.168.1.1 ~ 25" e "10.0.0.1 ~ 14": \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\. 0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>Test dell'espressione  
 Le espressioni Regex possono diventare piuttosto complesse, quindi è consigliabile usare uno strumento di verifica Regex. Se si esegue una ricerca in Internet per "generatore di espressioni Regex online", sono disponibili diverse utilità online utili che consentono di provare le espressioni sui dati di esempio.  

 Quando si esegue il test dell'espressione, è importante comprendere cosa si prevede di dover trovare la corrispondenza. Il sistema Exchange Online può inviare molti indirizzi IP separati da virgole. Le espressioni fornite sopra funzioneranno per questa operazione. Tuttavia, è importante considerare questo aspetto quando si testano le espressioni Regex. Ad esempio, è possibile usare l'input di esempio seguente per verificare gli esempi precedenti:  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1  

## <a name="claim-types"></a>Tipi di attestazione  
 AD FS in Windows Server 2012 R2 fornisce informazioni sul contesto della richiesta usando i tipi di attestazione seguenti:  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-Inoltred-client-IP  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Questa attestazione AD FS rappresenta un "tentativo migliore" di verificare l'indirizzo IP dell'utente (ad esempio, il client Outlook) che effettua la richiesta. Questa attestazione può contenere più indirizzi IP, incluso l'indirizzo di ogni proxy che ha inviato la richiesta.  Questa attestazione viene popolata da un HTTP. Il valore dell'attestazione può essere uno dei seguenti:  

-   Un singolo indirizzo IP, ovvero l'indirizzo IP del client connesso direttamente a Exchange Online  

> [!NOTE]
>  L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP dell'interfaccia esterna del proxy o del gateway dell'organizzazione in uscita.  

-   Uno o più indirizzi IP  

    -   Se Exchange Online non è in grado di determinare l'indirizzo IP del client che si connette, il valore verrà impostato in base al valore dell'intestazione x-inoltro-for, un'intestazione non standard che può essere inclusa nelle richieste basate su HTTP ed è supportata da molti client, bilanciamenti del carico e proxy sul mercato.  

    -   Più indirizzi IP che indicano l'indirizzo IP del client e l'indirizzo di ogni proxy che ha superato la richiesta saranno separati da una virgola.  

> [!NOTE]
>  Gli indirizzi IP correlati all'infrastruttura di Exchange Online non saranno presenti nell'elenco.  

> [!WARNING]
>  Exchange Online supporta attualmente solo indirizzi IPV4. non supporta gli indirizzi IPV6.  

### <a name="x-ms-client-application"></a>X-MS-client-applicazione  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Questa attestazione AD FS rappresenta il protocollo utilizzato dal client finale, che corrisponde liberamente all'applicazione utilizzata.  Questa attestazione viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:  

-   Nel caso di dispositivi che usano Exchange Active Sync, il valore è Microsoft. Exchange. ActiveSync.  

-   L'utilizzo del client Microsoft Outlook può causare uno dei valori seguenti:  

    -   Microsoft. Exchange. AutoDiscovery  

    -   Microsoft. Exchange. OfflineAddressBook  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices  

-   Gli altri valori possibili per questa intestazione includono i seguenti:  

    -   Microsoft. Exchange. PowerShell  

    -   Microsoft. Exchange. SMTP  

    -   Microsoft. Exchange. pop  

    -   Microsoft. Exchange. IMAP  

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Questa attestazione AD FS fornisce una stringa per rappresentare il tipo di dispositivo utilizzato dal client per accedere al servizio. Questo può essere usato quando i clienti vogliono impedire l'accesso per determinati dispositivi, ad esempio tipi specifici di smartphone.  I valori di esempio per questa attestazione includono (ma non sono limitati) i valori riportati di seguito.  

 Di seguito sono riportati alcuni esempi di ciò che il valore x-ms-User-Agent potrebbe contenere per un client il cui x-MS-client-Application è "Microsoft. Exchange. ActiveSync".  

- Vortice/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0.3  

  È anche possibile che questo valore sia vuoto.  

### <a name="x-ms-proxy"></a>X-MS-proxy  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Questa attestazione AD FS indica che la richiesta è passata attraverso il proxy dell'applicazione Web.  Questa attestazione viene popolata dal proxy dell'applicazione Web, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione al back-end Servizio federativo. AD FS quindi la converte in un'attestazione.  

 Il valore dell'attestazione è il nome DNS del proxy dell'applicazione Web che ha superato la richiesta.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo di attestazione: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Analogamente al tipo di attestazione x-MS-proxy precedente, questo tipo di attestazione indica se la richiesta ha superato il proxy dell'applicazione Web. Diversamente da x-MS-proxy, insidecorporatenetwork è un valore booleano true che indica una richiesta diretta al servizio federativo dall'interno della rete aziendale.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-endpoint-Absolute-Path (attivo rispetto a passivo)  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Questo tipo di attestazione può essere utilizzato per determinare le richieste originate da client "attivi" (avanzati) rispetto ai client "passivi" (basati su Web browser). In questo modo è possibile consentire le richieste esterne da applicazioni basate su browser, ad esempio Outlook Accesso Web, SharePoint Online o il portale di Office 365, mentre le richieste provenienti da client avanzati come Microsoft Outlook sono bloccate.  

 Il valore dell'attestazione è il nome del servizio AD FS che ha ricevuto la richiesta.  

## <a name="see-also"></a>Vedi anche  
 [Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
