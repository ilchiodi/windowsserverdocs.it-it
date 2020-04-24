---
title: IIS in Nano Server
description: Dettagli per la configurazione di IIS in Nano Server
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: d2522dc94c0d3b68c75e14fec19466529256aad0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826864"
---
# <a name="iis-on-nano-server"></a>IIS in Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

È possibile installare il ruolo server IIS (Internet Information Services) in Nano Server usando il parametro -Package con Microsoft-NanoServer-IIS-Package. Per informazioni sulla configurazione di Nano Server, inclusa l'installazione dei pacchetti, vedere [Installare Nano Server](Getting-Started-with-Nano-Server.md).  

In questa versione di Nano Server sono disponibili le funzionalità IIS seguenti:  

|Funzionalità|Abilitato per impostazione predefinita|  
|-----------|----------------------|  
|**Funzionalità HTTP comuni**||  
|Documento predefinito|x|  
|Esplorazione directory|x|  
|Errori HTTP|x|  
|Contenuto statico|x|  
|Reindirizzamento HTTP||  
|**Integrità e diagnostica**||  
|Registrazione HTTP|x|  
|Registrazioni personalizzate||  
|Monitor richieste||  
|Traccia||  
|**Prestazioni**||  
|Compressione contenuto statico|x|  
|Compressione contenuto dinamico||  
|**Sicurezza**||  
|Filtro richieste|x|  
|Autenticazione di base||  
|Autenticazione mapping certificati client||  
|Autenticazione del digest||  
|Autenticazione mapping certificati client IIS||  
|Restrizioni indirizzi IP e di dominio||  
|Autorizzazione URL||  
|Autenticazione di Windows||  
|**Sviluppo di applicazioni**||  
|Inizializzazione dell'applicazione||  
|CGI||  
|Estensioni ISAPI||  
|Filtri ISAPI||  
|Server-Side Include||  
|Protocollo WebSocket||  
|**Strumenti di gestione**||  
|Modulo di amministrazione IIS per Windows PowerShell|x|  

