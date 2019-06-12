---
title: Eseguire la migrazione di ADFS 2.0 server federativo
description: Vengono fornite informazioni sulla migrazione il proxy server federativo di ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eddb0d3432c69ecff4542ff1b8f2204b96ce0820
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445658"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Eseguire la migrazione di ADFS 2.0 server federativo
Questo documento fornisce informazioni dettagliate sulla migrazione di un server proxy federativo 2.0 di AD FS per Windows Server 2012.

## <a name="migrate-the-proxy"></a>Eseguire la migrazione del proxy

Per eseguire la migrazione di un proxy server 2.0 federation di AD FS per Windows Server 2012, seguire la procedura seguente:  
  
1.  Per ogni proxy server federativo che si intende eseguire la migrazione a Windows Server 2012, rivedere ed eseguire le procedure descritte in [prepararsi alla migrazione di AD FS 2.0 i Proxy Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Rimuovere un proxy server federativo dal bilanciamento del carico.  
  
3.  Eseguire un aggiornamento sul posto del sistema operativo nel server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del proxy AD FS nel server andrà persa e il ruolo del server AD FS 2.0 verrà rimosso. Suo posto verrà installato il ruolo server AD FS di Windows Server 2012, ma non è configurato. Per completare la migrazione del proxy server federativo è necessario creare manualmente la configurazione originale del proxy AD FS e ripristinare le impostazioni del proxy AD FS rimanenti.  
  
4. Creare la configurazione originale del proxy AD FS usando la **configurazione guidata del proxy server federativo AD FS**. Per altre informazioni, vedere [Configure a Computer for the Federation Server Proxy Role](configure-a-computer-for-the-federation-server-proxy-role.md). Quando si esegue la procedura guidata, usare le informazioni raccolte in Preparare la migrazione del proxy server federativo AD FS 2.0 come descritto di seguito:  
  
 
|**Opzione di input guidata del Proxy Server federativo**|**Usare il valore seguente**|
|-----|-----|  
|**Nome servizio federativo**|Immettere il valore BaseHostName dal file proxyproperties.txt|  
|Casella di controllo **Use an HTTP proxy server when sending requests to this Federation** Service|Selezionare questa casella se il file proxyproperties.txt contiene un valore per la proprietà ForwardProxyUrl|  
|**Indirizzo del server proxy HTTP**|Immettere il valore ForwardProxyUrl dal file proxyproperties.txt|  
|Richiesta di credenziali|Immettere le credenziali di un account amministratore del server federativo AD FS o l'account del servizio in cui viene eseguito il servizio federativo AD FS.|  
  
5. Aggiornare le pagine Web di ADFS nel server Se è stato eseguito il backup delle pagine Web proxy AD FS durante la preparazione del proxy server federativo per la migrazione, usare i dati di backup per sovrascrivere le pagine Web di ADFS che sono state create per impostazione predefinita nel **%systemdrive%\inetpub\adfs\ls** directory in seguito alla configurazione del proxy AD FS in Windows Server 2012.  
  
6. Aggiungere di nuovo il server al bilanciamento del carico.  
  
7. Per eseguire la migrazione di altri proxy server federativi AD FS 2.0, ripetere i passaggi da 2 a 6 per i computer rimanenti.  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di AD Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del Proxy Server 2.0 Federation di AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di AD Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del Proxy Server 2.0 Federation di AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)