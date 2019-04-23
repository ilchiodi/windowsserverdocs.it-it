---
title: Eseguire la migrazione di AD server federativo ADFS 2.0
description: Vengono fornite informazioni sulla migrazione di un server AD FS per Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6bc680d9a0de8946d6f39a5529a297138ee5e262
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876062"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Eseguire la migrazione di server AD FS 2.0 federation di AD FS in Windows Server 2012 R2

Per eseguire la migrazione di un server federativo ADFS che appartiene a una farm ADFS a nodo singolo, una farm WIF o una farm di SQL Server a Windows Server 2012 R2, è necessario eseguire le attività seguenti:  
  
1.  [Esportare ed eseguire il backup dei dati di configurazione di ADFS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Creare una server farm federativa di Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importare i dati di configurazione originali nella farm AD FS di Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Esportare i dati di configurazione di ADFS ed eseguirne il backup  
 Per esportare le impostazioni di configurazione di ADFS, eseguire le procedure seguenti:  
  
###  <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Assicurarsi di avere accesso ai certificati seguenti e alle relative chiavi private in un file con estensione pfx:  
  
    -   Il certificato SSL utilizzato dalla server farm federativa di cui si desidera eseguire la migrazione  
  
    -   Il certificato per le comunicazioni di servizi (se diverso dal certificato SSL) utilizzato dalla server farm federativa di cui si desidera eseguire la migrazione  
  
    -   Tutti i certificati di terze parti per la firma di token o la crittografia/decrittografia di token utilizzati dalla server farm federativa di cui si desidera eseguire la migrazione  
  
Per trovare il certificato SSL, aprire la console di gestione di Internet Information Services (IIS), selezionare **Sito Web predefinito** nel riquadro sinistro, fare clic su **Binding** nel riquadro **Azione** , trovare e selezionare il binding https, fare clic su **Modifica**e quindi su **Visualizza**.  
  
È necessario esportare il certificato SSL utilizzato dal servizio federativo e la relativa chiave privata in un file con estensione pfx. Per altre informazioni, vedere [Esportare la parte di chiave privata di un certificato di autenticazione server](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Se si prevede di distribuire Device Registration Service nell'ambito di ADFS in Windows Server 2012 R2, è necessario ottenere un nuovo certificato SSL. Per altre informazioni, consultare [Enroll an SSL Certificate for AD FS](enroll-an-ssl-certificate-for-ad-fs.md) e [Configure a federation server with Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Per visualizzare i certificati per la firma di token, la decrittografia di token e le comunicazioni di servizi utilizzati, eseguire il comando seguente di Windows PowerShell per creare un elenco di tutti i certificati in uso in un file:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  Esportare in un file le proprietà del servizio federativo ADFS, come il nome e il nome visualizzato del servizio e l'identificatore del server federativo.  
  
Per esportare le proprietà del servizio federativo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

Il file di output conterrà i valori di configurazione importanti seguenti:  
 
|**Nome della proprietà servizio federativo come riportato da Get-ADFSProperties**|**Nome della proprietà servizio federativo nella console di gestione di AD FS**|
|-----|-----|  
|HostName|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
|DisplayName|Nome visualizzato del servizio federativo|  
  
3.  Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione al database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il file `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Prendere nota della stringa di connessione al database in questo file, indicata subito dopo "policystore connectionstring=". Se la stringa di connessione specifica un database di SQL Server, il valore è necessario per il ripristino della configurazione ADFS originale nel server federativo.  
>   
>  Di seguito viene riportato un esempio di stringa di connessione WID:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Di seguito viene riportato un esempio di stringa di connessione di SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4.  Registrare l'identità dell'account del servizio federativo ADFS e la password dell'account.  
  
Per trovare il valore di identità, controllare la colonna **Accedi come** di **AD FS 2.0 Windows Service** nella console **Servizi** e registrare manualmente questo valore.  
  
> [!NOTE]
>  Per un servizio federativo autonomo viene utilizzato l'account predefinito SERVIZIO DI RETE.  In questo caso, non è necessario disporre di una password.  
  
5.  Esportare in un file l'elenco degli endpoint ADFS abilitati.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  Esportare in un file le eventuali descrizioni di attestazioni personalizzate.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  Se nel file web.config sono configurate impostazioni personalizzate come useRelayStateForIdpInitiatedSignOn, assicurarsi di eseguire il backup del file web.config per riferimento. È possibile copiare il file dalla directory mappata al percorso virtuale **"/adfs/ls"** in IIS. Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Per esportare le attendibilità del provider di attestazioni e del componente  
  
1.  Per esportare AD FS provider di attestazioni e attendibilità del componente, è necessario accedere come amministratore (tuttavia, non come Domain Administrator) in federativo server ed eseguire questo comando di Windows PowerShell script disponibile nella **supporti / server_support/ad FS** cartella del CD di installazione di Windows Server 2012 R2: `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  Lo script di esportazione accetta i parametri seguenti:  
>   
>  -   Export-FederationConfiguration.ps1 -Path <string\> [-ComputerName <string\>] [-Credential <pscredential\>] [-Force] [-CertificatePassword <securestring\>]  
> -   Export-FederationConfiguration.ps1-Path < stringa\> [-ComputerName < string\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >]  
> -   Export-FederationConfiguration.ps1-Path < stringa\> [-ComputerName < string\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
>   
>  **-RelyingPartyTrustIdentifier <string[]>**: il cmdlet esporta solo i trust della relying party con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del componente. Se non si specificano i parametri RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script esporterà tutte le attendibilità del componente e del provider di attestazioni.  
>   
>  **-ClaimsProviderTrustIdentifier <string[]>**: il cmdlet esporta solo i trust del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del provider di attestazioni.  
>   
>  **-RelyingPartyTrustName <string[]>**: il cmdlet esporta solo i trust della relying party con i nomi specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del componente.  
>   
>  **-ClaimsProviderTrustName <string[]>**: il cmdlet esporta solo i trust del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del provider di attestazioni.  
>   
>  **-Path < stringa\>**  -il percorso di una cartella che conterrà i file esportati.  
>   
>  **-ComputerName < string\>**  -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione da ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo è il nome host del server ADFS legacy.  
>   
>  **-Credenziale < PSCredential\>**  -specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
>   
>  **-Force** – specifica di non richiedere la conferma dell'utente.  
>   
>  **-CertificatePassword < SecureString\>**  -specifica una password per l'esportazione chiavi private dei certificati ADFS. Se questo parametro non viene specificato, lo script richiederà una password se è necessario esportare un certificato ADFS con chiave privata.  
>   
>  **Input**: Nessuno  
>   
>  **Output**: stringa - questo cmdlet restituisce il percorso della cartella di esportazione. È possibile reindirizzare l'oggetto restituito a Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Per eseguire il backup degli archivi di attributi personalizzati  
  
1.  È necessario esportare manualmente tutti gli archivi di attributi personalizzati che si desidera mantenere nella nuova farm ADFS in Windows Server 2012 R2.  
  
> [!NOTE]
>  In Windows Server 2012 R2, ADFS richiede che gli archivi di attributi personalizzati che si basano su .NET Framework 4.0 o versione successiva. Seguire le istruzioni in [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) per installare e configurare .NET Framework 4.5.  
  
Per trovare informazioni sugli archivi di attributi personalizzati usati da ADFS, è possibile eseguire il comando di Windows PowerShell seguente: 

``` powershell
Get-ADFSAttributeStore
```  

La procedura per aggiornare gli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
2.  È inoltre necessario esportare manualmente tutti i file DLL degli archivi attributi personalizzati che si desidera mantenere nella nuova farm ADFS in Windows Server 2012 R2. La procedura per aggiornare i file DLL degli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Creare una server farm federativa di Windows Server 2012 R2  
  
1.  Installare il sistema operativo Windows Server 2012 R2 in un computer che si desidera operare come un server federativo e quindi aggiungere il ruolo server AD FS. Per altre informazioni, vedere [Install the AD FS Role Service](install-the-ad-fs-role-service.md). Configurare quindi il nuovo servizio federativo tramite la Configurazione guidata Active Directory Federation Services o con Windows PowerShell. Per ulteriori informazioni, vedere la sezione dedicata alla configurazione del primo server federativo in una nuova server farm federativa nell'argomento relativo alla [configurazione di un server federativo](configure-a-federation-server.md).  

Durante l'esecuzione di questo passaggio è necessario seguire queste istruzioni:  
  
-   Per configurare il servizio federativo, è necessario disporre di privilegi di Domain Administrator.  
  
-   È necessario utilizzare lo stesso nome di servizio federativo (nome di farm) utilizzato in ADFS 2.0 o ADFS in Windows Server 2012. Se non si utilizza lo stesso nome servizio federativo, i certificati che è stato eseguito il backup nel servizio federativo di Windows Server 2012 R2 che si sta provando a configurare non funzionerà.  
  
-   Specificare se si tratta di una server farm federativa WID o SQL Server. Nel caso di una farm SQL Server, specificare il percorso e il nome di istanza del database di SQL Server.  
  
-   È necessario specificare un file pfx contenente il certificato di autenticazione del server SSL di cui è stato eseguito il backup nell'ambito dei preparativi del processo di migrazione di ADFS.  
  
-   È necessario specificare la stessa identità dell'account servizio utilizzata nella farm ADFS 2.0 o ADFS in Windows Server 2012.  
  
2.  Dopo aver configurato il nodo iniziale, è possibile aggiungere ulteriori nodi alla nuova farm. Per ulteriori informazioni, vedere la sezione dedicata all'aggiunta di un server federativo a una server farm federativa esistente nell'argomento relativo alla [configurazione di un server federativo](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importare i dati di configurazione originali nella farm ADFS di Windows Server 2012 R2  
 Ora che avete una server farm federativa ADFS in esecuzione in Windows Server 2012 R2, è possibile importare i dati di configurazione ADFS originali al suo interno.  
  
1.  Importare e configurare altri certificati ADFS personalizzati, inclusi i certificati registrati esternamente per la firma di token e la crittografia/decrittografia di token, nonché il certificato per le comunicazioni di servizi se è diverso dal certificato SSL.  
  
Nella console di gestione ADFS selezionare **Certificati**. Verificare i certificati per le comunicazioni di servizi, di crittografia/decrittografia di token e di firma di token, controllando ognuno in base ai valori esportati nel file certificates.txt durante i preparativi per la migrazione.  
  
Per cambiare i certificati di decrittografia di token o di firma di token da certificati autofirmati predefiniti a certificati esterni, è innanzitutto necessario disabilitare la funzionalità di rollover automatico dei certificati, abilitata per impostazione predefinita. A tale scopo, è possibile utilizzare il comando seguente di Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  Utilizzare il cmdlet Set-AdfsProperties per configurare eventuali impostazioni personalizzate del servizio ADFS, come AutoCertificateRollover o la durata SSO.  
  
3.  Per importare i trust della relying party in ADFS e provider di attestazioni, è necessario accedere come amministratore (tuttavia, non come Domain Administrator) in federativo server ed eseguire questo comando di Windows PowerShell script disponibile nella cartella \support\adfs del CD di installazione di Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  Lo script di importazione accetta i parametri seguenti:  
>   
>  -  Import-FederationConfiguration.ps1 -Path <string\> [-ComputerName <string\>] [-Credential <pscredential\>] [-Force] [-LogPath <string\>] [-CertificatePassword <securestring\>]  
> -   Import-FederationConfiguration.ps1-Path < stringa\> [-ComputerName < stringa\>] [-Credential < pscredential\>] [-Force] [-LogPath < stringa\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >  
> -   Import-FederationConfiguration.ps1-Path < stringa\> [-ComputerName < stringa\>] [-Credential < pscredential\>] [-Force] [-LogPath < stringa\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
>   
>  **-RelyingPartyTrustIdentifier <string[]>** - the cmdlet only imports relying party trusts whose identifiers are specified in the string array. Per impostazione predefinita NON viene importata alcuna attendibilità del componente. Se non si specificano i parametri RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script importerà tutte le attendibilità del componente e del provider di attestazioni.  
>   
>  **-ClaimsProviderTrustIdentifier <string[]>**: il cmdlet importa solo i trust del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del provider di attestazioni.  
>   
>  **-RelyingPartyTrustName <string[]>**: il cmdlet importa solo i trust della relying party con i nomi specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del componente.  
>   
>  **-ClaimsProviderTrustName <string[]>**: il cmdlet importa solo i trust del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del provider di attestazioni.  
>   
>  **-Path < stringa\>**  -il percorso di una cartella che contiene i file di configurazione da importare.  
>   
>  **-LogPath < stringa\>**  -il percorso di una cartella che conterrà il file di log di importazione. In questa cartella verrà creato un file di log denominato "import.log".  
>   
>  **-ComputerName < string\>**  -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione da ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo parametro deve essere impostato sul nome host del server ADFS legacy.  
>   
>  **-Credenziale < PSCredential\>**-specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
>   
>  **-Force** – specifica di non richiedere la conferma dell'utente.  
>   
>  **-CertificatePassword < SecureString\>**  -specifica una password per l'importazione di chiavi private dei certificati ADFS. Se questo parametro non viene specificato, lo script richiederà una password se è necessario importare un certificato ADFS con chiave privata.  
>   
>  **Input:** stringa - questo comando accetta il percorso della cartella di importazione come input. È possibile reindirizzare Export-FederationConfiguration su questo comando.  
>   
>  **Output:** Nessuno.  
  
Eventuali spazi finali nella proprietà WSFedEndpoint di un'attendibilità del componente potrebbero causare errori per lo script di importazione. In questo caso, rimuovere manualmente gli spazi dal file prima dell'importazione. Queste voci, ad esempio, potrebbero causare errori:  
  
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
>  Se esistono regole attestazione personalizzate, diverse da quelle predefinite di ADFS, nell'attendibilità del provider di attestazioni di Active Directory nel sistema di origine, non verranno incluse nella migrazione dagli script. Questo avviene perché Windows Server 2012 R2 esistono nuove impostazioni predefinite. Qualsiasi regola personalizzata deve essere unita aggiungendola manualmente all'attendibilità del provider di attestazioni Active Directory nella nuova farm di Windows Server 2012 R2.  
  
4.  Configurare tutte le impostazioni personalizzate per gli endpoint ADFS. Nella console di gestione ADFS selezionare **Endpoint**. Controllare gli endpoint ADFS abilitati confrontandoli con l'elenco degli endpoint ADFS abilitati esportato su file durante i preparativi della migrazione di ADFS.  
  
     \- e -  
  
     Configurare eventuali descrizioni di attestazioni personalizzate. Nella console di gestione ADFS selezionare **Descrizioni attestazione**. Controllare l'elenco delle descrizioni di attestazioni AD FS confrontandole con l'elenco delle descrizioni esportato su file durante i preparativi della migrazione di AD FS. Aggiungere le eventuali descrizioni di attestazioni personalizzate incluse nel file, ma non nell'elenco predefinito in ADFS. Si noti che l'identificatore di attestazione nella console di gestione corrisponde a ClaimType nel file.  
  
5.  Installare e configurare tutti gli archivi di attributi personalizzati di cui è stato eseguito il backup. In qualità di amministratore, assicurarsi che gli eventuali file binari degli archivi di attributi personalizzati siano aggiornati a .NET Framework 4.0 o versione successiva prima di aggiornare la configurazione di ADFS in modo che punti a tale file.  
  
6.  Configurare le proprietà del servizio mappate ai parametri del file web.config legacy.  
  
    -   Se **useRelayStateForIdpInitiatedSignOn** è stato aggiunto per il **Web. config** file nell'istanza di AD FS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS in Farm di Windows Server 2012 R2:  
  
        -   AD FS in Windows Server 2012 R2 include un' **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** file. Creare un elemento con la stessa sintassi di **Web. config** elemento del file: `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Includere questo elemento come parte del **< microsoft.identityserver.web >** sezione il **Microsoft.IdentityServer.Servicehost.exe.config** file.  
  
    -   Se **< persistIdentityProviderInformation abilitata = "true&#124;false" lifetimeInDays = enablewhrPersistence "90" = "true&#124;false" /\>**  è stato aggiunto per il **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS nella farm di Windows Server 2012 R2:  
  
        1.  In ADFS in Windows Server 2012 R2, eseguire il comando seguente di Windows PowerShell: `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
    -   Se **< singleSignOn abilitata = "true&#124;false" /\>**  è stato aggiunto per il **Web. config** file in ADFS 2.0 o ADFS in farm di Windows Server 2012, non è necessaria impostare eventuali servizi aggiuntivi proprietà in ADFS nella farm di Windows Server 2012 R2. Accesso Single sign-on è abilitata per impostazione predefinita in ADFS nella farm di Windows Server 2012 R2.  
  
    -   Se sono state aggiunte impostazioni localAuthenticationTypes al **Web. config** file nell'istanza di AD FS 2.0 o ADFS in farm di Windows Server 2012, sarà necessario configurare le proprietà del servizio seguenti in ADFS nella farm di Windows Server 2012 R2:  
  
        -   Integrated, Forms, TlsClient, Basic elenco nell'equivalente di ADFS in Windows Server 2012 R2 dispone di impostazioni di criteri di autenticazione globali per il supporto sia tipi di autenticazione proxy e del servizio federativo. Queste impostazioni possono essere configurate nello snap-in Gestione ADFS in **Criteri di autenticazione**.  
  
 Dopo aver importato i dati di configurazione originali, è possibile personalizzare le pagine di accesso ad ADFS in base alle specifiche esigenze. Per altre informazioni, vedere [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [Eseguire la migrazione del Proxy Server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)