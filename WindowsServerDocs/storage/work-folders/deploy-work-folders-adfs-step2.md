---
title: Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web - Passaggio 2, lavoro post-configurazione di AD FS
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 06/06/2019
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 6364c3f8dc35fbafa518a106780ae6b767d4d40c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365776"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: Passaggio 2, AD FS lavoro di post-configurazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive il secondo passaggio nella distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. È possibile trovare gli altri passaggi di questo processo negli argomenti seguenti:  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Panoramica](deploy-work-folders-adfs-overview.md)  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 1, configurare AD FS @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 3: configurare cartelle di lavoro @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 4: configurare il proxy dell'applicazione Web @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 5: configurare i client @ no__t-0  
  
> [!NOTE]
> Le istruzioni descritte in questa sezione si riferiscono a un ambiente Windows Server 2019 o Windows Server 2016. Se usi Windows Server 2012 R2, segui le [istruzioni di Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Nel passaggio 1, è stato installato e configurato AD FS. A questo punto, è necessario eseguire i seguenti passaggi di post-configurazione per AD FS.  
  
## <a name="configure-dns-entries"></a>Configurare le voci DNS

È necessario creare due voci DNS per AD FS. Queste sono le stesse due voci che sono state usate nei passaggi di pre-installazione quando hai creato il certificato con nome alternativo del soggetto (SAN).  
  
Le voci DNS si presentano nel formato:  
  
-   nome.dominio del servizio AD FS  
  
-   enterpriseregistration.dominio  
  
-   nome.dominio del server AD FS   (la voce DNS dovrebbe già essere presente. ad esempio, 2016-ADFS.contoso.com)
  
Nell'esempio di test, i valori sono:  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>Creare i record di A e CNAME per AD FS

Per creare i record di A e CNAME per AD FS, segui questi passaggi:  
  
1.  Nel tuo controller di dominio apri Gestore DNS.  
  
2.  Espandi la cartella Zone di ricerca diretta, fai clic col tasto destro del mouse su dominio e seleziona **Nuovo Host (A)** .  
  
3.  Verrà visualizzata la finestra di dialogo **Nuovo Host**. Nel campo **Nome**, immetti l'alias per il nome del servizio AD FS. Nell'esempio di test, si tratta di **blueadfs**.  
  
    L'alias deve corrispondere allo stesso soggetto presente nel certificato usato per AD FS. Ad esempio, se il soggetto fosse adfs.contoso.com, quindi l'alias immesso sarebbe **adfs**.  
  
    > [!IMPORTANT]  
    > Quando si configura AD FS utilizzando l'interfaccia utente di Windows Server invece di Windows PowerShell, è necessario creare un record di A anziché un record di CNAME per AD FS. Il motivo è che il nome dell'entità servizio (SPN) creato tramite l'interfaccia utente contiene solo l'alias utilizzato per configurare il servizio AD FS come host.  

4.  Nell' **indirizzo IP**, immetti l'indirizzo IP per il server AD FS. Nell'esempio di test, si tratta di **192.168.0.160**. Fare clic su **Aggiungi host**.  
  
5.  Nella cartella Zone di ricerca diretta, fai clic col tasto destro del mouse nuovamente sul tuo dominio e seleziona **Nuovo alias (CNAME)** .  
  
6.  Nella finestra **Nuovo Record di risorse**, aggiungi il nome dell'alias **enterpriseregistration** e immetti il FQDN per il server AD FS. Questo alias viene utilizzato per l'aggiunta del dispositivo e deve essere chiamato **enterpriseregistration**.
  
7.  Fare clic su **OK**.  
  
Per eseguire i passaggi equivalenti tramite Windows PowerShell, usa il seguente comando. Il comando deve essere eseguito sul controller di dominio.  
  
```Powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>Configurare il trust della relying party di AD FS per Cartelle di lavoro

È possibile impostare e configurare il trust della relying party per Cartelle di lavoro, anche se Cartelle di lavoro non è stata ancora configurata. Per abilitare le Cartelle di lavoro all'utilizzo di AD FS è necessario impostare il trust della relying party. Dato che ti trovi nel processo di impostazione di AD FS, ora è il momento ideale per eseguire questo passaggio.  
  
Per configurare il trust della relying party:  
  
1.  Apri **Server Manager** nel menu **Strumenti**, seleziona **Gestione AD FS**.  
  
2.  Nel riquadro a destra, in **Azioni**, fai clic su **Aggiungi attendibilità componente**.  
  
3.  Nel pagina **iniziale**, seleziona **grado di riconoscere attestazioni** e fai clic su **Avviare**.  
  
4.  Nella pagina **Seleziona origine dati**, seleziona **Immetti dati sul componente manualmente**, quindi fai clic su **Avanti**.  
  
5.  Nel campo **Nome visualizzato**, immetti **WorkFolders**, quindi fai clic su **Avanti**.  
  
6.  Nella pagina **Configura certificato**, fai clic su **Avanti**. I certificati di crittografia del token sono facoltativi e non sono necessari per la configurazione di test.  
  
7.  Nella pagina **Configura URL**, fai clic su **Avanti**.  
  
8. Nella pagina **Configura identificatori** aggiungere l'identificatore seguente: `https://windows-server-work-folders/V1`. Questo identificatore è un valore hardcoded utilizzato dalle Cartelle di lavoro e viene inviato dal servizio Cartelle di lavoro quando comunica con AD FS. Fare clic su **Avanti**.  
  
9. Nella pagina Scegliere Criteri di controllo di accesso, seleziona **Consenti tutti gli utenti** quindi fai clic su **Avanti**.  
  
10. Nella pagina **Aggiunta attendibilità** fare clic su **Avanti**.  
  
11. Dopo aver completato la configurazione, l'ultima pagina della procedura guidata indica che la configurazione è stata completata correttamente. Seleziona la casella di controllo per la modifica delle regole di attestazione e fai clic su **Chiudi**.  
  
12. Nello snap-in AD FS, seleziona l'attendibilità della relying party di **WorkFolders** e fai clic su **Modifica criteri di rilascio dell'attestazione** in Azioni.

13. Verrà visualizzata la finestra **Modifica dei criteri di rilascio dell'attestazione per WorkFolders**. Fai clic su **Aggiungi regola**.  
  
14. Nell'elenco a discesa **modello di regola attestazioni**, seleziona **Inviare attributi LDAP come attestazioni** e fai clic su **Avanti**.  
  
15. Nella pagina **Configura regola attestazione**, nel campo **Nome regola attestazione**, immetti **WorkFolders**.  
  
16. Nell'elenco a discesa **Archivio attributi**, seleziona **Active Directory**.  
  
17. Nella tabella di mapping, immetti questi valori:  
  
    -   Nome-entità utente: UPN  
  
    -   Nome visualizzato: Nome  
  
    -   Cognome Surname  
  
    -   Nome specificato: Nome (di battesimo)  
  
18. Scegliere **Fine**. Potrai visualizzare la regola WorkFolders elencata nella Scheda Regole di trasformazione rilascio e fai clic su **OK**.  
  
### <a name="set-relying-part-trust-options"></a>Impostare le opzioni dell'attendibilità componente

Dopo che è stata configurata l'attendibilità della relying party per AD FS, è necessario completare la configurazione eseguendo i cinque comandi in Windows PowerShell. Questi comandi impostano le opzioni necessarie per Cartelle di lavoro per comunicare correttamente con AD FS e non possono essere impostate tramite l'interfaccia utente. Le opzioni sono:  
  
-   Abilitare l'uso di JSON Web Token (JWT)  
  
-   Disabilitare le attestazioni crittografate  
  
-   Attivare l'\-aggiornamento automatico  
  
-   Impostare l'emissione di token di aggiornamento OAuth a tutti i dispositivi.  

-   Garantire l'accesso dei client al trust della relying party

Per impostare queste opzioni, utilizza i comandi seguenti:  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://windows-server-work-folders/V1" -AllowAllRegisteredClients -ScopeNames openid,profile  
```  
  
## <a name="enable-workplace-join"></a>Abilitare Workplace Join

L'abilitazione di Workplace Join è facoltativa, ma può essere utile quando desideri che gli utenti siano in grado di usare i propri dispositivi personali per accedere alle risorse dell'area di lavoro.  
  
Per abilitare la registrazione dei dispositivi per Workplace Join, è necessario eseguire i seguenti comandi di Windows PowerShell, che consentiranno di configurare la registrazione dei dispositivi e impostare i criteri di autenticazione globali:  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>Esportare il certificato AD FS

Successivamente, esporta il certificato AD FS auto\-firmato in modo che possa essere installato nei seguenti computer nell'ambiente di test:  
  
-   Il server utilizzato per Cartelle di lavoro  
  
-   Il server utilizzato per Proxy dell'applicazione Web  
  
-   Il client Windows aggiunto al dominio  
  
-   Il client Windows non aggiunto al dominio  
  
Per esportare il certificato, segui questi passaggi:  
  
1.  Fare clic su **Start**, quindi scegliere **Esegui**.  
  
2.  Tipo **MMC**.  
  
3.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
4.  Nella lista **Snap-in disponibili**, seleziona **Certificati**, quindi fai clic su **Aggiungi**. Verrà avviata la procedura guidata Snap-in certificati.  
  
5.  Selezionare **Account del computer** e quindi fare clic su **Avanti**.  
  
6.  Seleziona **Computer locale: (il computer su cui è in esecuzione questa console)** , quindi fai clic su **Fine**.  
  
7.  Fare clic su **OK**.  
  
8.  Espandi la cartella **Console Root\Certificates\(Local Computer) \Personal\Certificates**.  
  
9.  Fai clic con il pulsante destro del mouse sul **certificato AD FS**, fai clic su **Tutte le attività**, quindi fai clic su **Esporta...** .  
  
10. Verrà visualizzata l'Esportazione guidata certificati. Seleziona **Sì, esporta la chiave privata**.  
  
11. Nella pagina **Formato file di esportazione**, lascia selezionate le opzioni predefinite e fai clic su **Avanti**.  
  
12. Creare una password per il certificato. Questa è la password che verrà usata in seguito quando si importerà il certificato in altri dispositivi. Fare clic su **Avanti**.  
  
13. Immetti un percorso e nome per il certificato, quindi fai clic su **Fine**.  
  
L'installazione del certificato viene descritta in seguito nella procedura di distribuzione.  
  
## <a name="manage-the-private-key-setting"></a>Gestisci l'impostazione della chiave privata

È necessario assegnare l'autorizzazione dell'account di servizio AD FS per accedere alla chiave privata del nuovo certificato. È necessario concedere nuovamente questa autorizzazione quando si sostituisce il certificato di comunicazione dopo la scadenza. Per concedere l'autorizzazione, segui questi passaggi:  
  
1.  Fare clic su **Start**, quindi scegliere **Esegui**.  
  
2.  Tipo **MMC**.  
  
3.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
4.  Nella lista **Snap-in disponibili**, seleziona **Certificati**, quindi fai clic su **Aggiungi**. Verrà avviata la procedura guidata Snap-in certificati.  
  
5.  Selezionare **Account del computer** e quindi fare clic su **Avanti**.  
  
6.  Seleziona **Computer locale: (il computer su cui è in esecuzione questa console)** , quindi fai clic su **Fine**.  
  
7.  Fare clic su **OK**.  
  
8.  Espandi la cartella **Console Root\Certificates\(Local Computer) \Personal\Certificates**.  
  
9.  Fai clic con il pulsante destro del mouse su **certificato AD FS**, fai clic su **Tutte le attività**, quindi fai clic su **Gestisci chiavi private**.  
  
10. Nella finestra **Autorizzazioni**, fai clic su **Aggiungi**.  
  
11. Nella finestra **Tipi di oggetto**, seleziona **Account servizio**, quindi fai clic su **OK**.  
  
12. Digita il nome dell'account che esegue AD FS. Nell'esempio di test, si tratta di ADFSService. Fare clic su **OK**.  
  
13. Nella finestra **Autorizzazioni**, assegna all'account almeno le autorizzazioni di lettura e fai clic su **OK**.  
  
Se non si ha la possibilità di gestire le chiavi private, potrebbe essere necessario eseguire il comando seguente: `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>Verificare che AD FS sia operativo

Per verificare che AD FS sia operativo, aprire una finestra del browser e passare a `https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml`, modificando l'URL in modo che corrisponda all'ambiente.
  
I metadati del server federativo verranno visualizzati nella finestra del browser senza alcuna formattazione. Se è possibile visualizzare i dati senza errori o avvisi SSL, il server federativo è operativo.  
  
Passaggio successivo: Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 3: configurare cartelle di lavoro @ no__t-0  
  
## <a name="see-also"></a>Vedere anche  
[Panoramica di cartelle di lavoro](Work-Folders-Overview.md)