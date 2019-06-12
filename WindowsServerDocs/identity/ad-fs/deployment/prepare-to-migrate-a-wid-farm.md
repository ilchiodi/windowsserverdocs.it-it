---
title: Preparare la migrazione di una farm database interno di Windows di AD FS 2.0
description: Fornisce informazioni sulle operazioni preliminari per la migrazione di una farm database interno di Windows server 2.0 di ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0e4bb77003ab24e0e31268509fb8667a671bea6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445530"
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Preparare la migrazione di una farm database interno di Windows di AD FS 2.0  
 Per preparare la migrazione di ADFS 2.0 federation server che appartengono a una farm Database interno Windows (WID) a Windows Server 2012, è necessario esportare ed eseguire il backup dei dati di configurazione di ADFS da questi server.  
  
 Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1:: esportare le impostazioni del servizio](#step-1-export-service-settings)  
  
-   [Passaggio 2: Eseguire il backup degli archivi attributi personalizzati](#step-2-back-up-custom-attribute-stores)  
  
-   [Passaggio 3: Eseguire il backup delle personalizzazioni di pagine Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Passaggio 1: esportare le impostazioni del servizio  
 Per esportare le impostazioni del servizio, eseguire la procedura riportata di seguito:  
  
### <a name="to-export-service-settings"></a>Per esportare le impostazioni del servizio  
  
1.  Registrare il nome del soggetto del certificato e il valore di identificazione personale del certificato SSL usati dal servizio federativo. Per trovare il certificato SSL, aprire la console di Gestione Internet Information Services (IIS), selezionare **sito Web predefinito** nel riquadro sinistro, fare clic su **associazioni...** nel **azione** riquadro, trovare e selezionare il binding https, fare clic su **Edit**, quindi fare clic su **visualizzazione**.  
  
> [!NOTE]
>  Facoltativamente, è possibile esportare il certificato SSL e la relativa chiave privata in un file con estensione pfx. Per altre informazioni, vedere [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Questo passaggio è facoltativo, poiché questo è memorizzato nell'archivio dei certificati personali nel computer locale e verrà conservato durante l'aggiornamento del sistema operativo.  
  
2. Esportare eventuali certificati e chiavi di decrittografia di token, di firma di token e di comunicazioni di servizi non generati internamente, oltre ai certificati autofirmati.  
  
È possibile visualizzare tutti i certificati in uso nel server tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per visualizzare tutti i certificati che sono in uso nel server `PSH:>Get-ADFSCertificate`. Nell'output di questo comando sono inclusi i valori StoreLocation e StoreName che specificano il percorso dell'archivio per ogni certificato.  È quindi possibile attenersi alle indicazioni fornite in [Esportare la parte di chiave privata di un certificato di autenticazione server](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) per esportare ogni certificato con la relativa chiave privata in un file con estensione pfx.  
  
> [!NOTE]
>  Questo passaggio è facoltativo, poiché tutti i certificati esterni vengono conservati durante l'aggiornamento del sistema operativo.  
  
3. Registrare l'identità dell'account del servizio AD FS 2.0 federation e la password dell'account.  
  
Per trovare il valore di identità, controllare la **Accedi come** della colonna della **AD FS 2.0 Windows Service** nel **Services** della console e registrare manualmente il valore.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Passaggio 2: eseguire il backup degli archivi di attributi personalizzati  
 Per trovare informazioni sugli archivi di attributi personalizzati usati da ADFS, è possibile usare Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per trovare informazioni sugli archivi attributi personalizzati: `PSH:>Get-ADFSAttributeStore`. La procedura per aggiornare gli archivi di attributi personalizzati o eseguirne la migrazione varia.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Passaggio 3: eseguire il backup delle personalizzazioni di pagine Web  
 Per eseguire il backup delle personalizzazioni di qualsiasi pagina Web, copiare le pagine Web AD FS e il **Web. config** file dalla directory mappata al percorso virtuale **"/ adfs/ls"** in IIS. Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di AD Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del Proxy Server 2.0 Federation di AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di AD Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del Proxy Server 2.0 Federation di AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)