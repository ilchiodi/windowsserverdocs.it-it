---
title: L'utente non riesce ad autenticarsi o deve autenticarsi due volte
description: Risoluzione di un problema per cui l'utente non riesce a eseguire l'autenticazione o deve eseguirla due volte all'avvio di una connessione Desktop remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7cfda8814347f8bab9dc7b3f7632e3b992ecb
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857234"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>L'utente non riesce ad autenticarsi o deve autenticarsi due volte

Questo articolo illustra diversi fattori che possono causare problemi relativi all'autenticazione dell'utente.

## <a name="access-denied-restricted-type-of-logon"></a>Accesso negato, tipo di accesso con restrizioni

In questa situazione a un utente Windows 10 che tenta di connettersi a computer Windows 10 o Windows Server 2016 viene negato l'accesso con un messaggio simile al seguente:

> Connessione Desktop remoto:  
> L'amministratore di sistema ha limitato il tipo di accesso (in rete o interattivo) che è possibile usare. Per assistenza, contattare l'amministratore di sistema o il supporto tecnico.

Questo problema si verifica quando è necessaria l'Autenticazione a livello di rete per le connessioni RDP e l'utente non è membro del gruppo **Utenti desktop remoto**. Può verificarsi anche se il gruppo **Utenti desktop remoto** non è stato assegnato al diritto utente **Accedi al computer dalla rete**.

