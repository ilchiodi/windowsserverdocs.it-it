---
title: Preparare la migrazione di un server federativo ADFS autonomo
description: Fornisce informazioni sui preparativi eseguire la migrazione di un server autonomo ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Preparare la migrazione di un server federativo ADFS autonomo o una farm ADFS a nodo singolo  
 
Per prepararsi alla migrazione (stesso migrazione del server) server AD FS 2.0 federativo autonomo o una farm ADFS a nodo singolo a Windows Server 2012, è necessario esportare ed eseguire il backup dei dati di configurazione di AD FS da questo server.  
  
Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1: Esportare le impostazioni del servizio](#step-1-export-service-settings)  
  
-   [Passaggio 2: Esportare i trust di provider di attestazioni](#step-2-export-claims-provider-trusts)  
  
-   [Passaggio 3: Esportare i trust della relying party](#step-3-export-relying-party-trusts)  
  
-   [Passaggio 4: Eseguire il backup degli archivi di attributi personalizzati](#step-4-back-up-custom-attribute-stores)  
  
-   [Passaggio 5: Eseguire il backup delle personalizzazioni](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Passaggio 1: Esportare le impostazioni del servizio  
 Per esportare le impostazioni del servizio, eseguire la procedura seguente:  
  
### <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Registrare il certificato nome e l'identificazione personale del valore del soggetto del certificato SSL utilizzato dal servizio federativo. Per trovare il certificato SSL, aprire Gestione Internet Information Services (IIS), console, selezionare **sito Web predefinito** nel riquadro a sinistra, fare clic su **Binding...** nel **azione** riquadro, trovare e selezionare il binding https, fare clic su **modifica**, quindi fare clic su **visualizzazione**.  
  
> [!NOTE]
>  Facoltativamente, è anche possibile esportare il certificato SSL utilizzato dal servizio federativo e la relativa chiave privata in un file con estensione pfx. Per ulteriori informazioni, vedere [esportare la parte di chiave privata di un certificato di autenticazione Server ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  L'esportazione del certificato SSL è facoltativa perché questo certificato viene archiviato nell'archivio certificati personali del computer locale e viene mantenuto durante l'aggiornamento del sistema operativo.  
  
2.  Registrare la configurazione delle comunicazioni di servizio ADFS, i certificati di decrittografia di token e di firma di token.  Per visualizzare tutti i certificati vengono utilizzati, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per creare un elenco di tutti i certificati in uso in un file `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Facoltativamente, è anche possibile esportare eventuali certificati di firma di token, la crittografia di token o comunicazioni di servizi e le chiavi non generati internamente, oltre a tutti i certificati autofirmati. È possibile visualizzare tutti i certificati che sono in uso nel server tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell`. Quindi eseguire il comando seguente per visualizzare tutti i certificati che sono in uso nel server `PSH:>Get-ADFSCertificate`. L'output di questo comando include i valori StoreLocation e StoreName che specificano la posizione dell'archivio di ogni certificato. È quindi possibile utilizzare le indicazioni fornite in [esportare la parte di chiave privata di un certificato di autenticazione Server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) per esportare ogni certificato e la relativa chiave privata in un file con estensione pfx.  
>   
>  L'esportazione di questi certificati è facoltativa poiché tutti i certificati esterni vengono conservati durante l'aggiornamento del sistema operativo.  
  
3.  Esportazione AD FS 2.0 Proprietà servizio federativo, ad esempio il nome del servizio federativo, nome visualizzato del servizio federativo e identificatore del server federativo in un file.  
  
Per esportare le proprietà del servizio federativo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare le proprietà del servizio federativo:`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
Il file di output conterrà i valori di configurazione importanti seguenti:  
  
    
|**Nome della proprietà servizio federativo restituito da Get-ADFSProperties**|**Nome della proprietà servizio federativo nella console di gestione di ADFS**|
|------|------|
|Nome host|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
|DisplayName|Nome visualizzato del servizio federativo|  
  
4.  Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione del database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` file in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Prendere nota della stringa di connessione al database in questo file, indicata subito dopo "policystore connectionstring ="). Se la stringa di connessione specifica un database di SQL Server, il valore è necessario quando si ripristina la configurazione originale di ADFS nel server federativo.  
>   
>  Ecco un esempio di una stringa di connessione WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Di seguito è indicato un esempio di una stringa di connessione SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
5.  Registrare l'identità dell'account del servizio AD FS 2.0 federativo e la password dell'account.  
  
Per trovare il valore di identità, controllare il **Accedi come** colonna di **AD FS 2.0 Windows Service** nel **servizi** console e registrare manualmente questo valore.  
  
> [!NOTE]
>  Per un servizio federativo autonomo, viene utilizzato l'account servizio di rete predefinito.  In questo caso, non è necessario disporre di una password.  
  
6.  Esportare l'elenco degli endpoint ADFS abilitati in un file.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare in un file l'elenco degli endpoint ADFS abilitati:`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7.  Esportare eventuali descrizioni di attestazioni personalizzate in un file.  
  
A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare le eventuali descrizioni di attestazioni personalizzate in un file:`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Passaggio 2: Esportare i trust di provider di attestazioni  
 Per esportare i trust di provider di attestazioni, eseguire la procedura seguente:  
  
### <a name="to-export-claims-provider-trusts"></a>Per esportare attendibilità del provider di attestazioni  
  
1.  È possibile utilizzare Windows PowerShell per esportare tutti i trust di provider di attestazioni. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare tutti i trust di provider di attestazioni:`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Passaggio 3: Esportare i trust della relying party  
 Per esportare i trust della relying party, eseguire la procedura seguente:  
  
### <a name="to-export-relying-party-trusts"></a>Per esportare i trust della relying party  
  
1.  Per esportare i trust di tutte le relying party, aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare tutti di attendibilità:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Passaggio 4: Eseguire il backup degli archivi di attributi personalizzati  
 È possibile trovare informazioni sugli archivi di attributi personalizzati in uso da AD FS tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per trovare informazioni sugli archivi di attributi personalizzati:`PSH:>Get-ADFSAttributeStore`. La procedura per aggiornare o archivi di attributi personalizzati di eseguire la migrazione varia.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Passaggio 5: Eseguire il backup delle personalizzazioni  
 Per eseguire il backup delle personalizzazioni di qualsiasi pagina Web, copiare le pagine Web di ADFS e **Web. config** file dalla directory mappata al percorso virtuale **"/ADFS/ls"** in IIS. Per impostazione predefinita, in è il **%systemdrive%\inetpub\adfs\ls** directory.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)