---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Criteri di controllo di accesso in ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Criteri di controllo di accesso in Windows Server 2012 R2 e Windows Server 2012 AD ADFS

>Si applica a: Windows Server 2012 R2 e Windows Server 2012 

Rendere i criteri descritti in questo articolo usa due tipi di attestazioni  
  
1.  Attestazioni che ADFS crea in base alle informazioni di ADFS e proxy applicazione Web può esaminare e verificare, ad esempio l'indirizzo IP del client che si connettono direttamente in ADFS o il WAP.  
  
2.  Attestazioni ADFS crea in base alle informazioni inviate a AD FS dal client come intestazioni HTTP  
  
>**Importante**: I criteri come descritto di seguito si blocca aggiunta al dominio di Windows 10 e accedere in scenari che richiedono l'accesso per gli altri endpoint seguenti

Endpoint ADFS di Active Directory necessari per Windows 10 aggiunta a un dominio e accesso in
- [nome servizio federativo] / adfs/servizi al trust/2005/windowstransport
- [nome servizio federativo] / adfs/servizi al trust/13/windowstransport
- [nome servizio federativo] / adfs/servizi al trust/2005/usernamemixed
- [nome servizio federativo] / adfs/servizi al trust/13/usernamemixed 
- [nome servizio federativo] / adfs/servizi al trust/2005/certificatemixed
- [nome servizio federativo] / adfs/servizi al trust/13/certificatemixed

Per risolvere, aggiornare tutti i criteri che negano in base alle attestazioni di endpoint per consentire l'eccezione per gli endpoint precedenti.

Ad esempio, la regola seguente:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

verranno aggiornati per:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Le attestazioni da questa categoria devono essere utilizzate solo per implementare i criteri aziendali e non come criteri di sicurezza per proteggere l'accesso alla rete.  È possibile che i client non autorizzati a inviare le intestazioni con informazioni false come un modo per ottenere l'accesso.  
  
I criteri descritti in questo articolo devono sempre essere utilizzati con un altro metodo di autenticazione, ad esempio l'autenticazione nome utente e la password o più fattori.  
  
## <a name="client-access-policies-scenarios"></a>Scenari di criteri di accesso client  
  
|**Scenario**|**Descrizione**| 
| --- | --- | 
|Scenario 1: Bloccare completamente l'accesso esterno a Office 365|È consentito l'accesso di Office 365 da tutti i client nella rete azienda interna, ma le richieste da client esterni vengono negate in base all'indirizzo IP del client esterni.|  
|Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync|È consentito l'accesso di Office 365 da tutti i client nella rete azienda interna, oltre che da eventuali dispositivi client esterni, ad esempio Smartphone, che utilizzano Exchange ActiveSync. Tutti gli altri client esterni, ad esempio quelli che utilizzano Outlook, vengono bloccati.|  
|Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser|Blocca l'accesso esterno a Office 365, fatta eccezione per le applicazioni passive (basata sul browser), ad esempio Outlook Web Access o SharePoint Online.|  
|Scenario 4: Bloccare completamente l'accesso esterno a Office 365, ad eccezione designati gruppi di Active Directory|Questo scenario viene utilizzato per il test e convalida della distribuzione di criteri di accesso client. Blocca l'accesso esterno a Office 365 solo per i membri del gruppo di Active Directory di uno o più. Può essere utilizzato anche per fornire l'accesso esterno solo ai membri di un gruppo.|  
  
## <a name="enabling-client-access-policy"></a>Abilita il criterio di accesso Client  
 Per abilitare il criterio di accesso client in ADFS in Windows Server 2012 R2, è necessario aggiornare la piattaforma delle identità Microsoft Office 365 trust della relying party. Scegliere uno degli scenari di esempio seguenti per configurare le regole di attestazione nel **piattaforma delle identità di Microsoft Office 365** trust della relying party che meglio soddisfi le esigenze dell'organizzazione.  
  
###  <a name="scenario1"></a>Scenario 1: Bloccare completamente l'accesso esterno a Office 365  
 Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e i blocchi di tutti i client esterni in base all'indirizzo IP del client esterni. È possibile utilizzare le procedure seguenti per aggiungere le regole di autorizzazione di rilascio corrette per il trust della relying party per lo scenario scelto di Office 365.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Per creare regole per bloccare completamente l'accesso esterno a Office 365  
  
