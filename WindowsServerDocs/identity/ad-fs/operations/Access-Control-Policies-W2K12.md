---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Criteri di controllo degli accessi in AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc3de72aa993546151b103d6eeacd160f81a4270
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444378"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Criteri di controllo di accesso in Windows Server 2012 R2 e Windows Server 2012 AD ADFS


Verificare i criteri descritti in questo articolo usa due tipi di attestazioni  

1.  Attestazioni che ADFS crea in base alle informazioni di proxy applicazione Web e AD FS è possibile esaminare e verificare, ad esempio l'indirizzo IP del client la connessione diretta a AD FS o WAP.  

2.  Le attestazioni ADFS crea in base alle informazioni inviate al servizio AD FS dal client come intestazioni HTTP  

>**Important**: I criteri come descritto di seguito si blocca aggiunta a un dominio Windows 10 e accedere in scenari che richiedono l'accesso agli endpoint aggiuntivi seguenti

Endpoint ADFS di Active Directory necessari per Windows 10 aggiunta a dominio e accesso in
- [nome servizio federativo] / adfs/services/trust/2005/windowstransport
- [nome servizio federativo] / adfs/services/trust/13/windowstransport
- [nome servizio federativo] / adfs/services/trust/2005/usernamemixed
- [nome servizio federativo] / adfs/services/trust/13/usernamemixed 
- [nome servizio federativo] / adfs/services/trust/2005/certificatemixed
- [nome servizio federativo] / adfs/services/trust/13/certificatemixed

Per correggere, aggiornare tutti i criteri che negano l'attestazione di endpoint per consentire l'eccezione per gli endpoint precedenti in base.

Ad esempio, la regola seguente:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

verrebbe aggiornato per:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Le attestazioni da questa categoria devono essere usate solo per implementare i criteri di business e non come i criteri di sicurezza per proteggere l'accesso alla rete.  È possibile che il client non autorizzati di inviare intestazioni con false informazioni come un modo per ottenere l'accesso.  

I criteri descritti in questo articolo dovrebbero sempre utilizzabile con un altro metodo di autenticazione, ad esempio nome utente e password o con più l'autenticazione fattore.  

## <a name="client-access-policies-scenarios"></a>Scenari i criteri di accesso client  

|**Scenario**|**Descrizione**| 
| --- | --- | 
|Scenario 1: Bloccare completamente l'accesso esterno a Office 365|Accesso a Office 365 è consentita da tutti i client della rete aziendale interna, ma vengono negate le richieste dai client esterni in base all'indirizzo IP del client esterni.|  
|Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync|Accesso a Office 365 è consentita da tutti i client nella rete azienda interna, oltre che da tutti i dispositivi client esterni, ad esempio Smartphone, che usano Exchange ActiveSync. Tutti gli altri client esterni, ad esempio quelli in Outlook, vengono bloccate.|  
|Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser|Blocca l'accesso esterno a Office 365, fatta eccezione per le applicazioni passive (basata sul browser), ad esempio Outlook Web Access o SharePoint Online.|  
|Scenario 4: Bloccare completamente l'accesso esterno a Office 365 tranne designati gruppi di Active Directory|Questo scenario viene usato per il test e convalida della distribuzione dei criteri di accesso client. Consente di bloccare l'accesso esterno a Office 365 solo ai membri di uno o più gruppi di Active Directory. Può essere utilizzato anche per fornire l'accesso esterno solo ai membri di un gruppo.|  

## <a name="enabling-client-access-policy"></a>Abilitazione di criteri di accesso Client  
 Per abilitare il criterio di accesso client in AD FS di Windows Server 2012 R2, è necessario aggiornare la piattaforma delle identità Microsoft Office 365 trust della relying party. Scegliere uno degli scenari di esempio seguenti per configurare le regole di attestazione nel **piattaforma delle identità di Microsoft Office 365** trust della relying party che meglio soddisfa le esigenze della propria organizzazione.  

###  <a name="scenario1"></a> Scenario 1: Bloccare completamente l'accesso esterno a Office 365  
 Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e blocchi di tutti i client esterni in base all'indirizzo IP del client esterni. È possibile utilizzare le procedure seguenti per aggiungere le regole di autorizzazione rilascio corrette per il trust della relying party per gli scenari scelti di Office 365.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Per creare le regole per bloccare tutto l'accesso esterno a Office 365  

