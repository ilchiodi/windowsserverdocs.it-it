---
title: Come configurare gli account protetti
ms.prod: windows-server
ms.technology: security-auditing
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2467e28571ba6c782861d93497fe54badacec48d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861824"
---
# <a name="how-to-configure-protected-accounts"></a>Come configurare gli account protetti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Gli attacchi Pass-the-hash (PtH) consentono agli autori di attacchi di eseguire l'autenticazione a un server o un servizio remoto usando l'hash NTLM sottostante della password di un utente (o altri derivati delle credenziali). Microsoft ha in precedenza [pubblicato indicazioni](https://www.microsoft.com/download/details.aspx?id=36036) per ridurre gli attacchi pass-the-hash.  Windows Server 2012 R2 include nuove funzionalità per limitare ulteriormente tali attacchi. Per ulteriori informazioni sulle altre funzionalità di sicurezza per la protezione contro il furto di credenziali, vedere [Gestione e protezione delle credenziali](https://technet.microsoft.com/library/dn408190.aspx). In questo argomento viene illustrato come configurare le nuove funzionalità seguenti:  
  
-   [Utenti protetti](#protected-users)  
  
-   [Criteri di autenticazione](#authentication-policies)  
  
-   [Silo di criteri di autenticazione](#authentication-policy-silos)  
  
In Windows 8.1 e Windows Server 2012 R2 sono inoltre incorporate le funzionalità per la prevenzione dei furti di credenziali descritte negli argomenti seguenti:  
  
-   [Modalità di amministrazione limitata per Desktop remoto](https://blogs.technet.com/b/kfalde/archive/20../restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)  
  
-   [Protezione LSA](https://technet.microsoft.com/library/dn408187)  
  
## <a name="protected-users"></a>Utenti protetti  
Utenti protetti è un nuovo gruppo di sicurezza globale a cui è possibile aggiungere utenti nuovi o esistenti. I dispositivi Windows 8.1 e Windows Server 2012 R2 host hanno un comportamento speciale con i membri di questo gruppo per fornire una migliore protezione contro il furto di credenziali. Per un membro del gruppo, un host Windows Server 2012 R2 o un dispositivo Windows 8.1 non memorizzare nella cache le credenziali che non sono supportate per utenti protetti. I membri di questo gruppo non dispongono di alcuna protezione aggiuntiva se sono connessi a un dispositivo che esegue una versione di Windows precedenti a Windows 8.1.  
  
Gruppo di utenti protetti che sono firmati-on per i dispositivi Windows 8.1 e gli host Windows Server 2012 R2 possono *non è più* utilizzare:  
  
-   Delega delle credenziali predefinita (CredSSP): le credenziali in testo normale non vengono memorizzate nella cache, anche se è abilitato il criterio **Consenti delega credenziali predefinite**  
  
-   Digest di Windows: le credenziali in testo normale non vengono memorizzate nella cache, anche se sono abilitate  
  
-   Autenticazione NTLM: la funzione NTOWF non viene memorizzata nella cache  
  
-   Chiavi a lungo termine Kerberos: il ticket di concessione ticket (TGT) Kerberos viene acquisito all'accesso e non può essere riacquisito automaticamente  
  
-   Accesso offline: il verificatore dell'accesso memorizzato nella cache non viene creato  
  
Se il livello funzionale del dominio è Windows Server 2012 R2, i membri del gruppo è più possono:  
  
-   Usare l'autenticazione NTLM.  
  
-   Usare pacchetti di crittografia DES (Data Encryption Standard) o RC4 nella preautenticazione Kerberos.  
  
-   Essere delegati tramite delega vincolata o non vincolata.  
  
-   Rinnovare i ticket utente (TGT) di Kerberos oltre la durata iniziale di quattro ore.  
  
Per aggiungere utenti al gruppo, è possibile usare [strumenti dell'interfaccia utente](https://technet.microsoft.com/library/cc753515.aspx) come centro di amministrazione di Active Directory (ADAC) o Active Directory utenti e computer oppure uno strumento da riga di comando come il [gruppo dsmod](https://technet.microsoft.com/library/cc732423.aspx)o il cmdlet [Add-ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) di Windows PowerShell. Gli account per i servizi e i computer *non devono* essere membri del gruppo Utenti protetti. L'appartenenza per questi account non fornisce protezioni locali perché la password o il certificato è sempre disponibile nell'host.  
  
> [!WARNING]  
> Non sono disponibili soluzioni alternative per le restrizioni dell'autenticazione, di conseguenza i membri di gruppi con privilegi elevati, ad esempio il gruppo Admins o il gruppo Domain Admins sono soggetti alle stesse restrizioni degli altri membri del gruppo Utenti protetti. Se tutti i membri di tali gruppi vengono aggiunti al gruppo utenti protetti, è possibile che tutti gli account vengano bloccati. Non è mai necessario aggiungere tutti gli account con privilegi elevati al gruppo utenti protetti fino a quando non si ha testato accuratamente il potenziale impatto.  
  
I membri del gruppo Utenti protetti devono essere in grado di eseguire l'autenticazione tramite Kerberos con crittografia AES (Advanced Encryption Standards). Questo metodo richiede le chiavi AES per l'account in Active Directory. L'amministratore predefinito non dispone di una chiave AES a meno che la password è stata modificata in un controller di dominio che esegue Windows Server 2008 o versione successiva. Inoltre, qualsiasi account, che dispone di una password che è stata modificata in un controller di dominio che esegue una versione precedente di Windows Server, è bloccato. Attenersi quindi alle procedure consigliate seguenti:  
  
-   Non eseguire test in domini a meno che non **tutti i controller di dominio eseguono Windows Server 2008 o versione successiva**.  
  
-   **Modificare la password** per tutti gli account di dominio creati *prima* della creazione del dominio stesso. In caso contrario, non sarà possibile autenticare questi account.  
  
-   **Modifica della password** per ogni utente prima di aggiungere l'account per utenti protetti di gruppo o verificare che la password sia stata modificata di recente in un controller di dominio che esegue Windows Server 2008 o versione successiva.  
  
### <a name="requirements-for-using-protected-accounts"></a>Requisiti per l'uso degli account protetti  
Per gli account protetti sono previsti i requisiti di distribuzione seguenti:  
  
-   Per specificare restrizioni sul lato client per utenti protetti, gli host devono eseguire Windows 8.1 o Windows Server 2012 R2. Un utente deve semplicemente eseguire l'accesso con un account membro di un gruppo Utenti protetti. In questo caso, il gruppo utenti protetti può essere creato da [trasferire il ruolo di emulatore dominio primario (PDC) controller](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controller di dominio che esegue Windows Server 2012 R2. Dopo avere replicato l'oggetto gruppo in altri controller di dominio, il ruolo dell'emulatore PDC può essere ospitato in un controller di dominio che esegue una versione precedente di Windows Server.  
  
-   Per specificare restrizioni sul lato controller di dominio per utenti protetti, che consiste nel limitare l'utilizzo dell'autenticazione NTLM, e altre restrizioni, il livello funzionale del dominio deve essere Windows Server 2012 R2. Per ulteriori informazioni sui livelli di funzionalità, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../../identity/ad-ds/active-directory-functional-levels.md).  
  
### <a name="troubleshoot-events-related-to-protected-users"></a>Risolvere i problemi relativi agli eventi correlati a Utenti protetti  
In questa sezione vengono illustrati i nuovi registri che consentono di risolvere i problemi relativi agli eventi correlati a Utenti protetti e viene descritto il modo in cui il gruppo Utenti protetti può influire sulle modifiche per la risoluzione dei problemi relativi alla delega o alla scadenza dei ticket di concessione ticket (TGT).  
  
#### <a name="new-logs-for-protected-users"></a>Nuovi registri per Utenti protetti  
Due nuovi registri amministrativi sono disponibili per la risoluzione di eventi relativi a utenti protetti: utenti protetti - Client Log e Protected User Failures - Domain Controller Log. Questi due nuovi registri sono disponibili nel Visualizzatore eventi e sono disabilitati per impostazione predefinita. Per abilitare un registro, fare clic su **Registri applicazioni e servizi**, **Microsoft**, **Windows**, **Autenticazione**, sul nome del registro, quindi su **Azione** (o fare clic con il pulsante destro del mouse sul log) e infine scegliere **Attiva registro**.  
  
Per ulteriori informazioni sugli eventi in questi registri, vedere [criteri di autenticazione e silo di criteri di autenticazione](https://technet.microsoft.com/library/dn486813.aspx).  
  
#### <a name="troubleshoot-tgt-expiration"></a>Risolvere i problemi relativi alla scadenza del ticket di concessione ticket (TGT)  
In genere, il controller di dominio imposta la durata e il rinnovo del ticket di concessione ticket (TGT) in base ai criteri di dominio come illustrato nella finestra dell'Editor Gestione Criteri di gruppo seguente.  
  
![Screenshot della finestra di Editor Gestione Criteri di gruppo che Mostra come il controller di dominio imposta la durata e il rinnovo del TGT in base ai criteri di dominio](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
Per **Utenti protetti**, le impostazioni seguenti sono hardcoded:  
  
-   Durata massima ticket utente: 240 minuti  
  
-   Durata massima rinnovo ticket utente: 240 minuti  
  
#### <a name="troubleshoot-delegation-issues"></a>Risolvere i problemi relativi alla delega  
In generale, se si verifica un errore in una tecnologia che usa la delega Kerberos, l'account del client viene controllato per verificare se è impostata l'opzione **L'account è sensibile e non può essere delegato**. Se tuttavia l'account è un membro del gruppo **Utenti protetti**, questa opzione potrebbe non essere configurata nel Centro di amministrazione di Active Directory. Di conseguenza, per la risoluzione dei problemi relativi alla delega, verificare l'impostazione e l'appartenenza al gruppo.  
  
![Screenshot che mostra dove controllare * * l'account è sensibile e non può essere delegato * * elemento dell'interfaccia utente](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
### <a name="audit-authentication-attempts"></a>Controllare i tentativi di autenticazione  
Per controllare in modo esplicito i tentativi di autenticazione per i membri del gruppo **Utenti protetti**, è possibile continuare a raccogliere eventi di controllo del registro di protezione o a raccogliere i dati nei nuovi registri amministrativi. Per ulteriori informazioni su questi eventi, vedere [criteri di autenticazione e silo di criteri di autenticazione](https://technet.microsoft.com/library/dn486813.aspx).  
  
### <a name="provide-dc-side-protections-for-services-and-computers"></a>Fornire protezioni sul lato controller di dominio per servizi e computer  
Gli account per i servizi e i computer non possono essere membri del gruppo **Utenti protetti**. In questa sezione vengono illustrate le protezioni sul lato controller di dominio disponibili per questi account:  
  
-   Rifiutare l'autenticazione NTLM: configurabile solo tramite i [criteri di blocco NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx).  
  
-   Rifiutare Data Encryption Standard (DES) nella preautenticazione Kerberos: il controller di dominio di Windows Server 2012 R2 non accettano la crittografia DES per gli account computer a meno che non sono configurati per DES solo perché ogni versione di Windows rilasciata con Kerberos supporta anche RC4.  
  
-   Rifiutare la crittografia RC4 nella preautenticazione Kerberos: non configurabile.  
  
    > [!NOTE]  
    > Sebbene sia possibile [modificare la configurazione dei tipi di crittografia supportati](https://blogs.msdn.com/b/openspecification/archive/20../windows-configurations-for-kerberos-supported-encryption-type.aspx), non è consigliabile modificare tali impostazioni per gli account computer senza eseguire il test nell'ambiente di destinazione.  
  
-   Limitare i ticket utente (TGT) per una durata iniziale di 4 ore: usare i criteri di autenticazione.  
  
-   Negare la delega vincolata o non vincolata: per limitare un account, aprire Active Directory amministrativi CENTRO e selezionare il **Account è sensibile e non può essere delegato** casella di controllo.  
  
    ![Screenshot che mostra dove limitare un account](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
## <a name="authentication-policies"></a>Criteri di autenticazione  
Criteri di autenticazione è un nuovo contenitore in Servizi di dominio Active Directory che contiene gli oggetti Criteri di autenticazione. I criteri di autenticazione consentono di specificare impostazioni che permettono di attenuare l'esposizione al furto di credenziali, ad esempio la limitazione della durata del ticket di concessione ticket (TGT) per gli account o l'aggiunta di altre condizioni basate su attestazioni.  
  
In Windows Server 2012, controllo dinamico degli accessi ha introdotto una classe di oggetto di ambito della foresta di Active Directory denominata criteri di accesso centrale per fornire un modo semplice per configurare file server all'interno dell'organizzazione. In Windows Server 2012 R2, una nuova classe di oggetti denominata criteri di autenticazione (objectClass msDS-AuthNPolicies) può essere utilizzata per applicare la configurazione dell'autenticazione alle classi di account in domini di Windows Server 2012 R2. Di seguito sono riportate le classi di account Active Directory:  
  
-   Utente  
  
-   Computer  
  
-   Account del servizio gestito e Account del servizio gestito di gruppo  
  
### <a name="quick-kerberos-refresher"></a>Aggiornamento rapido Kerberos  
Il protocollo di autenticazione Kerberos è costituito da tre tipi di scambi, noti anche come protocolli secondari:  
  
![Screenshot che illustra i tre tipi di scambi del protocollo di autenticazione Kerberos, noti anche come sottoprotocolli](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbRefresher.gif)  
  
-   Scambio del Servizio di autenticazione (AS) (KRB_AS_*)  
  
-   Scambio del Servizio di concessione ticket (TGS) (KRB_TGS_*)  
  
-   Scambio client/server (AP) (KRB_AP_*)  
  
Lo scambio AS è in cui il client utilizza la password dell'account o la chiave privata per creare un preautenticatore e richiedere un ticket di concessione ticket (TGT). Questo avviene all'accesso dell'utente o la prima volta che è necessario un ticket di servizio.  
  
Lo scambio TGS è dove TGT dell'account viene utilizzato per creare un autenticatore e richiedere un ticket di servizio. Questo avviene quando è necessaria una connessione autenticata.  
  
Lo scambio client/server avviene normalmente nei dati all'interno del protocollo dell'applicazione e non è interessato dai criteri di autenticazione.  
  
Per ulteriori informazioni, vedere [Kerberos versione 5 autenticazione protocollo funzionamento del](https://technet.microsoft.com/library/cc772815(v=WS.10.aspx)).  
  
### <a name="overview"></a>Panoramica  
I criteri di autenticazione integrano il gruppo Utenti protetti, offrendo un modo per applicare restrizioni configurabili agli account, oltre a offrire restrizioni per gli account per servizi e computer. I criteri di autenticazione sono applicati durante lo scambio Kerberos AS o TGS.  
  
È possibile limitare l'autenticazione iniziale o lo scambio AS configurando gli elementi seguenti:  
  
-   Durata TGT  
  
-   Condizioni del controllo di accesso per limitare l'accesso utente, che devono essere soddisfatte dai dispositivi dai quali proviene lo scambio AS  
  
![Screenshot che illustra come limitare l'autenticazione iniziale configurando una durata TGT e condizioni di controllo di accesso per limitare l'accesso utente](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictAS.gif)  
  
È possibile limitare le richieste di ticket di servizio tramite uno scambio del servizio di concessione ticket (TGS) configurando gli elementi seguenti:  
  
-   Condizioni del controllo di accesso che devono essere soddisfatte dal client (utente, servizio, computer) o dispositivo da cui proviene lo scambio del servizio di concessione ticket (TGS)  
  
### <a name="requirements-for-using-authentication-policies"></a>Requisiti per l'uso dei criteri di autenticazione  
  
|Condizione|Requisiti|  
|-----|--------|  
|Specifica durate TGT personalizzate| Domini di account con livello funzionale di dominio Windows Server 2012 R2|  
|Limita l'accesso utente|-Domini di account con livello funzionale di dominio Windows Server 2012 R2 con il supporto di controllo dinamico degli accessi<br />-Dispositivi Windows 8, Windows 8.1, Windows Server 2012 o Windows Server 2012 R2 con supporto per il controllo dinamico degli accessi|  
|Limita il rilascio di ticket di servizio basati su account utente e gruppi di sicurezza| Domini di risorse a livello funzionale di dominio Windows Server 2012 R2|  
|Limita il rilascio di ticket di servizio basati su attestazioni utente o account del dispositivo, gruppi di sicurezza o attestazioni| Domini di risorse a livello funzionale di dominio di Windows Server 2012 R2 con il supporto di controllo dinamico degli accessi|  
  
### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Limitare un account utente a dispositivi e host specifici  
È opportuno che un account a valore elevato con privilegi amministrativi sia membro del gruppo **Utenti protetti**. Per impostazione predefinita, nessun account è membro del gruppo **Utenti protetti**. Prima di aggiungere account al gruppo, configurare il supporto per il controller di dominio e creare un criterio di controllo per verificare che non vi siano problemi di blocco.  
  
#### <a name="configure-domain-controller-support"></a>Configurare il supporto per il controller di dominio  
Il dominio dell'account utente deve essere a livello funzionale di dominio Windows Server 2012 R2 (Livello). Verificare che tutti i controller di dominio siano Windows Server 2012 R2 e quindi utilizzano Active Directory Domains and Trusts per [aumentare il livello di funzionalità DEL](https://technet.microsoft.com/library/cc753104.aspx) a Windows Server 2012 R2.  
  
**Per configurare il supporto per il controllo dinamico degli accessi**  
  
1.  Nel criterio Controller di dominio predefiniti fare clic su **Abilitato** per abilitare l'impostazione **Supporto client Centro distribuzione chiavi Kerberos per attestazioni, autenticazione composta e blindatura Kerberos** in Configurazione computer | Modelli amministrativi | Sistema | KDC.  
  
    ![Nel criterio controller di dominio predefiniti, fare clic su * * abilitato * * per abilitare il supporto client * * Centro distribuzione chiavi (KDC) per attestazioni, autenticazione composta e blindatura Kerberos * * in configurazione computer | Modelli amministrativi | Sistema | KDC](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)  
  
2.  Nell'elenco a discesa **Opzioni** selezionare **Fornisci sempre attestazioni**.  
  
    > [!NOTE]  
    > **Supportato** può anche essere configurato, ma poiché il dominio è Windows Server 2012 R2 DFL, con i controller di dominio sempre fornire attestazioni consentirà l'accesso basato sulle attestazioni utente si verificano quando si utilizzano dispositivi grado di riconoscere attestazioni non controlla e dagli host per connettersi ai servizi in grado di riconoscere attestazioni.  
  
    ![In * * opzioni * *, nella casella di riepilogo a discesa, selezionare * * Fornisci sempre attestazioni](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)  
  
    > [!WARNING]  
    > Configurazione **Rifiuta richieste di autenticazione non blindate** genererà errori di autenticazione da qualsiasi sistema operativo che non supporta la blindatura Kerberos, ad esempio Windows 7 e sistemi operativi precedenti, o sistemi operativi a partire da Windows 8, che non sono stati configurati in modo esplicito per supportarla.  
  
#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Creare un controllo dell'account utente per il criterio di autenticazione tramite il Centro di amministrazione di Active Directory  
  
1.  Aprire il Centro di amministrazione di Active Directory.  
  
    ![Screenshot che mostra Centro di amministrazione di Active Directory](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_OpenADAC.gif)  
  
    > [!NOTE]  
    > Selezionato **autenticazione** nodo è visibile per i domini sono funzionalità del Dominio di Windows Server 2012 R2. Se non viene visualizzato il nodo, quindi riprovare utilizzando un account di amministratore di dominio da un dominio al Dominio di Windows Server 2012 R2.  
  
2.  Fare clic su **Criteri di autenticazione** e quindi su **Nuovo** per creare un nuovo criterio.  
  
    ![Criteri di autenticazione](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)  
  
    I criteri di autenticazione devono avere un nome visualizzato e vengono imposti per impostazione predefinita.  
  
3.  Per creare un criterio di solo controllo, fare clic su **Solo restrizioni dei criteri di controllo**.  
  
    ![Solo restrizioni dei criteri di controllo](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)  
  
    I criteri di autenticazione vengono applicati in base al tipo di account di Active Directory. Per applicare un singolo criterio a tutti e tre i tipi di account, configurare le impostazioni per ogni tipo. I tipi di account sono:  
  
    -   Utente  
  
    -   Computer  
  
    -   Account del servizio gestito e Account del servizio gestito di gruppo  
  
    Se lo schema è stato esteso con nuove entità che possono essere usate dal Centro distribuzione chiavi (KDC), il nuovo tipo di account sarà classificato come il tipo di account derivato più vicino.  
  
4.  Per configurare una durata del ticket di concessione ticket (TGT), selezionare la casella di controllo **Specificare una durata Ticket Granting Ticket per gli account utente** e specificare il tempo in minuti.  
  
    ![Specificare la durata del ticket di concessione ticket per gli account utente](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTLifetime.gif)  
  
    Se ad esempio si vuole impostare una durata TGT massima di 10 ore, specificare **600**, come indicato. Se non è configurata una durata TGT, se l'account è membro del gruppo **Protected Users**, la durata e il rinnovo del ticket di concessione ticket (TGT) è di 4 ore. In caso contrario, la durata e il rinnovo del ticket di concessione ticket (TGT) sono impostati in base ai criteri di dominio, come illustrato nella finestra dell'Editor Gestione Criteri di gruppo seguente per un dominio con impostazioni predefinite.  
  
    ![Finestra Editor Gestione Criteri di gruppo per un dominio con le impostazioni predefinite](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
5.  Per limitare l'account utente a determinati dispositivi, fare clic su **Modifica** per definire le condizioni necessarie per il dispositivo.  
  
    ![Per limitare l'account utente alla selezione dei dispositivi, fare clic su * * modifica * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)  
  
6.  Nella finestra **Modifica condizioni di controllo di accesso** fare clic su **Aggiungi condizione**.  
  
    ![Modificare le condizioni del controllo di accesso](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCondition.png)  
  
##### <a name="add-computer-account-or-group-conditions"></a>Aggiungere condizioni per account computer o gruppi  
  
1.  Per configurare account computer o gruppi, nell'elenco a discesa selezionare la voce **Membro di ogni** e sostituirla con **Membro di qualsiasi**.  
  
    ![Configurare gli account computer o i gruppi](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompMember.png)  
  
    > [!NOTE]  
    > Questo controllo di accesso definisce le condizioni del dispositivo o dell'host dal quale l'utente esegue l'accesso. Nella terminologia relativa al controllo di accesso, l'account computer per il dispositivo o l'host equivale all'utente, ecco perché **Utente** è l'unica opzione disponibile.  
  
2.  Fare clic su **Aggiungi elementi**.  
  
    ![Aggiungi elementi](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddItems.png)  
  
3.  Per modificare i tipi di oggetto, fare clic su **Tipi di oggetto**.  
  
    ![Tipi di oggetto](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjects.gif)  
  
4.  Per selezionare gli oggetti computer in Active Directory, fare clic su **Computer** e quindi su **OK**.  
  
    ![Computer](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)  
  
5.  Digitare i nomi dei computer per i quali si vuole limitare l'utente e quindi fare clic su **Controlla nomi**.  
  
    ![Fare clic su Controlla nomi](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)  
  
6.  Fare clic su OK ed eventualmente creare altre condizioni per l'account computer.  
  
    ![Fare clic su OK e creare altre condizioni per l'account computer](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddConditions.png)  
  
7.  Al termine, fare clic su **OK** per visualizzare le condizioni definite per l'account computer.  
  
    ![Al termine, fare clic su * * OK * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompDone.png)  
  
##### <a name="add-computer-claim-conditions"></a>Aggiungere condizioni per le attestazioni computer  
  
1.  Per configurare le attestazioni computer, fare clic sull'elenco a discesa Gruppo per selezionare l'attestazione.  
  
    ![Per configurare attestazioni computer, gruppo a discesa per selezionare l'attestazione](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaim.gif)  
  
    Le attestazioni sono disponibili solo se ne è già stato effettuato il provisioning nella foresta.  
  
2.  Digitare il nome di unità organizzativa. L'account utente dovrebbe avere l'accesso limitato.  
  
    ![Digitare il nome di unità organizzativa. L'account utente dovrebbe avere l'accesso limitato.](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimOUName.gif)  
  
3.  Al termine, fare clic su OK per visualizzare le condizioni definite nella casella.  
  
    ![Al termine, fare clic su OK](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimComplete.gif)  
  
##### <a name="troubleshoot-missing-computer-claims"></a>Risolvere i problemi relativi ad attestazioni computer mancanti  
Se è stato eseguito il provisioning dell'attestazione ma questa non è disponibile, è possibile che sia configurata solo per le classi **Computer**.  
  
Si supponga che si desidera limitare l'autenticazione basata sull'unità organizzativa (OU) del computer, che era già configurato, ma solo per **Computer** classi.  
  
![Screenshot che illustra come limitare l'autenticazione basata sull'unità organizzativa (OU) del computer](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictComputers.gif)  
  
Affinché l'attestazione sia disponibile per la limitazione dell'accesso utente al dispositivo, selezionare la casella di controllo **Utente**.  
  
![Screenshot che illustra come limitare l'accesso utente al dispositivo selezionando la casella di controllo Seleziona * * utente * *.  ](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)  
  
#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Eseguire il provisioning di un account utente con un criterio di autenticazione tramite il Centro di amministrazione di Active Directory  
  
1.  Nell'account **Utente** fare clic su **Criterio**.  
  
    ![Dall'account * * User * * fare clic su * * policy * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicy.gif)  
  
2.  Selezionare la casella di controllo **Assegnare un criterio di autenticazione a questo account**.  
  
    ![Selezionare la casella di controllo * * assegna un criterio di autenticazione a questo account * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)  
  
3.  Selezionare quindi il criterio di autenticazione da applicare all'utente.  
  
    ![Selezionare i criteri di autenticazione da applicare all'utente](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicySelect.png)  
  
#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurare il supporto per il controllo dinamico degli accessi su dispositivi e host  
È possibile configurare durate del ticket di concessione ticket (TGT) senza configurare il controllo dinamico degli accessi. Il controllo dinamico degli accessi è necessario solo per il controllo di llowedToAuthenticateFrom e AllowedToAuthenticateTo.  
  
In Criteri di gruppo o nell'Editor Criteri di gruppo locale abilitare l'impostazione **Supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos** in Configurazione computer | Modelli amministrativi | Sistema | Kerberos.  
  
![Screenshot che illustra come usare Criteri di gruppo o Editor Criteri di gruppo locali per abilitare * * supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)  
  
### <a name="troubleshoot-authentication-policies"></a>Risolvere i problemi relativi ai criteri di autenticazione  
  
#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Individuare gli account a cui è assegnato direttamente un criterio di autenticazione  
Nella sezione Account in Criterio di autenticazione sono visualizzati gli account è cui è stato direttamente assegnato il criterio.  
  
![Screenshot della sezione account in criteri di autenticazione che Mostra gli account che hanno applicato direttamente il criterio](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AccountsAssigned.gif)  
  
#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Utilizzare gli errori dei criteri di autenticazione - registro amministrativo di Controller di dominio  
Un nuovo **Authentication Policy Failures - Controller di dominio** registro amministrativo in **registri applicazioni e servizi** > **Microsoft** > **Windows** > **autenticazione** è stata creata per semplificare l'individuazione degli errori a causa dei criteri di autenticazione. Questo registro è disabilitato per impostazione predefinita. Per abilitarlo, fare clic con il pulsante destro del mouse sul nome del registro e scegliere **Attiva registro**. I nuovi eventi presentano un contenuto molto simile a quello degli eventi di controllo del ticket di servizio e del ticket di concessione ticket (TGT) di Kerberos esistenti. Per ulteriori informazioni su questi eventi, vedere [criteri di autenticazione e silo di criteri di autenticazione](https://technet.microsoft.com/library/dn486813.aspx).  
  
### <a name="manage-authentication-policies-by-using-windows-powershell"></a>Gestire i criteri di autenticazione con Windows PowerShell  
Questo comando crea un criterio di autenticazione denominato **TestAuthenticationPolicy**. Il parametro **UserAllowedToAuthenticateFrom** specifica i dispositivi dai quali gli utenti possono effettuare l'autenticazione tramite una stringa SDDL nel file denominato someFile.txt.  
  
```  
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl  
```  
  
Questo comando recupera tutti i criteri di autenticazione corrispondenti al filtro specificato dal parametro **Filter**.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com  
  
```  
  
Questo comando modifica la descrizione e le proprietà **UserTGTLifetimeMins** del criterio di autenticazione specificato.  
  
```  
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45  
```  
  
Questo comando rimuove il criterio di autenticazione specificato dal parametro **Identity**.  
  
```  
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1  
```  
  
Questo comando usa il cmdlet **Get-ADAuthenticationPolicy** con il parametro **Filter** per recuperare tutti i criteri di autenticazione non imposti. Il set di risultati viene reindirizzato al cmdlet **Remove-ADAuthenticationPolicy**.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy  
```  
  
## <a name="authentication-policy-silos"></a>Silo di criteri di autenticazione  
Silo di criteri di autenticazione è un nuovo contenitore (objectClass msDS-AuthNPolicySilos) in Servizi di dominio Active Directory per gli account utente, computer e del servizio. I silo consentono di proteggere gli account a valore elevato. Tutte le organizzazioni proteggono i membri dei gruppi Enterprise Admins, Domain Admins e Schema Admins in quanto questi account potrebbero essere usati dagli autori di attacchi per accedere a tutti gli elementi nella foresta, ma anche altri account potrebbero richiedere una protezione.  
  
Alcune organizzazioni isolano i carichi di lavoro creando account univoci dedicati e applicando le impostazioni di Criteri di gruppo per liminare l'accesso interattivo locale e remoto e i privilegi amministrativi. In questo scenario, i silo di criteri di autenticazione consentono di definire una relazione tra gli account utente, computer e del servizio gestito. Gli account possono appartenere a un solo silo. È possibile configurare criteri di autenticazione per ogni tipo di account per controllare:  
  
1.  Durata TGT non rinnovabile  
  
2.  Condizioni di controllo di accesso per la restituzione del ticket di concessione ticket (TGT). Nota: non si applica ai sistemi perché è richiesta la blindatura Kerberos.  
  
3.  Condizioni di controllo di accesso per la restituzione del ticket di servizio  
  
Inoltre, gli account in un silo di criteri di autenticazione hanno un'attestazione silo che può essere usata da risorse in grado di riconoscere le attestazioni, quali i file server, per controllare gli accessi.  
  
È possibile configurare un nuovo descrittore di sicurezza per controllare il ticket di servizio emittente in base a:  
  
-   Utente, gruppi di sicurezza dell'utente e/o le attestazioni utente  
  
-   Dispositivo, il gruppo di sicurezza del dispositivo e/o attestazioni del dispositivo  
  
Queste informazioni ai controller di dominio della risorsa richiede controllo dinamico degli accessi:  
  
-   Attestazioni utente:  
  
    -   Client Windows 8 e versioni successive con il supporto di Controllo dinamico degli accessi  
  
    -   Il dominio dell'account supporta Controllo dinamico degli accessi e attestazioni  
  
-   Dispositivo e/o gruppo di sicurezza del dispositivo  
  
    -   Client Windows 8 e versioni successive con il supporto di Controllo dinamico degli accessi  
  
    -   Risorsa configurata per l'autenticazione composta  
  
-   Attestazioni dispositivo:  
  
    -   Client Windows 8 e versioni successive con il supporto di Controllo dinamico degli accessi  
  
    -   Il dominio del dispositivo supporta Controllo dinamico degli accessi e attestazioni  
  
    -   Risorsa configurata per l'autenticazione composta  
  
È possibile applicare i criteri di autenticazione a tutti i membri di un silo di criteri di autenticazione anziché ad account singoli o applicare criteri di autenticazione separati a diversi tipi di account all'interno di un silo. Ad esempio, è possibile applicare un criterio di autenticazione ad account utente con privilegi elevati e un altro agli account del servizio. È necessario creare almeno un criterio di autenticazione prima di poter creare un silo di criteri di autenticazione.  
  
> [!NOTE]  
> È possibile applicare un criterio di autenticazione ai membri di un silo di criteri di autenticazione oppure applicarlo indipendentemente dai silo per limitare l'ambito dell'account specifico. Ad esempio, per proteggere un singolo account o un piccolo set di account, è possibile impostare un criterio senza dover aggiungere gli account a un silo.  
  
È possibile creare un silo di criteri di autenticazione utilizzando il centro di amministrazione di Active Directory o Windows PowerShell. Per impostazione predefinita, un silo di criteri di autenticazione controlla solo i criteri del sito, che equivale a specificare il **WhatIf** parametro nel cmdlet di Windows PowerShell. In questo caso, le restrizioni al silo di criteri non si applicano ma vengono generati i controlli per indicare se si verificano errori in caso di applicazione delle restrizioni.  
  
#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Per creare un silo di criteri di autenticazione con il Centro di amministrazione di Active Directory  
  
1.  Aprire **Centro di amministrazione di Active Directory**, fare clic su **Autenticazione**, fare clic con il pulsante destro del mouse su **Silo di criteri di autenticazione**, scegliere **Nuovo** e quindi fare clic su **Silo di criteri di autenticazione**.  
  
    ![Aprire * * Centro di amministrazione di Active Directory * *, fare clic su * * autenticazione * *, fare clic con il pulsante destro del mouse su * * silo di criteri di autenticazione * *, fare clic su * * nuovo * * e quindi fare clic su * * silo di criteri di autenticazione * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)  
  
2.  In **Nome visualizzato** digitare un nome per il silo. In **Account consentiti** fare clic su **Aggiungi**, digitare i nomi degli account e quindi fare clic su **OK**. È possibile specificare account utente, computer o del servizio. Specificare quindi se usare un singolo criterio per tutte le entità o un criterio separato per ogni tipo di entità e il nome del criterio o dei criteri.  
  
    ![In * * nome visualizzato * * digitare un nome per il silo. In * * account consentiti * *, fare clic su * * Aggiungi * *, digitare i nomi degli account e quindi fare clic su * * OK * *](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)  
  
### <a name="manage-authentication-policy-silos-by-using-windows-powershell"></a>Gestire i silo di criteri di autenticazione con Windows PowerShell  
Questo comando crea un oggetto silo di criterio di autenticazione e lo impone.  
  
```  
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce  
```  
  
Questo comando recupera tutti i silo di criteri di autenticazione corrispondenti al filtro specificato dal parametro **Filter**. L'output viene quindi passato al cmdlet **Format-Table** per visualizzare il nome del criterio e il valore per **Enforce** in ogni criterio.  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize  
  
Name  Enforce  
--  ----  
silo     True  
silos   False  
  
```  
  
Questo comando usa il cmdlet **Get-ADAuthenticationPolicySilo** con il parametro **Filter** per recuperare tutti i silo di criteri di autenticazione non imposti e indirizza il risultato del filtro al cmdlet **Remove-ADAuthenticationPolicySilo**.  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo  
```  
  
Questo comando concede l'accesso al silo di criteri di autenticazione denominato *Silo* all'account utente denominato *User01*.  
  
```  
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01  
```  
  
Questo comando revoca l'accesso al silo di criteri di autenticazione denominato *Silo* all'account utente denominato *User01*. Poiché il parametro **Confirm** è impostato su **$False**, non viene visualizzato alcun messaggio di conferma.  
  
```  
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False  
```  
  
Nell'esempio viene prima usato il cmdlet **Get-ADComputer** per recuperare tutti gli account computer corrispondenti al filtro specificato dal parametro **Filter**. L'output di questo comando viene passato a **Set-ADAccountAuthenticatinPolicySilo** per assegnare gli account al silo di criteri di autenticazione denominato *Silo* e applicare loro il criterio di autenticazione denominato *AuthenticationPolicy02*.  
  
```  
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02  
```  
  