Una serie di articoli su altre configurazioni di IIS (ad esempio mediante l'uso di ASP.NET, PHP e Java), insieme ad altri contenuti correlati, sono pubblicati all'indirizzo [http://iis.net/learn](https://iis.net/learn).  

## <a name="installing-iis-on-nano-server"></a>Installazione di IIS in Nano Server  
È possibile installare questo ruolo server sia offline, con Nano Server disattivato, sia online, con Nano Server in esecuzione. L'installazione offline è l'opzione consigliata.  

Per l'installazione offline, aggiungere il pacchetto con il parametro -Package di New-NanoServerImage, come mostrato nell'esempio seguente:  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

Se si dispone di un file VHD esistente, è possibile installare IIS offline con DISM.exe montando il disco rigido virtuale e quindi usando l'opzione **Aggiungi pacchetto**.   
Nei passaggi dell'esempio seguente si presuppone che l'esecuzione avvenga dalla directory specificata dall'opzione BasePath, percorso creato dopo l'esecuzione di New-NanoServerImage.  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\it-it\Microsoft-NanoServer-IIS-Package_it-it.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir


> [!NOTE]  
> Si noti che al passaggio 4 viene aggiunto il Language Pack, in questo caso IT-IT.  

A questo punto è possibile avviare Nano Server con IIS.  

### <a name="installing-iis-on-nano-server-online"></a>Installazione di IIS in Nano Server online  
Anche se l'installazione offline di questo ruolo server è l'opzione consigliata, può essere necessario installare IIS online, con Nano Server in esecuzione, in scenari di contenitore. A tale scopo, effettuare le operazioni seguenti:  

1.  Copiare la cartella Packages dai supporti di installazione nell'istanza di Nano Server in esecuzione (ad esempio in C:\packages).  

2.  Creare un nuovo file Unattend.xml su un altro computer e copiarlo quindi in Nano Server. È possibile copiare e incollare il contenuto XML nel file XML creato:  



```  

    <unattend xmlns=urn:schemas-microsoft-com:unattend>  
    <servicing>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral />  
            <source location=c:\packages\Microsoft-NanoServer-IIS-Package.cab />  
        </package>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=en-US />  
            <source location=c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source= xmlns:cpi=urn:schemas-microsoft-com:cpi />  
</unattend>  
```  




3. Nel nuovo file XML creato (o copiato), sostituire C:\packages con la directory in cui è stato copiato il contenuto dei pacchetti.  

4. Passare alla directory contenente il file XML appena creato ed eseguire quanto segue:  

   **dism /online /apply-unattend:.\unattend.xml**  


5. Confermare che il pacchetto IIS e il Language Pack associato siano installati correttamente eseguendo quanto segue:  

   **dism /online /get-packages**  

   Package Identity: Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~~10.0.14393.1000 sarà elencato due volte, una per Release Type: Language Pack e una per Release Type: Feature Pack.  

6. Avviare il servizio W3SVC con **net start w3svc** oppure riavviando Nano Server.  

## <a name="starting-iis"></a>Avvio di IIS  
Una volta installato e in esecuzione, IIS è pronto per l'elaborazione delle richieste Web. Verificare che IIS sia in esecuzione esaminando la pagina Web IIS predefinita http://\<indirizzo IP di Nano Server>. In un computer fisico è possibile determinare l'indirizzo IP usando la Console di ripristino di emergenza. In una macchina virtuale è possibile ottenere l'indirizzo IP usando un prompt di Windows PowerShell ed eseguendo quanto segue:  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

Se non si è in grado di accedere alla pagina Web IIS predefinita, verificare l'installazione di IIS cercando la directory **c:\inetpub** in Nano Server.  

## <a name="enabling-and-disabling-iis-features"></a>Abilitazione e disabilitazione delle funzionalità IIS  
Alcune funzionalità IIS sono abilitate per impostazione predefinita quando installi il ruolo IIS (vedi la tabella riportata nella sezione relativa alla panoramica di IIS in Nano Server in questo argomento). È possibile abilitare o disabilitare funzionalità aggiuntive usando DISM.exe.

Ciascuna funzionalità di IIS è costituita da un set di elementi di configurazione. La funzionalità Autenticazione di Windows, ad esempio, è composta dai tre elementi seguenti:  

|Sezione|Elementi di configurazione|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><providers><add value=Negotiate /><add value=NTLM /><br /></providers><br /></windowsAuthentication>`|  

Il set completo di funzionalità secondarie di IIS è incluso nell'Appendice 1 di questo argomento, mentre gli elementi di configurazione corrispondenti sono riportati nell'Appendice 2 di questo argomento.  


### <a name="example-installing-windows-authentication"></a>Esempio: installazione dell'autenticazione di Windows  

1.  Aprire una console di sessione remota di Windows PowerShell in Nano Server.  

2.  Usare `DISM.exe` per installare il modulo di autenticazione di Windows:

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    Il commutatore `/all` installerà qualsiasi funzionalità da cui dipende la funzionalità scelta.

### <a name="example-uninstalling-windows-authentication"></a>Esempio: disinstallazione dell'autenticazione di Windows  

1.  Aprire una console di sessione remota di Windows PowerShell in Nano Server.  

2.  Usare `DISM.exe` per disinstallare il modulo di autenticazione di Windows:

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>Altre attività di configurazione di IIS comuni  
**Creazione di siti Web**  

Usare il cmdlet seguente:  

`PS D:\> New-IISSite -Name TestSite -BindingInformation *:80:TestSite -PhysicalPath c:\test`  

Sarà quindi possibile eseguire `Get-IISSite` per verificare lo stato del sito. Vengono restituiti il nome, l'ID, lo stato, il percorso fisico e le associazioni del sito Web.  

**Eliminazione di siti Web**  

Eseguire `Remove-IISSite -Name TestSite -Confirm:$false`.  

**Creazione di directory virtuali**  

È possibile creare directory virtuali usando l'oggetto IISServerManager restituito da Get-IISServerManager, che espone l'API .NET Microsoft.Web.Administration.ServerManager. In questo esempio questi comandi consentono l'accesso all'elemento Default Web Site della raccolta Sites e all'elemento radice dell'applicazione (/) della sezione Applications. Chiamano quindi il metodo Add() della raccolta VirtualDirectories per tale elemento dell'applicazione per creare la nuova directory:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir1, c:\test\virtualDirectory1)  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir2, c:\test\virtualDirectory2)  
PS C:\> $sm.CommitChanges()  
```  

**Creazione di pool di applicazioni**  

Analogamente alla procedura sopra riportata, è possibile usare Get-IISServerManager per creare pool di applicazioni:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add(DemoAppPool)  
```  

**Configurazione di HTTPS e certificati**  

Usare l'utilità Certoc.exe per importare certificati come nell'esempio seguente, dove viene mostrata la configurazione di HTTPS per un sito Web in Nano Server:  

1.  In un altro computer che non esegue Nano Server creare un certificato (usando i propri nome di certificato e password) e quindi esportarlo in c:\temp\test.pfx.  

    `$newCert = New-SelfSignedCertificate -DnsName www.foo.bar.com -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String YOUR_PFX_PASSWD -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  Copiare il file test.pfx file nel computer Nano Server.  

3.  In Nano Server importa il certificato nell'archivio personale (My) usando il comando seguente:  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  Recuperare l'identificazione personale del nuovo certificato (in questo esempio 61E71251294B2A7BB8259C2AC5CF7BA622777E73) con `Get-ChildItem Cert:\LocalMachine\my`.  

5.  Aggiungere il binding HTTPS al sito Web predefinito (o a qualsiasi altro sito Web a cui si vuole aggiungere il binding) usando i comandi di Windows PowerShell seguenti:  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites[Default Web Site].Bindings.Add(*:443:, $hash, My, 0)    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    Puoi usare anche l'indicazione del nome del server con un nome host specifico tramite la sintassi seguente: `$sm.Sites[Default Web Site].Bindings.Add(*:443:www.foo.bar.com, $hash, My, Sni.`  

## <a name="appendix-1-list-of-iis-sub-features"></a>Appendice 1: Elenco di funzionalità secondarie di IIS

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-DigestAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>Appendice 2: Elementi delle funzionalità HTTP  
Ciascuna funzionalità di IIS è costituita da un set di elementi di configurazione. Questa appendice elenca gli elementi di configurazione di tutte le funzionalità di questa versione di Nano Server  

### <a name="common-http-features"></a>Funzionalità HTTP comuni  
**Documento predefinito**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DefaultDocumentModule image=%windir%\System32\inetsrv\defdoc.dll />`|  
|`<modules>`|`<add name=DefaultDocumentModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DefaultDocumentModule resourceType=EiSecther requireAccess=Read />`|  
|`<defaultDocument>`|`<defaultDocument enabled=true><br /><files><br /> <add value=Default.htm /><br />        <add value=Default.asp /><br />        <add value=index.htm /><br />        <add value=index.html /><br />        <add value=iisstart.htm /><br />    </files><br /></defaultDocument>`|  

È possibile che la voce `StaticFile <handlers>` sia già presente. In questo caso, aggiungi semplicemente DefaultDocumentModule all'attributo \<modules> usando la virgola come separatore.  

**Esplorazione directory**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=DirectoryListingModule image=%windir%\System32\inetsrv\dirlist.dll />`|  
|`<modules>`|`<add name=DirectoryListingModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DirectoryListingModule resourceType=Either requireAccess=Read />`|  

È possibile che la voce `StaticFile <handlers>` sia già presente. In questo caso, aggiungi semplicemente DirectoryListingModule all'attributo \<modules> usando la virgola come separatore.  

**Errori HTTP**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=CustomErrorModule image=%windir%\System32\inetsrv\custerr.dll />`|  
|`<modules>`|`<add name=CustomErrorModule lockItem=true />`|  
|`<httpErrors>`|`<httpErrors lockAttributes=allowAbsolutePathsWhenDelegated,defaultPath><br />    <error statusCode=401    prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=401.htm ><br />    <error statusCode=403 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=403.htm /><br />    <error statusCode=404 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=404.htm /><br />    <error statusCode=405 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=405.htm /><br />    <error statusCode=406 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=406.htm /><br />    <error statusCode=412 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=412.htm /><br />    <error statusCode=500 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=500.htm /><br />    <error statusCode=501 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=501.htm /><br />    <error statusCode=502 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=502.htm /><br /></httpErrors>`|  

**Contenuto statico**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticFileModule image=%windir%\System32\inetsrv\static.dll />`|  
|`<modules>`|`<add name=StaticFileModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=StaticFileModule resourceType=Either requireAccess=Read />`|  

È possibile che la voce `StaticFile \<handlers>` sia già presente. In questo caso, aggiungi semplicemente StaticFileModule all'attributo \<modules> usando la virgola come separatore.  

**Reindirizzamento HTTP**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=HttpRedirectionModule image=%windir%\System32\inetsrv\redirect.dll />`|  
|`<modules>`|`<add name=HttpRedirectionModule lockItem=true />`|  
|`<httpRedirect>`|`<httpRedirect enabled=false />`|  

### <a name="health-and-diagnostics"></a>Integrità e diagnostica  
**Registrazione HTTP**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=HttpLoggingModule image=%windir%\System32\inetsrv\loghttp.dll />`|  
|`<modules>`|`<add name=HttpLoggingModule lockItem=true />`|  
|`<httpLogging>`|`<httpLogging dontLog=false />`|  

**Registrazione personalizzata**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CustomLoggingModule image=%windir%\System32\inetsrv\logcust.dll />`|  
|`<modules>`|`<add name=CustomLoggingModule lockItem=true />`|  

**Monitor richieste**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=RequestMonitorModule image=%windir%\System32\inetsrv\iisreqs.dll />`|  

**Traccia**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=TracingModule image=%windir%\System32\inetsrv\iisetw.dll \/><br /><add name=FailedRequestsTracingModule image=%windir%\System32\inetsrv\iisfreb.dll />`|  
|`<modules>`|`<add name=FailedRequestsTracingModule lockItem=true />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name=WWW Server guid\={3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}><br />        <areas><br />            <clear /><br />            <add name=Authentication value=2 /><br />            <add name=Security value=4 /><br />            <add name=Filter value=8 /><br />            <add name=StaticFile value=16 /><br />            <add name=CGI value=32 /><br />            <add name=Compression value=64 /><br />            <add name=Cache value=128 /><br />            <add name=RequestNotifications value=256 /><br />            <add name=Module value=512 /><br />            <add name=FastCGI value=4096 /><br />            <add name=WebSocket value=16384 /><br />        </areas><br />    </add><br />    <add name=ISAPI Extension guid={a1c2040e-8840-4c31-ba11-9871031a19ea}><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>Prestazioni  
**Compressione contenuto statico**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticCompressionModule image=%windir%\System32\inetsrv\compstat.dll />`|  
|`<modules>`|`<add name=StaticCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll /><br />   <staticTypes><br />        <add mimeType=text/* enabled=true /><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/javascript enabled=true \/><br />        <add mimeType=application/atom+xml enabled=true /><br />        <add mimeType=application/xaml+xml enabled=true /><br />        <add mimeType=\*\* enabled=false /><br />    </staticTypes><br /></httpCompression>`|  

**Compressione contenuto dinamico**  

|Sezione|Elementi di configurazione|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=DynamicCompressionModule image=%windir%\System32\inetsrv\compdyn.dll />`|  
|`<modules>`|`<add name=DynamicCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory\=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll \/><br />    \<dynamicTypes><br />        <add mimeType=text/* enabled=true \/><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/x-javascript enabled=true /><br />        <add mimeType=application/javascript enabled=true /><br />        <add mimeType=*/* enabled=false /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>Sicurezza  
**Filtro richieste**  


|       Sezione        |                                                                                                                                        Elementi di configurazione                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `<globalModules>`   |                                                                                                        `<add name=RequestFilteringModule image=%windir%\System32\inetsrv\modrqflt.dll />`                                                                                                        |
|     `<modules>`      |                                                                                                                       `<add name=RequestFilteringModule lockItem=true />`                                                                                                                        |
| \`<requestFiltering> | `<requestFiltering><br />    <fileExtensions allowUnlisted=true applyToWebDAV=true /><br />    <verbs allowUnlisted=true applyToWebDAV=true /><br />    <hiddenSegments applyToWebDAV=true><br />        <add segment=web.config /><br />    </hiddenSegments><br /></requestFiltering>` |

**Autenticazione di base**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=BasicAuthenticationModule image=%windir%\System32\inetsrv\authbas.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled=false />`|  

**Autenticazione mapping certificati client**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CertificateMappingAuthentication image=%windir%\System32\inetsrv\authcert.dll />`|  
|`<modules>`|`<add name=CertificateMappingAuthenticationModule lockItem=true />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled=false />`|  

**Autenticazione del digest**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DigestAuthenticationModule image=%windir%\System32\inetsrv\authmd5.dll />`|  
|`<modules>`|`<add name=DigestAuthenticationModule lockItem=true />`|  
|`<other>`|`<digestAuthentication enabled=false />`|  

**Autenticazione mapping certificati client IIS**  


|                  Sezione                   |                                         Elementi di configurazione                                         |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------|
|             `<globalModules>`              | `<add name=CertificateMappingAuthenticationModule image=%windir%\System32\inetsrv\authcert.dll />` |
|                `<modules>`                 |               `<add name=CertificateMappingAuthenticationModule lockItem=true `/>\`                |
| `<clientCertificateMappingAuthentication>` |                      `<clientCertificateMappingAuthentication enabled=false />`                      |

**Restrizioni per IP e domini**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name=IpRestrictionModule image=%windir%\System32\inetsrv\iprestr.dll /><br /><add name=DynamicIpRestrictionModule image=%windir%\System32\inetsrv\diprestr.dll />```|  
|`<modules>`|`<add name=IpRestrictionModule lockItem=true \/><br /><add name=DynamicIpRestrictionModule lockItem=true \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted=true />`|  

**Autorizzazione URL**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=UrlAuthorizationModule image=%windir%\System32\inetsrv\urlauthz.dll />`|  
|`<modules>`|`<add name=UrlAuthorizationModule lockItem=true />`|  
|`<authorization>`|`<authorization><br />    <add accessType=Allow users=* /><br /></authorization>`|  

**Autenticazione di Windows**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>Sviluppo di applicazioni  
**Inizializzazione dell'applicazione**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=ApplicationInitializationModule image=%windir%\System32\inetsrv\warmup.dll />`|  
|`<modules>`|`<add name=ApplicationInitializationModule lockItem=true />`|  

**CGI**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CgiModule image=%windir%\System32\inetsrv\cgi.dll /><br /><add name=FastCgiModule image=%windir%\System32\inetsrv\iisfcgi.dll />`|  
|`<modules>`|`<add name=CgiModule lockItem=true /><br /><add name=FastCgiModule lockItem=true />`|  
|`<handlers>`|`<add name=CGI-exe path=*.exe verb=\* modules=CgiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**Estensioni ISAPI**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiModule image=%windir%\System32\inetsrv\isapi.dll />`|  
|`<modules>`|`<add name=IsapiModule lockItem=true />`|  
|`<handlers>`|`<add name=ISAPI-dll path=*.dll verb=* modules=IsapiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**Filtri ISAPI**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiFilterModule image=%windir%\System32\inetsrv\filter.dll />`|  
|`<modules>`|`<add name=IsapiFilterModule lockItem=true />`|  

**Server-Side Include**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name=ServerSideIncludeModule image=%windir%\System32\inetsrv\iis_ssi.dll />`|  
|`<modules>`|`<add name=ServerSideIncludeModule lockItem=true />`|  
|`<handlers>`|`<add name=SSINC-stm path=*.stm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File \/><br /><add name=SSINC-shtm path=*.shtm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File /><br /><add name=SSINC-shtml path=*.shtml verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable=false />`|  

**Protocollo WebSocket**  

|Sezione|Elementi di configurazione|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WebSocketModule image=%windir%\System32\inetsrv\iiswsock.dll />`|  
|`<modules>`|`<add name=WebSocketModule lockItem=true />`|  