1.  Dal **Server Manager**, fare clic su **Tools**, quindi fare clic su **gestione di AD FS**.  

2.  Nell'albero della console, sotto **AD FS\Trust relazioni**, fare clic su **Relying Party Trusts**, fare doppio clic il **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  

3.  Nel **Modifica regole attestazione** finestra di dialogo, seleziona la **Issuance Authorization Rules** scheda e quindi fare clic su **Add Rule** per avviare la creazione guidata regola di attestazione.  

4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se è presente qualsiasi attestazione IP compreso nell'intervallo desiderato, Nega". Sotto **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore sopra per "x-ms-inoltrati-client-ip" con un'espressione valida di IP):  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nell'elenco delle regole di autorizzazione rilascio prima sul valore predefinito **Consenti accesso a tutti gli utenti** regola (regola di negazione avrà la precedenza anche se è presente in precedenza nell'elenco).  Se non è l'impostazione predefinita l'accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco usando il linguaggio di regola attestazione nel modo seguente:  </br>

    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Per salvare le nuove regole, il **Modifica regole attestazione** della finestra di dialogo fare clic su **OK**. L'elenco risultante dovrebbe essere simile al seguente.  

     ![Regole di autorizzazione rilascio](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario2"></a> Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync  
 Nell'esempio seguente consente di accedere a tutte le applicazioni di Office 365, inclusi Exchange Online, provenienti dai client interni tra cui Outlook. Consente di bloccare l'accesso dai client che si trovano all'esterno della rete azienda, come indicato dall'indirizzo IP del client, eccetto il client di Exchange ActiveSync, ad esempio Smartphone.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Per creare le regole per bloccare tutto l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync  

1.  Dal **Server Manager**, fare clic su **Tools**, quindi fare clic su **gestione di AD FS**.  

2.  Nell'albero della console, sotto **AD FS\Trust relazioni**, fare clic su **Relying Party Trusts**, fare doppio clic il **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  

3.  Nel **Modifica regole attestazione** finestra di dialogo, seleziona la **Issuance Authorization Rules** scheda e quindi fare clic su **Add Rule** per avviare la creazione guidata regola di attestazione.  

4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "se è presente qualsiasi attestazione IP compreso nell'intervallo desiderato, rilasciare ipoutsiderange attestazione". Sotto **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore sopra per "x-ms-inoltrati-client-ip" con un'espressione valida di IP):  

    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

7.  Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

8.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se è presente un indirizzo IP compreso nell'intervallo desiderato e viene rilevata un'attestazione di x-ms-client-applicazione non EAS, Nega ". Sotto **regola personalizzata**digitare o incollare la seguente sintassi del linguaggio di regola attestazione:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

11. Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

12. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

13. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "controllare se l'attestazione dell'applicazione esiste". Sotto **regola personalizzata**digitare o incollare la seguente sintassi del linguaggio di regola attestazione:  

   ```  
   NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

15. Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

16. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

17. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "Impedisci utenti con ipoutsiderange true e l'applicazione non". Sotto **regola personalizzata**digitare o incollare la seguente sintassi del linguaggio di regola attestazione:  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Scegliere **Fine**. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola precedente e il valore predefinito Consenti accesso a tutti gli utenti prima regola nell'elenco delle regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se è presente in precedenza nell'elenco).  </br>Se non è l'impostazione predefinita l'accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco usando il linguaggio di regola attestazione nel modo seguente:</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Per salvare le nuove regole, il **Modifica regole attestazione** finestra di dialogo fare clic su OK. L'elenco risultante dovrebbe essere simile al seguente.  

    ![Regole di autorizzazione di rilascio](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a> Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare le regole per bloccare tutto l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser  

1.  Dal **Server Manager**, fare clic su **Tools**, quindi fare clic su **gestione di AD FS**.  

2.  Nell'albero della console, sotto **AD FS\Trust relazioni**, fare clic su **Relying Party Trusts**, fare doppio clic il **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  

3.  Nel **Modifica regole attestazione** finestra di dialogo, seleziona la **Issuance Authorization Rules** scheda e quindi fare clic su **Add Rule** per avviare la creazione guidata regola di attestazione.  

4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "se è presente qualsiasi attestazione IP compreso nell'intervallo desiderato, rilasciare ipoutsiderange attestazione". Sotto **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore sopra per "x-ms-inoltrati-client-ip" con un'espressione valida di IP):  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

7.  Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

8.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se è presente un indirizzo IP compreso nell'intervallo desiderato e l'endpoint non/adfs/ls, Nega". Sotto **regola personalizzata**digitare o incollare la seguente sintassi del linguaggio di regola attestazione:  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nell'elenco delle regole di autorizzazione rilascio prima sul valore predefinito **Consenti accesso a tutti gli utenti** regola (regola di negazione avrà la precedenza anche se è presente in precedenza nell'elenco).  </br></br> Se non è l'impostazione predefinita l'accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco usando il linguaggio di regola attestazione nel modo seguente:  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Per salvare le nuove regole, il **Modifica regole attestazione** della finestra di dialogo fare clic su **OK**. L'elenco risultante dovrebbe essere simile al seguente.  

    ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a> Scenario 4: Bloccare completamente l'accesso esterno a Office 365 tranne designati gruppi di Active Directory  
 Nell'esempio seguente Abilita l'accesso da client interni basate sull'indirizzo IP. Blocca l'accesso dai client che si trovano all'esterno della rete azienda che hanno un indirizzo IP del client esterni, fatta eccezione per gli utenti in una Active Group.Use di Directory specificato la procedura seguente per aggiungere l'autorizzazione di rilascio corretta delle regole per la  **Piattaforma di identità di Microsoft Office 365** trust della relying party usando la procedura guidata regola di attestazione:  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Per creare le regole per bloccare tutto l'accesso esterno a Office 365, ad eccezione dei gruppi di Active Directory designata  

1.  Dal **Server Manager**, fare clic su **Tools**, quindi fare clic su **gestione di AD FS**.  

2.  Nell'albero della console, sotto **AD FS\Trust relazioni**, fare clic su **Relying Party Trusts**, fare doppio clic il **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  

3.  Nel **Modifica regole attestazione** finestra di dialogo, seleziona la **Issuance Authorization Rules** scheda e quindi fare clic su **Add Rule** per avviare la creazione guidata regola di attestazione.  

4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "se esiste qualsiasi attestazione IP compreso nell'intervallo desiderato, rilasciare attestazioni ipoutsiderange." Sotto **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore sopra per "x-ms-inoltrati-client-ip" con un'espressione valida di IP):  


~~~
`c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

7. Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

8. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "check SID del gruppo". Sotto **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire "groupsid" con l'effettivo SID del gruppo di Active Directory si usa):  

    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Scegliere **Fine**. Verificare che la nuova regola venga visualizzato nei **Issuance Authorization Rules** elenco.  

11. Successivamente, nella **Modifica regole attestazione** finestra di dialogo il **Issuance Authorization Rules** scheda, fare clic su **Add Rule** nuovamente ad avviare la creazione guidata regola di attestazione.  

12. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  

13. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "Impedisci agli utenti con ipoutsiderange true e groupsid esito negativo". Sotto **regola personalizzata**digitare o incollare la seguente sintassi del linguaggio di regola attestazione:  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Scegliere **Fine**. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola precedente e il valore predefinito Consenti accesso a tutti gli utenti prima regola nell'elenco delle regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se è presente in precedenza nell'elenco).  </br></br>Se non è l'impostazione predefinita l'accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco usando il linguaggio di regola attestazione nel modo seguente:  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Per salvare le nuove regole, il **Modifica regole attestazione** finestra di dialogo fare clic su OK. L'elenco risultante dovrebbe essere simile al seguente.  

     ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a> La compilazione dell'espressione di intervallo di indirizzi IP  
 L'attestazione di x-ms-inoltrati--indirizzo ip del client viene popolato da un'intestazione HTTP che è attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. Il valore dell'attestazione può essere uno dei seguenti:  

