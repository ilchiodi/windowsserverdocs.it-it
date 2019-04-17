---
title: Eseguire la migrazione di Active Directory server federativo ADFS 2.0
description: Fornisce informazioni sulla migrazione di un server ADFS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Eseguire la migrazione di server AD FS 2.0 federazione per AD FS in Windows Server 2012 R2

Per eseguire la migrazione di un server federativo ADFS che appartiene a una farm ADFS a nodo singolo, una farm WIF o una farm SQL Server per Windows Server 2012 R2, è necessario eseguire le attività seguenti:  
  
1.  [Esportare ed eseguire il backup di dati di configurazione di ADFS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Creare una server farm federativa di Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importare i dati di configurazione originali nella farm di Windows Server 2012 R2 AD ADFS](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Esportare ed eseguire il backup di dati di configurazione di ADFS  
 Per esportare le impostazioni di configurazione di ADFS, eseguire le procedure seguenti:  
  
###  <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Assicurarsi di disporre di accesso per i seguenti certificati e le chiavi private in un file con estensione pfx:  
  
    -   Il certificato SSL utilizzato dalla server farm federativa che si desidera eseguire la migrazione  
  
    -   Certificato di comunicazione del servizio (se è diverso dal certificato SSL) utilizzato dalla server farm federativa che si desidera eseguire la migrazione  
  
    -   Tutte le terze parti crittografia/decrittografia di token di firma di token i certificati utilizzati per la server farm federativa che si desidera eseguire la migrazione  
  
Per trovare il certificato SSL, aprire Gestione Internet Information Services (IIS), console, selezionare **sito Web predefinito** nel riquadro a sinistra, fare clic su **Binding...** nel **azione** riquadro, trovare e selezionare il binding https, fare clic su **modifica**, quindi fare clic su **visualizzazione**.  
  
È necessario esportare il certificato SSL utilizzato dal servizio federativo e la relativa chiave privata in un file con estensione pfx. Per ulteriori informazioni, vedere [esportare la parte di chiave privata di un certificato di autenticazione Server](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Se si prevede di distribuire il servizio DRS nell'ambito dell'esecuzione di AD FS in Windows Server 2012 R2, è necessario ottenere un nuovo certificato SSL. Per ulteriori informazioni, vedere [registrare un certificato SSL per ADFS](enroll-an-ssl-certificate-for-ad-fs.md) e [configurare un server federativo con Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Per visualizzare la firma di token, la decrittografia di token e certificati per le comunicazioni del servizio utilizzati, eseguire il comando seguente di Windows PowerShell per creare un elenco di tutti i certificati in uso in un file:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  Esportare le proprietà del servizio federativo AD FS, ad esempio il nome del servizio federativo, nome visualizzato del servizio federativo e l'identificatore del server federativo in un file.  
  
Per esportare le proprietà del servizio federativo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

Il file di output conterrà i valori di configurazione importanti seguenti:  
 
|**Nome della proprietà servizio federativo restituito da Get-ADFSProperties**|**Nome della proprietà servizio federativo nella console di gestione di ADFS**|
|-----|-----|  
|Nome host|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
|DisplayName|Nome visualizzato del servizio federativo|  
  
3.  Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione del database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` file in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Prendere nota della stringa di connessione al database in questo file, indicata subito dopo "policystore connectionstring =". Se la stringa di connessione specifica un database di SQL Server, il valore è necessario quando si ripristina la configurazione originale di ADFS nel server federativo.  
>   
>  Ecco un esempio di una stringa di connessione WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Di seguito è indicato un esempio di una stringa di connessione SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4.  Registrare l'identità dell'account del servizio federativo ADFS e la password dell'account.  
  
Per trovare il valore di identità, controllare il **Accedi come** colonna di **AD FS 2.0 Windows Service** nel **servizi** console e registrare manualmente questo valore.  
  
> [!NOTE]
>  Per un servizio federativo autonomo, viene utilizzato l'account servizio di rete predefinito.  In questo caso, non è necessario disporre di una password.  
  
5.  Esportare l'elenco degli endpoint ADFS abilitati in un file.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  Esportare eventuali descrizioni di attestazioni personalizzate in un file.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  Se si dispone di impostazioni personalizzate come useRelayStateForIdpInitiatedSignOn configurato nel file Web. config, assicurarsi di eseguire il backup il file Web. config per riferimento. È possibile copiare il file dalla directory mappata al percorso virtuale **"/ADFS/ls"** in IIS. Per impostazione predefinita, in è il **%systemdrive%\inetpub\adfs\ls** directory.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Per esportare attendibilità del provider di attestazioni e i trust della relying party  
  
1.  Per esportare AD FS provider di attestazioni e attendibilità, è necessario accedere come amministratore (tuttavia, non come Domain Administrator) su federativo server ed eseguire script Windows PowerShell seguente è disponibile nel **server_support/media/adfs** cartella del CD di installazione di Windows Server 2012 R2: `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  Lo script di esportazione accetta i parametri seguenti:  
>   
>  -   Export-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >]  
> -   Export-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < stringa [] >] [-ClaimsProviderTrustIdentifier < stringa [] >]  
> -   Export-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < stringa [] >] [-ClaimsProviderTrustName < stringa [] >]  
>   
>  **-RelyingPartyTrustIdentifier < stringa [] >** - il cmdlet Esporta solo le attendibilità del con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita viene esportata alcuna attendibilità. Se viene specificata alcuna del RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script esporterà tutte relying party e provider di attestazioni.  
>   
>  **-ClaimsProviderTrustIdentifier < stringa [] >** -il cmdlet Esporta solo le attendibilità del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita viene esportata alcuna attendibilità del provider di attestazioni.  
>   
>  **-RelyingPartyTrustName < stringa [] >** - il cmdlet Esporta solo le attendibilità i cui nomi sono specificati nella matrice di stringhe. Per impostazione predefinita viene esportata alcuna attendibilità.  
>   
>  **-ClaimsProviderTrustName < stringa [] >** -il cmdlet Esporta solo le attendibilità del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita viene esportata alcuna attendibilità del provider di attestazioni.  
>   
>  **-Path < string\ >** -il percorso di una cartella che conterrà i file esportati.  
>   
>  **-ComputerName < string\ >** -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione di ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo è il nome host del server ADFS legacy.  
>   
>  **-Credenziale < PSCredential\ >** -specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
>   
>  **-Force** – specifica di non richiedere conferma all'utente.  
>   
>  **-CertificatePassword < SecureString\ >** -specifica una password per l'esportazione delle chiavi private dei certificati ADFS. Se non specificato, lo script richiederà una password se è necessario un certificato ADFS con chiave privata esportabile.  
>   
>  **Input**: nessuna  
>   
>  **Output**: stringa - questo cmdlet restituisce il percorso della cartella esportazione. È possibile inviare tramite pipe l'oggetto restituito a Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Per eseguire il backup degli archivi di attributi personalizzati  
  
