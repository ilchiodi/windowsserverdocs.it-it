---
title: Preparare la migrazione di una farm di AD FS SQL
description: Fornisce informazioni su come preparare la migrazione di una farm SQL di AD FS server a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2d8bbe5021b876712862c992b643b7828095e869
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408211"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>Preparare la migrazione di una farm SQL Server  
 Per preparare la migrazione di AD FS server federativi 2,0 che appartengono a una farm di SQL Server a Windows Server 2012, è necessario esportare ed eseguire il backup dei dati di configurazione AD FS da questi server.  
  
 Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1: esportare le impostazioni del servizio](#step-1-export-service-settings)  
  
-   [Passaggio 2: eseguire il backup degli archivi di attributi personalizzati](#step-2-back-up-custom-attribute-stores)  
  
-   [Passaggio 3: eseguire il backup delle personalizzazioni di pagine Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Passaggio 1: esportare le impostazioni del servizio  
 Per esportare le impostazioni del servizio, eseguire la procedura riportata di seguito:  
  
### <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Registrare il nome del soggetto del certificato e il valore di identificazione personale del certificato SSL usati dal servizio federativo. Per trovare il certificato SSL, aprire la console di gestione Internet Information Services (IIS), selezionare **sito Web predefinito** nel riquadro sinistro, fare clic su **Binding.** nel riquadro **Azione** , trovare e selezionare il binding https, fare clic su **Modifica**e quindi su **Visualizza**.  
  
> [!NOTE]
>  Facoltativamente, è possibile esportare il certificato SSL e la relativa chiave privata in un file con estensione pfx. Per altre informazioni, vedere [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Questo passaggio è facoltativo, poiché questo è memorizzato nell'archivio dei certificati personali nel computer locale e verrà conservato durante l'aggiornamento del sistema operativo.  
  
2. Esportare eventuali altri certificati e chiavi di decrittografia di token, di firma di token e di comunicazioni di servizi non generati internamente da ADFS.  
  
È possibile visualizzare tutti i certificati in uso da parte di ADFS nel server tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Eseguire quindi il comando seguente per visualizzare tutti i certificati in uso nel server `PSH:>Get-ADFSCertificate`. Nell'output di questo comando sono inclusi i valori StoreLocation e StoreName che specificano il percorso dell'archivio per ogni certificato.  
  
> [!NOTE]
>  Facoltativamente, è quindi possibile attenersi alle indicazioni fornite in [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) per esportare ogni certificato con la relativa chiave privata in un file con estensione pfx. Questo passaggio è facoltativo, poiché tutti i certificati esterni vengono conservati durante l'aggiornamento del sistema operativo.  
  
3. Eseguire il backup del file di configurazione dell'applicazione. Tra le altre impostazioni, questo file contiene la stringa di connessione al database dei criteri.  
  
Per eseguire il backup del file di configurazione dell'applicazione, è necessario copiare manualmente il file `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` in una posizione sicura in un server di backup.  
  
> [!NOTE]
>  Registrare la stringa di connessione SQL Server dopo "PolicyStore ConnectionString =" nel file seguente: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`. Questa stringa è necessaria quando si ripristina la configurazione di AD FS originale nel server federativo.  
  
4. Registrare l'identità dell'account del servizio federativo AD FS 2,0 e la password di questo account.  
  
Per trovare il valore Identity, esaminare la colonna **Accedi come** del **servizio Windows ad FS 2,0** nella console **Servizi** e registrare manualmente il valore.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Passaggio 2: eseguire il backup degli archivi di attributi personalizzati  
 Per trovare informazioni sugli archivi di attributi personalizzati usati da ADFS, è possibile usare Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Eseguire quindi il comando seguente per trovare informazioni sugli archivi di attributi personalizzati: `PSH:>Get-ADFSAttributeStore`. La procedura per aggiornare gli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Passaggio 3: eseguire il backup delle personalizzazioni di pagine Web  
 Per eseguire il backup delle personalizzazioni di pagine Web, copiare le pagine Web AD FS e il file **Web. config** dalla directory mappata al percorso virtuale **"/adfs/ls"** in IIS. Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)