> [!NOTE]
>  Exchange Online supporta attualmente solo indirizzi IPV4 e non su IPV6.  

-   Un singolo indirizzo IP: L'indirizzo IP del client che è direttamente connesso a Exchange Online  

> [!NOTE]
> - L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP interfaccia esterna del proxy in uscita dall'organizzazione o un gateway.  
>   -   I client connessi alla rete aziendale tramite una VPN o da Microsoft DirectAccess (DA) potrebbero apparire come client aziendale interno o come i client esterni a seconda della configurazione della VPN o DA.  

-   Uno o più indirizzi IP: Quando Exchange Online non è possibile determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-forwarded-for, un'intestazione non standard che può essere inclusi in richieste basate su HTTP ed è supportata da molti client, i bilanciamenti del carico, e dai proxy sul mercato.  

> [!NOTE]
> 1. Più indirizzi IP, che indica l'indirizzo di ogni proxy che la richiesta, passati e indirizzo IP del client saranno separati da una virgola.  
>    2. Gli indirizzi IP correlati all'infrastruttura Exchange Online saranno non inclusi nell'elenco.  

### <a name="regular-expressions"></a>Espressioni regolari  
 Quando si dispone per la corrispondenza di un intervallo di indirizzi IP, diventa necessario costruire un'espressione regolare per eseguire il confronto. Nella serie successiva di questa procedura, concetto verranno forniti esempi per creare tale espressione in modo da corrispondere gli intervalli di indirizzi seguente (si noti che è necessario modificare questi esempi in modo da corrispondere l'intervallo di IP pubblici):  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1 – 10.0.0.14  

  In primo luogo, il modello di base che corrisponderà a un singolo indirizzo IP è il seguente: \b###\\. # # #\\. # # #\\. # # # \b  

  Questa estensione, è possibile associare come indicato di seguito due diversi indirizzi IP con un'espressione OR: \b###\\. # # #\\. # # #\\. # # # \b&#124;\b###\\. # # #\\. # # #\\. # # # \b  

  Pertanto, un esempio in modo che corrisponda solo due indirizzi (ad esempio 192.168.1.1 o 10.0.0.1) sarebbe: \b192\\.168\\.1\\. 1\b&#124;\b10\\,0\\,0\\. 1\b  

  Questo offre la tecnica mediante il quale è possibile immettere qualsiasi numero di indirizzi. In un intervallo di indirizzi devono essere consentiti, ad esempio 192.168.1.1: 192.168.1.25, la corrispondenza è necessario eseguire carattere per carattere: \b192\\.168\\.1\\. ( [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b  

  Tenere presente quanto segue:  

- L'indirizzo IP viene considerato come stringa e non un numero.  

- La regola è suddiviso come indicato di seguito: \b192\\.168\\.1\\.  

- Ciò corrisponde a inizio qualsiasi valore con 192.168.1.  

- Gli intervalli dopo il separatore decimale finale necessari per la parte dell'indirizzo corrisponde a quanto segue:  

  -   ([1-9] abbina gli indirizzi che terminano con 1 a 9  

  -   &#124;1 [0-9] corrisponde agli indirizzi che terminano con 10-19  

  -   &#124;2[0-5]) abbina gli indirizzi che terminano con 20-25  

- Si noti che le parentesi che devono essere posizionate in modo corretto, in modo da non avviare altre parti di indirizzi IP corrispondenti.  

- Con il blocco di 192 corrispondente, è possibile scrivere un'espressione simile per il blocco di 10: \b10\\,0\\,0\\. ( [1-9] &#124;1 [0-4]) \b  

- E inserendoli insieme, l'espressione seguente deve corrispondere tutti gli indirizzi per "192.168.1.1~25" e "10.0.0.1~14": \b192\\.168\\.1\\. ( [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\,0\\,0\\. ( [1-9] &#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>L'espressione di test  
 Le espressioni di espressione regolare possono diventare piuttosto complicate, pertanto è consigliabile usare uno strumento di verifica di espressione regolare. Se si esegue una ricerca in internet per "builder espressione regex online", troverai diverse buona utilità online che ti permetterà di provare le espressioni in base a dati di esempio.  

 Quando l'espressione di test, è importante comprendere che cosa si aspettano di avere in modo che corrispondano. Il sistema di Exchange online può inviare numero di indirizzi IP, separati da virgole. Le espressioni fornite in precedenza funzionerà per questo oggetto. Tuttavia, è importante considerare questo durante il test di espressioni di espressione regolare. Uno potrebbe ad esempio, usare l'esempio seguente di input per verificare gli esempi precedenti:  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  

## <a name="claim-types"></a>Tipi di attestazione  
 ADFS in Windows Server 2012 R2 fornisce informazioni sul contesto di richiesta usando i tipi di attestazione seguenti:  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 Tipo di attestazione: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Questa attestazione AD FS rappresenta un "miglior tentativo possibile" di verificare l'indirizzo IP dell'utente (ad esempio, il client di Outlook) effettua la richiesta. Questa attestazione può contenere più indirizzi IP, incluso l'indirizzo di ogni proxy che inoltrato la richiesta.  Questa attestazione viene popolata da HTTP. Il valore dell'attestazione può essere uno dei seguenti:  

-   Un singolo indirizzo IP: indirizzo IP del client che è direttamente connesso a Exchange Online  

> [!NOTE]
>  L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP interfaccia esterna del proxy in uscita dall'organizzazione o un gateway.  

-   Uno o più indirizzi IP  

    -   Se Exchange Online non è possibile determinare l'indirizzo IP del client che si connette, imposta il valore in base al valore dell'intestazione x-forwarded-for, un'intestazione non standard che può essere inclusi in basato su HTTP richiede ed è supportata da molti client, servizi di bilanciamento del carico e proxy sul mercato.  

    -   Più indirizzi IP che indica l'indirizzo di ogni proxy che trasmesso la richiesta e l'indirizzo IP del client saranno separati da una virgola.  

> [!NOTE]
>  Gli indirizzi IP correlati all'infrastruttura Exchange Online non saranno presenti nell'elenco.  

> [!WARNING]
>  Exchange Online supporta attualmente solo gli indirizzi IPV4. non supporta gli indirizzi IPV6.  

### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 Tipo di attestazione: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Questa attestazione AD FS rappresenta il protocollo usato dal client finale, che corrisponde a regime di controllo libero per l'applicazione in uso.  Questa attestazione viene popolata con un'intestazione HTTP che è attualmente impostato solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:  

-   Nel caso dei dispositivi che usano Exchange Active Sync, il valore è Microsoft.Exchange.ActiveSync.  

-   Utilizzo del client Microsoft Outlook può comportare uno qualsiasi dei valori seguenti:  

    -   Microsoft.Exchange.Autodiscover  

    -   Microsoft.Exchange.OfflineAddressBook  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

-   Altri valori possibili per questa intestazione includono quanto segue:  

    -   Microsoft.Exchange.Powershell  

    -   Microsoft.Exchange.SMTP  

    -   Microsoft.Exchange.Pop  

    -   Microsoft.Exchange.Imap  

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Tipo di attestazione: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Questa attestazione AD FS fornisce una stringa che rappresenti il tipo di dispositivo che il client usa per accedere al servizio. Può essere utilizzato quando i clienti desiderano impedire l'accesso per alcuni dispositivi (ad esempio particolari tipi di Smartphone).  I valori di esempio per questa attestazione includono (ma non sono limitati a) i valori riportati di seguito.  

 Di seguito sono riportati esempi di ciò che potrebbe contenere il valore di x-ms-user-agent per un client con cui x-ms-client-application "Microsoft.Exchange.ActiveSync"  

- Vortex/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0.3  

  È anche possibile che questo valore è vuoto.  

### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo di attestazione: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Questa attestazione AD FS indica che la richiesta è passata attraverso il proxy applicazione Web.  Questa attestazione viene popolata dal proxy applicazione Web, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per il back-end del servizio federativo. ADFS viene poi convertita in un'attestazione.  

 Il valore dell'attestazione è il nome DNS del proxy applicazione Web che trasmesso la richiesta.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo di attestazione: `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Tipo di attestazione è simile al precedente x-ms-proxy, questo tipo di attestazione indica se la richiesta è passata attraverso il proxy applicazione web. A differenza di x-ms-proxy, insidecorporatenetwork è un valore booleano con True per indicare una richiesta diretta al servizio federativo all'interno della rete azienda.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (attivo o passivo)  
 Tipo di attestazione: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Questo tipo di attestazione può essere utilizzato per determinare le richieste provenienti dai client (avanzate) "attivo" e "passivo" client (web basate su browser). In questo modo le richieste esterne da applicazioni basate su browser come Outlook Web Access, SharePoint Online o il portale di Office 365 deve essere autorizzato mentre vengono bloccate le richieste provenienti da rich client, ad esempio Microsoft Outlook.  

 Il valore dell'attestazione è il nome del servizio AD FS che ha ricevuto la richiesta.  

## <a name="see-also"></a>Vedere anche  
 [Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
