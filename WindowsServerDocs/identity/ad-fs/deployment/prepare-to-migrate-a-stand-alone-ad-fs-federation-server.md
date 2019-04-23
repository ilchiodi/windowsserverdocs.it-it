---
title: Preparare la migrazione di un server federativo ADFS autonomo
description: Fornisce informazioni sulle operazioni preliminari per la migrazione di un server ADFS autonomo per Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834492"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Preparare la migrazione di un server federativo ADFS autonomo o una farm ADFS a nodo singolo  
 
Per prepararsi alla migrazione (migrazione del server stesso) un server di 2.0 federativo ADFS autonomo o una farm ADFS a nodo singolo a Windows Server 2012, è necessario esportare ed eseguire il backup dei dati di configurazione di ADFS dal server.  
  
Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1:  Esportare le impostazioni del servizio](#step-1-export-service-settings)  
  
-   [Passaggio 2:  Esportare le attendibilità del provider di attestazioni](#step-2-export-claims-provider-trusts)  
  
-   [Passaggio 3:  Esportare i trust della relying party](#step-3-export-relying-party-trusts)  
  
-   [Passaggio 4:  Eseguire il backup degli archivi attributi personalizzati](#step-4-back-up-custom-attribute-stores)  
  
-   [Passaggio 5:  Eseguire il backup delle personalizzazioni di pagine Web](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Passaggio 1: esportare le impostazioni del servizio  
 Per esportare le impostazioni del servizio, eseguire la procedura riportata di seguito:  
  
### <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Registrare il nome del soggetto del certificato e il valore di identificazione personale del certificato SSL usati dal servizio federativo. Per trovare il certificato SSL, aprire la console di gestione di Internet Information Services (IIS), selezionare **Sito Web predefinito** nel riquadro sinistro, fare clic su **Binding** nel riquadro **Azione** , trovare e selezionare il binding https, fare clic su **Modifica**e quindi su **Visualizza**.  
  
> [!NOTE]
>  Facoltativamente, è possibile esportare il certificato SSL usato dal servizio federativo e la relativa chiave privata in un file con estensione pfx. Per altre informazioni, vedere [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  L'esportazione del certificato SSL è facoltativa, poiché questo è memorizzato nell'archivio dei certificati personali nel computer locale e viene conservato durante l'aggiornamento del sistema operativo.  
  
2.  Registrare la configurazione dei certificati per le comunicazioni di servizi ADFS, di decrittografia di token e di firma di token.  Per visualizzare tutti i certificati vengono usati, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per creare un elenco di tutti i certificati in uso in un file `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Facoltativamente, è anche possibile esportare certificati e chiavi di decrittografia di token, di firma di token e di comunicazioni di servizi non generati internamente, oltre a tutti i certificati autofirmati. È possibile visualizzare tutti i certificati in uso nel server tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell`. Quindi eseguire il comando seguente per visualizzare tutti i certificati che sono in uso nel server `PSH:>Get-ADFSCertificate`. Nell'output di questo comando sono inclusi i valori StoreLocation e StoreName che specificano il percorso dell'archivio per ogni certificato. È quindi possibile attenersi alle indicazioni fornite in [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) per esportare ogni certificato con la relativa chiave privata in un file con estensione pfx.  
>   
>  L'esportazione di questi certificati è facoltativa poiché tutti i certificati esterni vengono conservati durante l'aggiornamento del sistema operativo.  
  
3.  Esportazione di AD FS 2.0 Proprietà servizio federativo, ad esempio il nome del servizio federativo, nome visualizzato del servizio e identificatore del server federativo in un file.  
  
Per esportare le proprietà del servizio federativo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare le proprietà servizio federativo: `PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
Il file di output conterrà i valori di configurazione importanti seguenti:  
  
    
|**Nome della proprietà servizio federativo come riportato da Get-ADFSProperties**|**Nome della proprietà servizio federativo nella console di gestione di AD FS**|
|------|------|
|HostName|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
|DisplayName|Nome visualizzato del servizio federativo|  
  
4.  Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione al database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il file `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Prendere nota della stringa di connessione al database in questo file, indicata subito dopo "policystore connectionstring=". Se la stringa di connessione specifica un database di SQL Server, il valore è necessario per il ripristino della configurazione ADFS originale nel server federativo.  
>   
>  Di seguito viene riportato un esempio di stringa di connessione WID:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Di seguito viene riportato un esempio di stringa di connessione di SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
5.  Registrare l'identità dell'account del servizio AD FS 2.0 federation e la password dell'account.  
  
Per trovare il valore di identità, controllare la colonna **Accedi come** di **AD FS 2.0 Windows Service** nella console **Servizi** e registrare manualmente questo valore.  
  
> [!NOTE]
>  Per un servizio federativo autonomo viene utilizzato l'account predefinito SERVIZIO DI RETE.  In questo caso, non è necessario disporre di una password.  
  
6.  Esportare in un file l'elenco degli endpoint ADFS abilitati.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare l'elenco degli endpoint ADFS abilitati in un file: `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7.  Esportare in un file le eventuali descrizioni di attestazioni personalizzate.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare eventuali descrizioni di attestazioni personalizzate in un file: `Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Passaggio 2: esportare i trust del provider di attestazioni  
 Per esportare i trust del provider di attestazioni, eseguire la procedura riportata di seguito:  
  
### <a name="to-export-claims-provider-trusts"></a>Per esportare attendibilità del provider di attestazioni  
  
1.  È possibile usare Windows PowerShell per esportare i trust del provider di attestazioni. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare tutti i trust di provider di attestazioni: `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Passaggio 3: esportare i trust della relying party  
 Per esportare i trust della relying party, eseguire la procedura riportata di seguito:  
  
### <a name="to-export-relying-party-trusts"></a>Per esportare attendibilità del componente  
  
1.  Per esportare i trust di tutte le relying party, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare tutte le relying party trusts:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Passaggio 4: eseguire il backup degli archivi di attributi personalizzati  
 Per trovare informazioni sugli archivi di attributi personalizzati usati da ADFS, è possibile usare Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per trovare informazioni sugli archivi attributi personalizzati: `PSH:>Get-ADFSAttributeStore`. La procedura per aggiornare gli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Passaggio 5: eseguire il backup delle personalizzazioni di pagine Web  
 Per eseguire il backup delle personalizzazioni di qualsiasi pagina Web, copiare le pagine Web AD FS e il **Web. config** file dalla directory mappata al percorso virtuale **"/ adfs/ls"** in IIS. Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di AD Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del Proxy Server 2.0 Federation di AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di AD Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del Proxy Server 2.0 Federation di AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di AD agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)