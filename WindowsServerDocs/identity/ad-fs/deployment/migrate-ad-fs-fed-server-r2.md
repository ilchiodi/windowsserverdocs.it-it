---
title: Eseguire la migrazione del server federativo AD FS 2,0
description: Vengono fornite informazioni sulla migrazione di un server di AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9e947f1894516de232a0db50bcbb56c7452098cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359416"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Eseguire la migrazione del server federativo AD FS 2,0 alla AD FS in Windows Server 2012 R2

Per eseguire la migrazione di un server federativo AD FS che appartiene a una farm di AD FS a nodo singolo, una farm WIF o una farm di SQL Server a Windows Server 2012 R2, è necessario eseguire le attività seguenti:  
  
1.  [Esportare e eseguire il backup dei dati di configurazione AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Creare una Federazione di Windows Server 2012 R2 server farm](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
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
>  Se si prevede di distribuire il servizio Device Registration come parte dell'esecuzione del AD FS in Windows Server 2012 R2, è necessario ottenere un nuovo certificato SSL. Per altre informazioni, vedere [Enroll an SSL Certificate per AD FS](enroll-an-ssl-certificate-for-ad-fs.md) e [configurare un server federativo con Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Per visualizzare i certificati per la firma di token, la decrittografia di token e le comunicazioni di servizi utilizzati, eseguire il comando seguente di Windows PowerShell per creare un elenco di tutti i certificati in uso in un file:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2. Esportare in un file le proprietà del servizio federativo ADFS, come il nome e il nome visualizzato del servizio e l'identificatore del server federativo.  
  
Per esportare le proprietà del servizio federativo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

Il file di output conterrà i valori di configurazione importanti seguenti:  
 
|**Servizio federativo nome della proprietà come riportato da Get-ADFSProperties**|**Nome della proprietà Servizio federativo in AD FS Management Console**|
|-----|-----|  
|HostName|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
|DisplayName|Nome visualizzato del servizio federativo|  
  
3. Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione al database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il file `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Prendere nota della stringa di connessione al database in questo file, indicata subito dopo "policystore connectionstring=". Se la stringa di connessione specifica un database di SQL Server, il valore è necessario per il ripristino della configurazione ADFS originale nel server federativo.  
>   
>  Di seguito viene riportato un esempio di stringa di connessione WID:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Di seguito viene riportato un esempio di stringa di connessione di SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4. Registrare l'identità dell'account del servizio federativo ADFS e la password dell'account.  
  
Per trovare il valore di identità, controllare la colonna **Accedi come** di **AD FS 2.0 Windows Service** nella console **Servizi** e registrare manualmente questo valore.  
  
> [!NOTE]
>  Per un servizio federativo autonomo viene utilizzato l'account predefinito SERVIZIO DI RETE.  In questo caso, non è necessario disporre di una password.  
  
5. Esportare in un file l'elenco degli endpoint ADFS abilitati.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6. Esportare in un file le eventuali descrizioni di attestazioni personalizzate.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7. Se nel file web.config sono configurate impostazioni personalizzate come useRelayStateForIdpInitiatedSignOn, assicurarsi di eseguire il backup del file web.config per riferimento. È possibile copiare il file dalla directory mappata al percorso virtuale **"/adfs/ls"** in IIS. Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Per esportare le attendibilità del provider di attestazioni e del componente  
  
1.  Per esportare AD FS attendibilità del provider di attestazioni e relying party trust, è necessario accedere come amministratore (Tuttavia, non come amministratore di dominio) al server federativo ed eseguire il seguente script di Windows PowerShell che si trova in **media/server_support cartella/ADFS** del CD di installazione di Windows Server 2012 R2: `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  Lo script di esportazione accetta i parametri seguenti:  
> 
> - FederationConfiguration. ps1-PATH < stringa @ no__t-0 [-ComputerName < stringa @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-CertificatePassword < SecureString @ no__t-3]  
>   -   FederationConfiguration. ps1-PATH < String @ no__t-0 [-ComputerName < String @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-CertificatePassword < SecureString @ no__t-3] [-RelyingPartyTrustIdentifier < stringa [] >] [-ClaimsProviderTrustIdentifier < stringa [] >]  
>   -   FederationConfiguration. ps1-PATH < stringa @ no__t-0 [-ComputerName < stringa @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-CertificatePassword < SecureString @ no__t-3] [-RelyingPartyTrustName < stringa [] >] [- ClaimsProviderTrustName < String [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** : il cmdlet esporta solo i trust della relying party con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del componente. Se non si specificano i parametri RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script esporterà tutte le attendibilità del componente e del provider di attestazioni.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** : il cmdlet esporta solo i trust del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del provider di attestazioni.  
> 
>   **-RelyingPartyTrustName <string[]>** : il cmdlet esporta solo i trust della relying party con i nomi specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del componente.  
> 
>   **-ClaimsProviderTrustName <string[]>** : il cmdlet esporta solo i trust del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita non viene esportata alcuna attendibilità del provider di attestazioni.  
> 
>   **-PATH < String\>**  -il percorso di una cartella che conterrà i file esportati.  
> 
>   **-ComputerName < stringa\>**  -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione da ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo è il nome host del server ADFS legacy.  
> 
>   **-Credential < PSCredential\>**  -specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
> 
>   **-Force** – specifica di non richiedere la conferma dell'utente.  
> 
>   **-CertificatePassword < SecureString\>**  -specifica una password per l'esportazione delle chiavi private dei certificati ad FS. Se questo parametro non viene specificato, lo script richiederà una password se è necessario esportare un certificato ADFS con chiave privata.  
> 
>   **Input**: Nessuno  
> 
>   **Output**: stringa - questo cmdlet restituisce il percorso della cartella di esportazione. È possibile reindirizzare l'oggetto restituito a Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Per eseguire il backup degli archivi di attributi personalizzati  
  
1.  È necessario esportare manualmente tutti gli archivi di attributi personalizzati che si desidera memorizzare nella nuova farm AD FS in Windows Server 2012 R2.  
  
> [!NOTE]
>  In Windows Server 2012 R2 AD FS richiede archivi di attributi personalizzati basati su .NET Framework 4,0 o versione successiva. Seguire le istruzioni in [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) per installare e configurare .NET Framework 4.5.  
  
Per trovare informazioni sugli archivi di attributi personalizzati usati da ADFS, è possibile eseguire il comando di Windows PowerShell seguente: 

``` powershell
Get-ADFSAttributeStore
```  

La procedura per aggiornare gli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
2. È inoltre necessario esportare manualmente tutti i file con estensione dll degli archivi di attributi personalizzati che si desidera memorizzare nella nuova farm AD FS in Windows Server 2012 R2. La procedura per aggiornare i file DLL degli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Creare una server farm federativa di Windows Server 2012 R2  
  
1.  Installare il sistema operativo Windows Server 2012 R2 in un computer che si desidera fungere da server federativo e quindi aggiungere il ruolo del server AD FS. Per altre informazioni, vedere [Install the AD FS Role Service](install-the-ad-fs-role-service.md). Configurare quindi il nuovo servizio federativo tramite la Configurazione guidata Active Directory Federation Services o con Windows PowerShell. Per ulteriori informazioni, vedere la sezione dedicata alla configurazione del primo server federativo in una nuova server farm federativa nell'argomento relativo alla [configurazione di un server federativo](configure-a-federation-server.md).  

Durante l'esecuzione di questo passaggio è necessario seguire queste istruzioni:  
  
-   Per configurare il servizio federativo, è necessario disporre di privilegi di Domain Administrator.  
  
-   È necessario utilizzare lo stesso nome di servizio federativo (nome di farm) utilizzato in ADFS 2.0 o ADFS in Windows Server 2012. Se non si utilizza lo stesso nome di servizio federativo, i certificati di cui è stato eseguito il backup non funzioneranno nel servizio federativo di Windows Server 2012 R2 che si sta tentando di configurare.  
  
-   Specificare se si tratta di una server farm federativa WID o SQL Server. Nel caso di una farm SQL Server, specificare il percorso e il nome di istanza del database di SQL Server.  
  
-   È necessario specificare un file pfx contenente il certificato di autenticazione del server SSL di cui è stato eseguito il backup nell'ambito dei preparativi del processo di migrazione di ADFS.  
  
-   È necessario specificare la stessa identità dell'account servizio utilizzata nella farm ADFS 2.0 o ADFS in Windows Server 2012.  
  
2. Dopo aver configurato il nodo iniziale, è possibile aggiungere ulteriori nodi alla nuova farm. Per ulteriori informazioni, vedere la sezione dedicata all'aggiunta di un server federativo a una server farm federativa esistente nell'argomento relativo alla [configurazione di un server federativo](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importare i dati di configurazione originali nella farm ADFS di Windows Server 2012 R2  
 Ora che si dispone di un AD FS server farm federazione in esecuzione in Windows Server 2012 R2, è possibile importare i dati di configurazione originali del AD FS al suo interno.  
  
1.  Importare e configurare altri certificati ADFS personalizzati, inclusi i certificati registrati esternamente per la firma di token e la crittografia/decrittografia di token, nonché il certificato per le comunicazioni di servizi se è diverso dal certificato SSL.  
  
Nella console di gestione ADFS selezionare **Certificati**. Verificare i certificati per le comunicazioni di servizi, di crittografia/decrittografia di token e di firma di token, controllando ognuno in base ai valori esportati nel file certificates.txt durante i preparativi per la migrazione.  
  
Per cambiare i certificati di decrittografia di token o di firma di token da certificati autofirmati predefiniti a certificati esterni, è innanzitutto necessario disabilitare la funzionalità di rollover automatico dei certificati, abilitata per impostazione predefinita. A tale scopo, è possibile utilizzare il comando seguente di Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2. Utilizzare il cmdlet Set-AdfsProperties per configurare eventuali impostazioni personalizzate del servizio ADFS, come AutoCertificateRollover o la durata SSO.  
  
3. Per importare AD FS trust di relying party e dei trust del provider di attestazioni, è necessario accedere come amministratore (Tuttavia, non come amministratore di dominio) nel server federativo ed eseguire il seguente script di Windows PowerShell che si trova nella cartella \support\adfs del CD di installazione di Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  Lo script di importazione accetta i parametri seguenti:  
> 
> - FederationConfiguration. ps1-PATH < stringa @ no__t-0 [-ComputerName < stringa @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-LogPath < stringa @ no__t-3] [-CertificatePassword < SecureString @ no__t-4]  
>   -   FederationConfiguration. ps1-PATH < stringa @ no__t-0 [-ComputerName < stringa @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-LogPath < stringa @ no__t-3] [-CertificatePassword < SecureString @ no__t-4] [- RelyingPartyTrustIdentifier < String [] >] [-ClaimsProviderTrustIdentifier < String [] >  
>   -   FederationConfiguration. ps1-PATH < stringa @ no__t-0 [-ComputerName < stringa @ no__t-1] [-Credential < PSCredential @ no__t-2] [-Force] [-LogPath < stringa @ no__t-3] [-CertificatePassword < SecureString @ no__t-4] [- RelyingPartyTrustName < String [] >] [-ClaimsProviderTrustName < String [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** - the cmdlet only imports relying party trusts whose identifiers are specified in the string array. Per impostazione predefinita NON viene importata alcuna attendibilità del componente. Se non si specificano i parametri RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName, lo script importerà tutte le attendibilità del componente e del provider di attestazioni.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** : il cmdlet importa solo i trust del provider di attestazioni con gli identificatori specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del provider di attestazioni.  
> 
>   **-RelyingPartyTrustName <string[]>** : il cmdlet importa solo i trust della relying party con i nomi specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del componente.  
> 
>   **-ClaimsProviderTrustName <string[]>** : il cmdlet importa solo i trust del provider di attestazioni con i nomi specificati nella matrice di stringhe. Per impostazione predefinita NON viene importata alcuna attendibilità del provider di attestazioni.  
> 
>   **-PATH < String\>**  -il percorso di una cartella che contiene i file di configurazione da importare.  
> 
>   **-LogPath < stringa\>**  : il percorso di una cartella che conterrà il file di log di importazione. In questa cartella verrà creato un file di log denominato "import.log".  
> 
>   **-ComputerName < stringa\>**  -specifica il nome host del server STS. Il valore predefinito è il computer locale. Se si esegue la migrazione da ADFS 2.0 o ADFS in Windows Server 2012 ad ADFS in Windows Server 2012 R2, questo parametro deve essere impostato sul nome host del server ADFS legacy.  
> 
>   **-Credential < PSCredential\>** -specifica un account utente che dispone dell'autorizzazione per eseguire questa azione. Il valore predefinito è l'utente corrente.  
> 
>   **-Force** – specifica di non richiedere la conferma dell'utente.  
> 
>   **-CertificatePassword < SecureString\>**  -specifica una password per l'importazione delle chiavi private dei certificati di ad FS. Se questo parametro non viene specificato, lo script richiederà una password se è necessario importare un certificato ADFS con chiave privata.  
> 
>   **Input:** stringa - questo comando accetta il percorso della cartella di importazione come input. È possibile reindirizzare Export-FederationConfiguration su questo comando.  
> 
>   **Output:** No.  
  
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
>  Se esistono regole attestazione personalizzate, diverse da quelle predefinite di ADFS, nell'attendibilità del provider di attestazioni di Active Directory nel sistema di origine, non verranno incluse nella migrazione dagli script. Questo perché Windows Server 2012 R2 ha nuove impostazioni predefinite. Ogni regola personalizzata deve essere unita aggiungendola manualmente all'attendibilità del provider di attestazioni Active Directory nella nuova farm di Windows Server 2012 R2.  
  
4. Configurare tutte le impostazioni personalizzate per gli endpoint ADFS. Nella console di gestione ADFS selezionare **Endpoint**. Controllare gli endpoint ADFS abilitati confrontandoli con l'elenco degli endpoint ADFS abilitati esportato su file durante i preparativi della migrazione di ADFS.  
  
    \-E  
  
    Configurare eventuali descrizioni di attestazioni personalizzate. Nella console di gestione ADFS selezionare **Descrizioni attestazione**. Controllare l'elenco delle descrizioni di attestazioni AD FS confrontandole con l'elenco delle descrizioni esportato su file durante i preparativi della migrazione di AD FS. Aggiungere le eventuali descrizioni di attestazioni personalizzate incluse nel file, ma non nell'elenco predefinito in ADFS. Si noti che l'identificatore di attestazione nella console di gestione corrisponde a ClaimType nel file.  
  
5. Installare e configurare tutti gli archivi di attributi personalizzati di cui è stato eseguito il backup. In qualità di amministratore, assicurarsi che gli eventuali file binari degli archivi di attributi personalizzati siano aggiornati a .NET Framework 4.0 o versione successiva prima di aggiornare la configurazione di ADFS in modo che punti a tale file.  
  
6. Configurare le proprietà del servizio mappate ai parametri del file web.config legacy.  
  
   -   Se **useRelayStateForIdpInitiatedSignOn** è stato aggiunto al file **Web. config** nella ad FS 2,0 o ad FS nella farm di Windows Server 2012, è necessario configurare le proprietà del servizio seguenti nel ad FS della farm di Windows Server 2012 R2:  
  
       -   AD FS in Windows Server 2012 R2 include un file **%systemroot%\ADFS\Microsoft.IdentityServer.ServiceHost.exe.config** . Creare un elemento con la stessa sintassi dell'elemento del file **Web. config** : `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Includere questo elemento come parte della sezione **< Microsoft. IdentityServer. web >** del file **Microsoft. IdentityServer. ServiceHost. exe. config** .  
  
   -   Se **< persistIdentityProviderInformation Enabled = "true&#124;false" lifetimeInDays = "90" enablewhrPersistence = "true&#124;false"/\>**  è stato aggiunto al file **Web. config** nel ad FS 2,0 o ad FS in Windows Server 2012 Farm, è necessario configurare le proprietà del servizio seguenti nella AD FS della farm di Windows Server 2012 R2:  
  
       1.  In AD FS in Windows Server 2012 R2, eseguire il comando di Windows PowerShell seguente `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`:.  
  
   -   Se **< singleSignOn Enabled = "true&#124;false"/\>**  è stato aggiunto al file **Web. config** nella ad FS 2,0 o ad FS nella farm di Windows Server 2012, non è necessario impostare alcuna proprietà del servizio aggiuntiva nel ad FS in Windows Server 2012 Farm R2. Single Sign-on è abilitato per impostazione predefinita nella farm di Windows Server 2012 R2 AD FS.  
  
   -   Se le impostazioni di Localauthenticationtypes al sono state aggiunte al file **Web. config** nella ad FS 2,0 o ad FS nella farm di windows Server 2012, è necessario configurare le proprietà del servizio seguenti nel ad FS della farm di windows Server 2012 R2:  
  
       -   Integrated, Forms, TlsClient, Basic Transform list in AD FS equivalenti in Windows Server 2012 R2 con impostazioni di criteri di autenticazione globali per supportare sia il servizio federativo che i tipi di autenticazione proxy. Queste impostazioni possono essere configurate nello snap-in Gestione ADFS in **Criteri di autenticazione**.  
  
   Dopo aver importato i dati di configurazione originali, è possibile personalizzare le pagine di accesso ad ADFS in base alle specifiche esigenze. Per altre informazioni, vedere [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione dei servizi ruolo Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrazione del proxy server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione del AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)