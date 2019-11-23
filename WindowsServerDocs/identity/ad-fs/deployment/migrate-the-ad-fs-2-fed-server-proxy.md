---
title: Eseguire la migrazione del proxy server federativo AD FS 2,0
description: Vengono fornite informazioni sulla migrazione del proxy server federativo di AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e97a1b3b66d7c12c96570022b7e69caaf0e92f65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408224"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Eseguire la migrazione del proxy server federativo AD FS 2,0
In questo documento vengono fornite informazioni dettagliate sulla migrazione di un server proxy federativo AD FS 2,0 a Windows Server 2012.

## <a name="migrate-the-proxy"></a>Eseguire la migrazione del proxy

Per eseguire la migrazione di un proxy server federativo AD FS 2,0 a Windows Server 2012, seguire questa procedura:  
  
1.  Per ogni proxy server federativo di cui si intende eseguire la migrazione a Windows Server 2012, rivedere ed eseguire le procedure descritte in [preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Rimuovere un proxy server federativo dal bilanciamento del carico.  
  
3.  Eseguire un aggiornamento sul posto del sistema operativo in questo server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del proxy AD FS nel server andrà persa e il ruolo del server AD FS 2.0 verrà rimosso. Viene invece installato il ruolo server AD FS Windows Server 2012, ma non è configurato. Per completare la migrazione del proxy server federativo è necessario creare manualmente la configurazione originale del proxy AD FS e ripristinare le impostazioni del proxy AD FS rimanenti.  
  
4. Creare la configurazione originale del proxy AD FS usando la **configurazione guidata del proxy server federativo AD FS**. Per altre informazioni, vedere [Configure a Computer for the Federation Server Proxy Role](configure-a-computer-for-the-federation-server-proxy-role.md). Quando si esegue la procedura guidata, usare le informazioni raccolte in Preparare la migrazione del proxy server federativo AD FS 2.0 come descritto di seguito:  
  
 
|**Opzione di input della procedura guidata proxy server federativo**|**Usare il valore seguente**|
|-----|-----|  
|**Nome Servizio federativo**|Immettere il valore BaseHostName dal file proxyproperties.txt|  
|Casella di controllo **Use an HTTP proxy server when sending requests to this Federation** Service|Selezionare questa casella se il file proxyproperties.txt contiene un valore per la proprietà ForwardProxyUrl|  
|**Indirizzo del server proxy HTTP**|Immettere il valore ForwardProxyUrl dal file proxyproperties.txt|  
|Richiesta di credenziali|Immettere le credenziali di un account amministratore del server federativo AD FS o l'account del servizio in cui viene eseguito il servizio federativo AD FS.|  
  
5. Aggiornare le pagine Web di ADFS nel server Se è stato eseguito il backup delle pagine Web personalizzate del proxy AD FS durante la preparazione del proxy server federativo per la migrazione, usare i dati di backup per sovrascrivere le pagine Web predefinite AD FS create per impostazione predefinita nella directory **%SystemDrive%\Inetpub\Adfs\Ls** in seguito alla configurazione del proxy ad FS in Windows Server 2012.  
  
6. Aggiungere di nuovo il server al bilanciamento del carico.  
  
7. Per eseguire la migrazione di altri proxy server federativi AD FS 2.0, ripetere i passaggi da 2 a 6 per i computer rimanenti.  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)