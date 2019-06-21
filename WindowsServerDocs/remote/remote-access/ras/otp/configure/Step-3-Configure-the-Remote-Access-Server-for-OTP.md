---
title: Passaggio 3 configurare il Server di accesso remoto per OTP
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 093877657f19006bba2b80c10b92db1fb3b40fde
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280859"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Passaggio 3 configurare il Server di accesso remoto per OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver configurato il server RADIUS con i token di distribuzione software, verranno aperte le porte di comunicazione, è stato creato un segreto condiviso, sono stati creati account di utente corrispondente in Active Directory del server RADIUS e il server di accesso remoto dispone di stato configurato come un agente di autenticazione RADIUS, quindi il server di accesso remoto deve essere configurato per supportare OTP.  
  
|Attività|Descrizione|  
|----|--------|  
|[3.1 esenzione dall'autenticazione OTP (facoltativo)](#BKMK_Exempt)|Se utenti specifici saranno esentati da DirectAccess con l'autenticazione OTP, seguire questi passaggi preliminari.|  
|[3.2 configurazione del server di accesso remoto per supportare OTP](#BKMK_Config)|Aggiornare la configurazione di accesso remoto per supportare l'autenticazione OTP a due fattori nel server di accesso remoto.|  
|[3.3 Smart card per fornire autorizzazioni aggiuntive](#BKMK_Smartcard)|Informazioni aggiuntive relative all'utilizzo di smart card.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3.1 esenzione dall'autenticazione OTP (facoltativo)  
Se utenti specifici l'esenzione dall'autenticazione OTP, prima di eseguire la configurazione di accesso remoto è necessario eseguire questi passaggi:  
  
> [!NOTE]  
> È necessario attendere la replica tra i domini da completare quando si configura il gruppo di esenzione OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Crea gruppo di sicurezza di esenzione utente  
  
1.  Creare un gruppo di sicurezza in Active Directory per lo scopo esenzioni OTP.  
  
2.  Aggiungere tutti gli utenti per l'esenzione dall'autenticazione OTP a gruppo di sicurezza.  
  
    > [!NOTE]  
    > Assicurarsi di includere solo gli account utente e non per l'account computer, nel gruppo di sicurezza di esenzioni OTP.  
  
## <a name="BKMK_Config"></a>3.2 configurazione del server di accesso remoto per supportare OTP  
Per configurare accesso remoto per usare l'autenticazione a due fattori e OTP con il server RADIUS e la distribuzione del certificato nella sezione precedente, procedere come segue:  
  
#### <a name="configure-remote-access-for-otp"></a>Configurare l'accesso remoto per OTP  
  
1.  Aprire **Gestione accesso remoto** e fare clic su **configurazione**.  
  
2.  Nel **configurazione di DirectAccess** finestra, sotto **passaggio 2: Server di accesso remoto**, fare clic su **Edit**.  
  
3.  Fare clic su **successivo** tre volte e nel **autenticazione** sezione selezionare entrambe **a due fattori di autenticazione** e **Usa OTP**e assicurarsi che che **Usa certificati computer** sia selezionata.  
  
    > [!NOTE]  
    > Dopo aver abilitato la OTP sul server di accesso remoto, se si disabilita OTP deselezionando **Usa OTP**, le estensioni ISAPI e CGI verranno disinstallate nei server.  
  
4.  Se è necessario il supporto di Windows 7, selezionare la **i computer client di attivare Windows 7 alla connessione tramite DirectAccess** casella di controllo. Nota: Come illustrato nella sezione pianificazione, i client Windows 7 devono disporre di DCA 2.0 per il supporto di DirectAccess con OTP installato.  
  
5.  Fare clic su **Avanti**.  
  
6.  Nel **Server RADIUS OTP** , quindi fare doppio clic su bianco **nome Server** campo.  
  
7.  Nel **aggiungere un Server RADIUS** finestra di dialogo digitare il nome del server RADIUS nella **nome Server** campo. Fare clic su **Change** accanto al **segreto condiviso** campo, quindi digitare la stessa password usati durante la configurazione del server RADIUS nel **nuovo master secret** e  **Conferma segreto nuovo** campi. Fare clic su **OK** due volte, fare clic su **successivo**.  
  
    > [!NOTE]  
    > Se il server RADIUS si trova in un dominio diverso da quello del server di accesso remoto, il **nome Server** campo necessario specificare il FQDN del server RADIUS.  
  
8.  Nel **server CA OTP** sezione selezionare i server di autorità di certificazione da utilizzare per la registrazione dei certificati di autenticazione client OTP, e fare clic su **Add**. Fare clic su **Avanti**.  
  
9. Nel **modelli di certificato OTP** sezione fare clic su **Sfoglia** per selezionare il modello di certificato usato per la registrazione dei certificati emessi per l'autenticazione OTP.  
  
    > [!NOTE]  
    > Il modello di certificato per OTP certificati emessi dall'autorità di certificazione aziendale deve essere configurato senza l'opzione "Non includono le informazioni di revoca dei certificati emessi". Se questa opzione è selezionata durante la creazione di modelli di certificato, quindi i computer client OTP avrà esito negativo accedere in modo corretto.  
  
    Fare clic su **esplorare** per selezionare un modello di certificato usato per registrare il certificato utilizzato dal server di accesso remoto per firmare le richieste di registrazione di certificato OTP. Fare clic su **OK**. Fare clic su **Avanti**.  
  
10. Se impostando un'esenzione per utenti specifici di DirectAccess con OTP è necessaria, quindi nella **esenzioni OTP** sezione selezionare **non richiedono gli utenti del gruppo di sicurezza specificato per l'autenticazione usando l'autenticazione a due fattori** . Fare clic su **gruppo di sicurezza** e selezionare il gruppo di sicurezza che è stato creato per esenzioni OTP.  
  
11. Nel **configurazione Server di accesso remoto** pagina fare clic su **fine**.  
  
12. Nel **configurazione di DirectAccess** finestra, sotto **passaggio 3: server di infrastruttura**, fare clic su **Edit**.  
  
13. Fare clic su **successivo** due volte e nel **Management** sezione fare doppio clic sui **i server di gestione** campo.  
  
14. Immettere il **nome Computer** oppure **indirizzo** del server Autorità di certificazione che è configurato per emettere certificati OTP, fare clic su **OK**.  
  
15. Nel **configurazione dell'accesso remoto** windows fare clic su **fine**.  
  
16. Fare clic su **Finish** nel **guidato Expert DirectAccess**.  
  
17. Nel **verifica di accesso remoto** fare clic su finestra di dialogo **Apply**, attendere l'aggiornamento di criteri DirectAccess e fare clic su **Chiudi**.  
  
18. Nel **avviare** digitare**powershell.exe**, fare doppio clic su **powershell**, fare clic su **avanzate**, fare clic su **runas amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
19. Nella finestra di Windows PowerShell, digitare **gpupdate /force** e premere INVIO.  
  
Per configurare accesso remoto per OTP usando i comandi di PowerShell:  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell**  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Per configurare accesso remoto per usare l'autenticazione a due fattori in una distribuzione che attualmente usa l'autenticazione del certificato computer:  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Per configurare accesso remoto per usare l'autenticazione OTP tramite le impostazioni seguenti:  
  
-   Un server OTP denominato OTP.corp.contoso.com.  
  
-   Nome di un server di autorità di certificazione APP1.corp.contoso.com\corp-APP1-CA1.  
  
-   Un modello di certificato denominato DAOTPLogon utilizzato per la registrazione dei certificati emessi per l'autenticazione OTP.  
  
-   Un modello di certificato denominato DAOTPRA usato per registrare il certificato dell'autorità di registrazione usato dal server di accesso remoto per firmare le richieste di registrazione di certificato OTP.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Dopo avere eseguito i comandi di PowerShell completare i passaggi 12-19 della procedura precedente per configurare il server di accesso remoto per supportare OTP.  
  
> [!NOTE]  
> Assicurarsi di verificare di aver applicato le impostazioni OTP sul server di accesso remoto prima di aggiungere un punto di ingresso.  
  
## <a name="BKMK_Smartcard"></a>3.3 Smart card per fornire autorizzazioni aggiuntive  
Nella pagina di autenticazione del passaggio 2 nella configurazione guidata accesso remoto, è possibile richiedere l'uso di smart card per l'accesso alla rete interna. Quando questa opzione è selezionata, la configurazione guidata accesso remoto configura la regola di sicurezza di connessione IPsec per il tunnel intranet sul server DirectAccess per richiedere l'autorizzazione in modalità tunnel con smart card. Autorizzazione in modalità tunnel consente di specificare che solo i computer autorizzati o gli utenti possono stabilire un tunnel in entrata.  
  
Per poter utilizzare smart card con l'autorizzazione in modalità tunnel IPsec per il tunnel della Intranet, è necessario distribuire una infrastruttura a chiave pubblica con l'infrastruttura delle smart card.  
  
Poiché i client DirectAccess utilizzano smart card per l'accesso alla rete intranet, è anche possibile usare verifica del meccanismo di autenticazione, una funzionalità di Windows Server 2008 R2, per controllare l'accesso alle risorse, ad esempio file, cartelle e stampanti, a seconda che il utente connesso con un certificato basato su smart card. Verifica del meccanismo di autenticazione richiede un livello funzionale di dominio di Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Accesso per utenti con smart card inutilizzabili  
Per consentire l'accesso temporaneo agli utenti con smart card inutilizzabili, eseguire le operazioni indicate di seguito.  
  
1.  Creare un gruppo di sicurezza Active Directory che contenga gli account di utenti che non possono temporaneamente utilizzare smart card.  
  
2.  Per l'oggetto Criteri di gruppo del server DirectAccess, configurare le impostazioni IPsec globali per l'autorizzazione tunnel IPsec e aggiungere il gruppo di sicurezza di Active Directory all'elenco di utenti autorizzati.  
  
Per concedere l'accesso a un utente che non può utilizzare smart card, aggiungere temporaneamente l'account utente al gruppo di sicurezza Active Directory. Se la smart card è utilizzabile, rimuovere l'account utente dal gruppo.  
  
### <a name="under-the-covers-smart-card-authorization"></a>Dettagli: autorizzazione di smart card  
L'autorizzazione delle smart card si verifica abilitando l'autorizzazione in modalità tunnel nella regola di protezione delle connessioni del tunnel della Intranet del server DirectAccess per un ID di sicurezza (SID) specifico basato su Kerberos. Per l'autorizzazione di smart card, si tratta del ben noto SID (S-1-5-65-1), che effettua il mapping agli accessi basati su smart card. Questo SID è presente nel token Kerberos del client DirectAccess e viene definito come "Certificato di questa organizzazione" quando è configurata in IPsec globali delle impostazioni di autorizzazione in modalità tunnel.  
  
Quando si abilita l'autorizzazione di smart card nel passaggio 2 della configurazione guidata DirectAccess, la configurazione guidata DirectAccess configura l'impostazione dell'autorizzazione in modalità tunnel IPsec globali con questo SID per l'oggetto Criteri di gruppo del server DirectAccess. Per visualizzare questa configurazione in Windows Firewall con snap-in sicurezza avanzata per l'oggetto Criteri di gruppo del server DirectAccess, eseguire le operazioni seguenti:  
  
1.  Fare clic con il pulsante destro Windows Firewall con sicurezza avanzata e quindi fare clic su proprietà.  
  
2.  Nella scheda Impostazioni IPsec in autorizzazione tunnel IPsec, fare clic su Personalizza.  
  
3.  Fare clic sulla scheda utenti. Si dovrebbe essere il "NT organizzazione Authority\certificato" come un utente autorizzato.  
  