1.  È necessario esportare manualmente tutti gli archivi di attributi personalizzati che si desidera mantenere nella nuova farm ADFS in Windows Server 2012 R2.  
  
> [!NOTE]
>  In Windows Server 2012 R2, ADFS richiede archivi di attributi personalizzati basati su .NET Framework 4.0 o superiore. Seguire le istruzioni in [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) per installare e configurare .net Framework 4.5.  
  
Sono disponibili informazioni sugli archivi di attributi personalizzati usati da ADFS eseguendo il comando di Windows PowerShell seguente: 

``` powershell
Get-ADFSAttributeStore
```  

La procedura per aggiornare o archivi di attributi personalizzati di eseguire la migrazione varia.  
  
2.  È inoltre necessario esportare manualmente tutti i file DLL degli archivi di attributi personalizzati che si desidera mantenere nella nuova farm ADFS in Windows Server 2012 R2. La procedura per aggiornare o i file DLL degli archivi di attributi personalizzati di eseguire la migrazione varia.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Creare una server farm federativa di Windows Server 2012 R2  
  
1.  Installare il sistema operativo Windows Server 2012 R2 in un computer che si desidera operare come un server federativo e quindi aggiungere il ruolo server AD FS. Per ulteriori informazioni, vedere [installare il servizio ruolo ADFS](install-the-ad-fs-role-service.md). Configurare quindi il nuovo servizio federativo tramite la configurazione guidata servizi di Active Directory Federation o tramite Windows PowerShell. Per ulteriori informazioni, vedere "Configurare il primo server federativo in una nuova server farm federativa" in [configurare un Server federativo](configure-a-federation-server.md).  

Durante l'esecuzione di questo passaggio, è necessario seguire queste istruzioni:  
  
