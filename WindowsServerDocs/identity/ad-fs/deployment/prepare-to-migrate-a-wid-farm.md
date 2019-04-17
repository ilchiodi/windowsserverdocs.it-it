---
title: Preparare la migrazione di una farm database interno di Windows di AD FS 2.0
description: Fornisce informazioni sui preparativi eseguire la migrazione di una farm database interno di Windows di AD FS 2.0 server a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4985a8d16614bd12bce991e196d105464d37634d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Preparare la migrazione di una farm database interno di Windows di AD FS 2.0  
 Per prepararsi alla migrazione di ADFS 2.0 federativi che appartengono a una farm Database interno Windows (WID) a Windows Server 2012, è necessario esportare ed eseguire il backup di dati di configurazione di ADFS presenti in tali server.  
  
 Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1:: esportare le impostazioni del servizio](#step-1-export-service-settings)  
  
-   [Passaggio 2: Eseguire il backup degli archivi di attributi personalizzati](#step-2-back-up-custom-attribute-stores)  
  
-   [Passaggio 3: Eseguire il backup delle personalizzazioni](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Passaggio 1: Esportare le impostazioni del servizio  
 Per esportare le impostazioni del servizio, eseguire la procedura seguente:  
  
### <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Registrare il certificato nome e l'identificazione personale del valore del soggetto del certificato SSL utilizzato dal servizio federativo. Per trovare il certificato SSL, aprire la console di Gestione Internet Information Services (IIS), selezionare **sito Web predefinito** nel riquadro a sinistra, fare clic su **Binding...** Nel **azione** riquadro, trovare e selezionare il binding https, fare clic su **modifica**, quindi fare clic su **visualizzazione **.  
  
> [!NOTE]
>  Facoltativamente, è possibile esportare il certificato SSL e la relativa chiave privata in un file con estensione pfx. Per ulteriori informazioni, vedere [esportare la parte di chiave privata di un certificato di autenticazione Server ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Questo passaggio è facoltativo, poiché questo certificato viene archiviato nell'archivio certificati personali del computer locale e verrà conservato durante l'aggiornamento del sistema operativo.  
  
2.  Esportare eventuali certificati di firma di token, la crittografia di token o comunicazioni di servizi e le chiavi non generati internamente, oltre ai certificati autofirmati.  
  
È possibile visualizzare tutti i certificati che sono in uso nel server tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per visualizzare tutti i certificati che sono in uso nel server `PSH:>Get-ADFSCertificate`. L'output di questo comando include i valori StoreLocation e StoreName che specificano la posizione dell'archivio di ogni certificato.  È quindi possibile utilizzare le indicazioni fornite in [esportare la parte di chiave privata di un certificato di autenticazione Server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) per esportare ogni certificato e la relativa chiave privata in un file con estensione pfx.  
  
> [!NOTE]
>  Questo passaggio è facoltativo, poiché tutti i certificati esterni vengono conservati durante l'aggiornamento del sistema operativo.  
  
3.  Registrare l'identità dell'account del servizio AD FS 2.0 federativo e la password dell'account.  
  
Per trovare il valore di identità, controllare il **Accedi come** colonna di **AD FS 2.0 Windows Service** nel **servizi** console e registrare manualmente il valore.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Passaggio 2: Eseguire il backup degli archivi di attributi personalizzati  
 È possibile trovare informazioni sugli archivi di attributi personalizzati in uso da AD FS tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per trovare informazioni sugli archivi di attributi personalizzati:`PSH:>Get-ADFSAttributeStore`. La procedura per aggiornare o archivi di attributi personalizzati di eseguire la migrazione varia.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Passaggio 3: Eseguire il backup delle personalizzazioni  
 Per eseguire il backup delle personalizzazioni di qualsiasi pagina Web, copiare le pagine Web di ADFS e **Web. config** file dalla directory mappata al percorso virtuale **"/ADFS/ls"** in IIS. Per impostazione predefinita, in è il **%systemdrive%\inetpub\adfs\ls** directory.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)