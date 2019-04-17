---
title: Preparare la migrazione di AD FS 2.0 Server federativo
description: Fornisce informazioni sui preparativi per la migrazione del proxy di server di ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparare la migrazione di AD FS 2.0 Server federativo

Per prepararsi alla migrazione di un proxy server 2.0 federativo di ADFS a Windows Server 2012, è necessario esportare ed eseguire il backup di dati di configurazione di AD FS da questo proxy server.  I passaggi descritti in questo argomento si applicano a uno scenario con un server proxy federativo o più server proxy federativi.  
  
 Per esportare i dati di configurazione di ADFS, eseguire le attività seguenti:  
  
-   [Passaggio 1: Esportare le impostazioni del servizio proxy](#step-1-export-proxy-service-settings)  
  
-   [Passaggio 2: Eseguire il backup delle personalizzazioni](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>Passaggio 1: Esportare le impostazioni del servizio proxy  
 Per esportare impostazioni del servizio proxy server federativo, eseguire la procedura seguente:  
  
### <a name="to-export-proxy-service-settings"></a>Per esportare le impostazioni del servizio proxy  
  
1.  Esportare il certificato Secure Sockets Layer (SSL) e la relativa chiave privata in un file con estensione pfx. Per ulteriori informazioni, vedere [esportare la parte di chiave privata di un certificato di autenticazione Server](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Questo passaggio è facoltativo, poiché questo certificato non verrà conservato durante l'aggiornamento del sistema operativo.  
  
2.  Esportare le proprietà del proxy AD FS 2.0 federativo in un file. È possibile farlo tramite Windows PowerShell.  
  
Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per esportare le proprietà del proxy federativo in un file:`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3.  Assicurarsi di conoscere le credenziali di un account amministratore del server federativo AD FS o l'account del servizio con cui viene eseguito il servizio federativo AD FS.  Queste informazioni sono necessarie per la configurazione del trust proxy.  
  
 Il completamento di questo passaggio avrà come risultato la raccolta delle informazioni necessarie per la configurazione del proxy server federativo di ADFS seguenti:  
  
-   Nome servizio federativo AD FS  
  
-   Nome dell'account di dominio che è necessario per la configurazione del trust proxy  
  
-   L'indirizzo e porta del proxy HTTP (se è presente un proxy HTTP tra il proxy server federativo AD FS e i server federativi AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Passaggio 2: Eseguire il backup delle personalizzazioni  
 Per eseguire il backup delle personalizzazioni di pagine Web, copiare le pagine Web del proxy AD FS e **Web. config** file dalla directory mappata al percorso virtuale **"/ADFS/ls"** in IIS.  Per impostazione predefinita, in è il **%systemdrive%\inetpub\adfs\ls** directory.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)