---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisiti di risoluzione dei nomi per i proxy Server federativi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisiti di risoluzione dei nomi per i proxy Server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando i computer client in Internet tentano di accedere a un'applicazione protetta da Active Directory Federation Services \(AD FS\), devono prima autenticarsi al server federativo. Nella maggior parte dei casi, il server federativo non è in genere direttamente accessibile da Internet. I computer client Internet di conseguenza, devono essere reindirizzati al proxy server federativo invece. È possibile eseguire correttamente il reindirizzamento aggiungendo i record Domain Name System \(DNS\) appropriati per la zona DNS o le zone con connessione Internet.  
  
Il metodo che usi per reindirizzare i client Internet per il proxy server federativo è dipende da come si configura la zona DNS nella rete perimetrale o come si configura una zona DNS che è possibile controllare in Internet. I proxy server federativi devono essere utilizzati in una rete perimetrale. Riescono a reindirizzare le richieste client Internet ai server federativi correttamente solo quando DNS è stato configurato correttamente in tutte le zone con connessione Internet che è possibile controllare. Di conseguenza, la configurazione le zone con connessione Internet, se si dispone di una zona DNS che serve solo la rete perimetrale o una zona DNS che serve sia la rete perimetrale e dai client Internet, è importante.  
  
In questo argomento vengono descritti i passaggi da eseguire per configurare la risoluzione dei nomi quando si inserisce un proxy server federativo nella rete perimetrale. Per determinare la procedura da seguire, determinare innanzitutto quale degli scenari DNS seguenti rispecchia meglio l'infrastruttura DNS nella rete perimetrale dell'organizzazione. Quindi, seguire le istruzioni per tale scenario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS che serve solo la rete perimetrale  
In questo scenario, l'organizzazione dispone di una o due zone DNS nella rete perimetrale e l'organizzazione non controlla le zone DNS in Internet. Risoluzione dei nomi corretta per un proxy server federativo nella zona DNS che serve solo la rete perimetrale dipende dalle condizioni seguenti:  
  
-   Il proxy server federativo deve avere un'impostazione nel file hosts per risolvere il nome di dominio completo dell'URL endpoint server federativo a un indirizzo IP di un server federativo o un cluster di server federativo \(FQDN\) dei nomi.  
  
-   DNS nella rete perimetrale del partner account deve essere configurato in modo che consente di risolvere il nome di dominio completo dell'URL endpoint server federativo per l'indirizzo IP del proxy server federativo.  
  
La figura seguente e i passaggi corrispondenti mostrano come ognuna di queste condizioni si ottiene uno specifico esempio. In questa illustrazione, la tecnologia \(NLB\) bilanciamento carico di rete Microsoft fornisce un singolo nome FQDN del cluster e una singola, indirizzo IP del cluster per una server farm federativa esistente.  
  
![Requisiti dei nomi](media/adfs2_deploy_single_fs.gif)  
  
Per ulteriori informazioni sulla configurazione di un indirizzo IP del cluster o un nome di dominio completo del cluster con bilanciamento carico di rete, vedere [specifica dei parametri del Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurare il file hosts nel proxy server federativo  
Dato che DNS nella rete perimetrale è configurato per risolvere tutte le richieste di fs.fabrikam.com il proxy server federativo di account, il proxy server federativo partner account ha una voce nel file hosts locale per risolvere l'indirizzo IP del server federativo di account effettivo fs.fabrikam.com \ (o nome DNS del cluster per il farm\ server federativi) che è connesso alla rete aziendale. Questo rende possibile per il proxy server federativo di account risolvere il nome host fs.fabrikam.com al server federativo di account, invece che al suo interno, come avverrebbe se tentasse di cercare fs.fabrikam.com con il DNS perimetrale, in modo che il proxy server federativo possa comunicare con il server federativo.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurare il DNS perimetrale  
Perché non esiste solo un singolo ADFS nome host che i computer client vengono indirizzati a, se sono in una rete intranet o Internet, i computer client su Internet che utilizzano il server DNS perimetrale devono risolvere il FQDN per il \(fs.fabrikam.com\) server federativo account all'indirizzo IP del proxy server federativo account nella rete perimetrale. In modo che è possibile inoltrare i client del proxy server federativo di account quando tentano di risolvere fs.fabrikam.com, il DNS perimetrale contiene una zona DNS corp.fabrikam.com limitata con un record di risorse \(A\) host singolo per fs \(fs.fabrikam.com\) e l'indirizzo IP del proxy server federativo di account nella rete perimetrale.  
  
Per ulteriori informazioni su come modificare il file hosts del proxy server federativo e configurare DNS nella rete perimetrale, vedere [configurare la risoluzione dei nomi per un Proxy Server federativo in una zona DNS che serve solo la rete perimetrale](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS che serve sia la rete perimetrale e dai client Internet  
In questo scenario, la tua organizzazione controlla la zona DNS nella rete perimetrale e almeno una zona DNS su Internet. Risoluzione dei nomi corretta per un proxy server federativo in questo scenario dipende dalle condizioni seguenti:  
  
-   DNS nella zona Internet del partner account deve essere configurato in modo che consente di risolvere il FQDN del nome di host del server federativo per l'indirizzo IP del proxy server federativo nella rete perimetrale.  
  
-   DNS nella rete perimetrale del partner account deve essere configurato in modo che consente di risolvere il FQDN del nome di host del server federativo per l'indirizzo IP del server federativo nella rete aziendale.  
  
La figura seguente e i passaggi corrispondenti mostrano come ognuna di queste condizioni si ottiene uno specifico esempio.  
  
![Requisiti dei nomi](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurare il DNS perimetrale  
Per questo scenario, perché si presuppone che si configurerà la zona DNS Internet controllata per risolvere le richieste eseguite per uno specifico URL endpoint \ (ovvero, fs.fabrikam.com\) per il proxy server federativo nella rete perimetrale, è inoltre necessario configurare la zona nel DNS perimetrale per inoltrare queste richieste al server federativo nella rete aziendale.  
  
In modo che i client possono essere inoltrati al server federativo di account quando tentano di risolvere fs.fabrikam.com, DNS perimetrale è configurato con un record di risorse \(A\) host singolo per fs \(fs.fabrikam.com\) e l'indirizzo IP del server federativo di account nella rete aziendale. Questo rende possibile per il proxy server federativo di account risolvere il nome host fs.fabrikam.com al server federativo di account, invece che al suo interno, come avverrebbe se tentasse di cercare fs.fabrikam.com con il DNS Internet, in modo che il proxy server federativo possa comunicare con il server federativo.  
  
### <a name="2-configure-internet-dns"></a>2. Configurare DNS Internet  
Per la risoluzione dei nomi per riuscire in questo scenario, tutte le richieste dai computer client su Internet a fs.fabrikam.com devono essere risolte dalla zona DNS Internet controllata dall'utente. Di conseguenza, è necessario configurare la zona DNS Internet per inoltrare le richieste client per fs.fabrikam.com all'indirizzo IP del proxy server federativo account nella rete perimetrale.  
  
Per ulteriori informazioni su come modificare la rete perimetrale e le zone DNS Internet, vedere [configurare la risoluzione dei nomi per un Proxy Server federativo in un DNS zona che serve sia la rete perimetrale e dai client Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