-   Per configurare il servizio federativo, è necessario disporre dei privilegi di amministratore di dominio.  
  
-   È necessario utilizzare lo stesso nome servizio federativo (nome farm) utilizzato in ADFS 2.0 o ADFS in Windows Server 2012. Se non si utilizza lo stesso nome servizio federativo, i certificati che è stato eseguito il backup non funzionerà nel servizio federativo di Windows Server 2012 R2 che si desidera configurare.  
  
-   Specificare se si tratta di una farm di server federativi WID o SQL Server. Se si tratta di una farm SQL, specificare il percorso del database di SQL Server e il nome di istanza.  
  
-   È necessario fornire un file pfx contenente il certificato di autenticazione server SSL sottoposti a backup come parte della preparazione per il processo di migrazione di ADFS.  
  
-   È necessario specificare la stessa identità account di servizio che è stata utilizzata in ADFS 2.0 o ADFS in farm di Windows Server 2012.  
  
2.  Dopo aver configurato il nodo iniziale, è possibile aggiungere ulteriori nodi alla nuova farm. Per ulteriori informazioni, vedere "Aggiungere un server federativo a una server farm federativa esistente" in [configurare un Server federativo](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importare i dati di configurazione originali nella farm di Windows Server 2012 R2 AD ADFS  
 Ora che hai una server farm federativa ADFS in esecuzione in Windows Server 2012 R2, è possibile importare i dati di configurazione ADFS originali.  
  
1.  Importare e configurare altri certificati ADFS personalizzati, tra cui esternamente registrati certificati di firma di token e la decrittografia/decrittografia di token e certificato di comunicazione del servizio se diverso dal certificato SSL.  
  
Nella console di gestione di ADFS selezionare **certificati**. Verificare i certificati di crittografia/decrittografia di token e di firma di token le comunicazioni, servizio controllando ognuno in base ai valori esportati nel file certificates.txt durante i preparativi per la migrazione.  
  
Per modificare i certificati di decrittografia di token o firma di token da certificati autofirmati predefiniti a certificati esterni, è innanzitutto necessario disabilitare la funzionalità di attivazione automatica del certificato che è abilitata per impostazione predefinita. A tale scopo, è possibile utilizzare il comando di Windows PowerShell seguente:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  Configurare le impostazioni del servizio ADFS personalizzate, ad esempio AutoCertificateRollover o durata SSO utilizzando il cmdlet Set-AdfsProperties.  
  
3.  Per importare i trust della relying party AD FS e i provider di attestazioni, è necessario essere l'accesso come amministratore (tuttavia, non come Domain Administrator) su federativo server ed eseguire script di Windows PowerShell seguenti che si trova nella cartella \support\adfs del CD di installazione di Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  Lo script di importazione accetta i parametri seguenti:  
>   
>  -  Import-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   Import-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < stringa [] >] [-ClaimsProviderTrustIdentifier < stringa [] >  
> -   Import-federationconfiguration.ps1-Path < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < stringa [] >] [-ClaimsProviderTrustName < stringa [] >]  
>   
>  **-RelyingPartyTrustIdentifier < stringa [] >** - il cmdlet Importa solo le attendibilità del con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita viene importata alcuna attendibilità. Se viene specificata alcuna del RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script importerà tutte relying party e provider di attestazioni.  
>   
>  **-ClaimsProviderTrustIdentifier < stringa [] >** -il cmdlet Importa solo le attendibilità del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita viene importata alcuna attendibilità del provider di attestazioni.  
>   
>  **-RelyingPartyTrustName < stringa [] >** - il cmdlet Importa solo le attendibilità i cui nomi sono specificati nella matrice di stringhe. Per impostazione predefinita viene importata alcuna attendibilità.  
>   
>  **-ClaimsProviderTrustName < stringa [] >** -il cmdlet Importa solo le attendibilità del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita viene importata alcuna attendibilità del provider di attestazioni.  
>   
>  **-Path < string\ >** -il percorso di una cartella che contiene i file di configurazione da importare.  
>   
>  **-LogPath < string\ >** -il percorso di una cartella che conterrà il file di log di importazione. In questa cartella verrà creato un file di log denominato "import.log".  
>   
>  **-ComputerName < string\ >** -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione di ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo parametro deve essere impostato sul nome host del server ADFS legacy.  
>   
>  **-Credenziale < PSCredential\ >**-specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
>   
>  **-Force** – specifica di non richiedere conferma all'utente.  
>   
>  **-CertificatePassword < SecureString\ >** -specifica una password per l'importazione delle chiavi private dei certificati ADFS. Se non specificato, lo script richiederà una password se è necessario importare un certificato ADFS con chiave privata.  
>   
>  **Input:** stringa - questo comando accetta il percorso della cartella di importazione come input. È possibile inviare tramite pipe a Export-FederationConfiguration per questo comando.  
>   
>  **Output:** None.  
  