1.  Da **Server Manager**, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS**.  
  
2.  Nell'albero della console, in **AD FS\Trust relazioni**, fare clic su **attendibilità**, fare doppio clic su di **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  
  
3.  Nel **Modifica regole attestazione** la finestra di dialogo, seleziona il **regole di autorizzazione rilascio** scheda e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione.  
  
4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "nel caso di eventuali reclami IP compreso nell'intervallo desiderato, Impedisci". In **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore di sopra per "x-ms-inoltrati-client-ip" con un'espressione IP valida):  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Fare clic su **fine **. Verificare che la nuova regola viene visualizzato nell'elenco di regole di autorizzazione rilascio prima sul valore predefinito **Consenti accesso a tutti gli utenti** regola (la regola di negazione avrà la precedenza anche se è visualizzata in precedenza nell'elenco).  Se non è il valore predefinito accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco utilizzando il linguaggio di regola attestazione, come indicato di seguito:  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Per salvare le nuove regole nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK**. L'elenco risultante dovrebbe essere simile al seguente.  
  
     ![Regole di autorizzazione di rilascio](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync  
 L'esempio seguente consente di accedere a tutte le applicazioni di Office 365, tra cui Exchange Online, provenienti dai client interni inclusi Outlook. Consente di bloccare l'accesso da client che si trovano all'esterno della rete azienda, come indicato dall'indirizzo IP del client, fatta eccezione per i client di Exchange ActiveSync, ad esempio Smartphone.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Per creare regole per bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync  
  
1.  Da **Server Manager**, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS**.  
  
2.  Nell'albero della console, in **AD FS\Trust relazioni**, fare clic su **attendibilità**, fare doppio clic su di **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  
  
3.  Nel **Modifica regole attestazione** la finestra di dialogo, seleziona il **regole di autorizzazione rilascio** scheda e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione.  
  
4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "nel caso di eventuali reclami IP compreso nell'intervallo desiderato, rilasciare ipoutsiderange attestazione". In **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore di sopra per "x-ms-inoltrati-client-ip" con un'espressione IP valida):  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
7.  Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
8.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se esiste un indirizzo IP compreso nell'intervallo desiderato e non esiste un'attestazione di applicazione x-ms-client, non EAS Impedisci". In **regola personalizzata**digitare o incollare la sintassi del linguaggio di regola attestazione seguenti:  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
11. Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
12. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
13. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "controllare se applicazione esiste". In **regola personalizzata**digitare o incollare la sintassi del linguaggio di regola attestazione seguenti:  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
15. Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
16. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
17. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "Impedisci agli utenti con ipoutsiderange true e applicazione esito negativo". In **regola personalizzata**digitare o incollare la sintassi del linguaggio di regola attestazione seguenti:  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Fare clic su **fine **. Verificare che la nuova regola viene immediatamente visualizzato sotto la regola precedente e il valore predefinito Consenti accesso a tutti gli utenti prima regola nell'elenco di regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se è visualizzata in precedenza nell'elenco).  </br>Se non è il valore predefinito accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco utilizzando il linguaggio di regola attestazione, come indicato di seguito:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Per salvare le nuove regole nel **Modifica regole attestazione** finestra di dialogo fare clic su OK. L'elenco risultante dovrebbe essere simile al seguente.  
  
     ![Regole di autorizzazione rilascio](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare regole per bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser  
  
1.  Da **Server Manager**, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS**.  
  
2.  Nell'albero della console, in **AD FS\Trust relazioni**, fare clic su **attendibilità**, fare doppio clic su di **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  
  
3.  Nel **Modifica regole attestazione** la finestra di dialogo, seleziona il **regole di autorizzazione rilascio** scheda e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione.  
  
4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "nel caso di eventuali reclami IP compreso nell'intervallo desiderato, rilasciare ipoutsiderange attestazione". In **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore di sopra per "x-ms-inoltrati-client-ip" con un'espressione IP valida):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
7.  Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
8.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "se c'è un indirizzo IP compreso nell'intervallo desiderato e l'endpoint non è/adfs/ls, Impedisci". In **regola personalizzata**digitare o incollare la sintassi del linguaggio di regola attestazione seguenti:  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. Fare clic su **fine **. Verificare che la nuova regola viene visualizzato nell'elenco di regole di autorizzazione rilascio prima sul valore predefinito **Consenti accesso a tutti gli utenti** regola (la regola di negazione avrà la precedenza anche se è visualizzata in precedenza nell'elenco).  </br></br> Se non è il valore predefinito accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco utilizzando il linguaggio di regola attestazione, come indicato di seguito:  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. Per salvare le nuove regole nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **OK**. L'elenco risultante dovrebbe essere simile al seguente.  
  
     ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>Scenario 4: Bloccare completamente l'accesso esterno a Office 365, ad eccezione designati gruppi di Active Directory  
 L'esempio seguente consente l'accesso dai client interni basate sull'indirizzo IP. Viene bloccato l'accesso da client che si trovano all'esterno della rete azienda con un indirizzo IP client esterni, fatta eccezione per gli utenti in un Group.Use Directory Active specificato i passaggi seguenti per aggiungere le regole di autorizzazione di rilascio corrette per il **piattaforma delle identità di Microsoft Office 365** trust della relying party utilizzando la creazione guidata regola di attestazione:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Per creare regole per bloccare completamente l'accesso esterno a Office 365, ad eccezione di designato gruppi di Active Directory  
  
1.  Da **Server Manager**, fare clic su **strumenti**, quindi fare clic su **gestione di ADFS**.  
  
2.  Nell'albero della console, in **AD FS\Trust relazioni**, fare clic su **attendibilità**, fare doppio clic su di **piattaforma delle identità di Microsoft Office 365** attendibile e quindi fare clic su **Modifica regole attestazione**.  
  
3.  Nel **Modifica regole attestazione** la finestra di dialogo, seleziona il **regole di autorizzazione rilascio** scheda e quindi fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione.  
  
4.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione**, selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
5.  Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, per esempio "nel caso di eventuali reclami IP compreso nell'intervallo desiderato, emettere attestazioni ipoutsiderange." In **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (sostituire il valore di sopra per "x-ms-inoltrati-client-ip" con un'espressione IP valida):  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
7.  Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
8.  Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
9. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "controllare SID di gruppo". In **regola personalizzata**digitare o incollare la seguente sintassi regola attestazione (Sostituisci "SID di gruppo" con il SID del gruppo di Active Directory in uso effettivo):  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Fare clic su **fine **. Verificare che la nuova regola nel **regole di autorizzazione rilascio** elenco.  
  
11. Successivamente, nel **Modifica regole attestazione** della finestra di dialogo di **regole di autorizzazione rilascio** scheda, fare clic su **Aggiungi regola** per avviare la creazione guidata regola di attestazione nuovamente.  
  
12. Nel **Seleziona modello di regola** nella pagina **modello di regola attestazione,** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
13. Nel **configurare la regola** nella pagina **nome regola attestazione**, digitare il nome visualizzato per questa regola, ad esempio "Impedisci esito negativo di utenti con ipoutsiderange true e SID di gruppo". In **regola personalizzata**digitare o incollare la sintassi del linguaggio di regola attestazione seguenti:  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Fare clic su **fine **. Verificare che la nuova regola viene immediatamente visualizzato sotto la regola precedente e il valore predefinito Consenti accesso a tutti gli utenti prima regola nell'elenco di regole di autorizzazione rilascio (la regola di negazione avrà la precedenza anche se è visualizzata in precedenza nell'elenco).  </br></br>Se non è il valore predefinito accesso regola consentire l'accesso, è possibile aggiungere uno alla fine dell'elenco utilizzando il linguaggio di regola attestazione, come indicato di seguito:  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Per salvare le nuove regole nel **Modifica regole attestazione** finestra di dialogo fare clic su OK. L'elenco risultante dovrebbe essere simile al seguente.  
  
     ![Rilascio](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>La compilazione dell'espressione intervallo di indirizzi IP  
 L'attestazione x-ms-inoltrati-client-ip viene compilato da un'intestazione HTTP attualmente impostata solo da Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. Il valore dell'attestazione può essere uno dei seguenti:  
  
> [!NOTE]
>  Exchange Online supporta attualmente solo gli indirizzi IPV4 e IPV6 non.  
  
-   Un singolo indirizzo IP: indirizzo IP del client connessi direttamente a Exchange Online  
  
> [!NOTE]
>  -   L'indirizzo IP di un client nella rete aziendale verrà visualizzato come l'indirizzo IP di interfaccia esterna del proxy in uscita dell'organizzazione o un gateway.  
> -   I client connessi alla rete aziendale tramite una VPN o DirectAccess da Microsoft (DA) potrebbero apparire come client aziendale interno o come i client esterni a seconda della configurazione di VPN o DA.  
  
-   Uno o più indirizzi IP: quando Exchange Online non può determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-inoltrati-per, un'intestazione non standard che può essere inclusi in richieste basate su HTTP e supportata da molti client, i servizi di bilanciamento del carico e proxy sul mercato.  
  
> [!NOTE]
>  1.  Più indirizzi IP, che indica l'indirizzo IP del client e l'indirizzo di ciascun proxy che ha trasmesso la richiesta, saranno separati da una virgola.  
> 2.  Gli indirizzi IP relativi all'infrastruttura Exchange Online verranno non presente nell'elenco.  
  
### <a name="regular-expressions"></a>Espressioni regolari  
 Quando si deve corrispondere a un intervallo di indirizzi IP, si rende necessario costruire un'espressione regolare per eseguire il confronto. Nella successiva serie di passaggi, verranno forniti esempi su come costruire un'espressione di questo tipo per soddisfare gli intervalli di indirizzi seguenti (si noti che è necessario modificare questi esempi per l'intervallo di IP pubblico corrisponde):  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 Prima di tutto, il modello di base corrispondente a un singolo indirizzo IP è il seguente: \b###\\.###\\.###\\.###\b  
  
 Questa estensione, abbiamo possiamo corrispondano come segue due indirizzi IP diversi con un'espressione o: \b###\\.###\\.###\\.###\b & #124;\b###\\.###\\.###\\.###\b  
  
 Un esempio per trovare la corrispondenza solo due indirizzi (ad esempio 192.168.1.1 o 10.0.0.1), potrebbe essere: \b192\\.168\\.1\\.1\b & #124;\b10\\.0\\.0\\.1\b  
  
 In questo modo la tecnica mediante il quale è possibile immettere qualsiasi numero di indirizzi. In cui devono essere consentite, ad esempio 192.168.1.1: 192.168.1.25, un intervallo di indirizzi corrispondente operazione deve essere eseguito carattere dal carattere: \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b  
  
 Tenere presente quanto segue:  
  
-   L'indirizzo IP viene considerato come stringa e non un numero.  
  
-   La regola è suddiviso come segue: \b192\\.168\\.1\\.  
  
-   Questa corrisponde a partire qualsiasi valore da 192.168.1.  
  
-   Gli intervalli necessari per la parte dell'indirizzo dopo il punto decimale finale corrisponde a quanto segue:  
  
    -   ([1-9] corrispondenze indirizzi che terminano con 1-9  
  
    -   & #124; 1 [0-9] corrisponde a indirizzi che terminano con 10-19  
  
    -   & indirizzi corrispondenze #124;2[0-5]) che termina con 20-25  
  
-   Si noti che le parentesi devono essere posizionate correttamente, in modo che non verrà avviato corrispondenza altre parti di indirizzi IP.  
  
-   Con il blocco di 192 corrispondente, è possibile scrivere un'espressione simile per il blocco di 10: \b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
-   E inserirli insieme, l'espressione seguente deve corrispondere tutti gli indirizzi per "192.168.1.1~25" e "10.0.0.1~14": \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b & #124;\b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>L'espressione di test  
 Espressioni regolari espressioni possono diventare molto complessa, quindi ti consigliamo vivamente di uno strumento di verifica delle espressioni regolari. Se si esegue una ricerca in internet per "generatore di espressioni regex online", sono disponibili diverse utilità online buona che consente di provare le espressioni in base a dati di esempio.  
  
 Quando l'espressione di test, è importante comprendere che cosa prevede che siano disponibili trovare la corrispondenza. Il sistema di Exchange può inviare molti gli indirizzi IP, separati da virgole. Le espressioni fornite in precedenza funzionerà per questo. Tuttavia, è importante affrontare questa durante il test delle espressioni di espressioni regolari. Ad esempio, una possibile utilizzare il seguente codice di esempio di input per verificare gli esempi precedenti:  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>Tipi di attestazione  
 ADFS in Windows Server 2012 R2 offre informazioni di contesto richiesta utilizzando i seguenti tipi di attestazione:  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-inoltrati-Client-IP  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 L'attestazione ADFS rappresenta un "migliore tentativo di" accertare l'indirizzo IP dell'utente (ad esempio, il client Outlook) effettua la richiesta. L'attestazione può contenere più indirizzi IP, inclusi l'indirizzo di ogni proxy inoltrato la richiesta.  L'attestazione viene compilata da HTTP. Il valore dell'attestazione può essere uno dei seguenti:  
  
-   Un singolo indirizzo IP - l'indirizzo IP del client connessi direttamente a Exchange Online  
  
> [!NOTE]
>  L'indirizzo IP di un client nella rete aziendale verrà visualizzato come l'indirizzo IP di interfaccia esterna del proxy in uscita dell'organizzazione o un gateway.  
  
-   Uno o più indirizzi IP  
  
    -   Se Exchange Online non può determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-inoltrati-per un'intestazione non standard che può essere inclusi in basate su HTTP richieste ed è supportata da molti client, i servizi di bilanciamento del carico e proxy sul mercato.  
  
    -   Più indirizzi IP che indica l'indirizzo IP del client e l'indirizzo di ciascun proxy che ha trasmesso la richiesta saranno separati da una virgola.  
  
> [!NOTE]
>  Gli indirizzi IP relativi all'infrastruttura Exchange Online non sarà presenti nell'elenco.  
  
> [!WARNING]
>  Exchange Online attualmente supporta solo indirizzi IPV4. non supporta gli indirizzi IPV6.  
  
### <a name="x-ms-client-application"></a>Applicazione X-MS-Client  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 L'attestazione ADFS rappresenta il protocollo utilizzato dal client finali, che corrisponde a regime di controllo per l'applicazione in uso.  L'attestazione viene popolata con un'intestazione HTTP attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:  
  
-   Per i dispositivi che usano Exchange Active Sync, il valore è Microsoft.Exchange.ActiveSync.  
  
-   L'utilizzo del client di Microsoft Outlook può generare in uno dei valori seguenti:  
  
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
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 L'attestazione AD FS fornisce una stringa per rappresentare il tipo di dispositivo che utilizza il client di accedere al servizio. Può essere utilizzato quando si desiderano impedire l'accesso per alcuni dispositivi (ad esempio determinati tipi di Smartphone) ai clienti.  I valori di esempio per l'attestazione includono (ma non sono limitati a) valori riportati di seguito.  
  
 Di seguito sono riportati esempi di cosa potrebbe contenere il valore x-ms-user-agent per un client con cui x-ms-client-application "Microsoft.Exchange.ActiveSync"  
  
-   Vortex/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0,3  
  
 È anche possibile che questo valore è vuoto.  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 L'attestazione ADFS indica che la richiesta è passata tramite il proxy dell'applicazione Web.  L'attestazione viene popolata con il proxy applicazione Web, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per il back-end del servizio federativo. AD FS quindi lo converte in un'attestazione.  
  
 Il valore dell'attestazione è il nome DNS del proxy applicazione Web che ha trasmesso la richiesta.  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo di attestazione: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 Tipo di attestazione è simile al precedente x-ms-proxy, questo tipo di attestazione indica se la richiesta è passata mediante il proxy dell'applicazione web. A differenza di x-ms-proxy, insidecorporatenetwork è un valore booleano con True che indica una richiesta direttamente al servizio federativo all'interno della rete azienda.  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-percorso assoluto (vs Active passivo)  
 Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 Questo tipo di attestazione può essere utilizzato per determinare le richieste provenienti dai client (RTF) "active" e "passivo" client (browser basata su web). In questo modo richieste esterne dalle applicazioni basate su browser quali Outlook Web Access, SharePoint Online o il portale di Office 365 affinché sia consentita durante le richieste provenienti da rich client, ad esempio Microsoft Outlook sono bloccate.  
  
 Il valore dell'attestazione è il nome del servizio ADFS che ha ricevuto la richiesta.  
  
## <a name="see-also"></a>Vedere anche  
 [Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)