Per risolvere il problema, esegui una di queste operazioni:

  - [Modifica l'appartenenza dell'utente al gruppo o l'assegnazione dei diritti utente](#modify-the-users-group-membership-or-user-rights-assignment).
  - Disattiva l'Autenticazione a livello di rete (scelta non consigliata).
  - Usa client Desktop remoto diversi da Windows 10. Ad esempio, i client Windows 7 non presentano questo problema.

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificare l'appartenenza dell'utente al gruppo o l'assegnazione dei diritti utente

Se questo problema riguarda un utente singolo, la soluzione più semplice è quella di aggiungere tale utente al gruppo **Utenti desktop remoto**.

Se l'utente fa già parte di questo gruppo (o se più membri del gruppo hanno lo stesso problema), verifica la configurazione dei diritti utente nel computer Windows 10 o Windows Server 2016 remoto.

1. Apri l'Editor oggetti Criteri di gruppo ed effettua la connessione al criterio locale del computer remoto.
2. Passa a **Configurazione computer\\Impostazioni di Windows\\Impostazioni sicurezza\\Criteri locali\\Assegnazione diritti utente**, fai clic con il pulsante destro del mouse su **Accedi al computer dalla rete** e quindi scegli **Proprietà**.
3. Verifica l'elenco degli utenti e dei gruppi per **Utenti desktop remoto** (o un gruppo padre).
4. Se l'elenco non include **Utenti desktop remoto**  o un gruppo padre come **Everyone**, devi aggiungerlo. Se nella distribuzione sono presenti più computer, usa un oggetto Criteri di gruppo.  
    Ad esempio, come membro predefinito per **Accedi al computer dalla rete** è incluso **Everyone**. Se la distribuzione usa un oggetto Criteri di gruppo per rimuovere **Everyone**, potresti dover ripristinare l'accesso aggiornando tale oggetto in modo da aggiungere **Utenti desktop remoto**.

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accesso negato, una chiamata remota al database SAM è stata negata

Questo comportamento si verifica molto probabilmente se i controller di dominio eseguono Windows Server 2016 o versioni successive e gli utenti tentano di connettersi usando un'app di connessione personalizzata. In particolare, l'accesso verrà negato alle applicazioni che accedono alle informazioni del profilo dell'utente in Active Directory.

Tale comportamento dipende da una modifica apportata a Windows. In Windows Server 2012 R2 e versioni precedenti, quando un utente accede a un desktop remoto, Connection Manager di Accesso remoto contatta il controller di dominio per richiedere le configurazioni specifiche di Desktop remoto tramite query sull'oggetto utente in Active Directory Domain Services (AD DS). Queste informazioni vengono visualizzate nella scheda Profilo di Servizi Desktop remoto delle proprietà dell'oggetto di un utente nello snap-in di MMC Utenti e computer di Active Directory.

A partire da Windows Server 2016, Connection Manager di Accesso remoto non esegue più query sull'oggetto utente in AD DS. Se hai bisogno di Connection Manager di Accesso remoto per eseguire query su AD DS perché usi gli attributi di Servizi Desktop remoto, devi abilitare la query manualmente.

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.

Per abilitare il comportamento legacy di Connection Manager di Accesso remoto in un server Host sessione Desktop remoto, configura le voci seguenti del Registro di sistema e quindi riavvia il servizio **Servizi Desktop remoto**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<nome WinStation\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Digitare il comando seguente: **Reg\_DWORD**
      - Valore: **1** (Decimal)

Per abilitare il comportamento legacy di Connection Manager di Accesso remoto in un server che non sia un server Host sessione Desktop remoto, configura queste voci del Registro di sistema e la voce aggiuntiva seguente (e quindi riavvia il servizio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Per altre informazioni su questo comportamento, vedi l'articolo della Knowledge Base 3200967, [Modifiche relative a Connection Manager di Accesso remoto in Windows Server](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server).

## <a name="user-cant-sign-in-using-a-smart-card"></a>Un utente non riesce ad accedere usando una smart card

Questa sezione illustra tre scenari comuni in cui un utente non riesce ad accedere a un desktop remoto usando una smart card.

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>Impossibilità di accedere a una succursale con un controller di dominio di sola lettura usando una smart card

Questo problema si verifica nelle distribuzioni che includono un server Host sessione Desktop remoto presso una succursale che usa un controller di dominio di sola lettura. Il server Host sessione Desktop remoto è ospitato nel dominio radice. Gli utenti nella succursale appartengono a un dominio figlio e usano smart card per l'autenticazione. Il controller di dominio di sola lettura è configurato in modo da memorizzare nella cache le password utente (tale controller appartiene al gruppo **Ogg. autorizzati a replica passw. in controller sola lettura**). Quando gli utenti tentano di accedere a sessioni nel server Host sessione Desktop remoto, ricevono messaggi del tipo "Il tentativo di accesso non è valido a causa di un nome utente o di informazioni di autenticazione non validi".

Questo problema è dovuto al modo in cui il controller di dominio radice e il controller di dominio di sola lettura gestiscono la crittografia delle credenziali utente. Il primo usa una chiave di crittografia per crittografare le credenziali, mentre il secondo fornisce una chiave di decrittografia al client. Quando un utente riceve l'errore "non valido", significa che le due chiavi non corrispondono.

Per risolvere il problema, prova a eseguire una di queste operazioni:

- Cambia la topologia del controller di dominio disabilitando la memorizzazione delle password nella cache sul controller di dominio di sola lettura oppure distribuendo alla succursale un controller di dominio scrivibile.
- Sposta il server Host sessione Desktop remoto nello stesso dominio figlio in cui si trovano gli utenti.
- Consenti agli utenti di accedere senza smart card.

Tieni presente che tutte queste soluzioni richiedono compromessi a livello di prestazioni o sicurezza.

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>Impossibilità di accedere a un computer Windows Server 2008 SP2 usando una smart card

Questo problema si verifica quando gli utenti accedono a un computer Windows Server 2008 SP2 aggiornato con KB4093227 (2018.4B). Se gli utenti tentano di accedere usando una smart card, viene loro negato l'accesso con messaggi del tipo "Nessun certificato valido trovato. Controllare che la scheda sia inserita correttamente". Allo stesso tempo, il computer Windows Server registra l'evento applicazione "Errore durante la ricerca di un certificato digitale dalla smart card inserita: Firma non valida".

Per risolvere questo problema, aggiorna il computer Windows Server con il nuovo rilascio 2018.06 B oggetto dell'articolo della Knowledge Base 4093227, [Descrizione dell'aggiornamento di sicurezza per la vulnerabilità di tipo Denial of Service di Windows Remote Desktop Protocol (RDP) in Windows Server 2008: 10 aprile 2018](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Impossibilità di restare connessi con una smart card e blocco di Servizi Desktop remoto

Questo problema si verifica quando gli utenti accedono a un computer Windows o Windows Server aggiornato con KB 4056446. L'utente inizialmente può riuscire ad accedere al sistema tramite una smart card, ma successivamente riceve un messaggio di errore di tipo "SCARD\_E\_NO\_SERVICE". Il computer remoto potrebbe smettere di rispondere.

Per ovviare a questo problema, riavvia il computer remoto.

Per risolvere questo problema, aggiorna il computer remoto con la correzione appropriata:

  - Windows Server 2008 SP2: KB 4090928, [Windows perde gli handle nel processo lsm.exe e le applicazioni smart card possono visualizzare errori "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 maggio 2018 - KB4103724 (anteprima del rollup mensile)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versione 1607: KB 4103720, [17 maggio 2018 - KB4103720 (build sistema operativo 14393.2273)](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>Se il PC remoto è bloccato, l'utente deve immettere due volte la password

Questo problema può verificarsi quando un utente prova a connettersi a un desktop remoto che esegue Windows 10 versione 1709 in una distribuzione in cui le connessioni RDP non richiedono l'Autenticazione a livello di rete. In queste condizioni, se il desktop remoto è stato bloccato, l'utente deve immettere due volte le credenziali quando si connette.

Per risolvere questo problema, aggiorna il computer Windows 10 versione 1709 con KB 4343893, [30 agosto 2018 - KB4343893 (build sistema operativo 16299.637)](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893).

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Un utente non riesce ad accedere e riceve messaggi che indicano un "errore di autenticazione" e la "correzione Oracle di crittografia CredSSP"

Quando gli utenti provano ad accedere usando una versione qualsiasi di Windows, da Windows Vista SP2 e versioni successive a Windows Server 2008 SP2 e versioni successive, viene loro negato l'accesso e vengono visualizzati messaggi simili ai seguenti:

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

"Correzione Oracle di crittografia CredSSP" indica un set di aggiornamenti di sicurezza rilasciati a marzo, aprile e maggio 2018. CredSSP è un provider di autenticazione che elabora le richieste di autenticazione per altre applicazioni. L'aggiornamento "3B" del 13 marzo 2018 e gli aggiornamenti successivi hanno riguardato un exploit in cui l'autore di un attacco poteva inoltrare le credenziali utente per eseguire codice sul sistema di destinazione.

Gli aggiornamenti iniziali hanno aggiunto il supporto per un nuovo oggetto Criteri di gruppo, ovvero Correzione oracolo di crittografia, che può avere le impostazioni seguenti:

  - Vulnerabile. Le applicazioni client che usano CredSSP possono eseguire il fallback a versioni non sicure, ma questo comportamento espone i desktop remoti ad attacchi. I servizi che usano CredSSP accettano i client che non sono stati aggiornati.
  - Mitigato. Le applicazioni client che usano CredSSP non possono eseguire il fallback a versioni non sicure, ma i servizi che usano CredSSP accettano i client che non sono stati aggiornati.
  - Forza client aggiornati. Le applicazioni client che usano CredSSP non possono eseguire il fallback a versioni non sicure e i servizi che usano CredSSP non accetteranno i client senza patch. 
    > [!NOTE]
    > questa impostazione deve essere distribuita solo dopo che tutti gli host remoti supportano la versione più recente.

L'aggiornamento dell'8 maggio 2018 ha modificato l'impostazione predefinita di Correzione oracolo di crittografia da Vulnerabile a Mitigato. A causa di tale modifica, i client Desktop remoto con gli aggiornamenti non possono connettersi a server in cui gli aggiornamenti non sono installati (o a server aggiornati che non sono stati riavviati). Per altre informazioni sugli aggiornamenti di CredSSP, vedi l'articolo [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Per risolvere questo problema, aggiorna e riavvia tutti i sistemi. Per un elenco completo degli aggiornamenti e altre informazioni sulle vulnerabilità, vedi [CVE-2018-0886 | Vulnerabilità relativa all'esecuzione di codice in remoto CredSSP](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886).

Per ovviare a questo problema fino al completamento degli aggiornamenti, leggi l'articolo della Knowledge Base 4093492 per avere informazioni sui tipi di connessioni consentiti. Se non sono disponibili alternative fattibili, considera la possibilità di procedere in uno dei modi seguenti:

- Per i computer client interessati, reimposta il criterio Correzione oracolo di crittografia su **Vulnerabile**.
- Modifica i criteri seguenti nella cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Sicurezza**:  
  - **Richiedi l'utilizzo di un livello di sicurezza specificato per le connessioni remote (RDP)** : imposta su **Abilitato** e seleziona **RDP**.
  - **Richiedi autenticazione utente tramite Autenticazione a livello di rete per le connessioni remote**: imposta su **Disabilitato**.
    > [!IMPORTANT]  
    > La modifica di questi criteri di gruppo ha l'effetto di ridurre la sicurezza della distribuzione. Ti consigliamo di usarli solo temporaneamente, se proprio devi farlo.

Per altre informazioni sulla gestione dei criteri di gruppo, vedi [Modifica di un oggetto Criteri di gruppo che blocca](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo).

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Dopo l'aggiornamento dei computer client, alcuni utenti devono accedere due volte

Quando gli utenti accedono a Desktop remoto usando un computer che esegue Windows 7 o Windows 10, versione 1709, visualizzano immediatamente una seconda richiesta di accesso. Questo problema si verifica se nel computer client sono installati gli aggiornamenti seguenti:

  - Windows 7: KB 4103718, [8 maggio 2018 - KB4103718 (rollup mensile)](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 maggio 2018 - KB4103727 (build sistema operativo 16299.431)](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

Per risolvere questo problema, assicurati che i computer a cui gli utenti vogliono connettersi (e i server Host sessione Desktop remoto o RDVI) siano stati completamente aggiornati fino a giugno 2018. Sono inclusi gli aggiornamenti seguenti:

  - Windows Server 2016: KB 4284880, [12 giugno 2018 - KB4284880 (build sistema operativo 14393.2312)](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 giugno 2018 - KB4284815 (rollup mensile)](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 giugno 2018 - KB4284855 (rollup mensile)](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 giugno 2018 - KB4284826 (rollup mensile)](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB 4056564, [Descrizione dell'aggiornamento di sicurezza per la vulnerabilità relativa all'esecuzione di codice in remoto CredSSP in Windows Server 2008, Windows Embedded POSReady 2009 e Windows Embedded Standard 2009: 13 marzo 2018](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Agli utenti viene negato l'accesso per una distribuzione che usa Remote Credential Guard con più gestori connessione Desktop remoto

Questo problema si verifica nelle distribuzioni a disponibilità elevata che usano due o più gestori connessione Desktop remoto, se è in uso Windows Defender Remote Credential Guard. Gli utenti non riescono ad accedere ai desktop remoti.

Questo problema si verifica perché Remote Credential Guard usa Kerberos per l'autenticazione e limita NTLM. In una configurazione a disponibilità elevata con bilanciamento del carico i gestori connessione Desktop remoto, tuttavia, non possono supportare le operazioni Kerberos.

Se hai necessità di usare una tale configurazione con gestori connessione Desktop remoto con bilanciamento del carico, puoi ovviare a questo problema disabilitando Remote Credential Guard. Per altre informazioni su come gestire Windows Defender Remote Credential Guard, vedi [Proteggere le credenziali di Desktop remoto con Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).
