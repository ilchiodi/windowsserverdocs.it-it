---
title: Eseguire la migrazione di ADFS 2.0 server federativo
description: Fornisce informazioni sulla migrazione il proxy server federativo di ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Eseguire la migrazione di ADFS 2.0 server federativo
Questo documento fornisce informazioni dettagliate sulla migrazione di un server proxy federativo 2.0 di ADFS a Windows Server 2012.

## <a name="migrate-the-proxy"></a>La migrazione del proxy

Per eseguire la migrazione di un proxy server 2.0 federativo di ADFS a Windows Server 2012, eseguire la procedura seguente:  
  
1.  Per ogni proxy server federativo che si intende eseguire la migrazione a Windows Server 2012, rivedere ed eseguire le procedure descritte in [prepararsi alla migrazione di AD FS 2.0 i Proxy Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Rimuovere un proxy server federativo dal bilanciamento del carico.  
  
3.  Eseguire un aggiornamento sul posto del sistema operativo server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del proxy AD FS nel server andrà persa e il ruolo di AD FS 2.0 server viene rimosso. Suo posto verrà installato il ruolo server ADFS di Windows Server 2012, ma non è configurato. Manualmente, è necessario creare la configurazione originale del proxy AD FS e ripristinare le impostazioni del proxy AD FS rimanenti per completare la migrazione del proxy server federativo.  
  
4.  Creare la configurazione originale del proxy AD FS usando la **configurazione guidata di AD FS Federation Server Proxy**. Per ulteriori informazioni, vedere [configurare un Computer per il ruolo di Proxy Server federativo](configure-a-computer-for-the-federation-server-proxy-role.md). Quando si esegue la procedura guidata, utilizzare le informazioni raccolte in preparare per la migrazione di AD FS 2.0 i Proxy Server federativo come indicato di seguito:  
  
 
|**Opzione di input guidata del Proxy Server federativo**|**Utilizzare il valore seguente**|
|-----|-----|  
|**Nome servizio federativo**|Immettere il valore BaseHostName dal file proxyproperties.txt|  
|**Utilizzare un server proxy HTTP per inviare le richieste a questa federazione** casella di controllo del servizio|Selezionare questa casella se il file proxyproperties.txt contiene un valore per la proprietà ForwardProxyUrl|  
|**Indirizzo del server proxy HTTP**|Immettere il valore ForwardProxyUrl dal file proxyproperties.txt|  
|Richiesta di credenziali|Immettere le credenziali di un account amministratore del server federativo AD FS o l'account del servizio con cui viene eseguito il servizio federativo AD FS.|  
  
5.  Aggiornare le pagine Web di ADFS nel server. Se è stato eseguito il backup delle pagine di proxy ADFS durante la preparazione del proxy server federativo per la migrazione, è possibile utilizzare i dati di backup per sovrascrivere le pagine Web di ADFS che sono state create per impostazione predefinita nel **%systemdrive%\inetpub\adfs\ls** directory in seguito alla configurazione del proxy AD FS in Windows Server 2012.  
  
6.  Aggiungere il server al bilanciamento del carico.  
  
7.  Se hai altri AD FS 2.0 i proxy server federativi per eseguire la migrazione, ripetere i passaggi da 2 a 6 per i computer di proxy server federativo rimanenti.  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)