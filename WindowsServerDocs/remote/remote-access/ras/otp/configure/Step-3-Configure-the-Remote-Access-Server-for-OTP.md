---
title: Passaggio 3 configurare il server di accesso remoto per OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41cc5cc2df5ac9709818536df8fff098d2a0c297
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404339"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Passaggio 3 configurare il server di accesso remoto per OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Una volta che il server RADIUS è stato configurato con i token di distribuzione software, le porte di comunicazione sono aperte, è stato creato un segreto condiviso, gli account utente corrispondenti a Active Directory sono stati creati nel server RADIUS e il server di accesso remoto ha è stato configurato come un agente di autenticazione RADIUS, quindi il server di accesso remoto deve essere configurato per il supporto di OTP.  
  
|Attività|Descrizione|  
|----|--------|  
|[3,1 esentare gli utenti dall'autenticazione OTP (facoltativo)](#BKMK_Exempt)|Se utenti specifici verranno esentati da DirectAccess con autenticazione OTP, seguire questa procedura preliminare.|  
|[3,2 configurare il server di accesso remoto per il supporto di OTP](#BKMK_Config)|Nel server di accesso remoto aggiornare la configurazione di accesso remoto per supportare l'autenticazione a due fattori OTP.|  
|[3,3 Smart Card per autorizzazioni aggiuntive](#BKMK_Smartcard)|Informazioni aggiuntive sull'utilizzo delle smart card.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3,1 esentare gli utenti dall'autenticazione OTP (facoltativo)  
Se gli utenti specifici devono essere esentati dall'autenticazione OTP, questi passaggi devono essere eseguiti prima della configurazione di accesso remoto:  
  
> [!NOTE]  
> È necessario attendere il completamento della replica tra domini quando si configura il gruppo di esenzione OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Crea gruppo di sicurezza esenzione utente  
  
1.  Creare un gruppo di sicurezza in Active Directory per l'esenzione degli scopi OTP.  
  
2.  Aggiungere tutti gli utenti da esentare dall'autenticazione OTP al gruppo di sicurezza.  
  
    > [!NOTE]  
    > Assicurarsi di includere solo gli account utente e non gli account computer nel gruppo di sicurezza di esenzione OTP.  
  
## <a name="BKMK_Config"></a>3,2 configurare il server di accesso remoto per il supporto di OTP  
Per configurare l'accesso remoto per l'uso dell'autenticazione a due fattori e OTP con il server RADIUS e la distribuzione dei certificati dalle sezioni precedenti, seguire questa procedura:  
  
#### <a name="configure-remote-access-for-otp"></a>Configurare l'accesso remoto per OTP  
  
1.  Aprire **gestione accesso remoto** e fare clic su **configurazione**.  
  
2.  Nella finestra **Configurazione DirectAccess** , in **passaggio 2: server di accesso remoto**, fare clic su **modifica**.  
  
3.  Fare clic tre volte su **Avanti** e nella sezione **autenticazione** selezionare **autenticazione a due fattori** e **utilizzare OTP**e assicurarsi che l'opzione **Usa certificati computer** sia selezionata.  
  
    > [!NOTE]  
    > Dopo aver abilitato OTP sul server di accesso remoto, se si disabilita OTP deselezionando **use OTP**, le estensioni ISAPI e CGI verranno disinstallate nel server.  
  
4.  Se è necessario il supporto di Windows 7, selezionare la casella **di controllo Abilita computer client Windows 7 per la connessione tramite DirectAccess** . Nota: Come illustrato nella sezione relativa alla pianificazione, per il supporto di DirectAccess con OTP i client Windows 7 devono avere installato DCA 2,0.  
  
5.  Fare clic su **Avanti**.  
  
6.  Nella sezione **server RADIUS OTP** fare doppio clic sul campo **nome server** vuoto.  
  
7.  Nella finestra di dialogo **Aggiungi server RADIUS** Digitare il nome del server RADIUS nel campo **nome server** . Fare clic su **Cambia** accanto al campo **segreto condiviso** e digitare la stessa password utilizzata durante la configurazione del server RADIUS nel **nuovo segreto** e confermare i **nuovi campi segreti** . Fare clic due volte su **OK** e quindi su **Avanti**.  
  
    > [!NOTE]  
    > Se il server RADIUS si trova in un dominio diverso da quello del server di accesso remoto, il campo **nome server** deve specificare il nome di dominio completo (FQDN) del server RADIUS.  
  
8.  Nella sezione **server CA OTP** selezionare i server CA da usare per la registrazione dei certificati di autenticazione client OTP, quindi fare clic su **Aggiungi**. Fare clic su **Avanti**.  
  
9. Nella sezione **modelli di certificato OTP** fare clic su **Sfoglia** per selezionare il modello di certificato usato per la registrazione dei certificati rilasciati per l'autenticazione OTP.  
  
    > [!NOTE]  
    > Il modello di certificato per i certificati OTP rilasciati dalla CA aziendale deve essere configurato senza l'opzione "non includere le informazioni di revoca nei certificati rilasciati". Se questa opzione viene selezionata durante la creazione del modello di certificato, i computer client OTP non riusciranno ad accedere correttamente.  
  
    Fare clic su **Sfoglia** per selezionare un modello di certificato usato per registrare il certificato usato dal server di accesso remoto per firmare le richieste di registrazione del certificato OTP. Fare clic su **OK**. Fare clic su **Avanti**.  
  
10. Se è necessario esentare utenti specifici da DirectAccess con OTP, nella sezione **esenzioni OTP** selezionare non **richiedere agli utenti del gruppo di sicurezza specificato di eseguire l'autenticazione con l'autenticazione a due fattori**. Fare clic su **gruppo di sicurezza** e selezionare il gruppo di sicurezza creato per le esenzioni OTP.  
  
11. Nella pagina **installazione del server di accesso remoto** fare clic su **fine**.  
  
12. Nella finestra **Configurazione DirectAccess** , in **passaggio 3-server infrastruttura**, fare clic su **modifica**.  
  
13. Fare clic due volte su **Avanti** e, nella sezione **gestione** , fare doppio clic sul campo **server di gestione** .  
  
14. Immettere il **nome del computer** o l' **Indirizzo** del server della CA configurato per emettere i certificati OTP, quindi fare clic su **OK**.  
  
15. Nella finestra di **dialogo configurazione di accesso remoto** fare clic su **fine**.  
  
16. Fare clic su **fine** nella **procedura guidata esperto DirectAccess**.  
  
17. Nella finestra di dialogo **Verifica accesso remoto** fare clic su **applica**, attendere l'aggiornamento del criterio DirectAccess e fare clic su **Chiudi**.  
  
18. Nella schermata **Start** Digitare**PowerShell. exe**, fare clic con il pulsante destro del mouse su **PowerShell**, fare clic su **Avanzate**e quindi su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
19. Nella finestra di Windows PowerShell, digitare **gpupdate/force** e premere INVIO.  
  
Per configurare l'accesso remoto per OTP usando i comandi di PowerShell:  
  
](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**comandi equivalenti** di PowerShell per Windows PowerShell @no__t 0Windows  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Per configurare l'accesso remoto per l'uso dell'autenticazione a due fattori in una distribuzione che attualmente usa l'autenticazione del certificato del computer:  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Per configurare l'accesso remoto per l'uso dell'autenticazione OTP usando le impostazioni seguenti:  
  
-   Un server OTP denominato OTP.corp.contoso.com.  
  
-   Un server CA denominato APP1. Corp. contoso. com\corp-APP1-CA1.  
  
-   Modello di certificato denominato DAOTPLogon usato per la registrazione dei certificati emessi per l'autenticazione OTP.  
  
-   Modello di certificato denominato DAOTPRA usato per registrare il certificato dell'autorità di registrazione usato dal server di accesso remoto per firmare le richieste di registrazione del certificato OTP.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Dopo aver eseguito i comandi di PowerShell, completare i passaggi 12-19 della procedura precedente per configurare il server di accesso remoto per il supporto di OTP.  
  
> [!NOTE]  
> Prima di aggiungere un punto di ingresso, verificare di aver applicato le impostazioni OTP sul server di accesso remoto.  
  
## <a name="BKMK_Smartcard"></a>3,3 Smart Card per autorizzazioni aggiuntive  
Nella pagina autenticazione del passaggio 2 della configurazione guidata accesso remoto è possibile richiedere l'utilizzo di smart card per l'accesso alla rete interna. Quando questa opzione è selezionata, la configurazione guidata accesso remoto configura la regola di sicurezza della connessione IPsec per il tunnel Intranet nel server DirectAccess in modo da richiedere l'autorizzazione in modalità tunnel con le smart card. L'autorizzazione in modalità tunnel consente di specificare che solo i computer o gli utenti autorizzati possono stabilire un tunnel in ingresso.  
  
Per poter utilizzare smart card con l'autorizzazione in modalità tunnel IPsec per il tunnel della Intranet, è necessario distribuire una infrastruttura a chiave pubblica con l'infrastruttura delle smart card.  
  
Poiché i client DirectAccess utilizzano smart card per l'accesso alla Intranet, è inoltre possibile utilizzare il meccanismo di autenticazione di Assurance, una funzionalità di Windows Server 2008 R2, per controllare l'accesso alle risorse, ad esempio file, cartelle e stampanti, a seconda che il utente connesso con un certificato basato su smart card. La garanzia del meccanismo di autenticazione richiede un livello di funzionalità del dominio di Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Accesso per utenti con smart card inutilizzabili  
Per consentire l'accesso temporaneo agli utenti con smart card inutilizzabili, eseguire le operazioni indicate di seguito.  
  
1.  Creare un gruppo di sicurezza Active Directory che contenga gli account di utenti che non possono temporaneamente utilizzare smart card.  
  
2.  Per il server DirectAccess Criteri di gruppo oggetto, configurare le impostazioni IPsec globali per l'autorizzazione del tunnel IPsec e aggiungere il gruppo di sicurezza Active Directory all'elenco degli utenti autorizzati.  
  
Per concedere l'accesso a un utente che non può utilizzare smart card, aggiungere temporaneamente l'account utente al gruppo di sicurezza Active Directory. Se la smart card è utilizzabile, rimuovere l'account utente dal gruppo.  
  
### <a name="under-the-covers-smart-card-authorization"></a>Dettagli: autorizzazione di smart card  
L'autorizzazione delle smart card si verifica abilitando l'autorizzazione in modalità tunnel nella regola di protezione delle connessioni del tunnel della Intranet del server DirectAccess per un ID di sicurezza (SID) specifico basato su Kerberos. Per l'autorizzazione di smart card, si tratta del ben noto SID (S-1-5-65-1), che effettua il mapping agli accessi basati su smart card. Questo SID è presente in un token Kerberos del client DirectAccess e viene definito "certificato dell'organizzazione" quando è configurato nelle impostazioni di autorizzazione della modalità tunnel IPsec globale.  
  
Quando si Abilita l'autorizzazione della smart card nel passaggio 2 della configurazione guidata DirectAccess, la configurazione guidata DirectAccess configura l'impostazione di autorizzazione della modalità tunnel IPsec globale con questo SID per l'oggetto Criteri di gruppo del server DirectAccess. Per visualizzare questa configurazione nello snap-in Windows Firewall con sicurezza avanzata per l'oggetto server Criteri di gruppo DirectAccess, eseguire le operazioni seguenti:  
  
1.  Fare clic con il pulsante destro del mouse su Windows Firewall con sicurezza avanzata e quindi scegliere Proprietà.  
  
2.  Nella scheda Impostazioni IPsec, in Autorizzazione tunnel IPsec, fare clic su Personalizza.  
  
3.  Fare clic sulla scheda utenti. Verrà visualizzato il certificato "NT Authority\certificato Organization certificate" come utente autorizzato.  
  