Eventuali spazi finali nella proprietà WSFedEndpoint di un trust della relying party possono causare lo script di importazione per errore. In questo caso, rimuovere manualmente gli spazi dal file prima dell'importazione. Queste voci, ad esempio, potrebbero causare errori:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  Se hai eventuali (regole diverso le regole predefinite di AD FS) di regole attestazione personalizzate nell'attendibilità provider di attestazioni di Active Directory nel sistema di origine, questi non verranno migrati dagli script. Infatti, Windows Server 2012 R2 esistono nuove impostazioni predefinite. Qualsiasi regola personalizzata deve essere unita aggiungendola manualmente all'attendibilità del provider di attestazioni Active Directory nella nuova farm di Windows Server 2012 R2.  
  
4.  Configurare tutte le impostazioni personalizzate endpoint ADFS. Nella console di gestione di ADFS, selezionare **endpoint**. Controllare l'endpoint ADFS abilitati confrontandoli con l'elenco degli endpoint ADFS abilitati esportato in un file durante la preparazione per la migrazione di ADFS.  
  
     \ - E -  
  
     Configurare eventuali descrizioni di attestazioni personalizzate. Nella console di gestione di ADFS, selezionare **descrizioni di attestazioni**. Controllare l'elenco di descrizioni di attestazioni AD FS in base all'elenco di descrizioni di attestazioni esportato in un file durante la preparazione per la migrazione di ADFS. Aggiungere eventuali descrizioni di attestazioni personalizzate incluse nel file ma non inclusi nell'elenco predefinito di ADFS. Si noti che l'identificatore di attestazione nella console di gestione corrisponde a ClaimType nel file.  
  
5.  Installare e configurare tutti backup personalizzato archivi di attributi. Come amministratore, verificare che eventuali file binari degli archivi attributi personalizzati siano l'aggiornamento a .NET Framework 4.0 o versione successiva prima di aggiornare la configurazione di ADFS per farvi riferimento.  
  
6.  Configurare le proprietà del servizio mappate ai parametri del file Web. config legacy.  
  
    -   Se **useRelayStateForIdpInitiatedSignOn** è stato aggiunto il **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS nella farm di Windows Server 2012 R2:  
  
        -   ADFS in Windows Server 2012 R2 include un **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** file. Creare un elemento con la stessa sintassi di **Web. config** elemento del file: `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Includere l'elemento come parte di **< microsoft.identityserver.web >** sezione il **identityserver** file.  
  
    -   Se **< persistIdentityProviderInformation attivato = "true & #124; false" lifetimeInDays = enablewhrPersistence "90" = "true & #124; false" / \ >** è stato aggiunto al **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS nella farm di Windows Server 2012 R2:  
  
        1.  In ADFS in Windows Server 2012 R2, eseguire il comando di Windows PowerShell seguente: `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
    -   Se **< singleSignOn attivato = "true & #124; false" / \ >** è stato aggiunto al **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, non è necessario impostare alcuna proprietà servizio aggiuntiva in ADFS nella farm di Windows Server 2012 R2. Accesso Single sign-on è abilitato per impostazione predefinita in ADFS nella farm di Windows Server 2012 R2.  
  
    -   Se sono state aggiunte impostazioni localAuthenticationTypes al **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS nella farm di Windows Server 2012 R2:  
  
        -   Integrated, Forms, TlsClient, elenco di trasformazioni Basic nell'equivalente di ADFS in Windows Server 2012 R2 dispone di impostazioni di criteri di autenticazione globali per il supporto sia tipi di autenticazione proxy e del servizio federativo. Queste impostazioni possono essere configurate in AD FS nello snap-in Gestione nel **criteri di autenticazione**.  
  
 Dopo aver importato i dati di configurazione originali, è possibile personalizzare le pagine di accesso AD FS in base alle esigenze. Per ulteriori informazioni, vedere [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [La migrazione di Proxy Server federativo AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)