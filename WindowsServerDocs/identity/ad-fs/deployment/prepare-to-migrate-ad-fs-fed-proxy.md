---
title: Preparare la migrazione del proxy server federativo AD FS 2.0
description: Fornisce informazioni su come preparare la migrazione del proxy del server di AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 20fbf3ea9231706635df2bd4c1d541fde0c1484b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408208"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparare la migrazione del proxy server federativo AD FS 2.0

Per preparare la migrazione di un proxy server federativo AD FS 2,0 a Windows Server 2012, è necessario esportare ed eseguire il backup dei dati di configurazione AD FS da questo proxy server.  I passaggi illustrati in questo argomento sono applicabili a uno scenario con un server proxy federativo o più server proxy federativi.  
  
 Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1: Esporta impostazioni del servizio proxy @ no__t-0  
  
-   [Passaggio 2: Eseguire il backup delle personalizzazioni di pagine Web @ no__t-0  
  
##  <a name="step-1-export-proxy-service-settings"></a>Passaggio 1: Esportare le impostazioni del servizio proxy  
 Per esportare le impostazioni del servizio del proxy server federativo, seguire questa procedura:  
  
### <a name="to-export-proxy-service-settings"></a>Per esportare le impostazioni del servizio proxy  
  
1.  Esportare il certificato SSL (Secure Sockets Layer) e la rispettiva chiave privata in un file con estensione pfx. Per altre informazioni, vedere [Esportare la parte di chiave privata di un certificato di autenticazione server](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Questo passaggio è facoltativo, poiché il certificato viene mantenuto durante l'aggiornamento del sistema operativo.  
  
2. Esportare le proprietà del proxy federativo AD FS 2.0 in un file. Per eseguire questa operazione, usare Windows PowerShell.  
  
Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Eseguire quindi il comando seguente per esportare le proprietà del proxy federativo in un file: `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3. Assicurarsi di conoscere le credenziali di un account corrispondente a un amministratore del server federativo AD FS o un account del servizio con cui viene eseguito il servizio federativo AD FS.  Queste informazioni sono necessarie per la configurazione del trust del proxy.  
  
   Il completamento di questo passaggio avrà come risultato la raccolta delle informazioni seguenti, necessarie per la configurazione del proxy server federativo AD FS:  
  
-   Nome servizio federativo AD FS  
  
-   Nome dell'account di dominio necessario per la configurazione del trust del proxy  
  
-   Indirizzo e porta del proxy HTTP (se è presente un proxy HTTP tra il proxy server federativo AD FS e i server federativi AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Passaggio 2: eseguire il backup delle personalizzazioni di pagine Web  
 Per eseguire il backup delle personalizzazioni di pagine Web, copiare le pagine Web del proxy AD FS e il file **web.config** dalla directory mappata al percorso virtuale **“/adfs/ls”** in IIS.  Per impostazione predefinita, questa si trova nella directory **%systemdrive%\inetpub\adfs\ls**.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo di AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)